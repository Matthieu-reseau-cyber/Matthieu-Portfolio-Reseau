# CR Lab — EtherChannel (LACP / PAgP)

**Module :** Switching Saga — SS_04  
**Date :** <!-- à compléter -->  
**Durée :** <!-- ex: 1h -->  
**Outil :** Cisco Packet Tracer  
**Niveau :** Formip + Jeremy IT Lab Day 23

---

## 🎯 Objectifs du lab

- Agréger plusieurs liens physiques en un seul lien logique
- Configurer EtherChannel avec LACP (standard IEEE 802.3ad)
- Vérifier la répartition de charge et la redondance
- Observer le comportement en cas de panne d'un lien

---

## 🗺️ Topologie

```
SW1 ══[Po1 : Fa0/1 + Fa0/2 + Fa0/3]══ SW2

Port-channel 1 = 3 x 100 Mbps = 300 Mbps logique
Mode : LACP (active/active)
```

---

## ⌨️ Commandes utilisées

### Configuration SW1

```cisco
SW1(config)# interface range fa0/1 - 3
SW1(config-if-range)# channel-group 1 mode active
SW1(config-if-range)# exit
SW1(config)# interface port-channel 1
SW1(config-if)# switchport mode trunk
```

### Configuration SW2

```cisco
SW2(config)# interface range fa0/1 - 3
SW2(config-if-range)# channel-group 1 mode active
SW2(config-if-range)# exit
SW2(config)# interface port-channel 1
SW2(config-if)# switchport mode trunk
```

### Vérifications

```cisco
SW1# show etherchannel summary
SW1# show etherchannel port-channel
SW1# show interfaces port-channel 1
```

---

## ✅ Résultats

| Test | Résultat |
|---|---|
| Port-channel Po1 en Up/Up | ✅ |
| 3 liens membres actifs | ✅ |
| Trafic maintenu si 1 lien tombe | ✅ Redondance OK |
| show etherchannel summary : SU | ✅ |

---

## 💡 Ce que j'ai appris / Difficultés

<!-- À compléter -->
- 
- 

---

## 🔗 Liens utiles

- [Jeremy IT Lab Day 23 — EtherChannel](https://www.youtube.com/watch?v=...)
- [Formip SS_04](https://www.formip.com)
