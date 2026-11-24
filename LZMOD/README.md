# LZ MOD — Project plan

## First feature: LZLOVE / Knockout system

**Status:** Design specification only — not implemented.

### Player-facing behavior
- Detect a sufficiently strong in-game landing or collision.
- Trigger a short, non-graphic unconscious state only when the configured impact threshold is exceeded.
- Default unconscious duration: **10 seconds**.
- Recovery duration is configurable from **5 to 60 seconds**.
- The feature can be switched off entirely.
- While unconscious, the character remains in a lying/disabled state; normal control returns after the timer expires.
- Avoid repeated triggers while already unconscious and add a short recovery grace period to prevent immediate retriggering.

### LZLOVE settings tab
- Add a settings category named **LZLOVE**.
- Setting: **Unconsciousness** — On / Off.
- Setting: **Recovery duration** — slider from 5 to 60 seconds; default 10.
- If the feature is Off, impact detection must not change player movement or state.
- Save settings using the game's supported settings/configuration API.

### Suggested implementation components (requires an approved runtime gameplay API)
1. Impact detection component: consume the game's collision/landing events and read impact severity supplied by the game.
2. Knockout state component: enter/exit the state, guard against duplicate triggers, and manage the timer.
3. Character presentation: use a supported lying/knockout animation and a restrained screen effect.
4. Settings UI: register the LZLOVE category and persist the two settings.
5. Multiplayer: if supported by the game API, replicate the state and timer authoritatively so all clients see the same result.

## Compatibility findings

The repository currently contains a copy of installed game files and settings. It does not contain an Unreal project file, a mod plugin descriptor, source code, or a gameplay scripting project.

The official Rooftops & Alleys ModKit documentation currently describes four supported content types: level-editor objects, character body parts, pigeon model/animation replacements, and custom Unreal levels.

It does not document a supported API for runtime player-state logic, collision gameplay hooks, or custom settings tabs. Therefore the requested knockout mechanic and LZLOVE menu cannot honestly be implemented as a working official ModKit mod using the files currently in this repository.

## Next technical requirement

Before implementing runtime behavior, obtain an official/supported gameplay extension API or source project from the game developer. Once that exists, implement the components above against its real interfaces. Do not patch or overwrite the packaged game binaries as a substitute.