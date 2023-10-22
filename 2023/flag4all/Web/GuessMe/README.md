# GuessMe

## FR

L'admin a caché son flag, retrouvez le !

## EN

The admin hides the flag, find it!

https://guessme.flag4all.sh

Auteur : K_lfa (BZHack / ESDAcademy)


https://guessme.flag4all.sh

## Solve

we are redirected to:
https://guessme.flag4all.sh/index.php?page=cfcd208495d565ef66e7dff9f98764da
with the text `NOPE, NOTHING HERE`
MD5 of empty string => we need to md5 the page to access ?

trying with burp, got same answer with
```
c20ad4d76fe97759aa27a0c99bff6710
6512bd43d9caa6e02c990b0a82652dca
d3d9446802a44259755d38e6d163e820
c4ca4238a0b923820dcc509a6f75849b
c81e728d9d4c2f636f067f89cc14862c
eccbc87e4b5ce2fe28308fd9f2a7baf3
c51ce410c124a10e0db5e4b97fc2af39
a87ff679a2f3e71d9181a67b7542122c
aab3238922bcc25a6f606eb525ffdc56
9bf31c7ff062936a96d3c8bd1f8f2ff3
c74d97b01eae257e44aa9d5bade97baf
6f4922f45568161a8cdf4ad2299f6d23
98f13708210194c475687be6106a3b84
3c59dc048e8850243be8079a5c74d079
e4da3b7fbbce2345d7772b0674a318d5
b6d767d2f8ed5d21a44b0e5886680cb9
1679091c5a880faf6fb5e6087eb1b2dc
1f0e3dad99908345f7439f8ffabdffc4
1ff1de774005f8da13f42943881c655f
37693cfc748049e45d87b8c7d8b9aacd
70efdf2ec9b086079795c442636b55fb
02e74f10e0327ad868d138f2b4fdd6f0
4e732ced3463d06de0ca9a15b6153677
```
=> only hash of number => we might want to only iter on the MD5 of numbers

```
└─$ for i in $(seq 1 1000); do echo -n $i | md5sum | cut -d' ' -f1; done > number_to_md5.txt
└─$ ffuf -w number_to_md5.txt -u 'https://guessme.flag4all.sh/index.php?page=FUZZ' -fs 0 -fw 3

        /'___\  /'___\           /'___\       
       /\ \__/ /\ \__/  __  __  /\ \__/       
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\      
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/      
         \ \_\   \ \_\  \ \____/  \ \_\       
          \/_/    \/_/   \/___/    \/_/       

       v2.1.0-dev
________________________________________________

 :: Method           : GET
 :: URL              : https://guessme.flag4all.sh/index.php?page=FUZZ
 :: Wordlist         : FUZZ: /home/kali/number_to_md5.txt
 :: Follow redirects : false
 :: Calibration      : false
 :: Timeout          : 10
 :: Threads          : 40
 :: Matcher          : Response status: 200-299,301,302,307,401,403,405,500
 :: Filter           : Response size: 0
 :: Filter           : Response words: 3
________________________________________________

0bb4aec1710521c12ee76289d9440817 [Status: 200, Size: 32, Words: 1, Lines: 2, Duration: 4ms]
:: Progress: [1000/1000] :: Job [1/1] :: 46 req/sec :: Duration: [0:00:05] :: Errors: 0 ::
└─$ curl 'https://guessme.flag4all.sh/index.php?page=0bb4aec1710521c12ee76289d9440817'                                                                         
FLAG{I_Kn0w_M4ke_GET_1n_4_Lo0P}
```
