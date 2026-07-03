# Agent Flow Template

Base OpenCode generique pour piloter un projet avec un hub orchestrateur, un agent d'implementation autonome, des reviews read-only et des specs Markdown.

`AGENTS.md` est la source canonique du flow. Ce README resume seulement les points a adapter quand le template est copie.

## Principes

- `AGENTS.md` est la source canonique du flow.
- Le workflow se pilote par états, preuves et actions autorisées.
- `docs/project/IDEAS.md` contient les idées non cadrées.
- `docs/project/specs/` contient les specs non livrées: draft, validées ou intégrées selon l'état du workflow.
- `docs/project/delivered/` contient les specs livrées et validées.
- Le template est project-agnostique: le mode Git est configurable.

Mode Git recommandé par défaut:

```text
git_finalization_mode = pr-required
```

Un projet peut déclarer explicitement `direct-main` dans son `AGENTS.md` adapté s'il accepte le push direct sur `main`.

## Flow

```text
Idée
-> Spec draft
-> Spec validée par l'utilisateur
-> Spec intégrée selon le mode Git du projet
-> Implémentation seulement si Definition of Ready vraie
-> Reviews + tests + checklist de validation
-> Validation utilisateur explicite
-> Delivery + delivered
-> Finalisation selon le mode Git du projet
```

## Agents

- `hub`: facade utilisateur et orchestration.
- `spec`: cadrage read-only et proposition de specs validables.
- `implementation`: agent primary d'implementation autonome depuis une spec, dans le worktree d'implementation.
- `technical-review`: review read-only code, tests, architecture et integration.
- `product-review`: review read-only intention, scope, experience utilisateur et validation.
- `code-health-review`: audit read-only dette, simplification et architecture.

## Skills

- `ideate-project-options`: ideation et priorisation dans le hub.
- `frame-project-spec`: cadrage structure d'une spec.
- `maintain-project-vision`: protection de la vision et des non-objectifs.
- `prepare-manual-validation`: checklist de validation utilisateur.
- `review-code-health`: audit sante code et architecture.

## Utilisation

1. Copier ce template pour creer un nouveau projet.
2. Adapter `docs/project/VISION.md`, `DIRECTION.md` et `ARCHITECTURE.md`.
3. Adapter le chemin du worktree d'implementation dans `AGENTS.md` si la convention par defaut ne convient pas.
4. Redemarrer OpenCode apres toute modification de `.opencode/`, `opencode.json` ou des agents.
