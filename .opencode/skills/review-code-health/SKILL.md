---
name: review-code-health
description: Utiliser pour auditer read-only la qualite code, architecture, tests, dette, simplifications et suppressions candidates.
---

# Review Code Health

## Workflow court

1. Clarifier le perimetre audite: architecture, sous-systeme, tests, docs, tooling ou workflow.
2. Lire les fichiers, tests, specs et decisions qui expliquent pourquoi le code existe.
3. Identifier taille excessive, duplication, responsabilites melees, APIs fragiles, conventions floues, code mort probable et tests manquants.
4. Classer les findings par valeur pratique: suppression possible, simplification sure, refactor utile, dette acceptable, risque a surveiller.
5. Proposer des chantiers petits, testables et reversibles, sans appliquer les changements.

## Entrees attendues

- Question d'audit ou perimetre souhaite.
- Contraintes connues: pas de changement produit, cout acceptable, tests disponibles.

## Sorties attendues

- Diagnostic synthetique.
- Findings priorises avec impact, risque et verification recommandee.
- Elements a garder, simplifier, supprimer plus tard ou ne pas toucher.
- Chantiers candidats decoupes en petites tranches.
- Questions d'arbitrage si necessaire.

## Criteres de qualite

- Rester read-only.
- Ne pas confondre preference de style et probleme concret.
- Ne pas recommander une suppression sans verifier l'usage et la raison probable.
- Preferer des recommandations petites, testables et reversibles.

## References eventuelles

- `AGENTS.md`
- `docs/project/ARCHITECTURE.md`
- `docs/project/DIRECTION.md`
- `docs/project/DECISIONS.md`
- `docs/project/specs/`
- `docs/project/delivered/`
