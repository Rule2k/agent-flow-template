# Instructions OpenCode

Ce projet utilise OpenCode avec des agents specialises. L'utilisateur parle au hub en langage naturel; le hub orchestre les agents adaptes, puis restitue une synthese actionnable. Les commandes Git, tests, builds ou outils locaux sont des details internes que les agents choisissent et executent eux-memes.

## Principe Central

Le workflow repose sur des phases explicites. L'emplacement des specs aide a lire l'etat, mais l'action autorisee vient surtout de l'etat courant, des preuves disponibles et du mode de finalisation Git du projet.

- `docs/project/IDEAS.md` contient les idees non cadrees.
- `docs/project/specs/` contient les specs non livrees: draft, validees ou integrees selon l'etat du workflow.
- `docs/project/delivered/` contient les specs livrees et validees.
- Un agent n'avance que vers l'action autorisee par l'etat courant.

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

## Mode De Finalisation Git

Le template est project-agnostique: il ne suppose pas que tous les projets utilisent le meme mode d'integration.

Mode par defaut recommande:

```text
git_finalization_mode = pr-required
```

Modes supportes:

| Mode | Quand l'utiliser | Sortie d'une spec validee | Sortie d'une implementation |
| --- | --- | --- | --- |
| `pr-required` | Projet avec review ou integration protegee | PR spec mergée sur `main` | PR implementation ouverte, puis merge apres validation explicite |
| `direct-main` | Petit projet qui declare explicitement accepter le push direct | Commit spec present sur `main` | Commit livraison pousse sur `main` apres validation explicite |

Si un projet choisit `direct-main`, il doit le declarer explicitement dans son `AGENTS.md` adapte. Sinon, l'agent applique `pr-required`.

## Agents

- `hub`: interface principale utilisateur; gere les idees simples, lit les docs, explique l'etat courant, invoque les sous-agents read-only adaptes et oriente vers `implementation` seulement quand la Definition of Ready implementation est vraie. Il cree et met a jour les specs draft, puis les integre apres validation utilisateur. Il ne code pas.
- `spec`: sous-agent read-only de cadrage; transforme une idee en proposition de spec exploitable et retourne au hub les questions, arbitrages et contenus a valider.
- `implementation`: agent principal d'implementation; prend une spec integree selon le mode Git du projet, planifie le travail, orchestre les analyses read-only utiles, code dans le worktree d'implementation, teste, orchestre les reviews, prepare la validation manuelle, puis livre apres acceptation utilisateur.
- `technical-review`: sous-agent read-only; review code, tests, architecture, integration et risques techniques.
- `product-review`: sous-agent read-only; review intention utilisateur, scope, experience, criteres d'acceptation et validation manuelle.
- `code-health-review`: sous-agent read-only; audite dette, architecture, simplification, suppressions candidates et tests manquants.

Les agents de review ne corrigent pas directement. Ils produisent des findings actionnables. L'agent `implementation` reste responsable du diff final, des mutations, des corrections, des tests et de la livraison. La validation finale appartient toujours a l'utilisateur.

## Routage Des Demandes

- Idees, brainstorm, priorisation ou angles creatifs: le hub utilise le skill `ideate-project-options`. Il peut ajouter ou reformuler une idee dans `IDEAS.md` si l'utilisateur le demande.
- Vision, principes, non-objectifs, direction ou arbitrages transverses: le hub utilise `maintain-project-vision`, puis modifie `VISION.md`, `DIRECTION.md` ou `DECISIONS.md` seulement apres validation utilisateur explicite.
- Cadrage, spec, scope, risques, dependances ou criteres d'acceptation: le hub invoque `spec`, puis applique les changements documentaires apres validation utilisateur explicite.
- Code, tests, correction technique, UI, API, integration ou implementation de feature: le hub verifie la Definition of Ready implementation, puis transfere vers l'agent primary `implementation` dans le worktree d'implementation. Si la Definition of Ready n'est pas vraie, le hub revient a l'action autorisee par l'etat courant.
- Audit qualite code, simplification, suppression candidate ou dette technique sans mutation immediate: le hub invoque `code-health-review`.
- Analyse avant implementation ou review de diff: `implementation` peut invoquer `technical-review` ou `product-review` en read-only, puis reste responsable des decisions et mutations.
- Maintenance technique structurante sans changement produit: cadrer comme une spec technique dans `docs/project/specs/`.
- Exception sans spec: une correction urgente ou triviale peut etre traitee directement par `implementation` seulement si l'utilisateur la demande explicitement, que le scope est clair, qu'il n'y a pas de changement produit et que le mode Git du projet autorise la finalisation choisie.

