When we search the first flag, we see an interesting line in /etc/passwd

`flag01:42hDRfypTqqnw:3001:3001::/home/flag/flag01:/bin/bash`

We try su flag01 => Password but it's doesn't work

We try su dcode.fr => But it's doesn't work

The password is encripted, we do use john the riper to decripte.

ADD link to explain /etc/passwd file

We can't use John on the snow-crash, we use scp for copy /etc/passwd on notre own machine.

Now we have the password of flag01 => `abcdefg`