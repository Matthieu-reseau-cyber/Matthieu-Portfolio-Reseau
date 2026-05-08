[writeup_pyramid_of_pain.md](https://github.com/user-attachments/files/27519218/writeup_pyramid_of_pain.md)
# TryHackMe — Pyramid of Pain (PicoSecure)
**Plateforme :** TryHackMe  
**Difficulté :** Moyen  
**Tags :** `Malware Analysis` `Threat Detection` `MITRE ATT&CK` `SOC` `Blue Team`  
**Score :** 180 points — Room completed ✅

---

## Objectif

Dans ce challenge, j'incarne un analyste SOC chez PicoSecure. Un testeur de pénétration externe (Sphinx) tente d'exécuter des échantillons de malwares sur une station de travail virtuelle. Mon rôle est de détecter et bloquer chaque échantillon en utilisant les outils de sécurité de PicoSecure, en progressant à travers les niveaux de la **Pyramide de la Douleur**.

La Pyramide de la Douleur est un modèle de détection des menaces qui classe les indicateurs par ordre croissant de difficulté à contourner pour un attaquant :

```
         /\
        /  \         TTPs (le plus difficile à changer)
       /----\
      / Outils\
     /----------\
    /   Artefacts \
   /--------------\
  /  Domaines DNS  \
 /------------------\
/   Adresses IP      \
/--------------------\
     Hashes de fichiers  (le plus facile à changer)
```

---

## Outils utilisés

- **Malware Sandbox** — Analyse dynamique des échantillons
- **Manage Hashes** — Blocklist par hash (MD5, SHA1, SHA256)
- **Firewall Rule Manager** — Blocage par adresse IP
- **DNS Rule Manager** — Blocage par domaine
- **Sigma Rule Builder** — Règles de détection SIEM (Sysmon)
- **CyberChef** — Analyse de fichiers
- **Reverse Shell Generator** — Identification des paramètres C2

---

## Flag 1 — Hash de fichier (sample1.exe)

### Analyse
Scan de `sample1.exe` dans le Malware Sandbox :
- **Type :** Trojan.Metasploit.A
- **Comportement :** METASPLOIT detected, connects to unusual port

### Détection
Ajout du hash MD5 dans la **Hash Blocklist** :
```
MD5: cbda8ae000aa9cbe7c8b982bae006c2a
```
> ⚠️ Attention aux caractères exacts du hash — une erreur de transcription (986 vs 000) avait bloqué la progression.

### Flag
```
THM{f3cbf08151a11a6a331db9c6cf5f4fe4}
```

---

## Flag 2 — Adresse IP (sample2.exe)

### Analyse
Scan de `sample2.exe` dans le Malware Sandbox :
- **Type :** PEXE — Trojan.Metasploit.A (recompilé → hash différent)
- **Network Activity :** Connexion HTTP GET vers `154.35.10.113:4444`
- **URL :** `http://154.35.10.113:4444/uvLk8YI32`

Sphinx a recompilé le malware pour changer le hash → blocage par hash insuffisant.

### Détection
Création d'une règle **Firewall Egress Deny** dans le **Firewall Rule Manager** :
```
Type:        Egress
Source IP:   Any
Destination: 154.35.10.113
Action:      Deny
```
> ⚠️ Le champ Source IP doit être `Any` et non `0.0.0.0`.

### Flag
```
THM{2ff48a3421a938b388418be273f4806d}
```

---

## Flag 3 — Domaine DNS (sample3.exe)

### Analyse
Scan de `sample3.exe` dans le Malware Sandbox :
- **Comportement :** Downloads backdoor.exe, connects to unusual IP/port
- **DNS Requests :**
  - `services.microsoft.com` → légitime
  - `emudyn.bresonicz.info` → **malveillant** (Xplorika Cloud Services)

### Détection
Création d'une règle dans le **DNS Rule Manager** :
```
Rule Name:   Malicious C2 Domain
Category:    Malware
Domain:      emudyn.bresonicz.info
Action:      Deny
```

### Flag
```
THM{4eca9e2f61a19ecd5df34c788e7dce16}
```

---

## Flag 4 — Artefact hôte / Registre (sample4.exe)

### Analyse
Scan de `sample4.exe` dans le Malware Sandbox :
- **Comportement :** Désactive Windows Defender Real-Time Monitoring
- **Registry Activity :**
  ```
  Key:   HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Defender\Real-Time Protection
  Name:  DisableRealtimeMonitoring
  Value: 1
  ```

Cette technique correspond à **T1562.001 — Impair Defenses: Disable or Modify Tools** du framework MITRE ATT&CK.

### Détection
Création d'une règle **Sigma** dans le **Sigma Rule Builder** :
```
Log Source:    Sysmon Event Logs
Event Type:    Registry Modifications
Registry Key:  HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Defender\Real-Time Protection
Registry Name: DisableRealtimeMonitoring
Value:         1
ATT&CK ID:     Defense Evasion (TA0005)
```

### Flag
```
THM{c956f455fc076aea829799c0876ee399}
```

---

## Flag 5 — Outil / Comportement réseau (sample5.exe)

### Analyse
Sphinx fournit un log réseau sur 12 heures + `sample5.exe`.

Analyse du fichier `outgoing_connections.log` :
```
Pattern détecté :
- Destination: 51.102.10.19
- Port: 443
- Size: 97 bytes exactement
- Fréquence: toutes les 30 minutes (1800 secondes)
→ Beacon C2 "keep-alive"
```

Un **beacon** est une technique C2 où le malware contacte régulièrement le serveur attaquant pour recevoir des ordres, simulant du trafic légitime par sa régularité.

Technique MITRE ATT&CK : **T1071 — Application Layer Protocol**

### Détection
Création d'une règle **Sigma** :
```
Log Source:  Sysmon Event Logs
Event Type:  Network Connections
Remote IP:   Any
Remote Port: Any
Size:        97 bytes
Frequency:   1800 secondes
ATT&CK ID:   Command and Control (TA0011)
```

### Flag
```
THM{46b21c4410e47dc5729ceadef0fc722e}
```

---

## Flag 6 — TTPs (sample6.exe)

### Analyse
Sphinx fournit `commands.log` — les commandes de reconnaissance exécutées sur les victimes :
```batch
dir c:\ >> %temp%\exfiltr8.log
dir "c:\Documents and Settings" >> %temp%\exfiltr8.log
dir "c:\Program Files\" >> %temp%\exfiltr8.log
dir d:\ >> %temp%\exfiltr8.log
net localgroup administrator >> %temp%\exfiltr8.log
ver >> %temp%\exfiltr8.log
systeminfo >> %temp%\exfiltr8.log
ipconfig /all >> %temp%\exfiltr8.log
netstat -ano >> %temp%\exfiltr8.log
net start >> %temp%\exfiltr8.log
```

Toutes les commandes exfiltrent leurs résultats vers `%temp%\exfiltr8.log`.

Technique MITRE ATT&CK : **TA0010 — Exfiltration**

### Détection
Création d'une règle **Sigma** :
```
Log Source: Sysmon Event Logs
Event Type: File Creation and Modification
File Path:  %temp%
File Name:  exfiltr8.log
ATT&CK ID:  Exfiltration (TA0010)
```

### Flag
```
THM{c8951b2ad24bbcbac60c16cf2c83d92c}
```

---

## Résultat final

```
Great work matthieu.morel068! Room completed!
Points earned: 180 | Streak: 55
```

Sphinx a capitulé : *"I have officially given up. You managed to chase me to the very top of the Pyramid of Pain."*

---

## Leçons apprises

| Niveau | Indicateur | Outil utilisé | Difficulté pour l'attaquant |
|--------|-----------|---------------|----------------------------|
| 1 | Hash MD5 | Manage Hashes | Très faible (recompiler suffit) |
| 2 | Adresse IP | Firewall Manager | Faible (changer de serveur) |
| 3 | Domaine DNS | DNS Rule Manager | Modérée (enregistrer un nouveau domaine) |
| 4 | Artefact registre | Sigma Rule Builder | Élevée |
| 5 | Comportement réseau (beacon) | Sigma Rule Builder | Très élevée |
| 6 | TTPs | Sigma Rule Builder | Maximale (changer ses méthodes fondamentales) |

> **Conclusion :** Plus on monte dans la Pyramide de la Douleur, plus il est coûteux pour l'attaquant de contourner la détection. L'objectif d'un bon SOC est de détecter les TTPs plutôt que de se reposer uniquement sur les hashes.
