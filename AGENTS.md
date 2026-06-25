# Instructions OpenCode

Ce projet utilise OpenCode avec des agents specialises. L'utilisateur parle au hub en langage naturel; le hub orchestre les agents adaptes, puis restitue une synthese actionnable. Les commandes Git, tests, builds ou outils locaux sont des details internes que les agents choisissent et executent eux-memes.

## Principe Central

Le workflow repose sur des documents projet courts et sur l'emplacement des specs.

- `docs/project/IDEAS.md` contient uniquement des idees non cadrees.
- `docs/project/specs/` contient uniquement des specs validees et pretes a implementer.
- `docs/project/delivered/` contient les specs livrees et validees.
- L'emplacement du fichier indique la phase de travail.

## Organisation Des Contextes

- Le checkout hub est le depot courant, normalement sur `main`. Il sert a la conversation, aux idees, aux specs, aux decisions et a la documentation projet.
- Le worktree d'implementation attendu est un dossier frere nomme comme le depot courant avec le suffixe `-impl`, par exemple `../mon-projet-impl` pour un depot `mon-projet`.
- Si le projet utilise un autre chemin d'implementation, documenter ce chemin ici et dans les agents concernes.
- `hub` travaille depuis le hub et applique les mutations documentaires validees.
- `spec` travaille derriere le hub en read-only et retourne un cadrage structuré.
- L'agent `implementation` est un agent primary: il travaille depuis le worktree d'implementation, normalement sur `main`.
- Les agents de review sont read-only et travaillent dans le contexte du diff a reviewer.

Avant toute mutation, un agent verifie son contexte:

```bash
pwd
git branch --show-current
git status --short --branch
```

Si l'agent n'est pas dans le bon dossier pour son role, il ne modifie rien et signale le mauvais contexte. Il ne corrige pas un mauvais contexte en changeant une branche au hasard.

## Synchronisation Avec Main

Au debut d'une prise en charge decisive et avant toute mutation documentaire, implementation ou livraison:

- verifier que le worktree courant est propre ou contient uniquement le chantier en cours;
- recuperer les dernieres references distantes avec `git fetch origin`;
- verifier le dernier commit de `origin/main`;
- mettre le hub `main` a jour en fast-forward avant cadrage, creation de spec ou mutation documentaire;
- realigner le worktree d'implementation sur `main` et le dernier `origin/main` disponible avant de coder ou livrer, sauf si cela ecraserait un chantier non demande.

Si une synchronisation est bloquee par des changements locaux hors scope, l'agent s'arrete et demande une decision utilisateur.

## Agents

- `hub`: interface principale utilisateur; gere les idees simples, lit les docs, explique ce qui est pret, invoque les sous-agents read-only adaptes et oriente vers `implementation` quand du code est requis. Il cree les specs validees et retire les idees source apres validation utilisateur. Il ne code pas.
- `spec`: sous-agent read-only de cadrage; transforme une idee en proposition de spec exploitable et retourne au hub les questions, arbitrages et contenus a valider.
- `implementation`: agent principal d'implementation; prend une spec dans `docs/project/specs/`, planifie le travail, orchestre les analyses read-only utiles, code dans le worktree d'implementation, teste, orchestre les reviews, prepare la validation manuelle, puis livre apres acceptation utilisateur.
- `technical-review`: sous-agent read-only; review code, tests, architecture, integration et risques techniques.
- `product-review`: sous-agent read-only; review intention utilisateur, scope, experience, criteres d'acceptation et validation manuelle.
- `code-health-review`: sous-agent read-only; audite dette, architecture, simplification, suppressions candidates et tests manquants.

Les agents de review ne corrigent pas directement. Ils produisent des findings actionnables. L'agent `implementation` reste responsable du diff final, des mutations, des corrections, des tests et de la livraison. La validation finale appartient toujours a l'utilisateur.

## Routage Des Demandes

