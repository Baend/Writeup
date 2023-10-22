# bashcook

## FR

Connectez vous depuis la gateway au site web pour répondre à la question.

Information de connection à la gateway : 
  ssh -p 666 the_gate@bash-hell.flag4all.sh
  pass: iop
Information  de connection au site web :
  http://webcook/

Montrez nous votre maîtrise en BASH et WEB ! 

Fast and Hungry !

Format du FLAG : 
  FLAG{PenTh1um2_1s_4_devil}

Auteur : Penthium2 (BZHack)

## Solve

Similar to the `bashd5` challenge but with the md5 of `bzhack`:
```
user6@hell:~$ curl -i -c cookie.jar http://webcook ; curl -v -b cookie.jar -X POST --data 'answer=b1fee1d5fe5e03b8d8bccf115cbbc94a' http://webcook/check_answer
HTTP/1.0 200 OK
Content-Type: text/html; charset=utf-8
Content-Length: 436
Vary: Cookie
Set-Cookie: session=eyJfcGVybWFuZW50Ijp0cnVlLCJmbGFnNGFsbCI6ImZsYWc0YWxsXzQ4OTIifQ.ZTROWA._eu6KGZFN7K9X5Ymf2D6-jjTRJY; Expires=Sat, 21-Oct-2023 22:19:06 GMT; HttpOnly; Path=/
Server: Werkzeug/1.0.1 Python/3.9.2
Date: Sat, 21 Oct 2023 22:19:04 GMT

<!DOCTYPE html>
<html>
<head>
    <title>Quiz cook</title>
</head>
<body>
    <h1>Quiz cook</h1>
    <form action="/check_answer" method="post">
        <div>quel est le nom de l&#39;asso en minuscule qui organise ce CTF ?<p>donnez la réponse en md5</p></div>
        <input type="text" name="answer" placeholder="Réponse">
        <button type="submit">Vérifier</button><br>
        <p>Vous avez 2sec</p>
    </form>
</body>
</html>Note: Unnecessary use of -X or --request, POST is already inferred.
*   Trying 10.0.31.13:80...
* Connected to webcook (10.0.31.13) port 80 (#0)
> POST /check_answer HTTP/1.1
> Host: webcook
> User-Agent: curl/7.74.0
> Accept: */*
> Cookie: session=eyJfcGVybWFuZW50Ijp0cnVlLCJmbGFnNGFsbCI6ImZsYWc0YWxsXzQ4OTIifQ.ZTROWA._eu6KGZFN7K9X5Ymf2D6-jjTRJY
> Content-Length: 39
> Content-Type: application/x-www-form-urlencoded
> 
* upload completely sent off: 39 out of 39 bytes
* Mark bundle as not supporting multiuse
* HTTP 1.0, assume close after body
< HTTP/1.0 200 OK
< Content-Type: text/html; charset=utf-8
< Content-Length: 39
< Vary: Cookie
* Replaced cookie session="eyJfcGVybWFuZW50Ijp0cnVlLCJmbGFnNGFsbCI6ImZsYWc0YWxsXzQ4OTIifQ.ZTROWA._eu6KGZFN7K9X5Ymf2D6-jjTRJY" for domain webcook, path /, expire 1697926746
< Set-Cookie: session=eyJfcGVybWFuZW50Ijp0cnVlLCJmbGFnNGFsbCI6ImZsYWc0YWxsXzQ4OTIifQ.ZTROWA._eu6KGZFN7K9X5Ymf2D6-jjTRJY; Expires=Sat, 21-Oct-2023 22:19:06 GMT; HttpOnly; Path=/
< Server: Werkzeug/1.0.1 Python/3.9.2
< Date: Sat, 21 Oct 2023 22:19:04 GMT
< 
* Closing connection 0
voici le flag: FLAG{curl_Lov3_C0Ok1eS}.
```