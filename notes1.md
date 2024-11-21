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
    for i in {225..254}; do (ping -c 1 192.168.28.$i | grep "bytes from" &) ; done
________________________________________________________________________________________________________________
## Discovered IP's 192.168.28.X...
________________________________________________________________________________________________________________
## Cancel the first dynamic port forward: 
    ssh -S /tmp/jump -O cancel -D9050
________________________________________________________________________________________________________________
## To be able to establish new dynamic port forward: 
    ssh -S /tmp/t1 t1 -O forward -D9050
________________________________________________________________________________________________________________
## Scan the IP's you found: 
    proxychains nmap -T4 -Pn 100.200.25.30,35
________________________________________________________________________________________________________________
## Discovered ports 80 and 2222 on both devices...
________________________________________________________________________________________________________________
## Netcat the ports found for those devices: 
    proxychains nc 100.200.25.30 80
________________________________________________________________________________________________________________
## New port forward to these devices:
    ssh -S /tmp/t1 t1 -O forward -L2424:100.200.25.30:2222 -L3535:100.200.25.30:80
________________________________________________________________________________________________________________
## Open firefox and enter the loopback and port tied to the port forwards above:
    http://127.0.0.1:3333
________________________________________________________________________________________________________________
## *Authenticate* with second target using previous tunnel: 
    ssh -MS /tmp/t2 creds@127.0.0.1 -p2323 
________________________________________________________________________________________________________________
# Repeat steps (as needed) from the first *Authenticate* to the second *Authenticate*.
________________________________________________________________________________________________________________
