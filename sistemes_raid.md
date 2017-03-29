# Sistemes RAID

## 1. Raid

### Resum de sistemes RAID fent servir una taula com la vista a classe.

| TIPUS | Nº mín discs | Nº màx discs fallats | capacitat | read | write |
| ---------- | ---------- |-----------| ---------- | ---------- | ---------- |
| Raid 0   | 2   | 0          | 100%      | Excellent           |  Excellent |
| Raid 1   | 3   | 1          | 50%       | V.Good              | Good       |
| Raid 5   | 3   | 1          | 67~94%    | Good                | Good       |
| Raid 6   | 4   | 2          | 50~88%    | V.Good              | Good       |
| Raid 10  | 4   | 1/mirror   | 50%       | V.Good              | Good       |
| Raid 50  | 6   | 1/Raid5 set| 67~94%    | Excellent           | V.Good     |

### Descripció de la metodologia utilitzada a classe per a fer proves amb màquines virtuals.
Per crear raids utilitzem el mdadm. El create és per a crear-ho i allà mateix fiquem el nom del dispositiu. El 
level és per dir l'hi de quin nivell és el raid (raid1, raid5,...). A raid devices es fica el nombre de discs els quals estaren al raid.
Finalment es fiquen els discos del raid per exemple: /dev/vdb /dev/vdc
### Comandes i descripció de les mateixes per tal de crear un sistema RAID1
mdadm --create /dev/md0 --level=1 --raid-devices=2 /dev/vdb /dev/vdc
cat /proc/mdstat (per comprovar el estat del teu raid)
mkfs.ext4
mount /dev/md0 /mnt (per exemple ho montem a /mnt)
df -h (per comprovar que està muntat)
cd /mnt (entrem a mnt on està muntat)
i fem un: dd if=/dev/zero of=test.img bs=4k count=1000    bs(sectors)   count(els que et fa de sectors)
mdadm --detail /dev/md0 (et mostra el raid amb més detalls)
mdadm /dev/md0 --add /dev/vdd (si hi ha un lloc lliure a md0)
per parar tot: mdadm --manage /dev/md0 --fail /dev/vdc
mdadm --manage /dev/md0 --remove /dev/vdc
### Comandes i descripció de les mateixes per tal de crear un sistema RAID5
mdadm --create /dev/md0 --level=5 --raid-devices=3 /dev/vdb /dev/vdc /dev/vdd
tot el de més com abans
### Comandes i descripció de les mateixes per tal de crear un sistema RAID6
mdadm --create /dev/md0 --level=6 --raid-devices=4 /dev/vdb /dev/vdc /dev/vdd /dev/vde 
tot el demés com abans
### Comandes i descripció de les mateixes per tal de crear un sistema RAID10
mdadm --create /dev/md0 --level=10 --raid-devices=4 /dev/vdb /dev/vdc /dev/vdd /dev/vde
tot el de més com abans### Comandes i descripció de les mateixes per tal de crear un sistema RAID1

