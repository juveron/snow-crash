Pour ce niveau, encore un binaire + token

```bash
-rwsr-sr-x 1 flag09  level09 7640 Mar  5  2016 level09
----r--r-- 1 flag09  level09   26 Mar  5  2016 token
```
Avec un `cat token` : `f4kmm6p|=�p�n��DB�Du{��`

Apres analyse, ca ressemble fortement a une sortie du tableau ascii. Chose qu'on a deja pu experimenter dans les exos de manipulation de strings en piscine et en exam.

Avec un `ltrace ./level09 token`

```bash
__libc_start_main(0x80487ce, 2, 0xbffff7a4, 0x8048aa0, 0x8048b10 <unfinished ...>
ptrace(0, 0, 1, 0, 0xb7e2fe38)                      = -1
puts("You should not reverse this"You should not reverse this
)                 = 28
+++ exited (status 1) +++
```

Le "You should not reverse this" nous donne envie de le faire. Et on pense que ca fait forcement mention a `f4kmm6p|=�p�n��DB�Du{��`

Apres reflexion, utiliser un programme devrai etre la solution.

Sachant que le but est de reverse chaque caracteres. Mais comment ?

un `./level09 "11111"` nous montre :

```bash
level09@SnowCrash:~$ ./level09 "1111"
1234
```

Apres analyse on constate qu'a chaque tour de boucle l'index = index + n tour de boucle
donc index 4 = 3ieme tour de boucle donc 1 + 3 = 4.
Donc en revenversant l'addition par une soustraction cela donne quelque ligne de code :

```bash
#include <stdio.h>

int     main(int ac, char **av) {
        if (ac != 2) return (0);
        int i = 0;
        while (av[1][++i]) {
                av[1][i] -= i;
        }
        puts(av[1]);
        return (0);
}
```

on compile `gcc exploit.c -o exploit`

On execute `./exploit $(cat ~/token)` le `$(cat ~/token)` c'est pour avoir la sorti du cat donc ce qu'on veut reverse

Et on obtient : 

```bash
level09@SnowCrash:/tmp$ ./exploit $(cat ~/token)
f3iji1ju5yuevaus41q1afiuq
```

`su flag09 f3iji1ju5yuevaus41q1afiuq`

sa marche et le getflag -> `s5cAJpM8ev6XHw998pRWG728z`