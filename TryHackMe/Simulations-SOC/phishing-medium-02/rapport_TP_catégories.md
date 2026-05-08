# Rapport TP — Catégories d'attaques — phishing-medium-02

**Exercice :** Le Phishing se Dévoile | **Date :** 08/05/2026  
**Total TP identifiés :** 100% ✅

---

## 🎣 Catégorie 1 — Phishing initial

| Alerte | Expéditeur | Objet | Technique |
|--------|------------|-------|-----------|
| #1005 | john@hatmakereurope.xyz | Facture urgente | ZIP malveillant |

**Pattern :** Urgence artificielle + menace légale + pièce jointe déguisée en facture.

---

## 🔍 Catégorie 2 — Reconnaissance Active Directory

| Alerte | Outil | Action |
|--------|-------|--------|
| #1020 | PowerView.ps1 | Énumération AD — localisation partages réseau |

**Pattern :** Script PowerShell légitime détourné pour cartographier l'AD.

---

## 📁 Catégorie 3 — Collecte de données

| Alerte | Commande | Cible |
|--------|----------|-------|
| #1022 | net use Z: \\FILESRV-01\SSF-FinancialRecords | Montage partage financier |
| #1023 | Robocopy Z:\ → exfiltration\ /E | Copie récursive complète |

**Pattern :** Montage réseau + staging local avant exfiltration.

---

## 🫥 Catégorie 4 — Évasion

| Alerte | Commande | Objectif |
|--------|----------|----------|
| #1024 | net use Z: /delete | Suppression traces d'accès |

**Pattern :** Nettoyage post-collecte pour masquer l'activité.

---

## 📡 Catégorie 5 — Exfiltration DNS Tunneling

| Alertes | Outil | Destination | Volume |
|---------|-------|-------------|--------|
| #1025-#1034 | nslookup.exe | haz4rdw4re.io | 10 paquets Base64 |

**Pattern :** Données fragmentées en chunks Base64 exfiltrées via requêtes DNS
pour contourner les règles firewall HTTP/HTTPS.