---
title: "Admin local à la demande avec PIM et Intune"
date: 2026-11-18T03:10:14Z
draft: true
# Tags permis : entra, pim, intune, defender, azure, homelab, offensive, ai-security
tags: [entra, pim, intune]
summary: "[À COMPLÉTER] Comment retirer l'admin local permanent et le remplacer par une élévation juste-à-temps, approuvée et journalisée, avec Entra PIM et Intune."
translationKey: "pim-intune-local-admin"
---

## Contexte

[À COMPLÉTER] Environnement générique (taille du parc, Entra joined ou hybride, licences : PIM exige Entra ID P2 ou Entra ID Governance).

[À COMPLÉTER] Situation de départ : qui avait l'admin local, et pourquoi (support, développeurs, installations).

## Problème

[À COMPLÉTER] Les risques de l'admin local permanent : mouvement latéral, vol d'identifiants, contournement des contrôles.

[À COMPLÉTER] Les limites des options existantes (rôle « Microsoft Entra Joined Device Local Administrator » appliqué à tout le parc, LAPS partagé, etc.).

## Solution

[À COMPLÉTER] Vue d'ensemble : groupe de sécurité éligible via PIM pour les groupes → membre du groupe Administrateurs local via une stratégie Intune.

### 1. Groupe de sécurité et PIM pour les groupes

[À COMPLÉTER] Création du groupe, activation de PIM, paramètres d'activation (durée max, MFA, justification, approbation).

### 2. Stratégie Intune (Protection du compte / Utilisateurs et groupes locaux)

[À COMPLÉTER] Profil, action (ajouter ou remplacer), ciblage des appareils.

```powershell
# [À COMPLÉTER] p. ex. vérifier les membres du groupe Administrateurs local
Get-LocalGroupMember -SID S-1-5-32-544
```

### 3. Expérience utilisateur et délai d'application

[À COMPLÉTER] Ce que voit l'utilisateur, délai avant que l'élévation s'applique (nouveau jeton, fermeture de session), et comment la révocation fonctionne.

![[À COMPLÉTER] Description de la capture](capture-01.png)

## Preuve / résultat

[À COMPLÉTER] Journaux d'audit PIM et vérification sur le poste.

```kql
// [À COMPLÉTER] p. ex. activations PIM des 7 derniers jours
AuditLogs
| where TimeGenerated > ago(7d)
| where Category == "GroupManagement" and LoggedByService == "PIM"
| project TimeGenerated, OperationName, InitiatedBy, TargetResources
```

## Ce que j'ai appris

[À COMPLÉTER] Pièges, limites, ce que je ferais différemment.
<!--
=====================================================================
 CHECKLIST D'ANONYMISATION — à valider AVANT de passer draft: false
 (Ce commentaire est retiré du site publié : il n'apparaît ni à
 l'écran ni dans le code source de la page.)
=====================================================================
[ ] Aucun nom d'employeur ou de client (texte, captures, noms de fichiers)
[ ] Aucun tenant ID, ID d'abonnement, ID d'objet ou d'application réel (GUID)
[ ] Aucun nom de domaine réel (contoso.com / example.com à la place)
[ ] Aucune IP interne (utiliser 10.0.0.0/8 générique ou 192.0.2.0/24)
[ ] Aucun UPN ou nom d'utilisateur réel
[ ] Aucun nom de serveur, de poste ou d'appareil réel
[ ] Captures caviardées (en remplissant la zone, pas avec un flou réversible)
[ ] Métadonnées EXIF retirées des images : exiftool -all= *.png *.jpg
[ ] Scripts relus : aucun secret, jeton, clé ou chemin réseau réel
[ ] Plus aucun [À COMPLÉTER] dans le texte
-->
