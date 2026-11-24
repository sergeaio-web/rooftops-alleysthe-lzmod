# LZ MOD — Project plan

## First feature: LZLOVE / Knockout system

**Status:** Design specification — runtime implementation is blocked pending a supported gameplay extension API.

### Confirmed design
- Triggers: both sufficiently strong falls/landings and sufficiently strong collisions.
- Default unconscious duration: **10 seconds**.
- Configurable duration: **5–60 seconds**.
- The feature can be switched off entirely.
- While unconscious, the character lies down and player input is disabled.
- Presentation: screen darkening and blur while unconscious; restore the normal view on recovery.
- Do not trigger again while already unconscious; add a short recovery grace period.

### LZLOVE settings tab
- Add a settings category named **LZLOVE**.
- Setting: **Unconsciousness** — On / Off.
- Setting: **Recovery duration** — slider from 5 to 60 seconds; default 10.
- Settings must persist using the game's supported configuration API.
- If Unconsciousness is Off, impact detection must not alter player movement, animation, or camera.

### Runtime implementation plan (requires supported game hooks)
1. **Impact detection:** consume actual landing and collision events; use game-provided impact severity and separate thresholds to avoid triggering on ordinary movement.
2. **Knockout state:** enter once, disable player input, start the configured timer, then restore input; prevent duplicate triggers and immediate retriggering.
3. **Character presentation:** play a supported lying/knockout animation and return safely to the normal movement state.
4. **Camera presentation:** apply darkening and blur only for the unconscious duration; reliably clear both effects on recovery, respawn, pause, and teardown.
5. **Settings UI:** register LZLOVE and persist the enable toggle and 5–60 second duration.
6. **Multiplayer:** if supported by the game's extension API, replicate the state and remaining time authoritatively so other players see the same state.

## Compatibility findings

The repository currently contains a copy of installed game files and settings. It does not contain an Unreal project file, a mod plugin descriptor, source code, or a gameplay scripting project.

The official Rooftops & Alleys ModKit documentation describes four supported content types: level-editor objects, character body parts, pigeon model/animation replacements, and custom Unreal levels.

It does not document a supported API for runtime player-state logic, collision gameplay hooks, input blocking, camera effects, or custom settings tabs. The requested feature therefore cannot yet be implemented as a working official ModKit mod from the files currently in this repository.

## Next technical requirement

Obtain an official/supported gameplay extension API or source project from the game developer. Then implement the runtime behavior against its real interfaces. Do not patch or overwrite packaged game binaries as a substitute.