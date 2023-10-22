# bashd5

## FR

Connectez vous depuis la gateway au site web pour répondre à la question.

Information de connection à la gateway : 
  ssh -p 666 the_gate@bash-hell.flag4all.sh
  pass: iop
Information  de connection au site web :
  http://webd5/

Montrez nous votre maîtrise en BASH et WEB ! 

Fast and Hungry !

Format du FLAG :  
FLAG{PenTh1um2_1s_4_devil}

Auteur : Penthium2 (BZHack)

## Solve

```
user4@hell:~$ curl http://webd5/
<!DOCTYPE html>
<html>
<head>
    <title>Quiz MD5</title>
</head>
<body>
    <h1>Quiz MD5</h1>
    <form action="/check_answer" method="post">
        <div>quel est le nom du CTF en minuscule ?<p>donnez la réponse en md5</p></div>
        <input type="text" name="answer" placeholder="Réponse">
        <button type="submit">Vérifier</button>
    </form>
</body>
</html>
```

```
user4@hell:~$ echo -n 'flag4all' | md5sum
d3f98cb348224fe82d4ed679f38a901b  -
```

We need to reuse the cookie previously set,otherwise we have an error "Too slow":
```
user4@hell:~$ curl -i -c cookie.jar http://webd5 ; curl -v -b cookie.jar -X POST --data 'answer=d3f98cb348224fe82d4ed679f38a901b' http://webd5/check_answer
HTTP/1.0 200 OK
Content-Type: text/html; charset=utf-8
Content-Length: 373
Server: Werkzeug/1.0.1 Python/3.9.2
Date: Sun, 22 Oct 2023 01:19:31 GMT

<!DOCTYPE html>
<html>
<head>
    <title>Quiz MD5</title>
</head>
<body>
    <h1>Quiz MD5</h1>
    <form action="/check_answer" method="post">
        <div>quel est le nom du CTF en minuscule ?<p>donnez la réponse en md5</p></div>
        <input type="text" name="answer" placeholder="Réponse">
        <button type="submit">Vérifier</button>
    </form>
</body>
</html>Note: Unnecessary use of -X or --request, POST is already inferred.
*   Trying 10.0.31.15:80...
* Connected to webd5 (10.0.31.15) port 80 (#0)
> POST /check_answer HTTP/1.1
> Host: webd5
> User-Agent: curl/7.74.0
> Accept: */*
> Content-Length: 39
> Content-Type: application/x-www-form-urlencoded
> 
* upload completely sent off: 39 out of 39 bytes
* Mark bundle as not supporting multiuse
* HTTP 1.0, assume close after body
< HTTP/1.0 200 OK
< Content-Type: text/html; charset=utf-8
< Content-Length: 34
< Server: Werkzeug/1.0.1 Python/3.9.2
< Date: Sun, 22 Oct 2023 01:19:31 GMT
< 
* Closing connection 0
voici le flag: FLAG{curl_1s_ea5y}.
```