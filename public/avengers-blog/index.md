# Avengers Blog - TryHackMe

**Room Link: https://tryhackme.com/room/avengers**

**Room deficulty: easy**


## void main()

this is a beginner level room to practice enumeration, sql injection and command injection.

## [Task 2] Cookies

if you goto the room url and open dev tools -> Application -> Cookies, you will see the following cookies:
1. flag1
2. connect.sid

{{< image src="images/cookies.jpg" width="100%" caption="cookies" >}}

copy and paste the value of the `flag1` cookie in the answer field and submit.

## [Task 3] HTTP Headers

open Dev tools -> Network

refresh the page

click on the base url request and view Response headers in the headers tab.

{{< image src="images/headers.jpg" width="100%" caption="headers">}}

here you can find the `flag2`.

## [Task 4] Enumeration and FTP

scan the target machine with nmap.

```bash
nmap {machine_ip}
```
here is the result:
```bash
┌──(kali㉿kali)-[~]
└─$ nmap 10.10.76.129 -v                                                                                                            130 ⨯
Starting Nmap 7.92 ( https://nmap.org ) at 2021-12-11 02:08 EST
Initiating Ping Scan at 02:08
Scanning 10.10.76.129 [2 ports]
Completed Ping Scan at 02:08, 0.61s elapsed (1 total hosts)
Initiating Parallel DNS resolution of 1 host. at 02:08
Completed Parallel DNS resolution of 1 host. at 02:08, 0.00s elapsed
Initiating Connect Scan at 02:08
Scanning 10.10.76.129 [1000 ports]
Discovered open port 22/tcp on 10.10.76.129
Discovered open port 21/tcp on 10.10.76.129
Discovered open port 80/tcp on 10.10.76.129
Increasing send delay for 10.10.76.129 from 0 to 5 due to max_successful_tryno increase to 4
Increasing send delay for 10.10.76.129 from 5 to 10 due to max_successful_tryno increase to 5
Increasing send delay for 10.10.76.129 from 10 to 20 due to max_successful_tryno increase to 6
Increasing send delay for 10.10.76.129 from 20 to 40 due to 11 out of 14 dropped probes since last increase.
Connect Scan Timing: About 28.04% done; ETC: 02:09 (0:01:20 remaining)
Completed Connect Scan at 02:09, 67.75s elapsed (1000 total ports)
Nmap scan report for 10.10.76.129
Host is up (0.43s latency).
Not shown: 997 closed tcp ports (conn-refused)
PORT   STATE SERVICE
21/tcp open  ftp
22/tcp open  ssh
80/tcp open  http

Read data files from: /usr/bin/../share/nmap
Nmap done: 1 IP address (1 host up) scanned in 68.42 seconds
```
new we just need to connect to the port 21 and login with the username `groot` and password `iamgroot`.

use the following command:
```bash
ftp {machine_ip}
```
it will prompt you for username and password. just login as groot.
```bash
┌──(kali㉿kali)-[~]
└─$ ftp 10.10.76.129
Connected to 10.10.76.129.
220 (vsFTPd 3.0.3)
Name (10.10.76.129:kali): groot
331 Please specify the password.
Password:
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp>
```
now you just need to know what contents are avaliable there, to know that use `ls` command.
```bash
ftp> ls
200 PORT command successful. Consider using PASV.
150 Here comes the directory listing.
drwxr-xr-x    2 1001     1001         4096 Oct 04  2019 files
226 Directory send OK.
ftp>
```
as we can see there is a directory called `files` let's move into it with command `cd files`.
```bash
ftp> cd files
250 Directory successfully changed.
ftp>
```
let's see what files are there. with `ls` command.
```bash
ftp> ls
200 PORT command successful. Consider using PASV.
150 Here comes the directory listing.
-rw-r--r--    1 0        0              33 Oct 04  2019 flag3.txt
226 Directory send OK.
ftp>
```
as we can see there is a file called `flag3.txt` let's download it. with `get` command.
```bash
ftp> get flag3.txt
local: flag3.txt remote: flag3.txt
200 PORT command successful. Consider using PASV.
150 Opening BINARY mode data connection for flag3.txt (33 bytes).
226 Transfer complete.
33 bytes received in 0.00 secs (16.0411 kB/s)
ftp>
```
now just close the ftp connection with `exit` command. then see the contents of the file with `cat flag3.txt`.
```bash
┌──(kali㉿kali)-[~]
└─$ cat flag3.txt
8fc651a739befc58d450dc48e1f1fd2e
```
just copy the value of the `flag3` and submit it.
## [Task 5] GoBuster
go buster is a tool that can be used to discover directories and files on a web server.

but it requires a wordlist, threare some amazing wordlists available on kali linux by default.

I like `/usr/share/wordlists/dirb/small.txt`.
```bash
┌──(kali㉿kali)-[/usr/share/wordlists/dirbuster]
└─$ gobuster dir -u http://10.10.76.129 -w /usr/share/wordlists/dirb/small.txt
===============================================================
Gobuster v3.1.0
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.10.76.129
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/wordlists/dirb/small.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.1.0
[+] Timeout:                 10s
===============================================================
2021/12/11 02:35:35 Starting gobuster in directory enumeration mode
===============================================================
/assets               (Status: 301) [Size: 179] [--> /assets/]
/css                  (Status: 301) [Size: 173] [--> /css/]
/home                 (Status: 302) [Size: 23] [--> /]
/img                  (Status: 301) [Size: 173] [--> /img/]
/js                   (Status: 301) [Size: 171] [--> /js/]
/logout               (Status: 302) [Size: 29] [--> /portal]
/portal               (Status: 200) [Size: 1409]

===============================================================
2021/12/11 02:36:51 Finished
===============================================================
```
here we found `/portal` and `/logout` directories.
## [Task 7] SQL Injection

lets try to login with the username `groot` and password `iamgroot`.
but it says that the password is wrong. maybe groot is changed his password.

let's try an sql injection.
{{< image src="images/portal.jpg" width="100%" caption="sql injection" >}}

now we just need to view source of the page.
{{< image src="images/view-source.jpg" width="100%" caption="view source">}}
## [Task 7] Remote Code Execution and Linux
we can also use `less` command instead of `cat` command.

{{< image src="images/rce.jpg" width="100%" caption="remote code execution">}}
submit the flag you found.

**by [@rohitkumarankam](https://rohitkumarankam.com)**
