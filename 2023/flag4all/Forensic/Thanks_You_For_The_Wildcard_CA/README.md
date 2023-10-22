# Thanks You For The Wildcard CA

## FR

La société ESD Secure Message se vante d'avoir un système de messagerie en ligne chiffrée avec un mécanisme de chiffrement asymétrique réputé inviolable.

Elle vous a engagé pour réaliser un pentest sur un périmètre bien défini et pendant un lapse de temps très court.
À la suite de ce pentest vous avez pu récupérer deux éléments:
- Une interception d'échanges depuis un des serveurs non sécurisés 
- Un export brut d'un des messages chiffrés issu de la base de données de l'application de messagerie

Votre mission: Finir l'investigation des éléments récupérés et trouver le moyen de déchiffrer le message de l'application de ESD Secure Message.

Auteur : shadowmedicis (ESDAcademy - ENI Nantes - HESD03)

## Files
 - [Thank_you_for_the_Wildcard_CA.zip](./Thank_you_for_the_Wildcard_CA.zip)

## Solve

Wireshark => view TCP stream to see the SMTP exchange
```
Hi John,

I send you the new certificate for renew the certificate of htttps//account.esd-secure-message.fr.
You know the passphrase, it’s the same password of the latest certificate.
It’s legacy 😉 ..

Tell me when you finish to install this certificate.

Best regards,

Tom RIP
ESD Secure Message
+33(0)6 44 63 71 11
```
Extraact the file `certificate willcard esd-secure-message.fr.zip`
we have 2 file
- ca.crt
- private.key


```
└─$ ssh2john private.key > hash.txt
└─$ john --wordlist=/usr/share/wordlists/rockyou.txt hash.txt
Using default input encoding: UTF-8
Loaded 1 password hash (SSH, SSH private key [RSA/DSA/EC/OPENSSH 32/64])
Cost 1 (KDF/cipher [0=MD5/AES 1=MD5/3DES 2=Bcrypt/AES]) is 0 for all loaded hashes
Cost 2 (iteration count) is 2 for all loaded hashes
Will run 4 OpenMP threads

Press 'q' or Ctrl-C to abort, almost any other key for status
antiflag         (private.key)     
1g 0:00:00:00 DONE (2023-10-21 14:49) 2.173g/s 72834p/s 72834c/s 72834C/s babygurl06..alexander3
Use the "--show" option to display all of the cracked passwords reliably
Session completed. 
```

Checking
```
└─$ openssl rsa -noout -text -in private.key
Enter pass phrase for private.key:
Private-Key: (4096 bit, 2 primes)
modulus:
[...]
```
Decrypting
```
└─$ openssl smime -decrypt -in smime.p7m -out decrypted.txt -inkey private.key -inform DER
└─$ cat decrypted.txt               
Hi !

Congratulations you managed to decipher this message.

To validate this challenge, I invite you to enter the following FLAG: 
ESD{doesnt_send_private_keys_over_an_unsecured_channel}

Good luck for the future ;)
```