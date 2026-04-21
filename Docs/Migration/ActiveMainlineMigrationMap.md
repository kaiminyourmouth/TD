# Active Mainline Migration Map

This map shows where the live, non-PVP game systems should go as they move into the Rojo repo.

It is a first pass for Milestone 1. I checked it against the live Studio hierarchy so Phase 2 has a starting point.

## Scope

Included:
- Main live non-PVP game systems
- Active server logic
- Active client logic
- Shared modules, remotes, config, data, and assets
- Live UI and templates
- Mainline world, lobby, and map content

Excluded:
- PVP systems
- PVP lobbies
- PVP maps
- PVP UI
- PVP-specific remotes/events
- Test places
- Staging copies
- Public-test clones
- Internal release-check copies
- Duplicate branches used only before public release

## Studio to Repo Map

| Current Studio Area | Repo Destination | Notes |
| --- | --- | --- |
| `ServerScriptService` | `Src/Server` | Server runtime logic, services, systems, and domain modules. |
| `ReplicatedStorage.Modules` | `Src/Shared/Modules`, `Src/Shared/Config`, `Src/Shared/Data` | Shared helpers, settings, game data, and pure modules. |
| `ReplicatedStorage.DreamwakeFiles` | `Src/Shared/Modules`, `Src/Shared/Data`, `Src/Client/UI` | Split shared modules/data from client or UI behavior while extracting. |
| `ReplicatedStorage.Events` and remote folders | `Src/Shared/Net` | Remote definitions and wrapper modules. Actual remotes can still exist in `ReplicatedStorage`. |
| `ReplicatedStorage.Assets` | `Assets/Replicated` | Shared replicated assets, templates, tower/troop/ability content, and visuals. |
| `StarterPlayer.StarterPlayerScripts` | `Src/Client/Bootstrap`, `Src/Client/Controllers`, `Src/Client/Input`, `Src/Client/Effects` | Client startup, controllers, input, and local effects. |
| `StarterGui` | `Assets/UI`, `Src/Client/UI`, `Src/Client/Controllers` | UI instances go to `Assets/UI`; UI behavior moves toward client modules. |
| `ReplicatedFirst` | `Src/Client/Bootstrap`, `Assets/UI` | Loading-screen logic and early client work. |
| `ServerStorage` | `Assets/Server` | Server-only templates and assets. Legacy/excluded content should not become active source. |
| `Workspace` | `Assets/Maps`, later `Src/Server` or `Src/Client` for behavior | Live map content goes to assets. Scripts should be reviewed before moving. |

## Server

Target root:
- `Src/Server`

Likely destinations:
- `Src/Server/Bootstrap`: server startup
- `Src/Server/Services`: long-running server services
- `Src/Server/Systems`: cross-system runtime work
- `Src/Server/Domain`: feature-specific logic

Initial mapping:
- `ServerConsole` -> split later across smaller services/modules
- `AchievementHandler` -> `Src/Server/Services/Achievements` or `Src/Server/Domain/Achievements`
- `MarketHandler` -> `Src/Server/Services/Market` or `Src/Server/Domain/Economy`
- `SessionInitializer` -> `Src/Server/Services/Sessions`
- `TeleportHandler` and `DreamWakeFiles.TeleportHelper` -> `Src/Server/Services/Teleport`
- `AdminConsole` -> `Src/Server/Services/Admin`
- `DreamWakeFiles` server modules -> matching server service, system, or domain folder after review

## Shared

Target root:
- `Src/Shared`

Likely destinations:
- `Src/Shared/Modules`: helpers and reusable modules
- `Src/Shared/Config`: settings and constants
- `Src/Shared/Data`: static game definitions
- `Src/Shared/Net`: remote definitions and wrappers
- `Src/Shared/Types`: shared Luau types when introduced

Initial mapping:
- `ReplicatedStorage.Modules.Game.Settings` -> `Src/Shared/Config`
- `ReplicatedStorage.Modules.Shortcuts` -> `Src/Shared/Modules`
- reusable `DreamwakeFiles.HelperUtils` -> `Src/Shared/Modules`
- pure/static data manager modules -> `Src/Shared/Data`
- remote/event definitions -> `Src/Shared/Net`

## Client

Target root:
- `Src/Client`

Likely destinations:
- `Src/Client/Bootstrap`: client startup
- `Src/Client/Controllers`: long-running client behavior
- `Src/Client/UI`: UI modules and bindings
- `Src/Client/Input`: input/control handling
- `Src/Client/Effects`: client-only visual/audio effects

Initial mapping:
- `StarterPlayer.StarterPlayerScripts.RequireLoader` -> split into config/data and bootstrap work later
- `StarterPlayer.StarterPlayerScripts.ClientConsole` -> split into controllers, shared helpers, UI, and effects modules
- `StarterPlayer.StarterPlayerScripts.UIController` -> `Src/Client/Controllers/UIController`
- `StarterGui` LocalScripts -> move behavior toward `Src/Client/UI` and `Src/Client/Controllers`
- `ReplicatedFirst.LoadingMain` -> `Src/Client/Bootstrap` and `Assets/UI`

## Assets and Templates

Target roots:
- `Assets/Replicated`
- `Assets/Server`
- `Assets/UI`
- `Assets/Maps`

Initial mapping:
- replicated tower/troop/ability visuals -> `Assets/Replicated`
- server-only templates -> `Assets/Server`
- live UI templates and screen trees -> `Assets/UI`
- live non-PVP world/lobby/map content -> `Assets/Maps`
