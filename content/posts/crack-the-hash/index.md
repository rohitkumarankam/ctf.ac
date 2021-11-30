---
title: "Crack the hash - Tryhackme"
date: 2021-11-29
draft: false
tags: ["hashing","cryptography", "hashcat"]
categories: ["cryptography"]
featuredImage: "thumbnail.png"
summary: "Cracking hashes."
---


**Room Link: https://tryhackme.com/room/crackthehash**

There are two ways to crack the hash.

1. **Online cracking (https://crackstation.net)**
2. **Offline cracking (hashcat)**

first of all, we need to know the hash algorithm.
to know the hash algorithm, I use [Name-That-Hash](https://en.kali.tools/?p=1556).

to install you need to run the following command:

```bash
apt install -y name-that-hash
```
**usage:**
```bash
nth --text "hash"
```
<br>

## Level 1

level 1 has 5 challenges.

### Challenge 1

```hash
48bb6e862e54f2a795ffc4e541caed4d
```

we can find the hash type by running:

```bash
nth --text "48bb6e862e54f2a795ffc4e541caed4d"
```


{{< image src="hashtypefirst.png" width="100%" caption="name-that-hash result">}}

we can easily tell this is a md5 hash. because the string length is 32.
[crackstation](https://crackstation.net/) is a good to use here.

we can easily crack this by using [crackstation.net](https://crackstation.net/)

{{< image src="first-hash.png" width="100%" caption="`crackstation result`">}}

{{< admonition success "Answer of Level 1 Challenge 1" true >}}
**easy**
{{< /admonition >}}

<br>

### challenge 2

{{< image src="hashtypesecond.png" width="100%" caption="name-that-hash result">}}

we can solve **SHA-1** by using https://crackstation.net.

```hash
CBFDAC6008F9CAB4083784CBD1874F76618D2A97
```
{{< image src="second-hash.png" width="100%" caption="`crackstation result`">}}

{{< admonition success "Answer of Level 1 Challenge 2" true >}}
**password123**
{{</ admonition >}}

<br>

### challenge 3
```hash
1C8BFE8F801D79745C4631D09FFF36C82AA37FC4CCE4FC946683D7B336B63032
```
{{< image src="hashtypethird.png" width="100%" caption="name-that-hash result">}}

we can solve **SHA-256** by using https://crackstation.net.

{{< image src="third-hash.png" width="100%" caption="`crackstation result`">}}

{{< admonition success "Answer of Level 1 Challenge 3" true >}}
**letmein**
{{</ admonition >}}

<br>

### challenge 4
```hash
$2y$12$Dwt1BZj6pcyc3Dy1FWZ5ieeUznr71EeNkJkUlypTsgbX1H68wsRom
```
{{< image src="hashtypefourth.png" width="100%" caption="name-that-hash result">}}

We cannot solve **bcrypt** hashes with online cracking tools. We need to use offline cracking tools.
I use [hashcat](https://hashcat.net/hashcat/) because it uses gpu to crack the hashes faster.
```bash
hashcat -m 3200 hash.txt /usr/share/wordlists/rockyou.txt
```

{{< admonition danger "Warning" true >}}
**Don't try** to crack this hash. Because bcrypt hash is hard to calculate.
It took me **Three hours** with **GTX 1650 GPU**.
{{</ admonition >}}

{{< admonition success "Answer of Level 1 Challenge 4" true >}}
**bleh**
{{</ admonition >}}

<br>

### challenge 5
```
279412f945939ba78ce0758d3fd83daa
```
{{< image src="hashtypefifth.png" width="100%" caption="name-the-hash result">}}
{{< image src="fifth-hash.png" width="100%" caption="`crackstation result`">}}
{{< admonition success "Answer of Level 1 Challenge 5" true >}}
**Eternity22**
{{</ admonition >}}
<br>

## Level 2
all of the challenges in Level 2 are can be solved with hashcat and rockyou.txt.
### challenge 1
```
F09EDCB1FCEFC6DFB23DC3505A882655FF77375ED8AA2D1C13F640FCCC2D0C85
```
{{< image src="firsthash.png" width="100%" caption="name-the-hash result">}}

```bash
┌──(kali㉿kali)-[~]
└─$ hashcat -m 1400 "F09EDCB1FCEFC6DFB23DC3505A882655FF77375ED8AA2D1C13F640FCCC2D0C85" /usr/share/wordlists/rockyou.txt 
hashcat (v6.1.1) starting...

OpenCL API (OpenCL 1.2 pocl 1.6, None+Asserts, LLVM 9.0.1, RELOC, SLEEF, DISTRO, POCL_DEBUG) - Platform #1 [The pocl project]
=============================================================================================================================
* Device #1: pthread-Intel(R) Core(TM) i5-9400F CPU @ 2.90GHz, 5837/5901 MB (2048 MB allocatable), 4MCU

Minimum password length supported by kernel: 0
Maximum password length supported by kernel: 256

Hashes: 1 digests; 1 unique digests, 1 unique salts
Bitmaps: 16 bits, 65536 entries, 0x0000ffff mask, 262144 bytes, 5/13 rotates
Rules: 1

f09edcb1fcefc6dfb23dc3505a882655ff77375ed8aa2d1c13f640fccc2d0c85:paule
                                                 
Session..........: hashcat
Status...........: Cracked
Hash.Name........: SHA2-256
Hash.Target......: f09edcb1fcefc6dfb23dc3505a882655ff77375ed8aa2d1c13f...2d0c85
Time.Started.....: Tue Nov 30 11:18:03 2021 (1 sec)
Time.Estimated...: Tue Nov 30 11:18:04 2021 (0 secs)
Guess.Base.......: File (/usr/share/wordlists/rockyou.txt)
Guess.Queue......: 1/1 (100.00%)
Speed.#1.........:  1402.7 kH/s (0.39ms) @ Accel:1024 Loops:1 Thr:1 Vec:8
Recovered........: 1/1 (100.00%) Digests
Progress.........: 81920/14344385 (0.57%)
Rejected.........: 0/81920 (0.00%)
Restore.Point....: 77824/14344385 (0.54%)
Restore.Sub.#1...: Salt:0 Amplifier:0-1 Iteration:0-1
Candidates.#1....: superm -> janson

Started: Tue Nov 30 11:18:00 2021
Stopped: Tue Nov 30 11:18:05 2021
```
{{< admonition success "Answer of Level 2 Challenge 1" true >}}
**paule**
{{</ admonition >}}
<br>

### challenge 2
```
1DFECA0C002AE40B8619ECF94819CC1B
```
{{< image src="secondhash.png" width="100%" caption="name-the-hash result">}}
```bash
┌──(kali㉿kali)-[~]
└─$ hashcat -m 1000 "1DFECA0C002AE40B8619ECF94819CC1B" /usr/share/wordlists/rockyou.txt 
hashcat (v6.1.1) starting...

OpenCL API (OpenCL 1.2 pocl 1.6, None+Asserts, LLVM 9.0.1, RELOC, SLEEF, DISTRO, POCL_DEBUG) - Platform #1 [The pocl project]
=============================================================================================================================
* Device #1: pthread-Intel(R) Core(TM) i5-9400F CPU @ 2.90GHz, 5837/5901 MB (2048 MB allocatable), 4MCU

Minimum password length supported by kernel: 0
Maximum password length supported by kernel: 256

Hashes: 1 digests; 1 unique digests, 1 unique salts
Bitmaps: 16 bits, 65536 entries, 0x0000ffff mask, 262144 bytes, 5/13 rotates
Rules: 1

Watchdog: Hardware monitoring interface not found on your system.
Watchdog: Temperature abort trigger disabled.

Host memory required for this attack: 65 MB

1dfeca0c002ae40b8619ecf94819cc1b:n63umy8lkf4i    
                                                 
Session..........: hashcat
Status...........: Cracked
Hash.Name........: NTLM
Hash.Target......: 1dfeca0c002ae40b8619ecf94819cc1b
Time.Started.....: Tue Nov 30 11:27:03 2021 (1 sec)
Time.Estimated...: Tue Nov 30 11:27:04 2021 (0 secs)
Guess.Base.......: File (/usr/share/wordlists/rockyou.txt)
Guess.Queue......: 1/1 (100.00%)
Speed.#1.........:  3446.2 kH/s (0.16ms) @ Accel:1024 Loops:1 Thr:1 Vec:8
Recovered........: 1/1 (100.00%) Digests
Progress.........: 5242880/14344385 (36.55%)
Rejected.........: 0/5242880 (0.00%)
Restore.Point....: 5238784/14344385 (36.52%)
Restore.Sub.#1...: Salt:0 Amplifier:0-1 Iteration:0-1
Candidates.#1....: n6ri2fdkgm9y -> n1ckow3n

Started: Tue Nov 30 11:26:48 2021
Stopped: Tue Nov 30 11:27:06 2021
```
{{< admonition success "Answer of Level 2 Challenge 2" true >}}
**n63umy8lkf4i**
{{</ admonition >}}
<br>

### challenge 3
```
$6$aReallyHardSalt$6WKUTqzq.UQQmrm0p/T7MPpMbGNnzXPMAXi4bJMl9be.cfi3/qxIf.hsGpS41BqMhSrHVXgMpdjS6xeKZAs02.
```
<br>

{{< image src="thirdhash.png" width="100%" caption="name-the-hash result">}}
```ps
PS C:\Users\rohit> hashcat -m 1800 C:\hash\hash.txt C:\hash\rockyou.txt
hashcat (v6.2.5) starting

CUDA API (CUDA 11.5)
====================
* Device #1: NVIDIA GeForce GTX 1650, 3327/4095 MB, 14MCU

OpenCL API (OpenCL 3.0 CUDA 11.5.107) - Platform #1 [NVIDIA Corporation]
========================================================================
* Device #2: NVIDIA GeForce GTX 1650, skipped

Minimum password length supported by kernel: 0
Maximum password length supported by kernel: 256

Hashes: 1 digests; 1 unique digests, 1 unique salts
Bitmaps: 16 bits, 65536 entries, 0x0000ffff mask, 262144 bytes, 5/13 rotates
Rules: 1

Host memory required for this attack: 281 MB

Dictionary cache hit:
* Filename..: C:\hash\rockyou.txt
* Passwords.: 14344384
* Bytes.....: 139921497
* Keyspace..: 14344384

$6$aReallyHardSalt$6WKUTqzq.UQQmrm0p/T7MPpMbGNnzXPMAXi4bJMl9be.cfi3/qxIf.hsGpS41BqMhSrHVXgMpdjS6xeKZAs02.:waka99

Session..........: hashcat
Status...........: Cracked
Hash.Mode........: 1800 (sha512crypt $6$, SHA512 (Unix))
Hash.Target......: $6$aReallyHardSalt$6WKUTqzq.UQQmrm0p/T7MPpMbGNnzXPM...ZAs02.
Time.Started.....: Tue Nov 30 22:10:53 2021 (2 mins, 48 secs)
Time.Estimated...: Tue Nov 30 22:13:41 2021 (0 secs)
Kernel.Feature...: Pure Kernel
Guess.Base.......: File (C:\hash\rockyou.txt)
Guess.Queue......: 1/1 (100.00%)
Speed.#1.........:    17015 H/s (11.33ms) @ Accel:512 Loops:32 Thr:64 Vec:1
Recovered........: 1/1 (100.00%) Digests
Progress.........: 2850816/14344384 (19.87%)
Rejected.........: 0/2850816 (0.00%)
Restore.Point....: 2818048/14344384 (19.65%)
Restore.Sub.#1...: Salt:0 Amplifier:0-1 Iteration:4992-5000
Candidate.Engine.: Device Generator
Candidates.#1....: wardreen4 -> vs0924
Hardware.Mon.#1..: Temp: 61c Fan: 31% Util: 96% Core:2005MHz Mem:3993MHz Bus:16

Started: Tue Nov 30 22:10:52 2021
Stopped: Tue Nov 30 22:13:42 2021
```
{{< admonition success "Answer of Level 2 Challenge 3" true>}}
**waka99**
{{</ admonition >}}
<br>

### challenge 4
```
e5d8870e5bdd26602cab8dbe07a942c8669e56d6:tryhackme
```
{{< image src="fourthhash.png" width="100%" caption="**name-the-hash result**">}}
```bash
┌──(kali㉿kali)-[~]
└─$ hashcat -m 160 e5d8870e5bdd26602cab8dbe07a942c8669e56d6:tryhackme /usr/share/wordlists/rockyou.txt 
hashcat (v6.1.1) starting...

OpenCL API (OpenCL 1.2 pocl 1.6, None+Asserts, LLVM 9.0.1, RELOC, SLEEF, DISTRO, POCL_DEBUG) - Platform #1 [The pocl project]
=============================================================================================================================
* Device #1: pthread-Intel(R) Core(TM) i5-9400F CPU @ 2.90GHz, 5837/5901 MB (2048 MB allocatable), 4MCU

Minimum password length supported by kernel: 0
Maximum password length supported by kernel: 256

Hashes: 1 digests; 1 unique digests, 1 unique salts
Bitmaps: 16 bits, 65536 entries, 0x0000ffff mask, 262144 bytes, 5/13 rotates
Rules: 1

Host memory required for this attack: 65 MB

Dictionary cache hit:
* Filename..: /usr/share/wordlists/rockyou.txt
* Passwords.: 14344385
* Bytes.....: 139921507
* Keyspace..: 14344385

e5d8870e5bdd26602cab8dbe07a942c8669e56d6:tryhackme:481616481616
                                                 
Session..........: hashcat
Status...........: Cracked
Hash.Name........: HMAC-SHA1 (key = $salt)
Hash.Target......: e5d8870e5bdd26602cab8dbe07a942c8669e56d6:tryhackme
Time.Started.....: Tue Nov 30 12:09:16 2021 (4 secs)
Time.Estimated...: Tue Nov 30 12:09:20 2021 (0 secs)
Guess.Base.......: File (/usr/share/wordlists/rockyou.txt)
Guess.Queue......: 1/1 (100.00%)
Speed.#1.........:  3143.0 kH/s (0.53ms) @ Accel:1024 Loops:1 Thr:1 Vec:8
Recovered........: 1/1 (100.00%) Digests
Progress.........: 12316672/14344385 (85.86%)
Rejected.........: 0/12316672 (0.00%)
Restore.Point....: 12312576/14344385 (85.84%)
Restore.Sub.#1...: Salt:0 Amplifier:0-1 Iteration:0-1
Candidates.#1....: 48162450 -> 480281

Started: Tue Nov 30 12:09:01 2021
Stopped: Tue Nov 30 12:09:21 2021
```
{{< admonition success "Answer of Level 2 Challenge 4" true >}}
**481616481616**
{{</ admonition >}}
<br>

**by [@RohitKumarAnkam](https://Rohitkumarankam.com)**