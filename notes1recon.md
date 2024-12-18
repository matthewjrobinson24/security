# Resources & Creds:

Stack Number: 12

Username: MARO-007-M

Password: hKvKjAtprH9FS0U

Jump: 10.50.25.229
________________________________________________________________________________________________________________
vta.cybbh.space
________________________________________________________________________________________________________________
    10.50.20.103:8000

Username: MARO-007-M

Password: hKvKjAtprH9FS0U
________________________________________________________________________________________________________________
duckduckgo
________________________________________________________________________________________________________________
    https://sec.cybbh.io/public/security/latest/index.html
________________________________________________________________________________________________________________
# SCRIPTS INSIDE OPSTATION /USR/SHARE/NMAP/SCRIPTS
________________________________________________________________________________________________________________
## Creating our master socket to allow tunneling:
    ssh -MS /tmp/jump student@10.50.25.229
________________________________________________________________________________________________________________
## Scheme of Maneuver:
## >Jump Box
## ->Network scan: 192.168.28.96/27
## -->Network scan:192.168.150.224/27
________________________________________________________________________________________________________________
## Ping sweeping the network to see what devices are up: 
    for i in {97..126}; do (ping -c 1 192.168.28.$i | grep "bytes from" &) ; done
```
64 bytes from 192.168.28.100: icmp_seq=1 ttl=63 time=1.08 ms
64 bytes from 192.168.28.98: icmp_seq=1 ttl=63 time=1.87 ms
64 bytes from 192.168.28.99: icmp_seq=1 ttl=63 time=1.58 ms
64 bytes from 192.168.28.97: icmp_seq=1 ttl=64 time=0.111 ms
64 bytes from 192.168.28.105: icmp_seq=1 ttl=63 time=0.884 ms
64 bytes from 192.168.28.111: icmp_seq=1 ttl=63 time=0.631 ms
64 bytes from 192.168.28.120: icmp_seq=1 ttl=63 time=0.704 ms
```
________________________________________________________________________________________________________________
## Discovered IP's 192.168.28.97,98,99,100,105,111,120...
________________________________________________________________________________________________________________
## Making our dynamic port forward to utilize our tools: 
    ssh -S /tmp/jump jump -O forward -D9050
________________________________________________________________________________________________________________
## Nmapping the IP's that responded to identify ports: 
    proxychains nmap -T4 -Pn 192.168.28.97,98,99,100,105,111,120
```
Nmap scan report for 192.168.28.97
Host is up (0.00052s latency).
All 1000 scanned ports on 192.168.28.97 are closed
Nmap scan report for 192.168.28.98
Host is up (0.00064s latency).
Not shown: 999 closed ports

PORT   STATE SERVICE
53/tcp open  domain
---------------------------------------------------
Nmap scan report for 192.168.28.99
Host is up (0.00068s latency).
Not shown: 999 closed ports

PORT   STATE SERVICE
53/tcp open  domain
---------------------------------------------------
Nmap scan report for 192.168.28.100
Host is up (0.00073s latency).
Not shown: 998 closed ports

PORT     STATE SERVICE
80/tcp   open  http
2222/tcp open  EtherNetIP-1
---------------------------------------------------
Nmap scan report for 192.168.28.105
Host is up (0.00060s latency).
Not shown: 997 closed ports

PORT     STATE SERVICE
21/tcp   open  ftp
23/tcp   open  telnet
2222/tcp open  EtherNetIP-1
---------------------------------------------------
Nmap scan report for 192.168.28.111
Host is up (0.00060s latency).
Not shown: 997 closed ports

PORT     STATE SERVICE
80/tcp   open  http
2222/tcp open  EtherNetIP-1
8080/tcp open  http-proxy
---------------------------------------------------
Nmap scan report for 192.168.28.120
Host is up (0.00065s latency).
Not shown: 999 closed ports

PORT     STATE SERVICE
4242/tcp open  vrml-multi-use
Nmap done: 7 IP addresses (7 hosts up) scanned in 4.62 seconds
```
________________________________________________________________________________________________________________
## Netcat the ports found for those devices:
    proxychains nc 192.168.28.100 80
    proxychains nc 192.168.28.100 2222
    proxychains nc 192.168.28.105 21
    proxychains nc 192.168.28.105 23
    proxychains nc 192.168.28.105 2222
    proxychains nc 192.168.28.111 80
    proxychains nc 192.168.28.111 2222
    proxychains nc 192.168.28.111 8080
    proxychains nc 192.168.28.120 4242
