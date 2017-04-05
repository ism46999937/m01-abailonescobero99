# ==Gestió de Volums Lògics==
## Què són? (LVM "Logical Volume Management")

D'una forma simplificada podríem dir que LVM és una capa d'abstracció entre un dispositiu d'emmagatzematge (per exemple un disc) i un sistema de fitxers.
## Què volen dir les sigles, definició, analogies i exemples de comandes (explicant què fan) vistes a classe de:
### PV
Serien els "*physical volumes*" ~ (disc dur virtual) identificador de discs
### VG
Serien els "*volume groups*" ~ Discs Virtuals 
### LV
Serien els "*logical volumes*" ~ Prticions

## Entorn de pràctiques: Explicar com farem la pràctica detalladament (màquina virtual i afegir tres discs de 200M)

Obrim el *virt-manager* ("gestor de màquines virtuals") i anem a afegir hardware on ficarem tres discos nous virtio de 200M cadascú.

## Pràctica 1: Creació d'un volum lògic a partir d'un dels tres discs durs (vda per exemple). Aquest volum lògic ha de ser del total de capacitat del disc. El volum de grup s'ha de dir practica1 i el volum lògic dades.

+ pvcreate /dev/vda  (creem el physical volume) 
+ pvs (si ho volem comprovar)
+ vgcreate practica1 /dev/vda (creem el volume group que es dirà practica1)
+ vgs (si ho volem comprovar)
+ lvcreate -l 100%FREE -n dades practica1 (creem el logical volume de tot el disc d'espai amb el nom dades, al volume group practica1)
+ lvs (si volem comprovar-ho)

## Pràctica 2: Creació d'un sistema de fitxers xfs al volum lògic creat i muntatge a /mnt. També s'ha de crear un fitxer amb dd de 180MB.

+ mkfs.xfs /dev/practica1/dades (creem sistema de fitxers)
+ mount /dev/practica1/dades /mnt (el montem a /mnt)
+ dd if=/dev/zero of=test.img bs=1k count=180000 (creem fitxer amb dd de 180MB)

## Pràctica 3: Creació d'un RAID 1 als dos discos sobrants (vdb i vdc per exemple)

+ mdadm --create /dev/md0 --level=1 --raid-devices=2 /dev/vdb /dev/vdc (d'aquesta manera creem el raid)
+ cat /proc/mdstat (podem mirar l'estat dels raid actuals)

## Pràctica 4: Ampliació del volum lògic de dades al raid

+ pvcreate /dev/md0
+ lvextend -l 100%FREE /dev/practica1/dades /dev/md0
+ comprovació amb lsblk
+ i comprovació amb df -h

## Pràctica 5: Ampliació del sistema de fitxers xfs al tamany actual del volum lògic dades (s'ha de poder fer sense desmuntar-lo de /mnt ja que és xfs). Una vegada creat crearem un nou fitxer de 180M.

+ pvcreate /dev/md0
+ lvextend -l 100%FREE /dev/practica1/dades /dev/md0
+ comprovació amb lsblk
+ i comprovació amb df -h

## Pràctica 5: Ampliació del sistema de fitxers xfs al tamany actual del volum lògic dades (s'ha de poder fer sense desmuntar-lo de /mnt ja que és xfs). Una vegada creat crearem un nou fitxer de 180M.

+ xfs-growfs /dev/practica1/dades
+ 
