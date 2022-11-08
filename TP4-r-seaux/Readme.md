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
| Youtube | UDP (QUIC) | 142.250.178.142:443 | 58283 |
| Microsoft Teams | TCP | 52.113.194.132:443 | 50299 |
| Vectornator | TCP | 44.232.229.175:443 | 58327 |

### 🌞 Demandez l'avis à votre OS

+ Discord 
```
Discord   7023 florentinfallon   23u  IPv4 0x6d42cb910eaca74f      0t0  UDP 10.33.17.1:64295->162.159.128.233:443 (ESTABLISHED)
```
Fichier Wireshark [ici](tp4_Discord.pcapng)

+ NeverStop

Fichier Wireshark [ici](tp4_Neverstop.pcapng)

+ Youtube 

Fichier Wireshark [ici](tp4_Youtube.pcapng)

+ Microsoft Teams
```
Microsoft 7289 florentinfallon   27u  IPv4 0x6d42cb910e2a1087      0t0  TCP 10.33.17.1:50305->52.113.205.22:443 (ESTABLISHED)
```

Fichier Wireshark [ici](tp4_Microsoft.pcapng)

+ Vectornator

Fichier Wireshark [ici](tp4_Vectornator.pcapng)


## II. Mise en place

## 1. SSH

### 🌞 Examinez le trafic dans Wireshark

+ déterminez si SSH utilise TCP ou UDP

+ repérez le 3-Way Handshake à l'établissement de la connexion

+ repérez du trafic SSH

+ repérez le FIN ACK à la fin d'une connexion

### 🌞 Demandez aux OS