________________________________________________________________________________________________________________
## New port forward to these devices:
    ssh -S /tmp/jump jump -O forward -L20001:192.168.28.100:80 -L20002:192.168.28.100:2222 -L20003:192.168.28.105:21 -L20004:192.168.28.105:23 -L20005:192.168.28.105:2222 -L20006:192.168.28.111:80 -L20007:192.168.28.111:2222 -L20008:192.168.28.111:8080 -L20009:192.168.28.120:4242

```
-L20001:192.168.28.100:80 
-L20002:192.168.28.100:2222 
-L20003:192.168.28.105:21 
-L20004:192.168.28.105:23 
-L20005:192.168.28.105:2222 
-L20006:192.168.28.111:80 
-L20007:192.168.28.111:2222 
-L20008:192.168.28.111:8080 
-L20009:192.168.28.120:4242 
```
________________________________________________________________________________________________________________
## Open firefox and enter the loopback and port tied to the port forwards above:
    firefox http://127.0.0.1:20001
    firefox http://127.0.0.1:20006
    firefox http://127.0.0.1:20008
________________________________________________________________________________________________________________
## Search for transluscent/opacity text on ANY of the tabs that match what you are looking for...
## Also worth looking at the class= field...
________________________________________________________________________________________________________________
## *Authenticate* with first target: (found in FTP from wget -r also to the .120 - YourTempPassword)
    ssh -MS /tmp/t1 creds@127.0.0.1 -p2222 (ssh/alt ssh port)
________________________________________________________________________________________________________________
## Ping sweep the new network: 
    for i in {225..254}; do (ping -c 1 192.168.150.$i | grep "bytes from" &) ; done
```
64 bytes from 192.168.150.225: icmp_seq=1 ttl=64 time=0.840 ms
64 bytes from 192.168.150.226: icmp_seq=1 ttl=63 time=1.21 ms
64 bytes from 192.168.150.245: icmp_seq=1 ttl=127 time=1.10 ms
64 bytes from 192.168.150.227: icmp_seq=1 ttl=63 time=53.7 ms
64 bytes from 192.168.150.253: icmp_seq=1 ttl=63 time=0.570 ms
```
________________________________________________________________________________________________________________
## Discovered IP's 192.168.150.225,226,227,245,253...
________________________________________________________________________________________________________________
## Cancel the first dynamic port forward: 
    ssh -S /tmp/jump jump -O cancel -D9050
________________________________________________________________________________________________________________
## To be able to establish new dynamic port forward: 
    ssh -S /tmp/t1 t1 -O forward -D9050
________________________________________________________________________________________________________________
## Scan the IP's you found: 
    proxychains nmap -T4 -Pn 192.168.150.225,226,227,245,253
```
Nmap scan report for 192.168.150.226
Host is up (0.00096s latency).
Not shown: 999 closed ports
PORT   STATE SERVICE
53/tcp open  domain

Nmap scan report for 192.168.150.227
Host is up (0.00096s latency).
Not shown: 999 closed ports
PORT   STATE SERVICE
53/tcp open  domain

Nmap scan report for 192.168.150.245
Host is up (0.0010s latency).
Not shown: 995 closed ports
PORT     STATE SERVICE
135/tcp  open  msrpc
139/tcp  open  netbios-ssn
445/tcp  open  microsoft-ds
3389/tcp open  ms-wbt-server
9999/tcp open  abyss

Nmap scan report for 192.168.150.253
Host is up (0.00092s latency).
Not shown: 998 closed ports
PORT    STATE SERVICE
80/tcp  open  http
514/tcp open  shell

Nmap done: 5 IP addresses (5 hosts up) scanned in 4.52 seconds
```   
________________________________________________________________________________________________________________
## Netcat the ports found for those devices: 
    proxychains nc 192.168.150.226 53
    proxychains nc 192.168.150.227 53
    proxychains nc 192.168.150.245 135
    proxychains nc 192.168.150.245 139
    proxychains nc 192.168.150.245 3389
    proxychains nc 192.168.150.245 9999
    proxychains nc 192.168.150.253 80
    proxychains nc 192.168.150.253 514
________________________________________________________________________________________________________________
## Script scan the one with smb:
    proxychains nmap -T4 -Pn 192.168.150.245 --script=smb-os-discovery.nse
    proxychains nmap -T4 -Pn 192.168.150.245,253 --script=http-title.nse
________________________________________________________________________________________________________________
## New port forward to these devices:
________________________________________________________________________________________________________________
## Open firefox and enter the loopback and port tied to the port forwards above for http:
________________________________________________________________________________________________________________
## *Authenticate* with second target using previous tunnel: 
________________________________________________________________________________________________________________
# Repeat steps (as needed) from the first *Authenticate* to the second *Authenticate*.
________________________________________________________________________________________________________________
