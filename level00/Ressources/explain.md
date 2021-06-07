In the project video, we can see the README with "FIND" in upercase.

# first try

We try to get the password with this simpliest command: cat /etc/passwd
But we get the password of flag01 not flag00

# Second try

We try to use the find commande, after man reading and google search, we found the -user

So, we launch `find / -user flag00`
Many file are displayed, a lot were we don't have permission
So, go to eject this file in /dev/nul

`find / -user flag00 2>/dev/null`

We can see 2 beautiful result:
    - /usr/sbin/john
    - /rofs/usr/sbin/john

cat this files, is similare: `cdiiddwpgswtgt`

Try to su flag00 and put `cdiiddwpgswtgt` for the password
It doesn't work.

We try to use dcode.fr, with the cesar code and we can see on beautiful result:

nottoohardhere

su flag00 -> nottoohardhere

Now getflag -> x24ti5gi3x0ol2eh4esiuxias

God jobs !