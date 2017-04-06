# Gestió de Volums Lògics
## Què són? (LVM "Logical Volume Management")

D'una forma simplificada podríem dir que LVM és una capa d'abstracció entre un dispositiu d'emmagatzematge (per exemple un disc) i un sistema de fitxers.
## Què volen dir les sigles, definició, analogies i exemples de comandes (explicant què fan) vistes a classe de:
### PV
Serien els "*physical volumes*" ~ (disc dur virtual) identificador de discs
```
[root@localhost ~]# pvs
  PV         VG         Fmt  Attr PSize   PFree  
  /dev/vda2  fedora     lvm2 a--   19,51g      0 
  /dev/vdb   multimedia lvm2 a--  508,00m 108,00m
  /dev/vdc   multimedia lvm2 a--  508,00m 252,00m
```
*més comandes serien -pvcreate-, ...*
### VG
Serien els "*volume groups*" ~ Discs Virtuals
```
[root@localhost ~]# vgs
  VG         #PV #LV #SN Attr   VSize    VFree  
  fedora       1   2   0 wz--n-   19,51g      0 
  multimedia   2   2   0 wz--n- 1016,00m 360,00m
```
*més comandes serien -vgcreate-, vgextend, ...*
### LV
Serien els "*logical volumes*" ~ Particions
```
[root@localhost ~]# lvs
  LV     VG         Attr       LSize   Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert
  root   fedora     -wi-ao----  17,51g                                                    
  swap   fedora     -wi-ao----   2,00g                                                    
  musica multimedia -wi-a----- 256,00m                                                    
  videos multimedia -wi-a----- 400,00m                                                    
```
*més comandes serien -lvcreate-, lvextend, ...*
## Entorn de pràctiques: Explicar com farem la pràctica detalladament (màquina virtual i afegir tres discs de 200M)

Obrim el *virt-manager* ("gestor de màquines virtuals") i anem a afegir hardware on ficarem tres discos nous virtio de 200M cadascú.

## Pràctica 1: Creació d'un volum lògic a partir d'un dels tres discs durs (vda per exemple). Aquest volum lògic ha de ser del total de capacitat del disc. El volum de grup s'ha de dir practica1 i el volum lògic dades.

+ pvcreate /dev/vda  (creem el physical volume)

+ pvs (si ho volem comprovar)
```
[root@localhost ~]# pvcreate /dev/vdb
  Physical volume "/dev/vdb" successfully created.
[root@localhost ~]# pvs
  PV         VG     Fmt  Attr PSize   PFree  
  /dev/vda2  fedora lvm2 a--   19,51g      0 
  /dev/vdb          lvm2 ---  512,00m 512,00m

```
+ vgcreate practica1 /dev/vda (creem el volume group que es dirà practica1)

