# LZ MOD — Runtime Architecture

**Document status:** Planning scaffold only. This is not an executable Unreal project or a working game plugin.

## 1. Repository boundary

The existing repository is a snapshot of installed Rooftops & Alleys files and settings. Keep all original game files and packaged assets untouched.

The LZ MOD work area is isolated under `LZMOD/`. Do not edit, unpack-and-replace, or overwrite the game's `.pak`, `.ucas`, or `.utoc` files as an implementation strategy.

## 2. Target runtime feature

### LZLOVE — Knockout

| Property | Required behavior |
|---|---|
| Causes | Strong landing/fall impact and strong collision |
| Default duration | 10 seconds |
| Configurable duration | 5–60 seconds |
| Enable setting | On / Off |
| Character state | Lying/knockout presentation |
| Input | Disabled while unconscious |
| Screen | Darkening + blur |
| Recovery | Restore movement, input, and camera effects |
| Re-entry protection | Ignore triggers while unconscious and during a short recovery grace period |

When the feature is Off, the feature must not change movement, animation, input, or camera behavior.

## 3. Runtime modules — proposed boundaries

These are logical responsibilities, not confirmed ModKit classes or API names.

- **Impact Evaluation** — receives supported landing/collision events, evaluates impact severity, and rejects ordinary movement.
- **Knockout State** — owns the unconscious state, duration, duplicate-trigger guard, recovery, and grace period.
- **Character Presentation** — requests a supported lying animation and restores the previous movement presentation.
- **Camera Effects** — owns darkening and blur, including cleanup on recovery, respawn, pause, and teardown.
- **LZLOVE Settings** — owns the enable toggle and duration value, validates 5–60 seconds, and persists through a supported configuration mechanism.
- **Multiplayer State** — only added if the official extension API provides authoritative server-side state and replication.

No Unreal class, event, delegate, or settings API should be implemented by guessing its name. Bind these modules only after the game's supported gameplay extension interfaces are verified.

## 4. State model

Proposed state transitions:

`Normal → Unconscious → Recovering → Normal`

Rules:
1. A trigger is accepted only when the feature is enabled, the player is in Normal, and the recovery grace period has elapsed.
2. Entering Unconscious captures the state needed for safe restoration and starts the configured duration.
3. While Unconscious, player input remains blocked and the knockout presentation/effects are active.
4. Recovery restores only the state owned by LZ MOD; it must not overwrite unrelated game state.
5. Respawn, pause, level teardown, or plugin shutdown must run cleanup and clear camera effects/input locks.
6. A failed or unavailable animation must not leave the player permanently input-locked.

## 5. Settings contract

Settings category: **LZLOVE**

- **Unconsciousness:** Boolean, default On.
- **Recovery duration:** Integer/slider, minimum 5, maximum 60, default 10 seconds.
- Duration is ignored while the feature is Off.
- Invalid persisted values are clamped to the supported range.
- Settings persistence must use a verified game/mod configuration API; no invented config calls.

## 6. Multiplayer requirements

If the supported API exposes a server-authoritative gameplay layer:
- The server decides whether an impact qualifies and starts the knockout timer.
- The state and remaining duration are replicated to relevant clients.
- Clients render the character and camera presentation but cannot independently grant or cancel the authoritative state.

If no such API exists, multiplayer support remains out of scope rather than being simulated locally.

## 7. Integration gate — blocking requirement

Before writing runtime code, verify all of the following against official documentation or developer-provided source/API:

- A supported runtime extension/plugin entry point.
- Access to landing/fall impact data and collision impact data.
- A supported way to disable and restore player input.
- A supported lying/knockout animation or character state transition.
- Camera post-process access for darkening and blur.
- A supported custom settings category and persistent configuration API.
- Multiplayer authority/replication interfaces, if multiplayer is required.

The currently reviewed official ModKit documentation describes content authoring for editor items, body parts, pigeon definitions, and levels. It does not establish the runtime interfaces listed above. Therefore the integration gate is **not yet passed**.

## 8. Safe implementation sequence

1. Obtain confirmation of the supported gameplay extension mechanism from the developer or official ModKit maintainers.
2. Create a clean Unreal Engine 5.4 project from the official ModKit template, outside the installed game directory.
3. Verify the extension entry point with a minimal no-op test before adding gameplay behavior.
4. Implement and test impact evaluation independently.
5. Implement knockout/recovery and cleanup.
6. Add character and camera presentation.
7. Add LZLOVE settings and persistence.
8. Add multiplayer replication only after the authoritative API is verified.
9. Package and test without modifying the original game installation.

## 9. Current status

- Design: defined.
- Runtime API availability: unverified / blocking.
- Executable plugin: not created.
- Original game files: must remain unchanged.
