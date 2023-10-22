# Easy Analysis 2

## FR

Après avoir identifié où se connecte le malware, vérifiez ce qu'il envoi !

After figure out where the malware connects to, check what it sends!

Auteur : K_lfa (BZHack / ESDAcademy)

## Files : 
 - [chall.exe](./chall.exe)

## Solve

1. Use a Windows VM in addition to your Kali VM
2. Editing `C:\Windows\System32\drivers\etc\hosts` in Windows to set your kali VM ip for `bzhack.bzh`
3. On Kali : nc -lnvp 666
4. run the chall.exe on windows
5. ???
6. Profit:
```
└─$ nc -lnvp 666                          
Listening on 0.0.0.0 666
Connection received on 192.168.56.102 49851
FLAG{BZH4ck_kn0w_Y0ur_P0rT}
```
