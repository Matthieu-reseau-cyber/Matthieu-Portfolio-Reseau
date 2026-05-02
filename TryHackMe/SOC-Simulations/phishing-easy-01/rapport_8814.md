# 📋 Rapport d'incident — Alerte #8814

> **⚠️ Classification incorrecte — documentée à des fins d'apprentissage**

| Champ | Valeur |
|---|---|
| **ID Alerte** | 8814 |
| **Règle déclenchée** | E-mails reçus contenant des liens externes suspects |
| **Sévérité** | 🟡 Moyen |
| **Type** | Phishing |
| **Heure d'activité** | 05/01/2026 à 13:06:21 UTC |
| **Entité affectée** | `j.garcia@thetrydaily.thm` |
| **Classification soumise** | ❌ Faux Positif (incorrect) |
| **Classification correcte** | ✅ Vrai Positif |
| **Score** | -10 points |

---

## 📝 Rapport soumis

### Who (Qui)
Destinataire : `j.garcia@thetrydaily.thm`  
Expéditeur : `onboarding@hrconnex.thm`

### What (Quoi)
Email entrant provenant d'un domaine externe inconnu (`hrconnex.thm`) imitant un processus d'intégration RH. L'email contient une URL personnalisée intégrant le nom d'utilisateur du destinataire — indicateur classique de phishing ciblé.

### When (Quand)
05/01/2026 à 13:06:21 UTC

### Where (Où)
Boîte mail de `j.garcia@thetrydaily.thm`

### Why — Raison de classification (soumise)
Classification erronée comme **Faux Positif**.

### Raison d'escalade
Le destinataire peut avoir cliqué sur le lien suspect. Les logs firewall/proxy doivent être examinés pour confirmer une tentative d'accès à `https://hrconnex.thm/onboarding/15400654060/j.garcia`. Risque de credential harvesting.

---

## 🔴 IOCs identifiés

| Indicateur | Valeur |
|---|---|
| Expéditeur | `onboarding@hrconnex.thm` |
| URL suspecte | `https://hrconnex.thm/onboarding/15400654060/j.garcia` |
| Sujet | "Action nécessaire : Veuillez finaliser votre profil d'intégration" |

---

## 🛠️ Actions de remédiation recommandées

- [ ] Bloquer le domaine `hrconnex.thm` au niveau du pare-feu/proxy
- [ ] Informer immédiatement `j.garcia` et le service RH
- [ ] Vérifier si d'autres utilisateurs ont reçu des emails similaires
- [ ] Analyser les logs proxy pour détecter des connexions sortantes vers l'URL suspect
- [ ] Si clic confirmé : isoler l'endpoint et réinitialiser les credentials

---

## ❌ Pourquoi cette classification était incorrecte

> Voir [lessons_learned.md](./lessons_learned.md) pour l'analyse complète.

**Résumé de l'erreur :** L'email présentait tous les indicateurs d'un vrai phishing ciblé (domaine externe inconnu, URL personnalisée avec username, langage d'urgence, usurpation d'identité RH). La classification en Faux Positif était une erreur d'analyse — probablement causée par la méconnaissance du domaine `hrconnex.thm` comme domaine légitime de l'entreprise (il ne l'est pas).
