# CR Lab — STP / RSTP

**Module :** Switching Saga — SS_03  
**Date :** <!-- à compléter -->  
**Durée :** <!-- ex: 2h -->  
**Outil :** Cisco Packet Tracer  
**Niveau :** Formip + Jeremy IT Lab Day 20/21/22

---

## 🎯 Objectifs du lab

- Comprendre le rôle du STP (éviter les boucles de commutation)
- Identifier le Root Bridge et les rôles des ports (Root, Designated, Blocked)
- Configurer la priorité pour forcer un Root Bridge
- Activer PortFast et BPDU Guard sur les ports access
- Observer la convergence RSTP

---

## 🗺️ Topologie

```
         SW1 (Root Bridge)
        /                 \
      SW2 ─────────────── SW3
      
Priorités :
SW1 : 4096  (Root Bridge forcé)
SW2 : 32768 (défaut)
SW3 : 32768 (défaut)
```

---

## ⌨️ Commandes utilisées

### Forcer le Root Bridge

```cisco
SW1(config)# spanning-tree vlan 1 priority 4096
```

### Vérifier l'élection

```cisco
SW1# show spanning-tree vlan 1
SW2# show spanning-tree vlan 1
```

### PortFast + BPDU Guard (ports access)

```cisco
SW2(config)# interface fa0/1
SW2(config-if)# spanning-tree portfast
SW2(config-if)# spanning-tree bpduguard enable
```

### Activer RSTP

```cisco
SW1(config)# spanning-tree mode rapid-pvst
SW2(config)# spanning-tree mode rapid-pvst
SW3(config)# spanning-tree mode rapid-pvst
```

---

## ✅ Résultats

| Test | Résultat |
|---|---|
| SW1 élu Root Bridge | ✅ |
| Port bloqué identifié sur SW3 | ✅ |
| Convergence après coupure lien | ✅ ~2s avec RSTP |
| PortFast actif sur fa0/1 | ✅ |
| BPDU Guard : err-disabled si BPDU reçu | ✅ |

---

## 💡 Ce que j'ai appris / Difficultés

<!-- À compléter -->
- 
- 

---

## 🔗 Liens utiles

- [Jeremy IT Lab Day 20 — STP](https://www.youtube.com/watch?v=...)
- [Formip SS_03](https://www.formip.com)
