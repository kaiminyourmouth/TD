# TD Mainline Rojo Roadmap

This is the working plan for moving the live, non-PVP TD game into a Rojo repo.

The point is not to rewrite the game. The point is to make the project easier to read, safer to change, and less dependent on hidden Studio ordering or `_G` state.

## Scope

Included:
- Main live non-PVP game
- Active server, client, and shared code
- Live player-facing UI
- Shared runtime assets and templates
- Mainline world, lobby, and map content

Excluded:
- PVP and PVP-only systems
- Test places
- Staging places
- Public-test copies
- Duplicate release-check copies

## Current Read

From the Studio inspection, this is a real working game, not a tiny prototype. The main issue is that a lot of important logic is spread across Studio containers, UI trees, asset trees, and large central scripts.

Important notes:
- Some systems depend on `_G` and implicit startup order.
- Some logic is inside UI or asset containers.
- There are legacy, backup, disabled, and PVP-related pieces mixed near active content.
- The repo did not already exist locally before this migration work started.

This is still very workable. It just needs a staged migration instead of a one-shot export.

## Working Rules

I will keep these rules in mind while migrating:

1. Keep the live game playable after each meaningful step.
2. Move code first, then clean behavior once it is easier to see.
3. Replace `_G` gradually, one area at a time.
4. Archive before deleting anything uncertain.
5. Keep module boundaries explicit.
6. Do not pull PVP or test copies into the mainline repo unless the scope changes.

## Naming

Project-owned folders and source containers use PascalCase.

Examples:
- `Src/Shared/Modules`
- `Src/Server/Services`
- `Src/Client/Controllers`
- `Assets/Replicated`
- `Docs/Migration`

Tooling files keep their normal names because the tools expect them:
- `.gitignore`
- `default.project.json`
- `aftman.toml`
- `stylua.toml`
- `selene.toml`
- `wally.toml`

## Target Layout

```text
repo/
  default.project.json
  README.md
  aftman.toml
  stylua.toml
  selene.toml
  Src/
    Shared/
      Config/
      Constants/
      Data/
      Net/
      Modules/
      Types/
    Server/
      Bootstrap/
      Services/
      Systems/
      Domain/
    Client/
      Bootstrap/
      Controllers/
      UI/
      Effects/
      Input/
  Assets/
    Replicated/
    Server/
    UI/
    Maps/
  Tests/
  Docs/
```

## Folder Intent

- `Src/Server/Services`: server-owned long-running behavior
- `Src/Server/Systems`: startup and cross-system wiring
- `Src/Server/Domain`: feature-specific server logic
- `Src/Client/Controllers`: long-running client behavior
- `Src/Client/UI`: UI modules and bindings
- `Src/Shared`: shared modules, config, constants, data, networking, and types
- `Assets`: exported instances, templates, maps, models, and UI trees

Simple rule:
- Server services own authority and state.
- Client controllers own client behavior.
- Shared modules stay small and reusable.
- Assets should not hide major game logic unless there is a strong reason.

## Phase Roadmap

### Phase 0: Protect the Current Game

Before moving code, I need a safe fallback.

Checklist:
- [ ] Create a locked pre-migration place backup
- [ ] Record current place metadata and production context
- [ ] Keep an inventory of key scripts and active systems
- [ ] Mark PVP and test/staging copies as out of scope
- [ ] Decide the branch strategy for migration work

Done when:
- I can roll back if something goes wrong
- The included and excluded work is clear

### Phase 1: Create the Repo Foundation

This phase is complete.

Checklist:
- [x] Initialize the repo structure
- [x] Add `default.project.json`
- [x] Add `Src`, `Assets`, `Tests`, and `Docs`
- [x] Add Rojo tooling and formatting/linting config
- [x] Add a short README
- [x] Create the first clean repo state

Done when:
- The repo has a simple structure ready to receive code

### Phase 2: Inventory and Classification

Before moving systems, I need to sort what is active, what is shared, and what should wait.

Buckets:
- Active runtime code
- Shared reusable code
- Active assets/templates
- Archive, legacy, or dead content

Checklist:
- [ ] List active gameplay areas
- [ ] List shared modules worth moving first
- [ ] List live UI surfaces
- [ ] Identify logic tied to assets
- [ ] Identify logic tied to the world
- [ ] Identify archive candidates

Done when:
- Each major system has a likely destination

### Phase 3: Finish the Migration Map

