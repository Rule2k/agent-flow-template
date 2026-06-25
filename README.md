# Agent Flow Template

Base OpenCode generique pour piloter un projet avec un hub orchestrateur, un agent d'implementation autonome, des reviews read-only et des specs Markdown.

## Principes

- `docs/project/IDEAS.md` contient uniquement les idees non cadrees.
- `docs/project/specs/` contient les specs validees et pretes a implementer.
- `docs/project/delivered/` contient les specs livrees et validees.
- `docs/project/templates/spec.md` sert de base aux nouvelles specs.
- L'emplacement du fichier indique la phase de travail.

## Flow

```text
IDEAS.md
-> cadrage avec l'agent spec
-> creation docs/project/specs/<slug>.md apres validation utilisateur
-> suppression de l'idee source dans IDEAS.md
-> implementation depuis la spec
-> technical-review + product-review
-> checklist de validation manuelle
-> validation utilisateur explicite
-> ajout ## Delivery dans la spec
-> deplacement vers docs/project/delivered/<slug>.md
-> commit + push direct main
```

## Agents

- `hub`: facade utilisateur et orchestration.
- `spec`: cadrage read-only et proposition de specs validables.
- `implementation`: implementation autonome depuis une spec.
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
