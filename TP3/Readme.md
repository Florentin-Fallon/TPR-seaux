# TP3 : On va router des trucs
# 1. Echange ARP
### 🌞Générer des requêtes AR

Depuis John :

Résultat :
```
$ ping 192.168.30.12

PING 192.168.30.12 (192.168.30.12) 56(84) bytes of data.
64 bytes from 192.168.30.12: icmp_seq=1 ttl=64 time=1.80 ms
64 bytes from 192.168.30.12: icmp_seq=2 ttl=64 time=1.67 ms
64 bytes from 192.168.30.12: icmp_seq=3 ttl=64 time=2.09 ms
64 bytes from 192.168.30.12: icmp_seq=4 ttl=64 time=1.84 ms
```

Depuis Marcel :

Résultat :
```
$ ping 192.168.30.11

PING 192.168.30.11 (192.168.30.11) 56(84) bytes of data.
64 bytes from 192.168.30.11: icmp_seq=1 ttl=64 time=2.39 ms
64 bytes from 192.168.30.11: icmp_seq=2 ttl=64 time=1.90 ms
64 bytes from 192.168.30.11: icmp_seq=3 ttl=64 time=2.92 ms
```


**Table ARP**

De John :

```
$ ip n s 

192.168.30.1 dev enp0s6 lladdr 9e:3e:53:c8:12:66 REACHABLE
192.168.30.12 dev enp0s6 lladdr d2:79:fa:9d:4d:5f STALE
```
L'adresse MAC de Marcel est **d2:79:fa:9d:4d:5**

De Marcel :

```
$ ip n s

192.168.30.1 dev enp0s6 lladdr 9e:3e:53:c8:12:66 REACHABLE
192.168.30.11 dev enp0s6 lladdr 36:4c:2a:46:59:1b STALE
```

L'adresse MAC de John est **36:4c:2a:46:59:1b**

Prouver que l'info est correct :

+ Une commande pour voir la MAC de Marcel dans la table ARP de John :

```
$ ip neigh show 192.168.30.12

192.168.30.12 dev enp0s6 lladdr d2:79:fa:9d:4d:5f STALE
```

+ Une commande pour voir la MAC de Marcel depuis Marcel :

```
$ ip a

link/ether d2:79:fa:9d:4d:5f
```
# 2. Analyse de trames ##

### 🌞Analyse de trames ###


Les paquets sont [ici](tp3_arp.pcapng)

# II. Routage
# 1. Mise en place du routage
## 🌞Activer le routage sur le noeud router

**Activation du routage :**
```
$ sudo firewall-cmd --add-masquerade --zone=public
$ sudo firewall-cmd --add-masquerade --zone=public --permanent

$ sudo firewall-cmd --list-all
```

J'ai bien le **masquerade** mis en **yes**

# 3. Accès internet
## 🌞Donnez un accès internet à vos machines

+ La carte NAT déja activer
+ Création route John et Marcel 
```
$ sudo ip route add default via 192.168.30.3 dev enp0s1

```
+ Vérifiez que vous avez accès internet avec un ping
```
$ ping 8.8.8.8

PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data.
64 bytes from 8.8.8.8: icmp_seq=1 ttl=118 time=19.2 ms
64 bytes from 8.8.8.8: icmp_seq=2 ttl=118 time=19.2 ms
64 bytes from 8.8.8.8: icmp_seq=3 ttl=118 time=17.6 ms
```

+ Donnez leur aussi l'adresse d'un serveur DNS qu'ils peuvent utiliser
```
$ sudo cat /etc/resolv.conf
nameserver 1.1.1.1
```
```
$ curl gitlab.com
```
+ Retourn **rien** donc c'est good
```
$ dig gitlab.com

; <<>> DiG 9.16.23-RH <<>> gitlab.com
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 63139
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1232
;; QUESTION SECTION:
;gitlab.com.			IN	A

;; ANSWER SECTION:
gitlab.com.		222	IN	A	172.65.251.78

;; Query time: 20 msec
;; SERVER: 1.1.1.1#53(1.1.1.1)
;; WHEN: Thu Nov 03 10:40:39 CET 2022
;; MSG SIZE  rcvd: 55
```
```
$ ping gitlab.com

PING gitlab.com (172.65.251.78) 56(84) bytes of data.

64 bytes from 172.65.251.78 (172.65.251.78): icmp_seq=1 ttl=55 time=16.8 ms
64 bytes from 172.65.251.78 (172.65.251.78): icmp_seq=2 ttl=55 time=19.2 ms
64 bytes from 172.65.251.78 (172.65.251.78): icmp_seq=3 ttl=55 time=18.7 ms
```