The first map exists, but it should be refined as the inventory gets better.

Checklist:
- [ ] Map shared utilities to `Src/Shared`
- [ ] Map server-owned systems to `Src/Server`
- [ ] Map client-owned systems to `Src/Client`
- [ ] Map exported instance trees to `Assets`
- [ ] Keep inactive history out of active runtime folders

Done when:
- I know where each major system should move before touching it

### Phase 4: Add Bootstraps

Startup should become easy to find.

Checklist:
- [ ] Create a small server bootstrap
- [ ] Create a small client bootstrap
- [ ] Define service startup order
- [ ] Define controller startup order
- [ ] Route new initialization through those entry points

Done when:
- Startup can be understood from clear server/client entry points

### Phase 5: Extract Low-Risk Shared Modules

Start with small modules that are easy to verify.

Best first targets:
- Helper utilities
- Table/string/sort/formatting helpers
- Shared constants
- Static game config

Checklist:
- [ ] Move pure helper modules first
- [ ] Move constants next
- [ ] Move static config after that
- [ ] Add tests where they are cheap and useful

Done when:
- A small shared module group lives in source control and Studio still runs normally

### Phase 6: Replace `_G` Area by Area

Hidden globals should be removed carefully, not all at once.

Suggested order:
1. Utility helpers
2. Constants/config
3. Networking access
4. Player data helpers
5. UI helper logic
6. Gameplay cross-system helpers

Checklist:
- [ ] Find `_G` producers
- [ ] Find `_G` consumers
- [ ] Group them by area
- [ ] Replace one area with explicit modules
- [ ] Remove the old `_G` assignments only after the replacement is confirmed

Done when:
- Fewer systems rely on hidden global state

### Phase 7: Centralize Config and Game Data

Game data should be easy to find and review.

Checklist:
- [ ] Move pass IDs, badge IDs, prices, rewards, and constants into config modules
- [ ] Move balancing values into shared data/config
- [ ] Remove operational secrets from source where possible
- [ ] Standardize names for config modules

Done when:
- Important values are no longer buried inside large scripts

### Phase 8: Build the Networking Layer

Remotes should have clear ownership and names.

Checklist:
- [ ] Create shared remote definitions
- [ ] Add server-side wrappers/listeners by feature
- [ ] Add client-side wrappers/callers by feature
- [ ] Document expected payloads
- [ ] Group remotes by feature area

Done when:
- Networking is easier to trace from client to server

### Phase 9: Split Large Scripts

The largest scripts should be split only after the smaller groundwork is done.

High-value targets:
- `ServerConsole`
- `MarketHandler`
- Shop-related handlers/controllers
- Large client console-style scripts

Checklist:
- [ ] Split by responsibility
- [ ] Preserve current behavior first
- [ ] Pull internals into modules, services, or controllers
- [ ] Keep each moved piece clearly owned

Done when:
- The biggest systems are no longer locked inside huge central scripts

### Phase 10: Move Logic Out of UI Trees

UI trees should hold UI. Business logic should move into client modules/controllers.

Checklist:
- [ ] Move shop logic into controllers/modules
- [ ] Move reward logic out of button scripts
- [ ] Keep templates in UI trees
- [ ] Use small binders only where needed

Done when:
- UI changes no longer require digging through deeply nested instance scripts

### Phase 11: Move Logic Out of Asset and World Trees

Models and maps should not quietly own important behavior.

Checklist:
- [ ] Review scripts in `Workspace`
- [ ] Review scripts in `ServerStorage`
- [ ] Review scripts in `ReplicatedStorage.Assets`
- [ ] Keep script-in-model only when it is intentional
- [ ] Move other behavior to services/controllers

Done when:
- Content folders stop acting like the main app layer

### Phase 12: Archive and Prune

Old content should be separated from live runtime content.

Checklist:
- [ ] Create an archive area
- [ ] Move old-generation systems out of live runtime paths
- [ ] Verify disabled scripts before deletion
- [ ] Record what was archived and why

Done when:
- The live runtime path contains the product, not old experiments

### Phase 13: Harden the Final Environment

Once the main move is done, add guardrails so the project does not drift back.

Checklist:
- [ ] Keep formatting and linting easy to run
- [ ] Add tests for pure modules and important logic
- [ ] Document startup flow
- [ ] Document basic rules for future work

Done when:
- The repo is not just organized once, but easier to keep organized
