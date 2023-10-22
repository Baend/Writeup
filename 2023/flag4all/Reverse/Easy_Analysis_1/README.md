# Easy Analysis 1

## FR

Vous venez de trouver un programme étrange dans le appdata d'un utilisateur.
Le programme semble se connecter quelque part, retrouvez où !

Le flag est au format FLAG{nomdedomaine:port)

Auteur : K_lfa (BZHack / ESDAcademy)


## Files : 
 - [chall.exe](./chall.exe)

## Solve

File was previously uploaded on Virustotal: https://www.virustotal.com/gui/file/048f1047798f8135761027388f54e0a963c5b9feb191a4fd6cc2cca3b0a7ed85/behavior
```
#### DNS Resolutions

bzhack.bzh

- 51.68.107.167

img-prod-cms-rt-microsoft-com.akamaized.net

#### IP Traffic

- 192.229.211.108:80 (TCP)
- 20.96.52.198:443 (TCP)
- 23.216.147.64:443 (TCP)
- 51.68.107.167:666 (TCP)
```

=> `FLAG{bzhack.bzh:666}`