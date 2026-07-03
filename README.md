# Agent Flow Template

Base OMP generique pour piloter un projet avec des documents courts, des skills projet, des agents integres au CLI et des specs Markdown.

`AGENTS.md` est la source canonique du flow. Ce README resume seulement les points a adapter quand le template est copie.

## Principes

- `AGENTS.md` est la source canonique du flow.
- Le workflow se pilote par etats, preuves et actions autorisees.
- `docs/project/IDEAS.md` contient les idees non cadrees.
- `docs/project/specs/` contient uniquement les specs validees et pretes a implementer.
- `docs/project/delivered/` contient les specs livrees et validees.
- Les skills projet vivent dans `.agent/skills/`.
- Le template ne cree pas de systeme d'agents custom quand le CLI fournit deja exploration, implementation, review et audit.
- Le mode Git est toujours PR obligatoire.

## Flow

```text
Idee
-> Cadrage en conversation
-> Spec validee par l'utilisateur
-> Branche dediee + PR spec vers main
-> Merge humain explicite
-> Implementation seulement quand la spec est integree
-> Tests + reviews + checklist de validation
-> PR implementation/livraison vers main
-> Validation utilisateur explicite
-> Delivery + delivered sur la branche PR
-> Merge humain explicite
```

## Skills

- `ideate-project-options`: ideation, priorisation et tri d'idees.
- `frame-project-spec`: cadrage structure d'une spec.
- `maintain-project-vision`: protection de la vision et des non-objectifs.
- `prepare-manual-validation`: checklist de validation utilisateur.
- `review-code-health`: audit sante code et architecture.

## Agents

Utiliser les agents integres du CLI courant pour la mecanique:

- exploration read-only;
- planification;
- implementation;
- review technique;
- avis senior;
- audit.

Ne creer un agent custom projet que si un besoin specifique n'est pas couvert par les agents integres et qu'un skill ne suffit pas.

## Utilisation

1. Copier ce template pour creer un nouveau projet.
2. Adapter `docs/project/VISION.md`, `DIRECTION.md` et `ARCHITECTURE.md`.
3. Adapter le chemin du worktree d'implementation dans `AGENTS.md` si la convention `../<repo>-impl` ne convient pas.
4. Adapter les skills dans `.agent/skills/` si le projet a des contraintes metier specifiques.
5. Redemarrer OMP apres toute modification de `AGENTS.md` ou `.agent/skills/`.
