# CR Lab — VLAN & Trunk 802.1Q

**Module :** Switching Saga — SS_02  
**Date :** <!-- à compléter -->  
**Durée :** <!-- ex: 1h30 -->  
**Outil :** Cisco Packet Tracer  
**Niveau :** Formip + Jeremy IT Lab Day 16/17/18/19

---

## 🎯 Objectifs du lab

- Créer et nommer des VLANs sur un switch Cisco
- Attribuer des ports en mode access à un VLAN
- Configurer un lien Trunk 802.1Q entre deux switches
- Vérifier l'isolation et la communication inter-VLAN

---

## 🗺️ Topologie

```
PC1 (VLAN 10)──┐                    ┌──PC3 (VLAN 10)
               SW1 ═══[Trunk]═══ SW2
PC2 (VLAN 20)──┘                    └──PC4 (VLAN 20)

VLAN 10 : Compta   (192.168.10.0/24)
VLAN 20 : RH       (192.168.20.0/24)
```

---

## ⌨️ Commandes utilisées

### Création des VLANs

```cisco
SW1# conf t
SW1(config)# vlan 10
SW1(config-vlan)# name Compta
SW1(config-vlan)# exit
SW1(config)# vlan 20
SW1(config-vlan)# name RH
SW1(config-vlan)# exit
```

### Attribution des ports Access

```cisco
SW1(config)# interface fa0/1
SW1(config-if)# switchport mode access
SW1(config-if)# switchport access vlan 10
SW1(config-if)# exit

SW1(config)# interface fa0/2
SW1(config-if)# switchport mode access
SW1(config-if)# switchport access vlan 20
```

### Configuration du Trunk

```cisco
SW1(config)# interface gi0/1
SW1(config-if)# switchport mode trunk
SW1(config-if)# switchport trunk allowed vlan 10,20
SW1(config-if)# exit
```

### Vérifications

```cisco
SW1# show vlan brief
SW1# show interfaces trunk
SW1# show interfaces fa0/1 switchport
```

---

## ✅ Résultats

| Test | Résultat |
|---|---|
| PC1 → PC3 (VLAN 10) | ✅ Ping OK |
| PC2 → PC4 (VLAN 20) | ✅ Ping OK |
| PC1 → PC2 (inter-VLAN) | ❌ Isolés — Normal |
| show vlan brief | ✅ VLANs visibles |
| show interfaces trunk | ✅ Trunk actif |

---

## 💡 Ce que j'ai appris / Difficultés

<!-- À compléter avec ton ressenti -->
- 
- 

---

## 🔗 Liens utiles

- [Jeremy IT Lab Day 16 — VLANs](https://www.youtube.com/watch?v=...)
- [Formip SS_02](https://www.formip.com)
