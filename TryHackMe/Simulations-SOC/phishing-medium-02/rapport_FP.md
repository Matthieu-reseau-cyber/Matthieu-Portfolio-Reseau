# Rapport FP — Faux Positifs — phishing-medium-02

**Exercice :** Le Phishing se Dévoile | **Date :** 08/05/2026  
**Taux FP identifiés :** 58% ⚠️

---

## ❌ FP manqués — Processus Windows légitimes

Les alertes manquées concernaient des processus Windows natifs
déclenchant des alertes de relation parent-enfant suspecte.

| Processus | Parent | Raison FP |
|-----------|--------|-----------|
| TrustedInstaller.exe | services.exe | Installateur modules Windows |
| taskhostw.exe KEYROAMING | svchost.exe | Synchronisation profils |
| taskhostw.exe NGCKeyPregen | svchost.exe | Windows Hello PIN |
| WUDFHost.exe | services.exe | Framework pilotes Windows |
| rdpclip.exe | svchost.exe | Presse-papiers RDP |
| svchost.exe -k wsappx | services.exe | Service Windows Store |

---

## 🔍 Pattern commun

Tous ces FP partagent les mêmes caractéristiques :
- Processus Windows **natifs et légitimes**
- Relation parent-enfant **inhabituelle en apparence** mais normale
- Sévérité **faible** dans les alertes
- Aucune activité réseau ou fichier suspect associée

---

## ✅ Règle de décision apprise

> Si le processus est un binaire Windows signé Microsoft,
> que le parent est services.exe ou svchost.exe,
> et qu'il n'y a aucune activité réseau/fichier anormale associée
> → **Faux Positif**