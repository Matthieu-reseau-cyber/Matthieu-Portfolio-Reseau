# 📁 phishing-medium-02 — Le Phishing se Dévoile

![TryHackMe](https://img.shields.io/badge/TryHackMe-SOC%20Level%201-red)
![Statut](https://img.shields.io/badge/Statut-✅%20Terminé-brightgreen)
![Difficulté](https://img.shields.io/badge/Difficulté-🟠%20Moyen-orange)
![Points](https://img.shields.io/badge/Points-1695-blue)
![Date](https://img.shields.io/badge/Date-08%2F05%2F2026-lightgrey)

> **Parcours :** SOC Level 1 — SOC Simulator  
> **Plateforme :** TryHackMe  
> **Outil SIEM :** Splunk  
> **Scénario :** Le Phishing se Dévoile

---

## 🎯 Contexte

Simulation d'une journée type d'analyste SOC N1 : triage en temps réel de 36 alertes de sécurité, investigation, rédaction de rapports d'incidents et décision d'escalade.

L'objectif est de reconstituer une chaîne d'attaque complète à partir d'alertes individuelles, en classifiant chaque événement en Vrai Positif (TP) ou Faux Positif (FP).

---

## 📊 Résultats globaux

| Métrique | Résultat |
|----------|----------|
| Alertes traitées | 36 |
| Taux TP (vrais positifs) | ✅ 100% |
| Taux FP (faux positifs) | ⚠️ 58% |
| MTTR moyen | ~1 minute |
| Score final | 1695 pts |

---

## 🔗 Chaîne d'attaque reconstituée

```
[Phishing email]
      │
      ▼
[Ouverture ZIP malveillant]
      │
      ▼
[PowerView.ps1 — Reconnaissance Active Directory]
      │
      ▼
[Montage partage réseau financier (FILESRV-01\SSF-FinancialRecords)]
      │
      ▼
[Robocopy.exe — Staging des données (C:\Users\...\exfiltration)]
      │
      ▼
[DNS Tunneling — Exfiltration des données]
```

---

## 📂 Contenu du dossier

| Fichier | Description |
|---------|-------------|
| `README.md` | Ce fichier — contexte et résultats globaux |
| `rapport_synthese.md` | Statistiques détaillées des 36 alertes |
| `rapport_TP_categories.md` | Vrais positifs groupés par type d'attaque |
| `rapport_FP.md` | Faux positifs — patterns identifiés |
| `rapport_erreurs.md` | Erreurs de classification et analyse |
| `lessons_learned.md` | Axes d'amélioration pour le prochain exercice |

---

## 🧠 Compétences démontrées

- Triage et classification d'alertes (True Positive / False Positive)
- Identification des types de phishing : typosquatting, URL shorteners, brand impersonation
- Reconnaissance d'outils d'attaque : PowerView, Robocopy, DNS tunneling
- Rédaction de rapports d'incidents selon le modèle Who/What/When/Where/Why
- Décision d'escalade L1 → L2/L3
- Analyse des logs Splunk et corrélation d'événements

---

## 📈 Comparaison avec l'exercice précédent

| Exercice                                          | Difficulté  | Score     | TP      | FP         |
|----------                                         |-----------  |-------    |----     |----        |
| [phishing-easy-01](../phishing-easy-01/README.md) | 🟢 Facile  | 160/160   | 100%     | 100%      |
| phishing-medium-02 *(ce dossier)*                 | 🟠 Moyen   | 1695/1880 | 100%     | 58%       |

> **Progression :** maintien du score TP parfait sur un scénario plus complexe. Axe d'amélioration principal : identification des FP sur les processus Windows légitimes de faible gravité (`rdpclip.exe`, `taskhostw.exe`, etc.)

---

*Simulation réalisée dans le cadre du parcours SOC Level 1 de TryHackMe.*
