# CR Lab — IPv6

**Module :** Réseau Révolution — RR_06  
**Date :** <!-- à compléter -->  
**Durée :** <!-- ex: 1h30 -->  
**Outil :** Cisco Packet Tracer  
**Niveau :** Formip + Jeremy IT Lab Day 31/32/33

---

## 🎯 Objectifs du lab

- Comprendre le format d'une adresse IPv6
- Configurer des adresses IPv6 sur des interfaces Cisco
- Activer le routage IPv6
- Tester la connectivité avec ping6
- Comprendre les adresses Link-Local, Global Unicast, Multicast

---

## 📚 Rappel IPv6

### Format d'adresse

```
2001:0DB8:0000:0001:0000:0000:0000:0001/64
→ Simplifiée : 2001:DB8:0:1::1/64

Règles de simplification :
1. Supprimer les zéros en tête de chaque groupe
2. :: remplace un seul groupe de 0000 consécutifs
```

### Types d'adresses

| Type | Préfixe | Exemple |
|---|---|---|
| Global Unicast | 2000::/3 | 2001:DB8::/32 |
| Link-Local | FE80::/10 | FE80::1 |
| Loopback | ::1/128 | ::1 |
| Multicast | FF00::/8 | FF02::1 (all nodes) |

---

## 🗺️ Topologie

```
PC1 (2001:DB8:1::10/64) ── R1 ── R2 ── PC2 (2001:DB8:2::10/64)

R1 Gi0/0 : 2001:DB8:1::1/64
R1 Gi0/1 : 2001:DB8:12::1/64
R2 Gi0/0 : 2001:DB8:12::2/64
R2 Gi0/1 : 2001:DB8:2::1/64
```

---

## ⌨️ Commandes utilisées

### Activer IPv6 sur le routeur

```cisco
R1(config)# ipv6 unicast-routing
```

### Configurer une interface

```cisco
R1(config)# interface gi0/0
R1(config-if)# ipv6 address 2001:DB8:1::1/64
R1(config-if)# ipv6 enable
R1(config-if)# no shutdown
```

### Route statique IPv6

```cisco
R1(config)# ipv6 route 2001:DB8:2::/64 2001:DB8:12::2
```

### Vérifications

```cisco
R1# show ipv6 interface brief
R1# show ipv6 route
R1# ping ipv6 2001:DB8:2::10
```

---

## ✅ Résultats

| Test | Résultat |
|---|---|
| Interfaces IPv6 Up/Up | ✅ |
| Adresse Link-Local générée auto | ✅ FE80::... |
| Ping PC1 → PC2 en IPv6 | ✅ |
| show ipv6 route : routes statiques | ✅ |

---

## 💡 Ce que j'ai appris / Difficultés

<!-- À compléter -->
- 
- 

---

## 🔗 Liens utiles

- [Jeremy IT Lab Day 31 — IPv6](https://www.youtube.com/watch?v=...)
- [Formip RR_06](https://www.formip.com)
