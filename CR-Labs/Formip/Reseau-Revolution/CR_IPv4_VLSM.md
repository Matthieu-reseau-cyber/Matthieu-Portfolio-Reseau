# CR Lab — IPv4 & VLSM

**Module :** Réseau Révolution — RR_02  
**Date :** <!-- à compléter -->  
**Durée :** <!-- ex: 1h30 -->  
**Outil :** Cisco Packet Tracer  
**Niveau :** Formip + Jeremy IT Lab Day 07/08/10/13

---

## 🎯 Objectifs du lab

- Maîtriser le découpage en sous-réseaux (VLSM)
- Configurer les interfaces d'un routeur avec les bonnes adresses IP
- Vérifier la connectivité bout en bout
- Comprendre la notation CIDR

---

## 🗺️ Topologie

```
Réseau principal : 192.168.1.0/24

Sous-réseaux VLSM :
├── LAN A : 192.168.1.0/26   (62 hôtes) — Compta
├── LAN B : 192.168.1.64/27  (30 hôtes) — RH  
├── LAN C : 192.168.1.96/28  (14 hôtes) — Direction
└── WAN   : 192.168.1.112/30 (2 hôtes)  — Lien R1-R2

PC_A (192.168.1.1) ── R1 (Gi0/0: .1/26 | Gi0/1: .113/30) ── R2
```

---

## ⌨️ Commandes utilisées

### Configuration interface routeur

```cisco
R1(config)# interface gigabitEthernet 0/0
R1(config-if)# ip address 192.168.1.1 255.255.255.192
R1(config-if)# no shutdown
R1(config-if)# description LAN-Compta
R1(config-if)# exit

R1(config)# interface gigabitEthernet 0/1
R1(config-if)# ip address 192.168.1.113 255.255.255.252
R1(config-if)# no shutdown
R1(config-if)# description WAN-vers-R2
```

### Vérifications

```cisco
R1# show ip interface brief
R1# show ip route
R1# ping 192.168.1.65
```

---

## 🧮 Calcul VLSM — Méthode

| Réseau | Besoins | Masque | Plage | Broadcast |
|---|---|---|---|---|
| LAN A | 62 hôtes | /26 (255.255.255.192) | .0 → .63 | .63 |
| LAN B | 30 hôtes | /27 (255.255.255.224) | .64 → .95 | .95 |
| LAN C | 14 hôtes | /28 (255.255.255.240) | .96 → .111 | .111 |
| WAN | 2 hôtes | /30 (255.255.255.252) | .112 → .115 | .115 |

---

## ✅ Résultats

| Test | Résultat |
|---|---|
| Interfaces Up/Up | ✅ |
| Ping PC_A → Gi0/0 R1 | ✅ |
| show ip route : réseaux connectés | ✅ |
| Pas de chevauchement de sous-réseaux | ✅ |

---

## 💡 Ce que j'ai appris / Difficultés

<!-- À compléter -->
- 
- 

---

## 🔗 Liens utiles

- [Jeremy IT Lab Day 13 — Subnetting](https://www.youtube.com/watch?v=...)
- [Formip RR_02](https://www.formip.com)
