# First Extraction Priority List

After Milestone 1, the first code move should be small and easy to verify.

I do not want to start with shops, purchases, player data, teleporting, large UI trees, or anything that could break live progression. The first extraction should prove that the Rojo workflow is working before larger systems move.

## First Target

Start with:
- `ReplicatedStorage.Modules.Shortcuts`

Move to:
- `Src/Shared/Modules/Shortcuts`

Known modules:
- `Instances`
- `Maths`
- `Solvers`

Why this is first:
- It is already grouped like shared utility code
- It is smaller than the main runtime scripts
- It should be easy to review after moving
- It can test Rojo paths and require paths without touching major gameplay behavior

How to confirm:
- Rojo sync runs cleanly
- Studio opens without new require errors
- Scripts that use these helpers still resolve them
- No new output warnings appear for this module group

## Next Safe Targets

Move these only after the first target is confirmed:

1. `ReplicatedStorage.Modules.Game.Settings`
   - Destination: `Src/Shared/Config/GameSettings`
   - Reason: this looks like shared configuration and should be easier to review in source.

2. `ReplicatedStorage.Modules.Game.SettingsForGame`
   - Destination: `Src/Shared/Config/RuntimeGameSettings`
   - Reason: likely runtime settings, but it should be read before final naming.

3. `ReplicatedStorage.DreamwakeFiles.HelperUtils`
   - Destination: `Src/Shared/Modules/HelperUtils`
   - Reason: utility modules are good early candidates, but the folder should be checked before moving everything.

4. Small static data modules from `ReplicatedStorage.DreamwakeFiles`
   - Destination: `Src/Shared/Data`
   - Reason: pure data is safer to move than active runtime logic.

## I Will Not Start With

These should wait:
- `ServerConsole`
- `MarketHandler`
- `ClientConsole`
- large shop handlers/controllers
- `StarterGui` UI trees
- `Workspace` scripts
- `ServerStorage` legacy UI or old lobby folders
- asset-bound scripts inside tower, troop, or ability content
- anything PVP or RPVP related

Why they wait:
- They are more likely to depend on runtime state
- They are more likely to depend on `_G`
- They are harder to test quickly
- They have a higher chance of causing regressions

## Success Check

The first extraction is successful when:
- one small shared module group lives in `Src/Shared`
- Studio still loads cleanly
- require paths still work
- no gameplay behavior has intentionally changed
- I am comfortable using the same process for the next small target
