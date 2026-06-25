---
name: prepare-manual-validation
description: Utiliser en fin d'implementation pour transformer les criteres d'acceptation en checklist de validation utilisateur, puis analyser les retours.
---

# Prepare Manual Validation

## Workflow court

1. Relire la spec, les criteres d'acceptation, les decisions et les notes de review.
2. Transformer les criteres en scenarios courts et observables par l'utilisateur.
3. Distinguer ce que l'agent a verifie automatiquement de ce que l'utilisateur doit juger.
4. Produire une checklist: quoi lancer, quoi observer, cas limites, criteres d'acceptation et refus.
5. Pendant ou apres validation, classer les retours: bug, regression, comprehension, feeling, scope, nouvelle idee.
6. Si l'utilisateur accepte explicitement, recommander a `implementation` d'ajouter `## Delivery`, deplacer la spec vers `delivered/`, commit et push selon `AGENTS.md`.

## Entrees attendues

- Spec en fin d'implementation.
- Resultats de tests, reviews et observations techniques.
- Retours utilisateur ou demande de validation manuelle.

## Sorties attendues

- Checklist courte de validation manuelle.
- Classification des retours.
- Recommandation: corriger, retester, ouvrir une nouvelle idee, recadrer ou livrer.

## Criteres de qualite

- Ne pas considerer un test automatique comme validation utilisateur.
- Ne pas transformer une nouvelle envie en bug de la spec courante.
- Garder les scenarios rapides, observables et lies aux criteres d'acceptation.
- Exiger une acceptation utilisateur explicite avant livraison.

## References eventuelles

- `docs/project/specs/<slug>.md`
- `docs/project/delivered/`
- `docs/project/DIRECTION.md`
- `docs/project/ARCHITECTURE.md`
- `docs/project/DECISIONS.md`
