## Tunnel: 
    ssh -MS /tmp/jump student@10.50.x.x
_________________________________________________________________________________________________________________
## Dynamic Forward: 
    ssh -S /tmp/jump jump -O forward -D9050
_________________________________________________________________________________________________________________
## Ping Sweep: 
    for i in {1..254}; do (ping -c 1 X.X.X.$i | grep "bytes from" &) ; done
_________________________________________________________________________________________________________________
## Port Forward:
    ssh -S /tmp/jump jump -O forward -L12345:1.2.3.4:22
_________________________________________________________________________________________________________________
## SCP local "file.sh" TO remote host: 
    scp -P 12345 file.sh student@127.0.0.1:/home/student2/documents                ##Linux

    scp -P 12345 file.sh student@127.0.0.1:C:/users/student/desktop                ##Windows
_________________________________________________________________________________________________________________
## SCP file "script.sh" FROM remote host: 
    scp -P 12345 student@127.0.0.1:/home/student2/documents/script.sh .            ##Linux

    scp -P 12345 student@127.0.0.1:C:/users/student/desktop/script.sh .            ##Windows
_________________________________________________________________________________________________________________
# Scheme of Maneuver:
## >Jump Box
## ->T1: 192.168.28.100
## -->T3:x.x.x.9

## Target Section:

## T1
### Hostname: Donovian_Extranet
### IP: 192.168.28.100
### OS: CentOS
### Creds:Unknown
### Last Known SSH Port: 2222
### PSP: none
### Malware: none
### Action: Perform SSH masquerade and survey system. Identify redirection to the next target.

## T2
### Hostname: Intranet
### IP: 192.168.150.253
### Creds: comrade::StudentMidwayPassword
### Last Known SSH Port: 3201

## T3
### Hostname: Internal
### IP: 192.168.28.9
### OS: unknown
### Creds:comrade::StudentMidwayPassword
### Last Known SSH Port: unknown but you can use RDP 
    xfreerdp /v:192.168.28.9 /u:comrade /p:StudentMidwayPassword /size:1920x1000 +clipboard
### PSP: Unknown
### Malware: Unknown
### Action: Gain access; survey host and map Donovian internal Cyberspace.
```
Nmap scan report for 192.168.28.9
Host is up (0.00099s latency).
Not shown: 65519 closed ports
PORT      STATE SERVICE
135/tcp   open  msrpc
139/tcp   open  netbios-ssn
445/tcp   open  microsoft-ds
3389/tcp  open  ms-wbt-server
```



