# CR Lab — Sécurisation SSH & Enable Secret

**Module :** Switching Saga / Réseau Révolution — RR_SS_05  
**Date :** <!-- à compléter -->  
**Durée :** <!-- ex: 1h -->  
**Outil :** Cisco Packet Tracer  
**Niveau :** Formip + Jeremy IT Lab Day 42/48-51

---

## 🎯 Objectifs du lab

- Sécuriser l'accès au mode privileged (enable secret)
- Désactiver Telnet et activer SSH v2
- Créer un utilisateur local avec mot de passe chiffré
- Restreindre l'accès VTY à SSH uniquement
- Tester la connexion SSH depuis un PC

---

## 🗺️ Topologie

```
PC1 (192.168.1.10) ── SW1 (192.168.1.1)
                       └── R1 (192.168.1.254)

SSH accessible depuis PC1 uniquement
```

---

## ⌨️ Commandes utilisées

### Nom de domaine + clé RSA

```cisco
R1(config)# ip domain-name lab.local
R1(config)# crypto key generate rsa modulus 2048
R1(config)# ip ssh version 2
```

### Utilisateur local

```cisco
R1(config)# username admin secret cisco123
```

### Sécurisation Enable

```cisco
R1(config)# enable secret cisco1
```

### Configuration VTY (SSH uniquement)

```cisco
R1(config)# line vty 0 4
R1(config-line)# transport input ssh
R1(config-line)# login local
R1(config-line)# exec-timeout 5 0
R1(config-line)# exit
```

### Désactiver la console après inactivité

```cisco
R1(config)# line console 0
R1(config-line)# exec-timeout 10 0
R1(config-line)# logging synchronous
```

### Vérifications

```cisco
R1# show ip ssh
R1# show users
```

---

## ✅ Résultats

| Test | Résultat |
|---|---|
| SSH v2 actif | ✅ |
| Telnet refusé | ✅ |
| Connexion SSH depuis PC1 | ✅ |
| Mot de passe chiffré dans running-config | ✅ |
| Timeout VTY 5 min | ✅ |

---

## 💡 Ce que j'ai appris / Difficultés

<!-- À compléter -->
- ⚠️ Erreur classique : taper `secret cisco1` sans le mot `enable` devant → commande invalide
- 

---

## 🔗 Liens utiles

- [Jeremy IT Lab Day 42 — SSH](https://www.youtube.com/watch?v=...)
- [Formip RR_SS_05](https://www.formip.com)
