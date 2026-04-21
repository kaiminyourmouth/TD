# TD Mainline Rojo Repo

This repo is for the live, non-PVP TD game.

I am using it to move the project into Rojo in small, safe steps instead of trying to pull the whole Studio place apart at once. The first milestone is only the foundation: folders, Rojo config, basic tooling, and notes about what should move first.

## Scope

Included:
- Main live game
- Active server, client, and shared code
- Active player-facing UI
- Mainline assets, templates, and map content

Excluded:
- PVP
- Test places
- Staging copies
- Public-test clones
- Duplicate copies used only before release pushes

## Layout

```text
Src/
  Shared/   Shared modules, config, constants, networking, and types
  Server/   Server bootstraps, services, systems, and game logic
  Client/   Client bootstraps, controllers, UI logic, input, and effects

Assets/
  Replicated/  Shared replicated assets
  Server/      Server-only templates and assets
  UI/          UI templates or exported UI trees
  Maps/        World and map content

Tests/
  Shared/   Shared module tests
  Server/   Server-side tests where useful

Docs/
  Migration/     Migration notes and plans
  Architecture/  Architecture notes
```

## Status

Milestone 1 is complete.

Current focus:
- Review the live Studio project in more detail
- Confirm the first small modules to extract
- Keep PVP and test copies out of this repo

## Notes

- This repo starts small on purpose.
- No live gameplay systems have been moved yet.
- The next work should be inventory and then one low-risk shared module extraction.
