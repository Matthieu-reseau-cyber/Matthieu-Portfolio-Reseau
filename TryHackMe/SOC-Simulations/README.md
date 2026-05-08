[README.md](https://github.com/user-attachments/files/27305795/README.md)
# 🛡️ SOC Simulations — TryHackMe

> Simulations pratiques d'analyse d'alertes SOC réalisées sur la plateforme **TryHackMe SOC Simulator**.  
> Chaque scénario reproduit une journée type d'analyste L1 : triage, investigation, rédaction de rapport et décision d'escalade.

---

## 📁 Structure

```
SOC-Simulations/
└── phishing-easy-01/
    ├── README.md            ← Contexte & résultats globaux
    ├── rapport_8814.md      ← FP classifié TP — erreur documentée
    ├── rapport_8817.md      ← TP — Typosquatting Microsoft
    ├── rapport_8815.md      ← TP — Brand impersonation Amazon (bit.ly)
    ├── rapport_8816.md      ← TP — Clic confirmé + firewall block
    └── lessons_learned.md   ← Analyse des erreurs & axes d'amélioration
```

---

## 🏆 Simulations complétées

| Simulation | Niveau | Score | Classement | Date |
|---|---|---|---|---|
| [Phishing Easy #01](./phishing-easy-01/README.md) | 🟢 Facile | 160/160 pts | 🥇 1er | 01/05/2026 |
| [Phishing_Se_Dévoile](./phishing-medium-01/README.md) | 🟠 Moyen | 1695/1880 | 🥈 2ème | 08/05/2026 |
---

## 🎯 Compétences développées

- Triage et classification d'alertes (True Positive / False Positive)
- Rédaction de rapports d'incidents selon le modèle **5W** (Who/What/When/Where/Why)
- Identification d'IOCs phishing : typosquatting, URL shorteners, domaines lookalike
- Décision d'escalade L1 → L2
- Analyse des logs firewall et corrélation d'événements
- Auto-évaluation et amélioration continue sur retour IA
Reconnaissance de chaînes d'attaque complexes (phishing → AD recon → exfiltration DNS)
- Analyse de DNS tunneling et détection d'exfiltration de données
- Identification d'outils offensifs (PowerView, Robocopy staging)
- Corrélation d'alertes multi-événements via PID parent
- 
---

*Simulations réalisées dans le cadre du parcours **SAL1 — SOC Level 1** de TryHackMe.*
