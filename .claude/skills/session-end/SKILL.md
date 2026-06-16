---
name: session-end
description: Use when the user signals the end of a work session — phrases like « on s'arrête », « on arrête là », « fin de session », « c'est bon pour aujourd'hui », « stop pour ce soir », wrapping up or stopping work for the day. Triggers this repo's close-out routine.
---

# Session-End (fin de session)

## Overview

Routine de clôture **déterministe** : on synchronise la doc sur le code, on met à jour le journal de bord, on commite et on pousse. **Aucune improvisation Git.**

**Principe cœur — le code fait foi.** `docs/rules/architecture.md` doit refléter ce qui est **réellement implémenté dans le code**. Si le code a tranché autrement que la doc, on aligne **le fichier sur le code, jamais l'inverse**.

## When to use

Quand Marc signale l'arrêt : « on s'arrête », « on arrête là », « fin de session », « c'est bon pour aujourd'hui », « stop pour ce soir », etc.

## Fichiers de ce dépôt

| Fichier | Rôle |
|---|---|
| `docs/devlog/worklog.md` | Journal : une entrée datée par jour |
| `docs/devlog/state.md` | État courant pour reprendre demain (réécrit à chaque fois) |
| `docs/devlog/todo.md` | Idées / tâches à venir |
| `docs/rules/architecture.md` | Doc d'archi — miroir du code réel |

## Procédure (dans l'ordre, sans demander confirmation)

1. **État des lieux.** `git status`, `git diff`, puis `git log --oneline @{u}..HEAD` (commits non poussés). **Reste sur la branche courante** — ne crée PAS de branche, ne `git stash` PAS.
2. **Aligner `architecture.md` sur le code.** Lis le code réellement touché aujourd'hui. Si `docs/rules/architecture.md` décrit autre chose que le code, réécris la doc pour qu'elle décrive le code (pointe les fichiers concernés). Voir *La discipline* ci-dessous.
3. **worklog.** Ajoute une entrée datée du jour (date réelle du jour, format `YYYY-MM-DD`) dans `docs/devlog/worklog.md` : ce qui a été fait, décisions notables (dont tout réalignement archi et son *pourquoi*).
4. **state.** Réécris `docs/devlog/state.md` : où on en est, ce qui marche, ce qui reste à faire, sur quelle branche.
5. **todo.** Reporte dans `docs/devlog/todo.md` les nouvelles idées lâchées pendant la session, transformées en tâches concrètes.
6. **Présente les fichiers touchés.** Liste à Marc tous les fichiers modifiés avant de committer.
7. **Commit + push** (auto, sans demander) :
   - S'il reste du **code** non commité : commite-le d'abord avec un message descriptif court en français.
   - Puis commite le bookkeeping (`worklog.md`, `state.md`, `todo.md`, `architecture.md`) avec le message **exact** :
     ```
     chore: update worklog and state
     ```
   - `git push` vers l'upstream de la branche courante (`git push -u origin <branche>` s'il n'y a pas d'upstream).

## La discipline — code ↔ architecture.md

Le code est la réalité exécutée. Une doc qui ment est pire que pas de doc.

**Règle absolue :**
- Code ≠ doc → **réécris la doc** pour qu'elle décrive le code.
- **JAMAIS l'inverse** : ne modifie pas le code pour qu'il colle à la doc.
- Ne supprime pas une mention en silence : note le changement et son *pourquoi* dans le worklog.
- Tu peux **signaler** à Marc un changement d'archi qui semble non intentionnel — mais tu alignes la doc **quand même**. Signaler ≠ attendre.

## Red flags — STOP

Ces pensées veulent dire que tu déranges la routine :
- « Je crée une branche / un stash pour faire propre » → NON, reste sur la branche courante.
- « J'invente un meilleur message de commit » → NON, bookkeeping = `chore: update worklog and state`.
- « La doc dit X, je vais peut-être corriger le code » → NON, le code fait foi.
- « Le changement d'archi n'est peut-être pas voulu, j'attends » → aligne la doc, **puis** signale.
- « Je commite sans montrer les fichiers » → présente-les d'abord (étape 6).

## Rationalization table

| Excuse | Réalité |
|---|---|
| « Une branche WIP, c'est plus propre » | Marc veut le commit sur la branche courante, pas d'improvisation Git. |
| « Un message de commit plus parlant » | Le message bookkeeping est figé : `chore: update worklog and state`. |
| « La doc d'archi est sûrement la bonne intention » | Le code tourne, pas la doc. Aligne la doc sur le code. |
| « J'attends la confirmation avant de committer » | Auto : on commite et on pousse sans demander (présenter ≠ demander). |
| « Pas besoin de toucher architecture.md cette fois » | Toujours vérifier l'alignement doc/code en fin de session. |