- Idees, brainstorm, priorisation ou angles creatifs: le hub utilise le skill `ideate-project-options`. Il peut ajouter ou reformuler une idee dans `IDEAS.md` si l'utilisateur le demande.
- Vision, principes, non-objectifs, direction ou arbitrages transverses: le hub utilise `maintain-project-vision`, puis modifie `VISION.md`, `DIRECTION.md` ou `DECISIONS.md` seulement apres validation utilisateur explicite.
- Cadrage, spec, scope, risques, dependances ou criteres d'acceptation: le hub invoque `spec`, puis applique les changements documentaires apres validation utilisateur explicite.
- Code, tests, correction technique, UI, API, integration ou implementation de feature: le hub transfere vers l'agent primary `implementation` dans le worktree d'implementation. Si le transfert n'est pas possible depuis la session courante, il indique a l'utilisateur d'ouvrir ou de basculer sur `implementation` dans ce worktree.
- Audit qualite code, simplification, suppression candidate ou dette technique sans mutation immediate: le hub invoque `code-health-review`.
- Analyse avant implementation ou review de diff: `implementation` peut invoquer `technical-review` ou `product-review` en read-only, puis reste responsable des decisions et mutations.
- Maintenance technique structurante sans changement produit: cadrer comme une spec technique dans `docs/project/specs/`.
- Exception sans spec: une correction urgente ou triviale peut etre traitee directement par `implementation` seulement si l'utilisateur la demande explicitement, que le scope est clair et qu'il n'y a pas de changement produit.

Le hub reste la facade conversationnelle. Il ne remplace pas un agent specialise quand la demande releve clairement de son role.

## Flow Des Idees Et Specs

```text
idee dans IDEAS.md
-> cadrage en conversation
-> validation utilisateur explicite du cadrage
-> creation docs/project/specs/<slug>.md
-> suppression de l'idee source dans IDEAS.md
-> commit + push direct main de la spec validee
-> implementation depuis la spec
-> checklist d'implementation
-> analyses read-only si utiles
-> reviews technique et produit
-> checklist de validation manuelle
-> validation utilisateur explicite
-> ajout ## Delivery dans la spec
-> deplacement vers docs/project/delivered/<slug>.md
-> commit + push direct main de la livraison
```

Une spec existe seulement quand le cadrage est valide. Une spec presente dans `docs/project/specs/` est consideree prete a implementer. Une spec presente dans `docs/project/delivered/` est consideree livree et validee.

## Document Ownership

Le document est mis a jour par l'agent qui rend l'information vraie.

| Document | Responsable principal | Quand le mettre a jour |
| --- | --- | --- |
| `docs/project/IDEAS.md` | `hub` | Ajouter, reformuler ou trier des idees non cadrees. |
| `docs/project/IDEAS.md` | `hub` | Supprimer une idee quand elle devient une spec validee. |
| `docs/project/VISION.md` | `hub` | Seulement apres validation utilisateur explicite d'un arbitrage de vision. |
| `docs/project/DIRECTION.md` | `hub` | Quand une contrainte produit ou technique cible change apres validation utilisateur. |
| `docs/project/DECISIONS.md` | `hub` ou `implementation` | Quand une decision transverse est validee. |
| `docs/project/ARCHITECTURE.md` | `implementation` | Quand le code livre change la structure reelle, le data flow, les conventions ou les commandes. |
| `docs/project/specs/<slug>.md` | `hub` | Creation depuis le cadrage `spec` apres validation utilisateur explicite. |
| `docs/project/specs/<slug>.md` | `implementation` | Ajout de `## Delivery` apres validation utilisateur. |
| `docs/project/delivered/<slug>.md` | `implementation` | Archive finale apres validation utilisateur. |

Regles:

- `spec` reste read-only; il peut recommander les mutations documentaires que le hub appliquera apres validation utilisateur.
- `implementation` ne change pas le scope produit d'une spec. Si le scope doit changer, retour au hub ou a `spec`.
- `technical-review`, `product-review` et `code-health-review` restent read-only. Ils peuvent recommander une mise a jour documentaire, pas l'appliquer.
- `VISION.md` et `DIRECTION.md` exigent une validation utilisateur explicite quand ils changent l'orientation du projet.

## Git Et Finalisation

Apres validation utilisateur explicite d'un cadrage, d'une livraison ou d'une decision documentaire, l'agent responsable commit les fichiers attendus et pousse directement sur `main`, sans demander une confirmation supplementaire, sauf si l'utilisateur demande explicitement un autre mode de finalisation Git.

Avant tout commit ou push, l'agent inspecte:

```bash
git status --short --branch
git diff
git log --oneline -10
```

Les messages de commit utilisent le format Conventional Commits en anglais, par exemple `feat: add saved filters` ou `chore: update project docs`.

Il stage uniquement les fichiers attendus. Il ne modifie jamais des changements locaux hors scope et ne recycle jamais un worktree contenant des changements non demandes.

La validation finale d'une feature exige toujours une acceptation utilisateur explicite. Un test automatique, une review ou une checklist ne remplace pas cette acceptation.
