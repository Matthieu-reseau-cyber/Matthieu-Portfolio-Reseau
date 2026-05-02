# 📋 Rapport d'incident — Alerte #8815

| Champ | Valeur |
|---|---|
| **ID Alerte** | 8815 |
| **Règle déclenchée** | E-mails reçus contenant des liens externes suspects |
| **Sévérité** | 🟡 Moyen |
| **Type** | Phishing |
| **Heure d'activité** | 05/01/2026 à 13:09:34 UTC |
| **Entité affectée** | `h.harris@thetrydaily.thm` |
| **Classification** | ✅ Vrai Positif |
| **Escalade** | ✅ Oui |
| **Score classification** | 10/10 |
| **Score rapport (détails)** | 30/100 *(lacunes documentées)* |

---

## 📝 Rapport soumis

### Who (Qui)
Destinataire : `h.harris@thetrydaily.thm`  
Expéditeur : `urgents@amazon.biz`

### What (Quoi)
Email imitant Amazon via un domaine lookalike (`amazon.biz` au lieu de `amazon.com` ou `amazon.fr`). Contient un lien `bit.ly` raccourci masquant la destination réelle. Phishing de livraison classique avec urgence artificielle (délai 48h). Aucune communication légitime d'Amazon n'utilise des domaines `.biz` ni des raccourcisseurs d'URL.

### When (Quand)
05/01/2026 à 13:09:34 UTC

### Where (Où)
Boîte mail de `h.harris@thetrydaily.thm`

### Why — Raison de classification comme Vrai Positif
- Domaine expéditeur frauduleux : `amazon.biz` ≠ `amazon.com/fr`
- URL raccourcie `bit.ly/3sHkX3da12340` — destination réelle inconnue et potentiellement malveillante
- Technique d'urgence artificielle ("48h") pour forcer une action impulsive

### Raison d'escalade
Le destinataire peut avoir cliqué sur le lien `bit.ly`. La destination réelle est inconnue — analyse des logs proxy nécessaire pour identifier le domaine cible et détecter un éventuel credential harvesting ou livraison de malware.

---

## 🔴 IOCs identifiés

| Indicateur | Valeur |
|---|---|
| Expéditeur | `urgents@amazon.biz` |
| URL suspecte | `http://bit.ly/3sHkX3da12340` |
| Sujet | "Votre colis Amazon n'a pas pu être livré – Action requise" |
| Technique | Brand impersonation + URL shortener + urgence |

---

## 🛠️ Actions de remédiation recommandées

- [ ] Bloquer le domaine `amazon.biz` au niveau de la passerelle email
- [ ] Bloquer `bit.ly/3sHkX3da12340` au niveau proxy
- [ ] Notifier `h.harris` immédiatement
- [ ] Analyser les logs proxy pour des connexions sortantes vers le lien `bit.ly`
- [ ] Si clic confirmé : isoler l'endpoint et réinitialiser les credentials

---

## 📊 Analyse du rapport (retour IA)

> *Score détails : 30/100 — lacunes identifiées :*

| Point évalué | Statut |
|---|---|
| Identification de l'attaque (TP) | ✅ Correct |
| Explication URL shortener suspect | ✅ Correct |
| Heure et utilisateur identifiés | ✅ Correct |
| Expéditeur identifié | ✅ Correct |
| Confirmation du clic utilisateur | ❌ Non renseigné |
| Adresses IP (interne + destination) | ❌ Manquantes |
| Actions correctives (peut-être non nécessaires) | ⚠️ Recommandées mais firewall non confirmé |
| Chemin réseau (Where) | ❌ Insuffisant |
| Conséquences de l'attaque (Why) | ❌ Insuffisant |

→ Voir [lessons_learned.md](./lessons_learned.md) pour les corrections apportées.
