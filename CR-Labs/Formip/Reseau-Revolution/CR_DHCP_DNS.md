# CR Lab — DHCP & DNS

**Module :** Réseau Révolution — RR_03  
**Date :** <!-- à compléter -->  
**Durée :** <!-- ex: 1h -->  
**Outil :** Cisco Packet Tracer  
**Niveau :** Formip + Jeremy IT Lab Day 38/39

---

## 🎯 Objectifs du lab

- Configurer un serveur DHCP sur un routeur Cisco
- Exclure des adresses de la plage dynamique
- Vérifier l'attribution automatique d'IP sur les clients
- Configurer un relais DHCP (ip helper-address) entre sous-réseaux

---

## 🗺️ Topologie

```
PC1 ─┐
PC2 ─┤── SW1 ── R1 (DHCP Server)
PC3 ─┘          └── R2 ── PC4 (autre sous-réseau)

R1 Gi0/0 : 192.168.1.254/24  (LAN local)
R1 Gi0/1 : 10.0.0.1/30       (WAN vers R2)
R2 Gi0/0 : 192.168.2.254/24  (LAN distant)
```

---

## ⌨️ Commandes utilisées

### Serveur DHCP sur R1

```cisco
R1(config)# ip dhcp excluded-address 192.168.1.1 192.168.1.10
R1(config)# ip dhcp pool LAN1
R1(dhcp-config)# network 192.168.1.0 255.255.255.0
R1(dhcp-config)# default-router 192.168.1.254
R1(dhcp-config)# dns-server 8.8.8.8
R1(dhcp-config)# domain-name lab.local
R1(dhcp-config)# lease 7
R1(dhcp-config)# exit
```

### Relais DHCP (ip helper-address)

```cisco
R2(config)# interface gi0/0
R2(config-if)# ip helper-address 10.0.0.1
```

### Vérifications

```cisco
R1# show ip dhcp pool
R1# show ip dhcp binding
R1# show ip dhcp conflict
```

---

## ✅ Résultats

| Test | Résultat |
|---|---|
| PC1 obtient une IP en /24 | ✅ .11 (après exclusion) |
| Passerelle par défaut reçue | ✅ 192.168.1.254 |
| DNS reçu | ✅ 8.8.8.8 |
| PC4 obtient IP via relais | ✅ |
| show ip dhcp binding | ✅ 3 liaisons actives |

---

## 💡 Ce que j'ai appris / Difficultés

<!-- À compléter -->
- 
- 

---

## 🔗 Liens utiles

- [Jeremy IT Lab Day 38 — DHCP](https://www.youtube.com/watch?v=...)
- [Formip RR_03](https://www.formip.com)
