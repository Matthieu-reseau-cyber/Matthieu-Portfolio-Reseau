# Lessons Learned — phishing-medium-02

**Date :** 08/05/2026  
**Exercice :** SOC Simulator — Phishing Moyen  
**Score :** 1695 pts | TP : 100% | FP : 58%

---

## ✅ Ce qui a bien fonctionné

- 100% des vrais positifs identifiés correctement
- Reconstruction complète de la chaîne d'attaque (5 phases)
- Reconnaissance rapide du DNS tunneling (pattern Base64 + domaine C2)
- Bonne corrélation des alertes liées (1022 → 1023 → 1024)

---

## ❌ Axe d'amélioration principal : les FP à 58%

Les FP manqués = processus Windows légitimes à faible sévérité.

**Checklist anti-erreur pour le prochain exercice :**
- [ ] Binaire signé Microsoft ? → pencher vers FP
- [ ] Parent = services.exe ou svchost.exe ? → normal
- [ ] Aucune activité réseau/fichier anormale ? → FP confirmé
- [ ] Sévérité faible + pas de corrélation avec d'autres alertes ? → FP

---

## 🎯 Objectifs pour phishing-hard-03

- Atteindre 80%+ sur l'identification des FP
- Maintenir 100% sur les TP
- Réduire le temps de triage des FP grâce à la checklist