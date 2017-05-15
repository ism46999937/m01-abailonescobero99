# PROVA PRÀCTICA UF5
En aquesta pràctica haureu de canviar la clau de root del sistema operatiu de tarda i tornar-la a deixar com estava. Seguiu i documenteu els passos:
## 1. Procediment A. Iniciant el sistema que tenim a l'ordinador.
1. Arrenqueu des de la partició de matí i munteu la partició de sistema de fitxers de la tarda sobre /mnt. Indiqueu les comandes:
```
mount /dev/sda5 /mnt
```
2. Convertiu-vos en root d'aquest nou sistema de fitxers de tarda. Indiqueu les comandes.
```
chroot /mnt
```
3. Canvieu-ne la clau de root per la clau: 123456. Indiqueu les comandes.
```
passwd root
123456
```
4. Desmunteu el sistema de fitxers de /mnt. Indiqueu les comandes.
```
umount /mnt
```
5. Reinicieu amb el sistema operatiu de tarda i comproveu si funciona.
```
ho comprovem i fem:
```
6. Canvieu-ne la clau per la de sempre (deixar-ho com estava originalment)
```
passwd root
jupiter
```
## 2. Procediment B. Editant les opcions d'arrencada de GRUB.
1. En iniciar l'ordinador editeu la línia d'arrencada del GRUB per a la partició de matí. Indiqueu les opcions i comandes.
```
cliquem la e (edit) sobre la partició de matí.
anem fins a la línia on posa linux o linux16, en aquella línia al final afegim: rw init=/bin/bash
i fem ctrl-x per arrencar en mode comandes no gràfic.
```
2. Com a root canvieu-ne la clau per 123456. Indiqueu les comandes.
```
passwd root
123456
```
3. Reinicieu amb el sistema operatiu de matí i comproveu si ha funcionat.
```
Un cop dins de matí obrim un terminal i fem:
```
4. Canvieu-ne la clau per la de sempre.
```
passwd root
jupiter
```
## 3. Procediment C. Iniciant un sistema operatiu 'live' per xarxa.
1. Arrenqueu amb el Fedora que tenim per xarxa (PXE).
```
Arrenquem al menu de boot per PXE amb un fedora24 live
```
2. Indiqueu les comandes (i sortida per pantalla) que feu servir per identificar la partició de matí.
```

```
3. Munteu la partició de matí a /mnt i canvieu-ne la clau: 123456. Indiqueu les comandes.
```
mount /dev/sda6 /mnt
chroot /mnt
passwd root
123456
```
4. Reinicieu amb el sistema operatiu de matí i comproveu que ha funcionat.
```
Reiniciem a matí i comprovem que la password esta canviada i la tornem a establir la antiga
passwd root
jupiter
```