Le hub reste la facade conversationnelle. Il ne remplace pas un agent specialise quand la demande releve clairement de son role.

## Flow Des Idees Et Specs

| Etat | Preuve requise | Actions autorisees | Sortie attendue |
| --- | --- | --- | --- |
| Idee | Demande utilisateur ou entree `docs/project/IDEAS.md` | Clarifier, trier, ecrire une idee, proposer un cadrage | PR idee, commit idee ou brouillon de spec selon mode Git |
| Spec draft | Fichier spec draft dans `docs/project/specs/<slug>.md` | Modifier ce fichier spec uniquement, poser les questions de cadrage restantes | Validation explicite utilisateur du fichier spec |
| Spec validee | L'utilisateur a valide explicitement le fichier spec | Integrer la spec selon le mode Git du projet | Spec integree |
| Spec integree | `pr-required`: PR spec mergée sur `main`; `direct-main`: commit spec present sur `main` | Demarrer l'implementation dans le worktree d'implementation | Branche ou chantier implementation pret |
| Implementation | Definition of Ready implementation vraie | Code, tests, analyses read-only, reviews | Livraison proposee |
| Validation | Tests/reviews/checklist disponibles | Validation utilisateur, corrections de bugs dans le scope | Acceptation explicite ou retour cadre |
| Livraison | Acceptation explicite utilisateur | Ajouter `## Delivery`, deplacer vers `docs/project/delivered/`, finaliser selon mode Git | Spec livree |

Definition of Ready implementation:

```text
spec fichier existe dans docs/project/specs/
spec fichier validee explicitement par l'utilisateur
spec integree selon le mode Git du projet
worktree d'implementation propre
worktree d'implementation aligne sur main/origin-main attendu
```

Si un point manque, l'action autorisee est de completer ce point. Le code attend.

Regle de repli:

```text
si une action a ete lancee hors etat autorise:
1. arreter l'action;
2. isoler, dropper ou mettre de cote les changements hors phase;
3. revenir a la prochaine action autorisee;
4. signaler l'etat reel a l'utilisateur.
```

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
| `docs/project/specs/<slug>.md` | `hub` | Creation et mise a jour du fichier spec draft depuis le cadrage `spec`; integration selon mode Git apres validation utilisateur explicite. |
| `docs/project/specs/<slug>.md` | `implementation` | Ajout de `## Delivery` apres validation utilisateur. |
| `docs/project/delivered/<slug>.md` | `implementation` | Archive finale apres validation utilisateur. |

Regles:

- `spec` reste read-only; il peut recommander les mutations documentaires que le hub appliquera apres validation utilisateur.
- `implementation` ne change pas le scope produit d'une spec. Si le scope doit changer, retour au hub ou a `spec`.
- `technical-review`, `product-review` et `code-health-review` restent read-only. Ils peuvent recommander une mise a jour documentaire, pas l'appliquer.
- `VISION.md` et `DIRECTION.md` exigent une validation utilisateur explicite quand ils changent l'orientation du projet.

## Git Et Finalisation

Le mode de finalisation Git decide comment une spec, une implementation ou une livraison quitte le worktree local.

### Mode `pr-required`

Apres validation utilisateur explicite d'un cadrage, l'agent responsable:

```text
commit sur branche dediee
push branche
ouvre une PR vers main
attend review / validation / merge selon le projet
```

L'implementation demarre seulement apres integration de la spec.

### Mode `direct-main`

Disponible seulement si le projet adapte le declare explicitement. Apres validation utilisateur explicite d'un cadrage, d'une livraison ou d'une decision documentaire, l'agent responsable peut commit et push directement sur `main`.

Avant tout commit, push ou PR, l'agent inspecte:

```bash
git status --short --branch
git diff
git log --oneline -10
```

Les messages de commit utilisent le format Conventional Commits en anglais, par exemple `feat: add saved filters` ou `chore: update project docs`.

Il stage uniquement les fichiers attendus. Il ne modifie jamais des changements locaux hors scope et ne recycle jamais un worktree contenant des changements non demandes.

La validation finale d'une feature exige toujours une acceptation utilisateur explicite. Un test automatique, une review ou une checklist ne remplace pas cette acceptation.
