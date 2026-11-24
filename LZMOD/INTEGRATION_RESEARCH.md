# LZ MOD — Integration Research

**Reviewed:** 2026-11-24  
**Status:** Runtime gameplay integration is not verified.

## Official sources reviewed

1. Official ModKit repository: https://github.com/Shine-Publishing/Rooftops-and-Alleys-Modkit
2. Official Modding User Guide: https://github.com/Shine-Publishing/Rooftops-and-Alleys-Modkit/blob/main/Documentation/ModdingUserGuide.md
3. Game update announcement, “#8 - Full Modding Support!” (10 November 2026): https://steamcommunity.com/app/2703850/announcements/

## What the official guide confirms

The guide documents these supported mod content types:
- Level-editor items.
- Character body parts.
- Pigeon model and animation replacements.
- Custom Unreal levels.

It requires Unreal Engine 5.4 and ModKit 1.0.0. It explains creating a mod package, metadata, the four corresponding Data Asset types, and packaging for distribution.

The guide also says custom mod levels are sanitized to prevent execution of outside code and recommends avoiding Blueprint Classes inside mod levels. This statement is specifically about mod levels; it should not be generalized into a claim that every possible plugin/runtime extension is forbidden.

## What remains unconfirmed for LZLOVE

The reviewed public guide does not document:
- A supported runtime gameplay plugin entry point for arbitrary player logic.
- Landing/fall impact callbacks with impact severity.
- Collision callbacks suitable for gameplay state changes.
- A supported API to lock and restore player input.
- A supported API to request a lying/knockout player animation.
- Camera post-process controls for temporary blur and darkening.
- Registration of custom settings tabs or persistent user settings.
- Authoritative multiplayer state replication for a custom gameplay mechanic.

Absence from this guide is not proof that no private, experimental, or separately documented API exists. It means LZ MOD must not assume these interfaces exist.

## Current conclusion

The official modding announcement describes custom maps, outfits, and pigeon skins, and points creators to the ModKit. The public guide remains content-oriented. No verified public interface has been found that can implement LZLOVE's gameplay behavior.

**Decision:** Keep LZLOVE as a documented feature specification. Do not create guessed C++/Blueprint calls, inject code into the game, or alter packaged game files.

## Evidence needed to pass the integration gate

A developer/maintainer response or official technical document must identify:
1. The supported mechanism for adding runtime gameplay logic.
2. The supported player/physics event interfaces.
3. The supported input, animation, camera, and settings interfaces.
4. Whether the mechanism works in multiplayer and how authority/replication are handled.

Once that evidence is available, update the architecture with exact API names and build a minimal test extension before implementing the complete feature.
