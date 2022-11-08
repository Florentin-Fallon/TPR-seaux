# Rendu TP_Reseau_n°2

# I. Setup IP

## Mettez en place une configuration réseau fonctionnelle entre les deux machines

Commande : `% sudo ipconfig set en5 INFORM 10.69.1.11 255.255.252.0`

```
en5: flags=8863<UP,BROADCAST,SMART,RUNNING,SIMPLEX,MULTICAST> mtu 1500
    options=400<CHANNEL_IO>
    ether f8:e4:3b:bd:7e:c0 
    inet6 fe80::88e:d574:ae7d:d680%en5 prefixlen 64 secured scopeid 0xa 
    inet 10.69.1.11 netmask 0xfffffc00 broadcast 10.69.3.255
    nd6 options=201<PERFORMNUD,DAD>
    media: autoselect (1000baseT <full-duplex>)
    status: active
```
```
-[ipv4 : 10.69.1.11/22] - 0

[CIDR]
Host address        - 10.69.1.11
Host address (decimal)    - 172294411
Host address (hex)    - A45010B
Network address        - 10.69.0.0
Network mask        - 255.255.252.0
Network mask (bits)    - 22
Network mask (hex)    - FFFFFC00
Broadcast address    - 10.69.3.255
Cisco wildcard        - 0.0.3.255
Addresses in network    - 1024
Network range        - 10.69.0.0 - 10.69.3.255
Usable range        - 10.69.0.1 - 10.69.3.254
```
## Prouver que la connexion est fonctionnelle entre les deux machines 
```
 % ping 10.69.1.12
PING 10.69.1.12 (10.69.1.12): 56 data bytes
64 bytes from 10.69.1.12: icmp_seq=0 ttl=128 time=1.204 ms
64 bytes from 10.69.1.12: icmp_seq=1 ttl=128 time=1.047 ms
64 bytes from 10.69.1.12: icmp_seq=2 ttl=128 time=1.242 ms
64 bytes from 10.69.1.12: icmp_seq=3 ttl=128 time=1.126 ms
64 bytes from 10.69.1.12: icmp_seq=4 ttl=128 time=0.629 ms
64 bytes from 10.69.1.12: icmp_seq=5 ttl=128 time=0.606 ms
```

## Wireshark it

Type de packet PING = `8` (echo (ping) request)

Type de packet PONG = `0` (echo (ping) reply)

# II. ARP my bro

## Check the ARP table
### Utilisez une commande pour afficher votre table ARP
```
% arp -a
? (10.33.19.102) at f0:18:98:77:25:f9 on en0 ifscope [ethernet]
? (10.33.19.151) at 3e:1a:c8:e8:73:c0 on en0 ifscope [ethernet]
? (10.33.19.179) at 66:53:b9:4e:1c:58 on en0 ifscope [ethernet]
? (10.33.19.197) at ee:98:75:64:98:69 on en0 ifscope [ethernet]
? (10.33.19.219) at 56:7:a3:66:71:b7 on en0 ifscope [ethernet]
? (10.33.19.254) at 0:c0:e7:e0:4:4e on en0 ifscope [ethernet]
? (10.33.19.255) at ff:ff:ff:ff:ff:ff on en0 ifscope [ethernet]
? (10.69.1.12) at d8:bb:c1:1e:ae:fa on en5 ifscope [ethernet]
 ```

## Déterminez la MAC de votre binome depuis votre table ARP
```
? (10.69.1.12) at d8:bb:c1:1e:ae:fa on en5 ifscope [ethernet]
```

### Déterminez la MAC de la gateway de votre réseau
```
? (10.33.19.254) at 0:c0:e7:e0:4:4e on en0 ifscope [ethernet]
? (10.33.19.255) at ff:ff:ff:ff:ff:ff on en0 ifscope [ethernet]
```

## Manipuler la table ARP

### Utilisez une commande pour vider votre table ARP

