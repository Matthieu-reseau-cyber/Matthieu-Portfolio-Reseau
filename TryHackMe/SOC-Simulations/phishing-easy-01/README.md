🎯 Phishing Easy #01 — SOC Simulator TryHackMe
> \*\*Date :\*\* 01/05/2026  
> \*\*Niveau :\*\* 🟢 Facile  
> \*\*Score final :\*\* 🥇 160 points — 1ère place  
> \*\*Taux de détection correct (True Positives) :\*\* ✅ 100%  
> \*\*Taux de faux positifs traités :\*\* — (aucun FP à traiter dans ce scénario)  
> \*\*Temps moyen de résolution :\*\* 2 minutes  
> \*\*Temps moyen de séjour :\*\* 10 minutes  
> \*\*Alertes désactivées/non actives :\*\* 4
---
📋 Résumé du scénario
Simulation d'une session d'analyse d'alertes SOC L1 centrée sur la détection de campagnes de phishing par email.  
Les alertes proviennent du SIEM de l'organisation fictive TheTryDaily (`thetrydaily.thm`).  
Chaque alerte nécessite : classification (TP/FP), rédaction d'un rapport 5W et décision d'escalade.
---
📊 Résultats par alerte
ID	Règle déclenchée	Sévérité	Classification	Score	Escalade
8814	E-mails reçus contenant des liens externes suspects	Moyen	❌ FP → erreur (was TP)	-10 pts	—
8817	E-mails reçus contenant des liens externes suspects	Moyen	✅ True Positive	10/10	✅ Escaladé
8815	E-mails reçus contenant des liens externes suspects	Moyen	✅ True Positive	10/10	✅ Escaladé
8816	L'accès aux URL externes figurant sur la liste noire	Élevé	✅ True Positive	10/10	✅ Escaladé
---
🔍 Techniques d'attaque identifiées
Technique	Alerte	Description
HR Onboarding Phishing	8814	Domaine externe inconnu (`hrconnex.thm`) imitant un processus RH
Typosquatting	8817	`m1crosoftsupport.co` — "i" remplacé par "1", TLD `.co`
Brand Impersonation + URL Shortener	8815	`amazon.biz` + lien `bit.ly` masquant la destination réelle
Click Confirmed + Firewall Block	8816	Clic utilisateur confirmé par logs firewall — tentative bloquée
---
⚡ Prochain scénario déverrouillé
> \*\*Persistance : T1543\*\* — \*"Nouvelle installation de service détectée"\*  
> Règle déclenchée sur `THM-MKT-WS`  
> Niveau : 🟡 Moyen · 9 minutes · +130 points
---
📚 Leçons apprises
→ Voir lessons_learned.md pour l'analyse complète des erreurs et axes d'amélioration.
