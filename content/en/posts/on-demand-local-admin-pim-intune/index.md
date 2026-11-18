---
title: "On-Demand Local Admin with PIM and Intune"
date: 2026-11-18T03:10:14Z
draft: true
# Tags permis : entra, pim, intune, defender, azure, homelab, offensive, ai-security
tags: [entra, pim, intune]
summary: "[À COMPLÉTER] How to remove standing local admin and replace it with just-in-time, approved and logged elevation using Entra PIM and Intune."
translationKey: "pim-intune-local-admin"
---

## Context

[À COMPLÉTER] Generic environment (fleet size, Entra joined or hybrid, licensing: PIM requires Entra ID P2 or Entra ID Governance).

[À COMPLÉTER] Starting point: who had local admin, and why (support, developers, installs).

## Problem

[À COMPLÉTER] Risks of standing local admin: lateral movement, credential theft, control bypass.

[À COMPLÉTER] Limits of existing options ("Microsoft Entra Joined Device Local Administrator" role applied fleet-wide, shared LAPS, etc.).

## Solution

[À COMPLÉTER] Overview: security group made eligible through PIM for Groups → added to the local Administrators group by an Intune policy.

### 1. Security group and PIM for Groups

[À COMPLÉTER] Group creation, PIM onboarding, activation settings (max duration, MFA, justification, approval).

### 2. Intune policy (Account protection / Local users and groups)

[À COMPLÉTER] Profile, action (add or replace), device targeting.

```powershell
# [À COMPLÉTER] e.g. check local Administrators group membership
Get-LocalGroupMember -SID S-1-5-32-544
```

### 3. User experience and propagation delay

[À COMPLÉTER] What the user sees, how long until elevation applies (new token, sign-out), and how revocation works.

![[À COMPLÉTER] Screenshot description](screenshot-01.png)

## Proof / result

[À COMPLÉTER] PIM audit logs and on-device verification.

```kql
// [À COMPLÉTER] e.g. PIM activations over the last 7 days
AuditLogs
| where TimeGenerated > ago(7d)
| where Category == "GroupManagement" and LoggedByService == "PIM"
| project TimeGenerated, OperationName, InitiatedBy, TargetResources
```

## What I learned

[À COMPLÉTER] Pitfalls, limits, what I'd do differently.
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
