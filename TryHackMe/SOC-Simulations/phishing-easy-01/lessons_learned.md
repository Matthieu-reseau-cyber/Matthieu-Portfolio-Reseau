# 📚 Lessons Learned — Phishing Easy #01

> *"Si tu ne peux pas l'expliquer simplement, c'est que tu ne le comprends pas encore assez."*  
> — Principe fondamental de la rédaction de rapports SOC

---

## 🎯 Résumé de la session

| Métrique | Résultat |
|---|---|
| Score global | 🥇 160 points — 1ère place |
| Taux TP correct | ✅ 100% |
| Classification incorrecte | ❌ 1 alerte (8814 — FP alors que TP) |
| Qualité des rapports | ⚠️ Correcte sur le fond, lacunes sur le détail |

---

## ❌ Erreur #1 — Mauvaise classification de l'alerte 8814

### Ce qui s'est passé
L'alerte 8814 (email de `onboarding@hrconnex.thm` vers `j.garcia`) a été classifiée en **Faux Positif** alors qu'il s'agissait d'un **Vrai Positif**.

### Pourquoi c'était une erreur
L'email présentait tous les indicateurs classiques de phishing ciblé :
- Domaine externe inconnu (`hrconnex.thm`) non répertorié dans les domaines partenaires
- URL personnalisée intégrant le username du destinataire → phishing **ciblé** (spear phishing)
- Langage d'urgence ("Action requise")
- Usurpation d'identité du service RH

### Réflexe à adopter
> Avant de classer en FP, poser systématiquement ces questions :
> 1. Le domaine expéditeur est-il dans la liste des domaines approuvés de l'entreprise ?
> 2. L'URL contient-elle des données personnelles de la victime (username, email) ?
> 3. Le sujet crée-t-il une urgence artificielle ?

**Si ≥ 2 réponses "oui" → classifier en TP et investiguer.**

---

## ⚠️ Erreur #2 — Rapports incomplets (5W non totalement renseignés)

### Lacunes récurrentes sur les alertes 8817, 8815, 8816

#### ❌ Clic utilisateur non renseigné (8817, 8815)
**Ce qui manquait :** confirmation ou infirmation que l'utilisateur a cliqué sur le lien.  
**Impact :** sans cette information, l'urgence des actions de remédiation ne peut pas être évaluée correctement.  
**Correction :** toujours consulter les **logs proxy** pour vérifier les connexions sortantes vers le domaine suspect avant de soumettre le rapport.

```
Vérification proxy → Connexion sortante vers [domaine] ?
  OUI → Clic confirmé → Urgence HIGH → Isolation endpoint
  NON → Clic non confirmé → Surveillance + blocage domaine
```

#### ❌ Adresses IP manquantes (8817, 8815)
**Ce qui manquait :** IP interne de l'endpoint affecté + IP destination de l'attaquant.  
**Impact :** sans les IPs, impossible de tracer le chemin réseau complet (Where) et de bloquer précisément la menace.  
**Correction :** toujours inclure dans le rapport :

| Champ obligatoire | Exemple |
|---|---|
| IP interne (endpoint) | `10.20.2.17` |
| IP destination (C2/phishing) | `67.199.248.11` |
| Port | `80` (HTTP), `443` (HTTPS) |

#### ⚠️ Justification des actions correctives vs blocage firewall (8816)
**Ce qui manquait :** si le firewall a bloqué la connexion, justifier **pourquoi** on recommande quand même des actions correctives.  
**Correction :** même si la menace est bloquée, l'endpoint peut avoir été compromis par un autre vecteur. Le rapport doit expliquer ce raisonnement :

> *"Bien que le firewall ait bloqué la tentative de connexion à 13:10:48, l'endpoint 10.20.2.17 doit être investigué pour exclure toute compromission préalable ou parallèle. Le blocage firewall ne garantit pas l'absence d'activité malveillante sur le poste."*

---

## ✅ Points forts identifiés

- **Identification précise des IOCs :** typosquatting (`m1crosoftsupport.co`), domaine lookalike (`amazon.biz`), URL shortener (`bit.ly`), phishing ciblé (URL avec username)
- **Analyse des techniques d'attaque :** social engineering, urgence, brand impersonation correctement identifiés
- **Escalade systématique** des alertes True Positive → bon réflexe L1
- **Corrélation alerte 8815 → 8816 :** lien établi entre l'email Amazon et le clic confirmé par logs firewall

---

## 📋 Checklist rapport post-session

À appliquer systématiquement avant de soumettre tout rapport SOC :

```
WHO  ✅ Utilisateur affecté + expéditeur identifiés
WHAT ✅ Nature de l'attaque + IOCs listés
WHEN ✅ Heure précise de l'événement
WHERE ⬜ IP interne endpoint + IP destination + port + chemin réseau
WHY  ⬜ Justification du verdict + conséquences potentielles si non traité
      ⬜ Clic confirmé ou infirmé (logs proxy consultés)
      ⬜ Firewall a-t-il bloqué ? → impact sur l'urgence des actions
```

---

## 🔜 Prochain objectif

Appliquer cette checklist sur le prochain scénario **Persistance T1543** pour atteindre un score rapport > 80/100.
