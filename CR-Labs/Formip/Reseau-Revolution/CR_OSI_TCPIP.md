# CR Lab — Modèle OSI & TCP/IP

**Module :** Réseau Révolution — RR_01  
**Date :** <!-- à compléter -->  
**Durée :** <!-- ex: 1h -->  
**Outil :** Cisco Packet Tracer (mode simulation)  
**Niveau :** Formip + Jeremy IT Lab Day 03/04/05/06

---

## 🎯 Objectifs du lab

- Identifier les 7 couches OSI et leurs rôles
- Comprendre l'encapsulation / décapsulation
- Observer les PDU (Protocol Data Units) dans Packet Tracer
- Comprendre les différences OSI vs TCP/IP

---

## 📚 Rappel des couches

### Modèle OSI

| Couche | Nom | PDU | Protocoles / Rôle |
|---|---|---|---|
| 7 | Application | Data | HTTP, FTP, DNS, DHCP |
| 6 | Présentation | Data | SSL/TLS, chiffrement |
| 5 | Session | Data | Gestion sessions |
| 4 | Transport | Segment | TCP, UDP |
| 3 | Réseau | Paquet | IP, ICMP, ARP |
| 2 | Liaison | Trame | Ethernet, MAC, Switch |
| 1 | Physique | Bits | Câbles, hubs, signaux |

### Modèle TCP/IP (4 couches)

| Couche TCP/IP | Équivalent OSI |
|---|---|
| Application | 7 + 6 + 5 |
| Transport | 4 |
| Internet | 3 |
| Accès réseau | 2 + 1 |

---

## 🔬 Observation dans Packet Tracer

### Mode Simulation — Observer un ping

1. Passer en mode **Simulation** (horloge en bas à droite)
2. Filtrer sur **ICMP**
3. Lancer un ping : `PC> ping 192.168.1.2`
4. Cliquer sur les enveloppes pour voir les PDU

### Ce qu'on observe

```
PC → Switch → Router → Switch → PC

Couche 2 : Trame Ethernet (MAC src/dst)
Couche 3 : Paquet IP (IP src/dst)
Couche 4 : Segment ICMP
```

---

## ✅ Résultats

| Observation | Résultat |
|---|---|
| Encapsulation visible couche par couche | ✅ |
| MAC src/dst change à chaque saut | ✅ |
| IP src/dst reste identique | ✅ |
| PDU ICMP identifié | ✅ |

---

## 💡 Ce que j'ai appris / Difficultés

<!-- À compléter -->
- 
- 

---

## 🔗 Liens utiles

- [Jeremy IT Lab Day 03 — OSI Model](https://www.youtube.com/watch?v=...)
- [Formip RR_01](https://www.formip.com)
