# 📋 Rapport d'incident — Alerte #8816

| Champ | Valeur |
|---|---|
| **ID Alerte** | 8816 |
| **Règle déclenchée** | L'accès aux URL externes figurant sur la liste noire |
| **Sévérité** | 🔴 Élevé |
| **Type** | Phishing / Firewall Block |
| **Heure d'activité** | 05/01/2026 à 13:10:48 UTC |
| **Entité affectée** | `h.harris@thetrydaily.thm` — IP interne `10.20.2.17` |
| **Classification** | ✅ Vrai Positif |
| **Escalade** | ✅ Oui |
| **Score classification** | 10/10 |
| **Score rapport (détails)** | 65/110 |

---

## 📝 Rapport soumis

### Who (Qui)
Utilisateur : `h.harris@thetrydaily.thm`  
Endpoint interne : `10.20.2.17`  
Expéditeur initial : `urgents@amazon.biz`

### What (Quoi)
Les logs firewall confirment que le destinataire a cliqué sur le lien `bit.ly` suspect à 13:10:48. La connexion a été **bloquée** par la règle firewall "Sites Web bloqués". Tentative de connexion sortante depuis `10.20.2.17` vers `67.199.248.11` sur le port 80.

### When (Quand)
05/01/2026 à 13:10:48 UTC — 1 minute 14 secondes après réception de l'email (alerte 8815 à 13:09:34)

### Where (Où)
- **Endpoint interne :** `10.20.2.17` (poste de `h.harris`)
- **IP destination :** `67.199.248.11` (résolution de `bit.ly/3sHkX3da12340`)
- **Port :** 80 (HTTP)
- **Résultat :** Connexion bloquée par le firewall

### Why — Raison de classification comme Vrai Positif
Les logs firewall confirment que l'utilisateur a physiquement cliqué sur le lien malveillant. Bien que le firewall ait bloqué la tentative, l'endpoint `10.20.2.17` doit être investigué pour déterminer s'il n'a pas été compromis par d'autres vecteurs ou activités préalables.

### Raison d'escalade
Clic utilisateur confirmé par logs firewall. L'endpoint `10.20.2.17` doit être investigué pour tout signe de compromission, même si la connexion a été bloquée.

---

## 🔴 IOCs identifiés

| Indicateur | Valeur |
|---|---|
| Expéditeur | `urgents@amazon.biz` |
| URL cliquée | `http://bit.ly/3sHkX3da12340` |
| IP destination | `67.199.248.11` port 80 |
| IP interne (endpoint) | `10.20.2.17` (`h.harris`) |
| Règle firewall | "Sites Web bloqués" |

---

## 🛠️ Actions de remédiation recommandées

- [ ] Notifier `h.harris` — sensibilisation aux indicateurs de phishing
- [ ] Investiguer l'endpoint `10.20.2.17` pour tout signe de compromission
- [ ] Bloquer définitivement l'IP `67.199.248.11` et le domaine `amazon.biz`
- [ ] Confirmer qu'aucune exfiltration de données n'a eu lieu avant le blocage

---

## 📊 Analyse du rapport (retour IA)

> *Score détails : 65/110 — rapport le plus complet de la session*

| Point évalué | Statut |
|---|---|
| Heure précise de l'incident | ✅ Correct |
| Utilisateur et entités identifiés | ✅ Correct |
| Adresses IP (interne + destination) | ✅ Fournies |
| URL et détails techniques | ✅ Fournis |
| Blocage firewall noté | ✅ Mentionné |
| Impact réel de l'incident (pertes de données) | ❌ Non évalué |
| Actions correctives vs preuves de menace | ⚠️ Firewall a bloqué — actions immédiates peut-être excessives |
| Justification du "Pourquoi" | ❌ Insuffisant — pourquoi des actions correctives si le firewall a bloqué ? |

→ Voir [lessons_learned.md](./lessons_learned.md) pour les corrections apportées.