+ vgs (si ho volem comprovar)
```
[root@localhost ~]# vgcreate practica1 /dev/vdb
  Volume group "practica1" successfully created
[root@localhost ~]# vgs
  VG        #PV #LV #SN Attr   VSize   VFree  
  fedora      1   2   0 wz--n-  19,51g      0 
  practica1   1   0   0 wz--n- 508,00m 508,00m

```
+ lvcreate -l 100%FREE -n dades practica1 (creem el logical volume de tot el disc d'espai amb el nom dades, al volume group practica1)

+ lvs (si volem comprovar-ho)
```
[root@localhost ~]# lvcreate -l 100%FREE -n dades practica1
  Logical volume "dades" created.
[root@localhost ~]# lvs
  LV    VG        Attr       LSize   Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert
  root  fedora    -wi-ao----  17,51g                                                    
  swap  fedora    -wi-ao----   2,00g                                                    
  dades practica1 -wi-a----- 508,00m 
```
## Pràctica 2: Creació d'un sistema de fitxers xfs al volum lògic creat i muntatge a /mnt. També s'ha de crear un fitxer amb dd de 180MB.

+ mkfs.xfs /dev/practica1/dades (creem sistema de fitxers)

+ mount /dev/practica1/dades /mnt (el montem a /mnt)
```
[root@localhost ~]# mkfs.xfs /dev/practica1/dades
meta-data=/dev/practica1/dades   isize=512    agcount=4, agsize=32512 blks
         =                       sectsz=512   attr=2, projid32bit=1
         =                       crc=1        finobt=1, sparse=0
data     =                       bsize=4096   blocks=130048, imaxpct=25
         =                       sunit=0      swidth=0 blks
naming   =version 2              bsize=4096   ascii-ci=0 ftype=1
log      =internal log           bsize=4096   blocks=855, version=2
         =                       sectsz=512   sunit=0 blks, lazy-count=1
realtime =none                   extsz=4096   blocks=0, rtextents=0
[root@localhost ~]# mount /dev/practica1/dades /mnt/

```
+ dd if=/dev/zero of=test.img bs=1k count=180000 (creem fitxer amb dd de 180MB)
```
[root@localhost ~]# dd if=/dev/zero of=test.img bs=1k count=180000
180000+0 registros leídos
180000+0 registros escritos
184320000 bytes (184 MB, 176 MiB) copied, 0,886537 s, 208 MB/s

```
## Pràctica 3: Creació d'un RAID 1 als dos discos sobrants (vdb i vdc per exemple)

+ mdadm --create /dev/md0 --level=1 --raid-devices=2 /dev/vdb /dev/vdc (d'aquesta manera creem el raid)

+ cat /proc/mdstat (podem mirar l'estat dels raid actuals)
```
[root@localhost ~]# mdadm --create /dev/md0 --level=1 --raid-devices=2 /dev/vdc /dev/vdd
mdadm: Note: this array has metadata at the start and
    may not be suitable as a boot device.  If you plan to
    store '/boot' on this device please ensure that
    your boot-loader understands md/v1.x metadata, or use
    --metadata=0.90
Continue creating array? yes
mdadm: Defaulting to version 1.2 metadata
mdadm: array /dev/md0 started.
[root@localhost ~]# cat /proc/mdstat
Personalities : [raid1] 
md0 : active raid1 vdd[1] vdc[0]
      523712 blocks super 1.2 [2/2] [UU]
      
unused devices: <none>

```
## Pràctica 4: Ampliació del volum lògic de dades al raid

+ pvcreate /dev/md0
```
[root@localhost ~]# pvcreate /dev/md0
  Physical volume "/dev/md0" successfully created.
```  
+ vgextend nomVL raid
```
[root@localhost ~]# vgextend practica1 /dev/md0
  Volume group "practica1" successfully extended
```
+ comprovació amb lsblk
```
[root@localhost ~]# lsblk
NAME              MAJ:MIN RM   SIZE RO TYPE  MOUNTPOINT
vdd               252:48   0   512M  0 disk  
└─md0               9:0    0 511,4M  0 raid1 
vdb               252:16   0   512M  0 disk  
└─practica1-dades 253:2    0   508M  0 lvm   /mnt
vdc               252:32   0   512M  0 disk  
└─md0               9:0    0 511,4M  0 raid1 
vda               252:0    0    20G  0 disk  
├─vda2            252:2    0  19,5G  0 part  
│ ├─fedora-swap   253:1    0     2G  0 lvm   [SWAP]
│ └─fedora-root   253:0    0  17,5G  0 lvm   /
└─vda1            252:1    0   500M  0 part  /boot

```
+ i comprovació amb df -h
```
[root@localhost ~]# df -h
S.ficheros                  Tamaño Usados  Disp Uso% Montado en
devtmpfs                      728M      0  728M   0% /dev
tmpfs                         750M   504K  749M   1% /dev/shm
tmpfs                         750M   1,3M  749M   1% /run
tmpfs                         750M      0  750M   0% /sys/fs/cgroup
/dev/mapper/fedora-root        18G   4,8G   12G  30% /
tmpfs                         750M    88K  750M   1% /tmp
/dev/vda1                     477M   129M  319M  29% /boot
tmpfs                         150M    28K  150M   1% /run/user/42
tmpfs                         150M    16K  150M   1% /run/user/1000
/dev/mapper/practica1-dades   505M    26M  480M   6% /mnt

```
## Pràctica 5: Ampliació del sistema de fitxers xfs al tamany actual del volum lògic dades (s'ha de poder fer sense desmuntar-lo de /mnt ja que és xfs). Una vegada creat crearem un nou fitxer de 180M.
+ ampliació del sistema de fitxers xfs al tamany actual del volum lògic dades fent *xfs_growfs* :
```
[root@localhost ~]# xfs_growfs /dev/practica1/dades 
meta-data=/dev/mapper/practica1-dades isize=512    agcount=4, agsize=32512 blks
         =                       sectsz=512   attr=2, projid32bit=1
         =                       crc=1        finobt=1 spinodes=0
data     =                       bsize=4096   blocks=130048, imaxpct=25
         =                       sunit=0      swidth=0 blks
naming   =version 2              bsize=4096   ascii-ci=0 ftype=1
log      =internal               bsize=4096   blocks=855, version=2
         =                       sectsz=512   sunit=0 blks, lazy-count=1
realtime =none                   extsz=4096   blocks=0, rtextents=0

```
+ Creem nou fitxer de 180M *dd if=/dev/zero of=test2.img bs=1k count=180000* 
```
[root@localhost ~]# dd if=/dev/zero of=test2.img bs=1k count=180000
180000+0 registros leídos
180000+0 registros escritos
184320000 bytes (184 MB, 176 MiB) copied, 0,265282 s, 695 MB/s`
```
```
[root@localhost ~]# lsblk
NAME              MAJ:MIN RM   SIZE RO TYPE  MOUNTPOINT
vdd               252:48   0   512M  0 disk  
└─md0               9:0    0 511,4M  0 raid1 
vdb               252:16   0   512M  0 disk  
└─practica1-dades 253:2    0   508M  0 lvm   /mnt
vdc               252:32   0   512M  0 disk  
└─md0               9:0    0 511,4M  0 raid1 
vda               252:0    0    20G  0 disk  
├─vda2            252:2    0  19,5G  0 part  
│ ├─fedora-swap   253:1    0     2G  0 lvm   [SWAP]
│ └─fedora-root   253:0    0  17,5G  0 lvm   /
└─vda1            252:1    0   500M  0 part  /boot
[root@localhost ~]# df -h
S.ficheros                  Tamaño Usados  Disp Uso% Montado en
devtmpfs                      728M      0  728M   0% /dev
tmpfs                         750M   512K  749M   1% /dev/shm
tmpfs                         750M   1,3M  749M   1% /run
tmpfs                         750M      0  750M   0% /sys/fs/cgroup
/dev/mapper/fedora-root        18G   5,0G   12G  31% /
tmpfs                         750M    88K  750M   1% /tmp
/dev/vda1                     477M   129M  319M  29% /boot
tmpfs                         150M    28K  150M   1% /run/user/42
tmpfs                         150M    16K  150M   1% /run/user/1000
/dev/mapper/practica1-dades   505M    26M  480M   6% /mnt
```
```
