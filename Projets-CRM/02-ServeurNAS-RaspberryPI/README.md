# 🖥️ Projet Serveur NAS — Raspberry Pi 5

![Statut](https://img.shields.io/badge/Statut-🔧%20En%20cours-orange)
![Matériel](https://img.shields.io/badge/Matériel-Raspberry%20Pi%205-red)
![OS](https://img.shields.io/badge/OS-Linux%20Ubuntu-purple)
![Protocole](https://img.shields.io/badge/Partage-Samba%20SMB-blue)
![Sécurité](https://img.shields.io/badge/Sécurité-SSH%20%2B%20IDS-green)
![CRM](https://img.shields.io/badge/Centre-CRM%20Mulhouse-blue)

> **Cadre :** Projet technologique de fin de formation — CRM Mulhouse  
> **Objectif :** Concevoir un serveur NAS sécurisé pour particuliers sur Raspberry Pi 5

---

## 🎯 Contexte & Objectif

En lien avec la prochaine formation qualifiante, ce projet consiste à créer un **serveur NAS (Network Attached Storage)** avec un **Raspberry Pi 5** afin de sécuriser et partager des données pour des particuliers.

**Les enjeux :** sécurisation et partage des données en réseau local.

### Fonctionnalités NAS visées

| Fonctionnalité | Description |
|----------------|-------------|
| **Centralisation** | Stocker et organiser un grand volume de données |
| **Sauvegarde** | Backup automatique des postes de travail |
| **Partage** | Accès partagé depuis PC, smartphones, tablettes |
| **Synchronisation** | Sync des données entre appareils |
| **Sécurisation** | Réplication RAID — si disque 1 tombe, disque 2 prend le relais |
| **Hébergement** | Sites web et services locaux |

---

## 🛠️ Matériel utilisé

| Composant | Rôle |
|-----------|------|
| Raspberry Pi 5 | Serveur central |
| Carte SD | Système d'exploitation |
| Disque(s) dur(s) USB | Stockage des données |
| Câble Ethernet | Connexion réseau filaire |
| Alimentation USB-C | Alimentation du Pi |

---

## 🏗️ Architecture réseau

```
Utilisateurs (PC, smartphones, tablettes)
         │
         ▼
   Réseau local (routeur/switch)
         │
         ▼
  Raspberry Pi 5 — Serveur NAS
  ├── Samba (partage SMB)
  ├── SSH (administration distante)
  ├── DHCP/DNS
  └── IDS (Snort/Suricata/Fail2ban)
         │
         ▼
  Disques durs USB (stockage)
```

---

## ⚙️ Procédure d'installation

### 1. Préparation du Raspberry Pi

```bash
# Mise à jour du système
sudo apt update && sudo apt upgrade

# Création des dossiers de partage
sudo mkdir /home/shares
sudo mkdir /home/shares/public
sudo chown -R root:users /home/shares/public
sudo chmod -R ug=rwx,o=rx /home/shares/public
```

### 2. Activation SSH

```bash
# Activer SSH via raspi-config
sudo raspi-config
# → Interfacing Options → SSH → Yes

# Connexion SSH depuis Linux/Mac
ssh pi@adresse_ip_raspberry

# Connexion SSH depuis Windows → PuTTY
# Host: adresse_ip | Port: 22
```

### 3. Installation de Samba (partage réseau)

```bash
# Installation
sudo apt install samba samba-common-bin

# Configuration
sudo nano /etc/samba/smb.conf
```

Ajouter en bas du fichier :
```ini
[public]
  comment = Public Storage
  path = /home/shares/public
  valid users = @users
  force group = users
  create mask = 0660
  directory mask = 0771
  read only = no
```

```bash
# Redémarrer Samba
sudo /etc/init.d/smbd restart

# Ajouter un utilisateur
sudo smbpasswd -a pi
```

### 4. Ajout d'un disque dur externe

```bash
# Détecter le disque
dmesg

# Formater en ext4 (si nécessaire)
umount /dev/sda1
sudo mkfs.ext4 /dev/sda1

# Monter le disque
sudo mkdir /home/shares/public/disk1
sudo chown -R root:users /home/shares/public/disk1
sudo chmod -R ug=rwx,o=rx /home/shares/public/disk1
sudo mount /dev/sda1 /home/shares/public/disk1

# Montage automatique au démarrage
sudo nano /etc/fstab
# Ajouter : /dev/sda1 /home/shares/public/disk1 auto noatime,nofail 0 0
```

### 5. Connexion au NAS

| OS | Chemin d'accès |
|----|----------------|
| Windows | `\\raspberrypi\public` ou `\\adresse_ip\public` |
| Linux | `smb://raspberrypi/public` |
| Android | Application File Expert |
| iOS | Application File Explorer |

---

## 🔐 Sécurisation — Extension IDS

Extension prévue avec un système de **détection d'intrusion (IDS)** :

| Outil | Rôle |
|-------|------|
| **Snort** | IDS réseau — détection des intrusions |
| **Suricata** | IDS/IPS — analyse du trafic réseau |
| **Fail2ban** | Blocage automatique des IP suspectes (brute force SSH) |

---

## 📊 État d'avancement

| Étape | Statut |
|-------|--------|
| Installation OS Raspberry Pi 5 | ✅ Fait |
| Activation SSH | ✅ Fait |
| Installation Samba | ✅ Fait |
| Configuration partage réseau | ✅ Fait |
| Ajout disque dur externe | ✅ Fait |
| Montage automatique au démarrage | ✅ Fait |
| Sécurisation SSH (clés RSA) | 🔧 En cours |
| Installation IDS (Snort/Suricata) | ⏳ Prévu |
| Configuration Fail2ban | ⏳ Prévu |

---

## 📚 Sources & références

- [Créer un NAS avec Raspberry Pi et Samba](https://raspberry-pi.fr/raspberry-pi-nas-samba/)
- [Activer SSH sur Raspberry Pi](https://raspberry-pi.fr/activer-ssh/)
- Documentation Samba officielle
- CRM Mulhouse — Cahier des charges projet NAS

---

*Projet réalisé dans le cadre de la formation TRI RNCP35295 — CRM Mulhouse*