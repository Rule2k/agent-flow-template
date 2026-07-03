---
name: frame-project-spec
description: Utiliser pour cadrer une idee en spec projet implementable, avec scope, hors scope, risques, dependances et criteres d'acceptation.
---

# Frame Project Spec

## Workflow court

1. Lire la vision, la direction, l'architecture, les decisions, l'idee source, les specs liees et les livraisons pertinentes.
2. Diagnostiquer la maturite: idee brute, spec cadrable, extension, maintenance, question de vision ou chantier trop large.
3. Clarifier l'intention utilisateur, le probleme, le scope, le hors scope, les dependances et les criteres d'acceptation.
4. Proposer 2 ou 3 options de scope quand l'idee est ambigue.
5. Faire un stress test du cadrage: hypotheses fragiles, ambiguites qui changeraient l'implementation, risques de sur-scope, criteres d'acceptation faibles et version plus petite possible.
6. Recommander le plus petit scope qui teste vraiment l'intention.
7. Decouper les idees trop larges en specs ordonnees.
8. Identifier l'orientation technique legere utile a l'implementation sans imposer une architecture bas niveau.
9. Garder le brouillon en conversation jusqu'a validation utilisateur explicite.

## Stress test du cadrage

Avant de recommander une spec comme validable, challenger le cadrage avec ces angles:

- hypotheses fragiles: ce qui est suppose vrai mais pas encore verifie;
- ambiguites bloquantes: ce qui changerait vraiment l'implementation;
- sur-scope probable: ce qui risque d'ajouter une deuxieme feature cachee;
- criteres faibles: ce qui ne permet pas d'accepter ou refuser clairement la livraison;
- version plus petite: tranche minimale qui teste la meme intention.

Limiter les questions a 3-5 decisions vraiment utiles. Transformer les points non bloquants en risques, hors scope ou questions ouvertes.

## Checklist spec-ready

Creer une spec dans `docs/project/specs/` seulement si ces points sont assez clairs:

- intention utilisateur;
- probleme ou opportunite;
- scope concret;
- hors scope explicite;
- dependances;
- risques;
- orientation technique legere;
- stress test du cadrage effectue;
- criteres d'acceptation observables;
- validation manuelle possible;
- questions bloquantes tranchees; questions non bloquantes explicitees.

## Entrees attendues

- Idee, demande utilisateur ou probleme a resoudre.
- Documents projet et contexte technique.

## Sorties attendues

- Spec courte et implementable.
- Stress test du cadrage avec risques, flous et version plus petite si pertinente.
- Decoupage si l'idee initiale est trop large.
- Questions ou arbitrages utilisateur.
- Recommandation: continuer a cadrer, creer la spec, ou reporter.

## Criteres de qualite

- Ne pas surspecifier le projet final.
- Ne pas cacher une decision de vision dans une spec.
- Laisser la session responsable creer le fichier de spec apres validation utilisateur explicite.
- Rendre le perimetre assez clair pour que l'implementation travaille sans relire toute la conversation.

## References eventuelles

- `docs/project/VISION.md`
- `docs/project/DIRECTION.md`
- `docs/project/ARCHITECTURE.md`
- `docs/project/DECISIONS.md`
- `docs/project/IDEAS.md`
- `docs/project/specs/`
- `docs/project/delivered/`
- `docs/project/templates/spec.md`