### Prouvez que ça fonctionne en l'affichant et en constatant les changements
```
% sudo arp -d -a
Password:
10.33.16.11 (10.33.16.11) deleted
10.33.16.19 (10.33.16.19) deleted
10.33.16.81 (10.33.16.81) deleted
10.33.16.83 (10.33.16.83) deleted
10.33.16.85 (10.33.16.85) deleted
10.33.16.88 (10.33.16.88) deleted
10.33.16.97 (10.33.16.97) deleted
10.33.16.109 (10.33.16.109) deleted
10.33.16.251 (10.33.16.251) deleted
10.33.17.6 (10.33.17.6) deleted
10.33.17.9 (10.33.17.9) deleted
10.33.17.21 (10.33.17.21) deleted
10.33.17.24 (10.33.17.24) deleted
10.33.17.28 (10.33.17.28) deleted
10.33.17.46 (10.33.17.46) deleted
10.33.17.108 (10.33.17.108) deleted
10.33.17.163 (10.33.17.163) deleted
10.33.17.182 (10.33.17.182) deleted
10.33.17.183 (10.33.17.183) deleted
10.33.18.80 (10.33.18.80) deleted
10.33.18.84 (10.33.18.84) deleted
10.33.18.87 (10.33.18.87) deleted
10.33.19.191 (10.33.19.191) deleted
10.33.19.254 (10.33.19.254) deleted
10.69.1.12 (10.69.1.12) deleted
169.254.16.79 (169.254.16.79) deleted
```
## Ré-effectuez des pings, et constatez la ré-apparition des données dans la table ARP
```
% ping 10.69.1.12
PING 10.69.1.12 (10.69.1.12): 56 data bytes
64 bytes from 10.69.1.12: icmp_seq=0 ttl=128 time=0.544 ms
64 bytes from 10.69.1.12: icmp_seq=1 ttl=128 time=0.780 ms
64 bytes from 10.69.1.12: icmp_seq=2 ttl=128 time=0.631 ms
```
```
% arp -a 
? (10.33.16.32) at 4a:e8:8f:11:6:bf on en0 ifscope [ethernet]
? (10.33.16.42) at 8e:5d:2f:a5:51:a5 on en0 ifscope [ethernet]
? (10.69.1.12) at d8:bb:c1:1e:ae:fa on en5 ifscope [ethernet]
```
## Wireshark it

### Mettez en évidence les deux trames ARP échangées lorsque vous essayez de contacter quelqu'un pour la "première" fois

### Déterminez, pour les deux trames, les adresses source et destination

j'ai vidé la table ARP avec la commande :
`% sudo arp -d -a`  

Puis, j'ai ping le routeur 10.33.19.254 et j'ai capturer [ces paquets](ARP.pcapng)

## Déterminez à quoi correspond chacune de ces adresses

**Paquet 1 : ARP Broadcast**
- Sources : `Apple_8c:90:1b`
- Destination : `Broadcast`

**Paquet 2 : ARP Reply**
- Sources : `Fiberdat_e0:04:4e`
- Destination : `Apple_8c:90:1b`

Ces adresse corespondent aux adresses MAC des cartes réseaux de mon mac (`Apple_8c:90:1b:`) ainsi que du routeur (`Fiberdat_e0:04:4e`)

## III. DHCP you too my brooo

### Wireshark it

On peut voir les 4 Trames DHCP *DORA* [Sur ce lien ](DORA.pcapng)

**Paquets 1 et 3**

- Source : `0.0.0.0` (C'est une adresse IP spéciale, elle est généralement utilisée pour dire qu'on écoute sur toutes les interfaces réseau sur un système)

- Destination : `255.255.255.255` (Adresse de broadcast : le PC communique avec tout le monde)

**Paquets 2 et 4**

- Source : `10.33.19.254` (L'adresse du routeur)

- Destination : `10.33.17.1` (Mon adresse IP)

### Identifiez dans ces 4 trames les informations 1, 2 et 3 dont on a parlé juste au dessus

- L'adresse IP à utiliser : `10.33.17.1`
- L'adresse IP de la passerelle du réseau : `10.33.19.254`
- L'adresse d'un serveur DNS joignable depuis le réseau : 
`8.8.8.8` (le DNS de Google)