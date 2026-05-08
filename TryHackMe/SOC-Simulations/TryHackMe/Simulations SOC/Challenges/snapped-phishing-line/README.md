[Snapped-Phishing-Line-README.md](https://github.com/user-attachments/files/27516974/Snapped-Phishing-Line-README.md)
# 🎣 TryHackMe — Snapped Phish-ing Line

![TryHackMe](https://img.shields.io/badge/TryHackMe-SOC%20Level%201-red)
![Statut](https://img.shields.io/badge/Statut-✅%20Terminé-brightgreen)
![Points](https://img.shields.io/badge/Points-330-blue)
![Difficulté](https://img.shields.io/badge/Difficulté-Facile-green)

> **Parcours :** SOC Level 1 → Analyse du phishing  
> **Type :** Défi  
> **Durée estimée :** 30 min  
> **Date de complétion :** Mai 2026

---

## 🎯 Objectif

Analyser une campagne de phishing ciblant les employés de **SwiftSpend Finance**.  
En tant qu'analyste SOC, identifier les éléments clés de l'attaque : expéditeur, infrastructure malveillante, kit de phishing utilisé, et données compromises.

---

## 🛠️ Outils utilisés

| Outil | Usage |
|-------|-------|
| Terminal Linux (`cat`, `find`, `grep`) | Analyse des fichiers `.eml` |
| `sha256sum` | Hash de la pièce jointe |
| Navigateur web | Exploration du site de phishing |
| VirusTotal | Analyse du fichier malveillant |
| CyberChef | Décodage Base64 + Reverse du flag |
| `base64 -d` + `rev` | Décodage en ligne de commande |

---

## 🔍 Analyse pas à pas

### 1. Identification de l'expéditeur et de la cible

Les fichiers `.eml` analysés se trouvaient dans :
```
/home/damianhall/Desktop/phish-emails/
```

Emails ciblés (employés de SwiftSpend Finance) :
- `derick.marshall@swiftspend.finance`
- `michael.ascot@swiftspend.finance`
- `michelle.chen@swiftspend.finance`
- `zoe.duncan@swiftspend.finance`

| Champ | Valeur |
|-------|--------|
| **Expéditeur** | `William McClean` |
| **Adresse Reply-To attaquant** | `accounts.phishing@genericwebguide.com` |

---

### 2. Analyse de la pièce jointe

```bash
# Hash SHA256 du fichier joint
sha256sum ~/Downloads/Update365.zip
```

| Champ | Valeur |
|-------|--------|
| **Nom du fichier** | `Update365.zip` |
| **Domaine du kit** | `kennaroads.buzz` |
| **Page de phishing** | `Microsoft` (fausse page de connexion) |

> ⚠️ La page imitait parfaitement la page de connexion Microsoft avec le logo et l'interface authentiques, mais sans certificat SSL valide — le navigateur affichait "This connection is not secure".

---

### 3. Exploration du kit de phishing

Structure du kit extrait :
```
Update365.zip
└── Update365/
    └── office365/
        ├── index.php
        ├── blocker.php
        ├── error_log
        ├── robots.txt
        ├── updat.cmd
        ├── delete.php
        ├── scr/
        ├── Scriptup/
        ├── update/
        ├── Validation/
        │   └── submit.php   ← Email collecteur des identifiants
        └── flag.txt
```

**Adresse email de collecte des identifiants :**
```bash
cat ~/Downloads/Update365/office365/Validation/submit.php
# → sotinsylvia@gmail.com
```

---

### 4. Analyse des victimes compromises

```bash
# Fichier journal des identifiants volés
cat ~/Downloads/Update365/office365/CredentialStealer.log
```

Identifiant compromis trouvé :
- `michael.ascot@swiftspend.finance`

---

### 5. Décodage du flag

**Récupération :**
```
http://kennaroads.buzz/data/Update365/office365/flag.txt
```

**Contenu brut :**
```
fUxSVV8zSHRfaFQxd195NExwe01IVAo=
```

**Décodage (Base64 + Reverse) :**
```bash
echo "fUxSVV8zSHRfaFQxd195NExwe01IVAo=" | base64 -d | rev
```

**Flag :**
```
THM{TH3_5T1ng_0p3r4t10n}
```

---

## 📊 Résumé des IoC (Indicateurs de Compromission)

| Type | Valeur | Risque |
|------|--------|--------|
| Domaine phishing | `kennaroads.buzz` | 🔴 Haut |
| Email collecteur | `sotinsylvia@gmail.com` | 🔴 Haut |
| Page imitée | Microsoft O365 | 🔴 Haut |
| Fichier malveillant | `Update365.zip` | 🔴 Haut |
| Victime compromise | `michael.ascot@swiftspend.finance` | 🔴 Haut |

---

## 🧠 Techniques d'attaque identifiées (MITRE ATT&CK)

| Technique | ID | Description |
|-----------|-----|-------------|
| Phishing | T1566 | Envoi d'emails malveillants avec lien |
| Credential Harvesting | T1056 | Capture des identifiants via fausse page |
| Web Site Spoofing | T1598 | Imitation de la page Microsoft O365 |

---

## ✅ Conclusion

Campagne de **credential harvesting** ciblée contre les employés de SwiftSpend Finance :
- Kit de phishing hébergé sur `kennaroads.buzz`
- Fausse page Microsoft O365 pour voler les identifiants
- Au moins un compte compromis identifié
- Les identifiants collectés envoyés à `sotinsylvia@gmail.com`

**Verdict : Campagne de phishing O365 confirmée — credential harvesting actif**

---

## 🏆 Résultat

| Métrique | Valeur |
|----------|--------|
| Points gagnés | 330 |
| Difficulté | Facile |
| Classement Ligue Rubis | 2ème (1494 pts) |

---

*Writeup rédigé dans le cadre du parcours SOC Level 1 — TryHackMe*  
*Portfolio : [github.com/Matthieu-reseau-cyber/Matthieu-Portfolio-Reseau](https://github.com/Matthieu-reseau-cyber/Matthieu-Portfolio-Reseau)*
