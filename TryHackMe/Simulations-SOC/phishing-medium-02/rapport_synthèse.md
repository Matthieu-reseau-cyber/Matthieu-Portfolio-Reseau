# 🎯 TryHackMe — Le Phishing se Dévoile (SOC Simulator)

![TryHackMe](https://img.shields.io/badge/TryHackMe-SOC%20Level%201-red)
![Statut](https://img.shields.io/badge/Statut-✅%20Terminé-brightgreen)
![Points](https://img.shields.io/badge/Points-1695-blue)
![Difficulté](https://img.shields.io/badge/Difficulté-🟠%20Moyen-orange)
![TP Rate](https://img.shields.io/badge/True%20Positive%20Rate-100%25-brightgreen)

> **Parcours :** SOC Level 1 → Simulateur SOC — Scénario 2
> **Outil SIEM :** Splunk | **Date :** 08/05/2026

---

## 📊 Résultats

| Métrique | Valeur |
|----------|--------|
| Alertes traitées | 36 |
| Taux TP | ✅ 100% |
| Taux FP | ⚠️ 58% |
| MTTR | 1 minute |
| Points | 1695 pts |

---

## 🔗 Chaîne d'attaque reconstituée

| Alerte | Heure | Événement | Verdict |
|--------|-------|-----------|---------|
| #1005 | 10:01 | Phishing email — FactureImportante-Février.zip | 🔴 TP |
| #1020 | 10:22 | PowerView.ps1 dans Downloads d