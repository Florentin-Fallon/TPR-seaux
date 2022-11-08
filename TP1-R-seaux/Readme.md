# Rendu TP Réseaux 

## 1. Affichage d'informations sur la pile TCP/IP locale

Résultat optenue : 

```
% ifconfig
en0: flags=8863<UP,BROADCAST,SMART,RUNNING,SIMPLEX,MULTICAST> mtu 1500
	options=6463<RXCSUM,TXCSUM,TSO4,TSO6,CHANNEL_IO,PARTIAL_CSUM,ZEROINVERT_CSUM>
	ether 9c:3e:53:8c:90:1b 
	inet6 fe80::1c3a:9bdf:9f87:befd%en0 prefixlen 64 secured scopeid 0xb 
	inet 10.33.16.181 netmask 0xfffffc00 broadcast 10.33.19.255
	nd6 options=201<PERFORMNUD,DAD>
	media: autoselect
	status: active
```
### Carte ethernet :

Je n'en ai pas 

## Affichez votre gateway

Résultat optenue : 
```
 % route get default | grep gateway
    gateway: 10.33.19.254
```

## Déterminer la MAC de la passerelle

Résultat obtenue :

```
% arp -a
? (10.33.19.254) at 0:c0:e7:e0:4:4e on en0 ifscope [ethernet]
```

## Trouvez comment afficher les informations sur une carte IP (change selon l'OS)

Résultat obtenue :


