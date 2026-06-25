# Template De Spec

Utiliser ce template pour creer une spec validee dans `docs/project/specs/<slug>.md` apres validation utilisateur explicite du cadrage.

## Intention Utilisateur

Ce que l'utilisateur doit percevoir, accomplir, comprendre ou obtenir.

## Probleme Ou Opportunite

Pourquoi ce changement est utile maintenant.

## Scope

Ce qui doit etre implemente dans cette tranche.

## Hors Scope

Ce qui est explicitement exclu pour eviter le sur-scope.

## Criteres D'Acceptation

- Critere observable 1.
- Critere observable 2.

## Stress Test

- Hypotheses fragiles:
- Ambiguites qui changeraient l'implementation:
- Risques de sur-scope:
- Criteres d'acceptation faibles:
- Version plus petite possible:

## Validation Manuelle

Scenarios courts que l'utilisateur pourra executer ou observer pour accepter ou refuser la livraison.

## Orientation Technique Legere

- Parties du code ou conventions a reutiliser.
- Points d'architecture a surveiller.
- Tests utiles.

Ne pas imposer une architecture bas niveau sauf decision deja validee.

## Dependances

- Specs, decisions, services, donnees ou contraintes necessaires.

## Risques

- Risques de scope, regression, dette ou ambiguite.

## Decisions

- Decisions validees propres a cette spec, avec raison et reversibilite si utile.

## Questions Ouvertes

- Questions non bloqueantes ou arbitrages restant a relayer. Les questions bloqueantes doivent etre tranchees avant implementation.

<!--
Ne pas inclure `## Delivery` dans la spec initiale.

Apres validation utilisateur explicite, `implementation` ajoute une section
`## Delivery` avec:

- resume de livraison;
- tests, lint, build ou validations lances;
- resultat technical-review;
- resultat product-review;
- validation utilisateur explicite;
- risques restants.

Puis le fichier est deplace vers `docs/project/delivered/`.
-->
