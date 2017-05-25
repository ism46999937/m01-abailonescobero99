# Tallafocs

###  Què es un sistema tallafocs? Quina és la seva finalitat?
```
És una part de un sistema o una xarxa que està disenyada per bloquejar el accés no autoritzat, permetent al mateix temps comunicacions autoritzades.
```
### Quines generacions de tallafocs hi ha hagut i què millorava cadascun?
```
La primera generació es el talafocs de xarxa, per filtrat de paquets(es feia inspecció de paquets, si el paquet coincidia amb el conjunt de regles del filtre el paquet seria reduït o seria eliminat).
La segona generació es el tallafocs de estat, aquest era capaç de determinar si un paquet indica l'inici de una nova connexió, es part de una conexió existent, o es un paquet erroni. Aquest tipusde tallafocs poden ajudar a preveure atacs contra connexions en curs o certs atacs de denegació de servei.
La tercera generació es el tallafocs de aplicació, son aquells que actuan sobre la capa del model OSI. Permet detectar si un protocol no desitjat s'ha colat a través d'un port no estàndar o si s'està abusant d'un protocol de forma perjudicial.
```
### Quines capes té el model OSI?
```
7. Aplicació
6. Presentació
5. Sessió
4. Transport 
3. Xarxa
2. Enllaç de dades
1. Física
```
### Quines capes té el model TCP/IP? En aquest cas feu una breu descripció de les funcionalitats de cada capa.
```
4. Aplicació
3. Transport
2. Internet
1. Accés al medi
```
### A quina capa/capes sol treballar tradicionalment un tallafocs?
```
A la de transport i la de xarxa
```
# Tallafocs Linux

### Busqueu quins són els tradicionals sistemes de tallafocs incorporats en linux i anomeneu-los
```
· Nivell de aplicació de pasarel·la
· Circuit a nivell de pasarel·la
· Tallafocs de capa de xarxa o de filtrat de paquets
· Tallafocs de capa de aplicació
· Tallafocs personal
```
### Quins dels anteriors tallafocs estan instal.lats al fedora de classe? Com ho comproveu?
```
Es comprova així: 'iptables -L'
-L --> list
S'utilitza el firewalld
```
### Algun dels anteriors tallafocs es troba activat?
```
No
```
### Instal.leu el servidor web httpd o nginx i activeu-ne el servei (dnf installl ...  ; systemctl ....). Indiqueu les comandes i comproveu que des d'una altra màquina podeu accedir via web a la vostra IP (digueu-li a un company). Hauria de sortir la plana per defecte.
````
dnf install -y nginx
```
### Activeu el servei firewalld. Indiqueu com ho feu.
```

```
### Comproveu si ara es pot seguir accedint.
```
No es pot ja que el firewall esta activat.
```
# Win7

### Porta aquest SO algun tallafocs incorporat?
```
Si, el que passa es que s'ha d'activar.
```
### Arrenqueu una màquina win7 a isard.escoladeltreball.org
```
Ok.
```
### Indiqueu com arribar al tallafocs (passos i pantalles)
```
Un cop iniciat W7, anar a panell de control, sistema i seguretat i firewall de Windows.
```
### Es troba activat en aquest windows?
```
Si, en cas de que no estigues hi ha una opcio que diu turn off or turn on i ja està.
```
### Busqueu un altre tallafocs per windows. Indiqueu la plana web i les prestacions que ens dona. Intenteu que NOMÉS sigui tallafocs.
```
He trobat el tallafocs agnitum outpost free. 
També el Comodo Firewall
ZoneAlarm Free
PC tools Firewall Plus 7
Y més:
Pagina info buscada: www.emezeta.com/articulos/10-firewalls-gratuitos-alternativos

```
