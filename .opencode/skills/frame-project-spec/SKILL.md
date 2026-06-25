---
name: frame-project-spec
description: Utiliser pour cadrer une idee en spec projet implementable, avec scope, hors scope, risques, dependances et criteres d'acceptation.
---

# Frame Project Spec

## Workflow court

1. Lire la vision, la direction, l'architecture, les decisions, l'idee source et les specs liees.
2. Diagnostiquer la maturite: idee brute, spec cadrable, extension, maintenance, question de vision ou chantier trop large.
3. Clarifier l'intention utilisateur, le probleme, le scope, le hors scope, les dependances et les criteres d'acceptation.
4. Proposer 2 ou 3 options de scope quand l'idee est ambigue.
5. Recommander le plus petit scope qui teste vraiment l'intention.
6. Decouper les idees trop larges en specs ordonnees.
7. Identifier l'orientation technique legere utile a `implementation` sans imposer une architecture bas niveau.
8. Garder le brouillon en conversation jusqu'a validation utilisateur explicite.

## Checklist spec-ready

Le hub peut creer une spec dans `docs/project/specs/` seulement si ces points sont assez clairs:

- intention utilisateur;
- probleme ou opportunite;
- scope concret;
- hors scope explicite;
- dependances;
- risques;
- orientation technique legere;
- criteres d'acceptation observables;
- validation manuelle possible;
- questions bloqueantes tranchees ou explicitees.

## Entrees attendues

- Idee, demande utilisateur ou probleme a resoudre.
- Documents projet et contexte technique.

## Sorties attendues

- Spec courte et implementable.
- Decoupage si l'idee initiale est trop large.
- Questions ou arbitrages utilisateur.
- Recommandation: continuer a cadrer, creer la spec, ou reporter.

## Criteres de qualite

- Ne pas surspecifier le projet final.
- Ne pas cacher une decision de vision dans une spec.
- Laisser le hub creer le fichier de spec apres validation utilisateur explicite.
- Rendre le perimetre assez clair pour que `implementation` travaille sans relire toute la conversation.

## References eventuelles

- `docs/project/VISION.md`
- `docs/project/DIRECTION.md`
- `docs/project/ARCHITECTURE.md`
- `docs/project/DECISIONS.md`
- `docs/project/IDEAS.md`
- `docs/project/templates/spec.md`
