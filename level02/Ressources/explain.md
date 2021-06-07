Dans le level02 nous avons un fichier pcap dont le proprietaire est flag02

----r--r-- 1 flag02  level02 8302 Aug 30  2015 level02.pcap

Apres quelques recherches, on apprend que l'extension .pcap designe l'enregistrement de packets d'un reseau

Ce fichier est souvent lie a un logiciel, wireshark, qui est un analyseur de packet.

Nous avons donc scp ce fichier pour l'avoir sur notre machine et l'ouvrir avec wireshark.

En observant les differentes packet, il y en a un ou il y a ecrit "password:", on fait donc clic droit => follow => TCP stream

On peut voir password: ft_wandr...NDRel.L0L, nous avons remarque que un point = une supprssion car nous voyons plus haut : l.le.ev.ve.el.lX.X => levelX

Donc le mots de passe est ft_waNDReL0L