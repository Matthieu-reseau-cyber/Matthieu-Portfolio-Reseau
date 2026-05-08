[README (1).md](https://github.com/user-attachments/files/27516926/README.1.md)
# 🎣 TryHackMe — Le Phish de Greenholt

![TryHackMe](https://img.shields.io/badge/TryHackMe-SOC%20Level%201-red)
![Statut](https://img.shields.io/badge/Statut-✅%20Terminé-brightgreen)
![Points](https://img.shields.io/badge/Points-360-blue)
![Badge](https://img.shields.io/badge/Badge-Phish%20Hunter-gold)

> **Parcours :** SOC Level 1 → Analyse du phishing  
> **Difficulté :** Débutant  
> **Durée estimée :** 30 min  
> **Date de complétion :** Mai 2026

---

## 🎯 Objectif

Analyser un courriel malveillant signalé par un cadre commercial chez GreenholtPLC.  
Le message présentait plusieurs signaux d'alerte : formule de salutation générique, demande de virement inattendue, pièce jointe non sollicitée.

---

## 🛠️ Outils utilisés

| Outil | Usage |
|-------|-------|
| Thunderbird / fichier `.eml` | Lecture du courriel brut |
| Terminal Linux (`grep`, `cat`) | Extraction des en-têtes |
| `nslookup -type=TXT` | Vérification SPF et DMARC |
| `sha256sum` | Hash de la pièce jointe |
| VirusTotal | Analyse du fichier joint |
| `file` | Identification du type de fichier |

---

## 🔍 Analyse pas à pas

### 1. Informations de base du courriel

| Champ | Valeur |
|-------|--------|
| **Objet** | `Transfer Reference Number` |
| **Nom d'affichage expéditeur** | Mr. James Jackson |
| **Adresse e-mail expéditeur** | `info@mutawamarine.com` |
| **Reply-To** | `info@mutawamarine.naval.com` |

> ⚠️ **Indicateur de phishing :** L'adresse `Reply-To` est différente de l'adresse `From` — les réponses seraient redirigées vers un domaine distinct (`naval.com`).

---

### 2. Analyse des en-têtes

```bash
# Extraction de l'IP d'origine
grep -i "X-Originating-IP" challenge.eml
# → 192.119.71.157

# Propriétaire de l'IP
whois 192.119.71.157
# → HostWinds LLC
```

| Champ | Valeur |
|-------|--------|
| **IP d'origine** | `192.119.71.157` |
| **Propriétaire IP** | HostWinds LLC |

---

### 3. Vérification SPF

```bash
nslookup -type=TXT mutawamarine.com
```

**Résultat :**
```
v=spf1 include:spf.protection.outlook.com -all
```

> ⚠️ **SPF : FAIL** — L'IP d'envoi (`192.119.71.157`) n'est pas autorisée par l'enregistrement SPF du domaine. L'e-mail ne provient pas d'un serveur légitime de `mutawamarine.com`.

---

### 4. Vérification DMARC

```bash
nslookup -type=TXT _dmarc.mutawamarine.com
```

**Résultat :**
```
v=DMARC1; p=quarantine; fo=1
```

> ℹ️ La politique DMARC est `quarantine` — les e-mails non conformes devraient être mis en quarantaine. Malgré cela, le mail a été reçu, ce qui indique une mauvaise application côté destinataire.

---

### 5. Analyse de la pièce jointe

```bash
# Trouver le fichier extrait par Thunderbird
find / -name "SWT_*" 2>/dev/null
# → /home/ubuntu/Downloads/thunderbird.tmp/pid-2618/SWT_#09674321____PDF__.CAB

# Hash SHA256
sha256sum "/home/ubuntu/Downloads/thunderbird.tmp/pid-2618/SWT_#09674321____PDF__.CAB"
```

| Champ | Valeur |
|-------|--------|
| **Nom du fichier** | `SWT_#09674321____PDF__.CAB` |
| **Hash SHA256** | `2e763765ce89dfb37b1f06bba0c2c6e2c43c2082d96476c66a50caef8d0a79c9` |
| **Taille** | ~486 KB |
| **Type réel** | CAB (Cabinet Windows) |

> 🚨 **Indicateur critique :** Le fichier se fait passer pour un PDF via son nom, mais est en réalité un **fichier CAB (Cabinet Windows)** — archive exécutable Windows pouvant contenir des malwares.

---

## 📊 Résumé des indicateurs de compromission (IoC)

| Type | Valeur | Risque |
|------|--------|--------|
| IP expéditeur | `192.119.71.157` | 🔴 Haut |
| Domaine Reply-To | `mutawamarine.naval.com` | 🔴 Haut |
| SPF | FAIL | 🔴 Haut |
| DMARC | non appliqué | 🟠 Moyen |
| Pièce jointe | `.CAB` déguisé en PDF | 🔴 Haut |
| Hash SHA256 | `2e763765...` | 🔴 Détecté sur VirusTotal |

---

## ✅ Conclusion

Cet e-mail présente **plusieurs signaux forts de phishing** :
- Usurpation d'identité d'un client connu
- SPF en échec (serveur d'envoi non autorisé)
- Adresse Reply-To différente du domaine expéditeur
- Pièce jointe malveillante déguisée (CAB → faux PDF)

**Verdict : E-mail de phishing confirmé — campagne de fraude au virement (BEC - Business Email Compromise)**

---

## 🏆 Résultat

| Métrique | Valeur |
|----------|--------|
| Tâches complétées | 1/1 |
| Points gagnés | 360 |
| Badge obtenu | 🥇 Phish Hunter |
| Classement Ligue Rubis | 3ème |

---

*Writeup rédigé dans le cadre du parcours SOC Level 1 — TryHackMe*  
*Portfolio : [github.com/Matthieu-reseau-cyber/Matthieu-Portfolio-Reseau](https://github.com/Matthieu-reseau-cyber/Matthieu-Portfolio-Reseau)*