![Interface adresse IP](https://cdn.discordapp.com/attachments/989814795857436733/1026424191311949824/Capture_decran_2022-10-03_a_11.21.33.png)

![Adresse Mac](https://cdn.discordapp.com/attachments/989814795857436733/1026425017799548948/Capture_decran_2022-10-03_a_11.25.25.png)

![Gateway](https://cdn.discordapp.com/attachments/989814795857436733/1026425484386508800/Capture_decran_2022-10-03_a_11.27.58.png)

## 2. Modifications des informations

### A. Modification d'adresse IP (part 1)

Résultat obtenue :

![Modification adresse IP](https://cdn.discordapp.com/attachments/989814795857436733/1026427968005885964/Capture_decran_2022-10-03_a_11.37.15.png)
```
Si on perd l'acces a internet c'est parce que l'on a demander une adresse IP deja occuper par un autre utilisateur vue qu'il y étais en premier il reste prioritaire 
```
# II. Exploration locale en duo

## Modifiez l'IP des deux machines pour qu'elles soient dans le même réseau

Résultat obtenue :

![Modification adresse IP](https://cdn.discordapp.com/attachments/989814795857436733/1026437371585114112/Capture_decran_2022-10-03_a_12.15.22.png)
```
J'ai changer l'adresse IP 10.10.10.2, puis le masque 255.255.255.0 et les autres cases vide
```

## Vérifier à l'aide d'une commande que votre IP a bien été changée

Résultat obtenue :
````
% ifconfig
en5: flags=8863<UP,BROADCAST,SMART,RUNNING,SIMPLEX,MULTICAST> mtu 1500
    options=400<CHANNEL_IO>
    ether f8:e4:3b:bd:7e:c0 
    inet6 fe80::88e:d574:ae7d:d680%en5 prefixlen 64 secured scopeid 0xa 
    inet 10.10.10.2 netmask 0xffffff00 broadcast 10.10.10.255
    nd6 options=201<PERFORMNUD,DAD>
    media: autoselect (1000baseT <full-duplex>)
    status: active
````
## Vérifier que les deux machines se joignent

L'ordinateur a envoyer un ping et nous avons reçu :
```
% ping 10.10.10.1
PING 10.10.10.1 (10.10.10.1): 56 data bytes
64 bytes from 10.10.10.1: icmp_seq=0 ttl=128 time=0.578 ms
64 bytes from 10.10.10.1: icmp_seq=1 ttl=128 time=0.553 ms
64 bytes from 10.10.10.1: icmp_seq=2 ttl=128 time=0.747 ms
64 bytes from 10.10.10.1: icmp_seq=3 ttl=128 time=0.680 ms
64 bytes from 10.10.10.1: icmp_seq=4 ttl=128 time=0.689 ms
64 bytes from 10.10.10.1: icmp_seq=5 ttl=128 time=0.637 ms
64 bytes from 10.10.10.1: icmp_seq=6 ttl=128 time=1.064 ms
64 bytes from 10.10.10.1: icmp_seq=7 ttl=128 time=0.661 ms
64 bytes from 10.10.10.1: icmp_seq=8 ttl=128 time=0.666 ms
64 bytes from 10.10.10.1: icmp_seq=9 ttl=128 time=0.627 ms
64 bytes from 10.10.10.1: icmp_seq=10 ttl=128 time=0.620 ms
64 bytes from 10.10.10.1: icmp_seq=11 ttl=128 time=0.699 ms
```

## Déterminer l'adresse MAC de votre correspondant
J'ai effectuer la commande dans mon terminal pour connaitre l'adresse MAC de l'autre ordinateur qui est branché par le cable ethernet 

Résultat obtenue : 
```
% arp -a 
? (10.10.10.1) at d8:bb:c1:1e:ae:fa on en5 ifscope [ethernet]
```
# 4. Utilisation d'un des deux comme gateway

## Tester l'accès internet
```
% ping 1.1.1.1
PING 1.1.1.1 (1.1.1.1): 56 data bytes
64 bytes from 1.1.1.1: icmp_seq=0 ttl=54 time=22.000 ms
64 bytes from 1.1.1.1: icmp_seq=1 ttl=54 time=20.916 ms
64 bytes from 1.1.1.1: icmp_seq=2 ttl=54 time=21.239 ms
64 bytes from 1.1.1.1: icmp_seq=3 ttl=54 time=21.547 ms
64 bytes from 1.1.1.1: icmp_seq=4 ttl=54 time=21.045 ms
64 bytes from 1.1.1.1: icmp_seq=5 ttl=54 time=23.876 ms
64 bytes from 1.1.1.1: icmp_seq=6 ttl=54 time=23.541 ms
64 bytes from 1.1.1.1: icmp_seq=7 ttl=54 time=22.580 ms
64 bytes from 1.1.1.1: icmp_seq=8 ttl=54 time=22.231 ms
64 bytes from 1.1.1.1: icmp_seq=9 ttl=54 time=21.988 ms
64 bytes from 1.1.1.1: icmp_seq=10 ttl=54 time=24.344 ms
64 bytes from 1.1.1.1: icmp_seq=11 ttl=54 time=22.650 ms
64 bytes from 1.1.1.1: icmp_seq=12 ttl=54 time=22.828 ms
64 bytes from 1.1.1.1: icmp_seq=13 ttl=54 time=21.234 ms
64 bytes from 1.1.1.1: icmp_seq=14 ttl=54 time=21.386 ms
64 bytes from 1.1.1.1: icmp_seq=15 ttl=54 time=21.674 ms
64 bytes from 1.1.1.1: icmp_seq=16 ttl=54 time=21.331 ms
64 bytes from 1.1.1.1: icmp_seq=17 ttl=54 time=22.311 ms
64 bytes from 1.1.1.1: icmp_seq=18 ttl=54 time=24.649 ms
64 bytes from 1.1.1.1: icmp_seq=19 ttl=54 time=22.844 ms
```
## Prouver que la connexion Internet passe bien par l'autre PC
```
$ traceroute 1.1.1.1
traceroute to 1.1.1.1 (1.1.1.1), 64 hops max, 52 byte packets
 1  192.168.137.1 (192.168.137.1)  0.967 ms
```
# 5. Petit chat privé

## Sur le PC serveur avec par exemple l'IP 192.168.1.1

Le programme netcat a été considéré comme un virus par Windows (c'est un faux-positif, la paranoïa de Microsoft). J'ai donc dû temporairement désactiver cet antivirus qu'on me force d'utiliser.

Ici, je suis le PC serveur

J'ai entré la commande : `nc -l -p 8888`

Retour : 
```
vuyoht
bonjour
Bonjour.
Bonjours
Bonjourno
Bonjouras
Bonjournias
Bonjournias
```

Messages provenant du client : 
```
vuyoht
bonjour
```

Messages envoyés du serveur :
```
Bonjour.
Bonjours
Bonjourno
Bonjouras
Bonjournias
Bonjournias
```

## Sur le PC client avec par exemple l'IP 192.168.1.2

 Aprés la connection du client sur le serveur nous pouvons envoyer les messages 
```
% nc 192.168.137.1 8888                         
vuyoht 
bonjour 
Bonjour.
Bonjours
Bonjourno
```
## Visualiser la connexion en cours

### Coté client :

Commande : `netstat -a -n`

Retour :
```
% netstat -a -n
Active Internet connections (including servers)
Proto Recv-Q Send-Q  Local Address          Foreign Address        (state)    
tcp4       0      0  192.168.137.2.54320    192.168.137.1.8888     ESTABLISHED
tcp4       0      0  127.0.0.1.6463         *.*                    LISTEN     
tcp4       0      0  10.33.16.254.54301     162.159.136.234.443    ESTABLISHED
tcp6       0      0  *.5000                 *.*                    LISTEN     
```
### Coté serveur :

Commande : `netstat -a -n -b`

Retour : 

Connexions actives

```
TCP    192.168.137.1:8888     192.168.137.2:54320    ESTABLISHED
 [nc.exe]
TCP    192.168.211.1:139      0.0.0.0:0              LISTENING
```

## Pour aller un peu plus loin

On peut observer ceci :

```
TCP    192.168.211.1:139      0.0.0.0:0              LISTENING
```

`0.0.0.0:0` veut dire que le serveur écoute toutes les adresses IP, donc toutes les interfaces. On peut donc s'y connecter en Wi-Fi.

Lancement d'un serveur netcat sur l'interface Ethernet uniquement

Commande : `nc -l -p 8888 -s 192.168.137.1`

Retour (message du client) : 
```
sdfghjk
```

Résultat de la commande `netstat -a -n -b` :
```
  TCP    192.168.137.1:139      0.0.0.0:0              LISTENING
 Impossible d’obtenir les informations de propriétaire
  TCP    192.168.137.1:8888     0.0.0.0:0              LISTENING
  ```

  # 6. Firewall

  ## Activez et configurez votre firewall

  Résultat obtenue : 

  ![Activation couvre feu](https://cdn.discordapp.com/attachments/989814795857436733/1027493993166155776/Capture_decran_2022-10-06_a_10.13.27.png)

  ![Parametre couvre feu](https://cdn.discordapp.com/attachments/989814795857436733/1027496813416169472/Capture_decran_2022-10-06_a_10.25.12.png)

  # III. Manipulations d'autres outils/protocoles côté client

  # 1. DHCP

  ## Exploration du DHCP, depuis votre PC
# identifier le nom du périphérique réseau:

Commande : `networksetup -listallhardwareports`

Retour : 
```
Hardware Port: Wi-Fi
Device: en0
Ethernet Address: 9c:3e:53:8c:90:1b
```
# Détail du DHCP :

Commande : `ipconfig getpacket en0``

Retour : 
```
dhcp_message_type (uint8): ACK 0x5
server_identifier (ip): 10.33.19.254
lease_time (uint32): 0x150a5
subnet_mask (ip): 255.255.252.0
router (ip_mult): {10.33.19.254}
domain_name_server (ip_mult): {8.8.8.8, 8.8.4.4, 1.1.1.1}
end (none): 
```
Pour connaitre la durée du bail DHCP, il est afficher dans le terminal en Hexadecimal `0x150a5` et nous devons le convertir en Decimal ce qui donne `86181`.
La durée du bail en secondes, ce qui équivaut à `86181` secondes = `23 heures 56 minutes 21 secondes`

# 2. DNS

## ** Trouver l'adresse IP du serveur DNS que connaît votre ordinateur**

Commande : `scutil --dns | grep 'nameserver\[[0-9]*\]'``

Retour : 
```
nameserver[0] : 8.8.8.8
  nameserver[1] : 8.8.4.4
  nameserver[2] : 1.1.1.1
  nameserver[0] : 8.8.8.8
  nameserver[1] : 8.8.4.4
  nameserver[2] : 1.1.1.1
  ```
## Utiliser, en ligne de commande l'outil nslookup (Windows, MacOS) ou dig (GNU/Linux, MacOS) pour faire des requêtes DNS à la main

### Google 

Résultat : 
```
% nslookup
> google.com
Server:		8.8.8.8
Address:	8.8.8.8#53

Non-authoritative answer:
Name:	google.com
Address: 172.217.18.206
```
### Ynov

Résultat :
```
% nslookup                                  
> ynov.com
Server:		8.8.8.8
Address:	8.8.8.8#53

Non-authoritative answer:
Name:	ynov.com
Address: 104.26.10.233
Name:	ynov.com
Address: 104.26.11.233
Name:	ynov.com
Address: 172.67.74.226
```
### Interprétation

Nom de domaine : `google.com`
Adresse IP : `172.217.18.206`
Adresse serveur DNS : `8.8.8.8`

Nom de domaine : `ynov.com`
Adresse IP : `104.26.10.233`
Adresse serveur DNS : `8.8.8.8`

## Reverse lookup de `231.34.113.12`

Commande : `nslookup 231.34.113.12`
```
Server:		8.8.8.8
Address:	8.8.8.8#53
** server can't find 12.113.34.231.in-addr.arpa: NXDOMAIN
```

L'adresse IP `231.34.113.12`ne correspond à aucun nom de domaine 

## Reverse lookup de `78.34.2.17`

Commande : `nslookup 78.34.2.17`
```
Server:		8.8.8.8
Address:	8.8.8.8#53

Non-authoritative answer:
17.2.34.78.in-addr.arpa	name = cable-78-34-2-17.nc.de.
```
L'adresse IP `78.34.2.17` correspond au nom de domaine `cable-78-34-2-17.nc.de.` sur le DNS Google 

# IV. Wireshark

## Utilisez le pour observer les trames qui circulent entre vos deux carte Ethernet. Mettez en évidence :

Commande : `ping 10.33.19.254`

Résultat depuis Wireshark : 

![Résultat Ping](https://cdn.discordapp.com/attachments/989814795857436733/1027518114319048734/Capture_decran_2022-10-06_a_11.49.42.png)

### Netcat
![](https://media.discordapp.net/attachments/741645926447317123/1027521355773648906/unknown.png?width=1440&height=67)

### Requête DNS

![](https://media.discordapp.net/attachments/741645926447317123/1027520925995905095/unknown.png?width=1440&height=44)

Le serveur DNS est à l'adresse : `8.8.8.8`

# 2. Bonus : avant-goût TCP et UDP

## Wireshark it : 
### Déterminez à quelle IP et quel port votre PC se connecte quand vous regardez une vidéo Youtube 

![Youtube](https://cdn.discordapp.com/attachments/989814795857436733/1027525616133750794/Capture_decran_2022-10-06_a_12.19.02.png)

Adresse IP : `172.217.18.206` 
Port : `52585` (source)
Destination port : `443` (Destination)
