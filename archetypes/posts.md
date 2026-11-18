{{- $fr := eq .Site.Language.Lang "fr" -}}
---
title: "{{ replace .File.ContentBaseName "-" " " | title }}"
date: {{ .Date }}
draft: true
# Tags permis : entra, pim, intune, defender, azure, homelab, offensive, ai-security
tags: []
summary: "{{ if $fr }}[À COMPLÉTER] Une ou deux phrases qui résument l'article.{{ else }}[À COMPLÉTER] One or two sentences summarizing the post.{{ end }}"
# Pour relier la traduction : même valeur dans la version EN et FR.
translationKey: ""
---

{{ if $fr -}}
## Contexte

[À COMPLÉTER] Environnement (générique), objectif, contraintes.

## Problème

[À COMPLÉTER] Ce qui ne marchait pas ou le risque à réduire.

## Solution

[À COMPLÉTER] Étapes, configuration, scripts. Captures dans ce dossier :
`![Description](capture-01.png)`

```powershell
# [À COMPLÉTER]
```

## Preuve / résultat

[À COMPLÉTER] Ce qui démontre que ça fonctionne (requête KQL, journal, capture).

## Ce que j'ai appris

[À COMPLÉTER]
{{- else -}}
## Context

[À COMPLÉTER] Environment (generic), goal, constraints.

## Problem

[À COMPLÉTER] What was broken, or the risk to reduce.

## Solution

[À COMPLÉTER] Steps, configuration, scripts. Screenshots go in this folder:
`![Description](screenshot-01.png)`

```powershell
# [À COMPLÉTER]
```

## Proof / result

[À COMPLÉTER] What shows it works (KQL query, log, screenshot).

## What I learned

[À COMPLÉTER]
{{- end }}

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
