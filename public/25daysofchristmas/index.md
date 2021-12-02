# Advent of Cyber 1 [2019] - TryHackMe


**Room Link: https://tryhackme.com/room/25daysofchristmas**

**Room Difficulty: Easy**

<br>

## [Day 1] Inventory Management
deploy the room machine.
go to http://machineip:3000/
then it will redirect you to login page.
{{< image src="images/day1/loginpage.png" width="100%" caption="login page">}}
register a new user account and login with it.

credidentials i used:

* username: fakeuserrohit
* password: fakepasswordrohit

now you see a home page like this.
{{< image src="images/day1/homepage.png" width="100%" caption="home page">}}

### what is the name of the cookie used for authentication?
Open devtools>storage>cookies 
{{< image src="images/day1/cookie.png" width="100%" caption="cookies">}}
{{< admonition success "Answer" false >}}
**authid**
{{</ admonition >}}
### if you decode the cookie, what is the value of the fixed part of the cookie?
{{< admonition note "Note" true >}}
cookie is encoded with base64
{{</ admonition >}}
```bash
echo "base64encodedstring" | base64 -d
```
```bash
┌──(kali㉿kali)-[~]
└─$ echo "ZmFrZXVzZXJyb2hpdHY0ZXI5bGwxIXNz" | base64 -d
fakeuserrohitv4er9ll1!ss
```
cookie format is: **username+fixedpart**
{{< admonition success "Answer" false >}}
**v4er9ll1!ss**
{{</ admonition >}}
### after accessing his account, what did the user mcinventory request?
to find the answer to this question we need to encode *mcinventory* username with the fixed part of the cookie.
```bash
┌──(kali㉿kali)-[~]
└─$ echo -n "mcinventoryv4er9ll1!ss" | base64
bWNpbnZlbnRvcnl2NGVyOWxsMSFzcw==
```
set cookie to `bWNpbnZlbnRvcnl2NGVyOWxsMSFzcw` and refresh the page
{{< image src="images/day1/inventory.png" width="100%" caption="inventory page">}}

{{< admonition success "Answer" false >}}
**firewall**
{{</ admonition >}}
<br>

## [Day 2] Arctic Forum
deploy the room machine.
go to http://machineip:3000/
then it will redirect you to login page.
{{< image src="images/day2/loginpage.png" width="100%" caption="login page">}}
there is no link to register a new user account.
bruit force is not useful here.
### what is the path of the hidden page?
so let's launch dirbuster with medium dictionary.
{{< image src="images/day2/dirbuster.png" width="100%" caption="dirbuster">}}
{{< admonition success "Answer" false >}}
**/sysadmin**
{{</ admonition >}}
### what is the password you found?
lets try to login with some default credentials.
{{< image src="images/day2/defaultpass.png" width="100%" caption="default credentials">}}
{{< admonition success "Answer" false >}}
**defaultpass**
{{</ admonition >}}
### what do you have to take to the 'partay'?
after you login, you will be redirected to the home page.
{{< image src="images/day2/homepage.png" width="100%" caption="home page">}}
you can see the answer at all entries.
{{< admonition success "Answer" false >}}
**Eggnog**
{{</ admonition >}}
<br>

## [Day 3] Evil Elf
<br>

## [Day 4] Training
<br>

## [Day 5] Ho-Ho-Hosint
<br>

## [Day 6] Data Elf-iltration
<br>

## [Day 7] Skilling Up
<br>

## [Day 8] SUID Shenanigans
<br>

## [Day 9] Requests
<br>

## [Day 10] Metasploit-a-ho-ho-ho
<br>

## [Day 11] Elf Applications
<br>

## [Day 12] Elfcryption
<br>

## [Day 13] Accumulate
<br>

## [Day 14] Unknown Storage
<br>

## [Day 15] LFI
<br>

## [Day 16] File Confusion
<br>

## [Day 17] Hydra-ha-ha-haa
<br>

## [Day 18] ELF JS
<br>

## [Day 19] Commands
<br>

## [Day 20] Cronjob Privilege Escalation
<br>

## [Day 21] Reverse Elf-inneering
<br>

## [Day 22] If Santa, Then Christmas
<br>

## [Day 23] LapLANd (SQL Injection)
<br>

## [Day 24] Elf Stalk
<br>

## [Day 25] Challenge-less
<br>
