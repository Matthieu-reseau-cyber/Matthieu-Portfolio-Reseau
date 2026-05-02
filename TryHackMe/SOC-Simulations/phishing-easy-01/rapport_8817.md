# 📋 Rapport d'incident — Alerte #8817

| Champ | Valeur |
|---|---|
| **ID Alerte** | 8817 |
| **Règle déclenchée** | E-mails reçus contenant des liens externes suspects |
| **Sévérité** | 🟡 Moyen |
| **Type** | Phishing |
| **Heure d'activité** | 05/01/2026 à 13:11:52 UTC |
| **Entité affectée** | `c.allen@thetrydaily.thm` |
| **Classification** | ✅ Vrai Positif |
| **Escalade** | ✅ Oui |
| **Score classification** | 10/10 |
| **Score rapport (détails)** | 35/100 *(lacunes documentées)* |

---

## 📝 Rapport soumis

### Who (Qui)
Destinataire : `c.allen@thetrydaily.thm`  
Expéditeur : `no-reply@m1crosoftsupport.co`

### What (Quoi)
Email imitant Microsoft Security via un domaine **typosquatté** (`m1crosoftsupport.co` — "i" remplacé par "1", TLD `.co`). Contient une fausse alerte de sécurité mentionnant une connexion suspecte depuis Lagos, Nigéria (IP `102.89.222.143`) pour créer un sentiment d'urgence. Lien malveillant pointant vers le même domaine frauduleux pour récolter les credentials.

### When (Quand)
05/01/2026 à 13:11:52 UTC

### Where (Où)
Boîte mail de `c.allen@thetrydaily.thm`

### Why — Raison de classification comme Vrai Positif
- Domaine expéditeur typosquatté : `m1crosoftsupport.co` ≠ `microsoftsupport.com`
- Technique d'ingénierie sociale : fausse alerte sécurité + IP et géolocalisation réelles pour renforcer la crédibilité
- Lien malveillant vers `https://m1crosoftsupport.co/login` — credential harvesting

### Raison d'escalade
Ingénierie sociale très convaincante avec données IP réelles. Vérifier les logs proxy pour déterminer si `c.allen` a cliqué sur le lien et tenté de s'authentifier sur le faux portail Microsoft.

---

## 🔴 IOCs identifiés

| Indicateur | Valeur |
|---|---|
| Expéditeur | `no-reply@m1crosoftsupport.co` |
| URL malveillante | `https://m1crosoftsupport.co/login` |
| Typosquatting | `m1crosoft` ("1" à la place de "i") |
| Fausse IP source dans le corps | `102.89.222.143` (Lagos, Nigéria) |
| Technique | Account security impersonation + urgence + typosquatting |

---

## 🛠️ Actions de remédiation recommandées

- [ ] Bloquer le domaine `m1crosoftsupport.co` au niveau de la passerelle email et du proxy
- [ ] Notifier `c.allen` immédiatement — ne pas cliquer sur le lien
- [ ] Analyser les logs proxy pour des connexions sortantes vers `m1crosoftsupport.co`
- [ ] Si clic confirmé : supposer que les credentials sont compromis — réinitialiser le compte Microsoft et activer MFA
- [ ] Signaler le domaine à l'équipe Microsoft Abuse

---

## 📊 Analyse du rapport (retour IA)

> *Score détails : 35/100 — lacunes identifiées :*

| Point évalué | Statut |
|---|---|
| Identification de l'attaque (TP) | ✅ Correct |
| Tactiques ingénierie sociale expliquées | ✅ Correct |
| Recommandation examen logs proxy | ✅ Mentionné |
| Confirmation du clic utilisateur | ❌ Non renseigné |
| Autorisation/blocage firewall | ❌ Non renseigné |
| Adresse IP interne de l'endpoint | ❌ Manquante |
| Adresse IP destination | ❌ Manquante |
| Chemin réseau (Where détaillé) | ❌ Insuffisant |
| Conséquences potentielles (Why) | ❌ Insuffisant |

→ Voir [lessons_learned.md](./lessons_learned.md) pour les corrections apportées.