## 🌞Analyse de trames ##

+ Effectuez un ping 8.8.8.8 depuis john

Le ssh ne veut pas fonctionner malgrés l'aide de greg 
```
% ssh tp3@192.168.30.11
kex_exchange_identification: read: Connection reset by peer
Connection reset by 192.168.30.11 port 22
```

# III. DHCP
# 1. Mise en place du serveur DHCP 
## 🌞 Sur la machine john, vous installerez et configurerez un serveur DHCP 

+ installation du serveur sur john

```
$ sudo dnf install dhcp-server -y
```
```
$ sudo nano /etc/dhcp/dhcpd.conf
```
+ Création du contenue du fichier

```
default-lease-time 900;
max-lease-time 10800;

authoritative;

subnet 192.168.30.0 netmask 255.255.255.0 {
range 192.168.30.2 192.168.30.253;
option routers 192.168.30.3;
option subnet-mask 255.255.255.0;
option domain-name-servers 1.1.1.1;
}
```

+ Ajouter la régle du Pare-feu

```
$ sudo firewall-cmd --add-port=67/udp --permanent
```

+ Recharger le Pare-feu pour l'appliquer

```
$ sudo firewall-cmd --reload
```

+ Démarrage et activation 

```
$ sudo systemctl enable --now dhcpd
```

+ Pour vérifier l'activation du serveur DHCP

```
$ systemctl status dhcpd
```

**Création de bob et de son IP** 

```
$ cd /etc/sysconfig/network-scripts/
```
```
$ sudo nano ifcfg-enp0s6
```
+ Création du contenue du fichier

```
DEVICE=enp0s6

BOOTPROTO=dhcp
ONBOOT=yes
```
+ Repérer la connexion 

```
$ nmcli con show
```

+ Redémarrage 

```
$ sudo nmcli con up "System enp0s8"
```
## 🌞 Améliorer la configuration du DHCP

+ Ajoutez de la configuration à votre DHCP

Pour ajouter une route par défaut, j'ai effectuer une modification dans le fichier **dhcpd.conf** en mettant l'adresse ip du subnet à **0.0.0.0**

Pour le DNS, il y étais déja dans le fichier avec la commande :
```
option domain-name-servers 1.1.1.1
```

+ Récupérez de nouveau une IP en DHCP sur bob 

J'ai effectuer une modification du fichier **ifcfg-enp0s6** et je l'ai redémarrer avec les commandes :

```
$ sudo nmcli con reload
$ sudo nmcli con up "System enp0s8"
$ sudo systemctl restart NetworkManager
```

 La commande :
 ```
$ ip a
```
+ Lancer un ping pour tester la connexion avec la passerelle 

```
$ ping 192.168.30.3

PING gitlab.com 192.168.30.3 (192.168.30.3) 56(84) bytes of data.

64 bytes from 192.168.30.3: icmp_seq=1 ttl=64 time=2.29 ms
64 bytes from 192.168.30.3: icmp_seq=2 ttl=64 time=1.71 ms
64 bytes from 192.168.30.3: icmp_seq=3 ttl=64 time=2.23 ms
```

+ Il doit avoir une route par défaut

La commande :

```
$ ip route show

default via 192.168.30.3 dev enp0s1
192.168.30.0/24 dev enp0s1 proto kernel scope link src 192.168.30.2 metric 100 
```

+ Vérification du ping de la route vers une ip 

```
$ ping 8.8.8.8

PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data.
64 bytes from 8.8.8.8: icmp_seq=1 ttl=118 time=19.2 ms
64 bytes from 8.8.8.8: icmp_seq=2 ttl=118 time=19.2 ms
64 bytes from 8.8.8.8: icmp_seq=3 ttl=118 time=17.6 ms
```

+ Il doit connaître l'adresse d'un serveur DNS pour avoir de la résolution de noms
```
$ dig gitlab.com

; <<>> DiG 9.16.23-RH <<>> gitlab.com
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 63139
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1232
;; QUESTION SECTION:
;gitlab.com.			IN	A

;; ANSWER SECTION:
gitlab.com.		222	IN	A	172.65.251.78

;; Query time: 20 msec
;; SERVER: 1.1.1.1#53(1.1.1.1)
;; WHEN: Thu Nov 03 10:40:39 CET 2022
;; MSG SIZE  rcvd: 55
```

# 2. Analyse de trames
## 🌞 Analyse de trames

**Mes vm ne fonctionne plus, elle ne veulent plus ping 8.8.8.8**

**j'ai essayer de chercher une solution mais je pense que c'est UTM qui doit avoir un problème**