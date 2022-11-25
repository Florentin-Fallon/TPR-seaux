# TP4 : TCP, UDP et services réseau
## I. First steps
### 🌞 Déterminez, pour ces 5 applications, si c'est du TCP ou de l'UDP

## Top 5 des applications 

+ Discord 
+ NeverStop (mon site internet)
+ Youtube
+ Microsoft Teams
+ Vectornator 

#### IP et port du serveur auquel vous vous connectez 

| Nom de l'application | Protocole | IP et port du serveur distant | Port local ouvert |
|----------------------|-----------|--------------------------------|--------------------|
|Discord | UDP (QUIC) | 162.159.135.232:443 | 54009 |
| NeverStop | TCP | 23.227.38.71:443 | 51875 |
| Youtube | TCP | 142.250.74.238:443 | 58283 |
| Microsoft Teams | TCP | 52.113.194.132:443 | 50299 |
| Vectornator | TCP | 44.240.84.34:443 | 58327 |

### 🌞 Demandez l'avis à votre OS

+ Discord 
```
% lsof -i -P

Discord   7023 florentinfallon   23u  IPv4 0x6d42cb910eaca74f      0t0  UDP 10.33.17.1:64295->162.159.128.233:443 (ESTABLISHED)
```
Fichier Wireshark [ici](tp4_Discord.pcapng)

+ NeverStop
```
% lsof -i -P

Google    20617 florentinfallon   64u  IPv4 0x6d42cb910e295b97      0t0  TCP 10.33.17.1:56129->23.227.38.74:443 (ESTABLISHED)
```

Fichier Wireshark [ici](tp4_Neverstop.pcapng)

+ Youtube 
```
% lsof -i -P

Google    20617 florentinfallon   47u  IPv4 0x6d42cb910e2966a7      0t0  TCP 10.33.17.1:56120->par21s22-in-f10.1e100.net:443 (CLOSED)
```

Fichier Wireshark [ici](tp4_Youtube.pcapng)

+ Microsoft Teams
```
% lsof -i -P

Microsoft 7289 florentinfallon   27u  IPv4 0x6d42cb910e2a1087      0t0  TCP 10.33.17.1:50305->52.113.205.22:443 (ESTABLISHED)
```

Fichier Wireshark [ici](tp4_Microsoft.pcapng)

+ Vectornator
```
% lsof -i -P

Code\x20H 20216 florentinfallon   30u  IPv4 0x6d42cb910e2971b7      0t0  TCP 10.33.17.1:56082->51.144.164.215:443 (CLOSED)
```

Fichier Wireshark [ici](tp4_Vectornator.pcapng)


## II. Mise en place

## 1. SSH

### 🌞 Examinez le trafic dans Wireshark & Demandez aux OS
SSH utilise bien évidemment TCP (vérifier)


Connexion au ssh via mon terminal mac **ssh tp3@192.168.30.11**

Requête reçu sur wireshark [ici](Tp4-SSH.pcapng)

On peut voir la connexion SSH depuis mon hôte :
```
ssh       21785 florentinfallon    3u  IPv4 0x6d42cb910e2a1b97      0t0  TCP 192.168.30.1:56280->node1.tp4.b1:22 (ESTABLISHED)
```

Et depuis la VM :
```
tcp            ESTAB           0                0                                                       192.168.30.11:ssh                                    192.168.30.1:56340 
```

## 2. Routage

...

## III. DNS

...

### 🌞 Dans le rendu, je veux


### 🌞 Ouvrez le bon port dans le firewall


## 3. Test

### 🌞 Sur la machine node1.tp4.b1

### 🌞 Sur votre PC

Je suis désolée mais j'ai un probleme avec UTM, le ssh ne veut meme plus ce connecter et je n'arrive pas a installez le dns.
J'ai tout essayer mais ça ne fonctionne pas 




