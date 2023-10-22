# bashcat

## FR

Répondez aux 10 questions de notre serveur bashcat pour récupérer le FLAG.
Vous avez 1 seconde/question.

Le seul problème pour vous est que ce serveur est caché par une gateway BASH nommée : bash-hell.flag4all.sh.

Information de connection à la gateway : 
  ssh -p 666 the_gate@bash-hell.flag4all.sh
  pass: iop
Information  de connection au bashcat :
  nc bashcat 666

Montrez nous votre maitrise en BASH ! Bonne change !

Format du FLAG : 
  FLAG{PenTh1um2_1s_4_devil}

## Solve

```bash
#!/bin/bash
exec 3<>/dev/tcp/bashcat/666

read -N 3842 -u 3 header
read -r -u 3 q
echo $q
while [[ -n "$q" ]]; do
	col=$(echo $q | grep -o 'ro [[:digit:]]*' | cut -d' ' -f2)
	first=$(echo $q | sed "s/.*premier champ '\([^']*\)'.*/\1/")
	task=$(echo $q | grep -o '.*champ num')
	line=$(grep "^\"$first\"" list.csv)
	word=$(echo $line | cut -d"," -f"$col" | sed 's/"//g')
	case "$task" in
		*"Combien de cara"*)
			answer=$(echo -n "$word" | wc -c)
			;;
		*"Quelle est la valeur"*)
			answer="$word"
			;;
		*)
			echo "Unknown question for $q"
			;;
	esac
	echo "$answer" >&3
	read -r -u 3 q
	echo $q
done

exec 3<&-
exec 3>&-
```

```
Quelle est la valeur du champ numéro 5 sur la ligne qui a comme premier champ 'steel'?
Quelle est la valeur du champ numéro 2 sur la ligne qui a comme premier champ 'straw'?
Combien de caractères contient le champ numéro 2 sur la ligne qui a comme premier champ 'cry'?
Quelle est la valeur du champ numéro 3 sur la ligne qui a comme premier champ 'here'?
Combien de caractères contient le champ numéro 1 sur la ligne qui a comme premier champ 'straw'?
Combien de caractères contient le champ numéro 5 sur la ligne qui a comme premier champ 'local'?
Quelle est la valeur du champ numéro 3 sur la ligne qui a comme premier champ 'explore'?
Combien de caractères contient le champ numéro 3 sur la ligne qui a comme premier champ 'beyond'?
Quelle est la valeur du champ numéro 3 sur la ligne qui a comme premier champ 'ran'?
Quelle est la valeur du champ numéro 5 sur la ligne qui a comme premier champ 'original'?
the flag is : FLAG{B4sH_is_Your_FriEnd}
cut: option requires an argument -- 'f'
Try 'cut --help' for more information.
Unknown question for the flag is : FLAG{B4sH_is_Your_FriEnd}
```