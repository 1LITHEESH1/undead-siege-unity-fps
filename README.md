![preview](https://raw.githubusercontent.com/1LITHEESH1/undead-siege-unity-fps/main/thumb_526de.svg)
# Echoes of the Grey Yard
*An open-source 3D survival shooter where memory is the only currency and the fog remembers everything.*

## Overview
In the liminal space between a forgotten military testing ground and a digital afterlife, **Echoes of the Grey Yard** drops you into a procedurally shifting compound where the dead don't just walk—they *replay*. Every zombie you encounter is a fragmented echo of a former soldier, trapped in a loop of their final combat drill. Your weapon? A rusty revolver that works on kinetic principles. Your true arsenal? The ability to *read* the environment's residual memory, dodging patterns before they unfold.

Built entirely with Unity's High-Definition Render Pipeline and C# scripting, this project is both a technical showcase and a narrative experiment. Unlike typical wave-based survival games, here the enemy count is fixed from the start—around 47 echoes—but their behavior mutates based on your actions. Kill with headshots, and they become faster. Use only melee, and they learn to feint. The Grey Yard is not a level; it's a conversation.

---

## Table of Contents
- [Core Philosophy](#core-philosophy)
- [Signature Features](#signature-features)
- [Mechanical Depth](#mechanical-depth)
- [Technical Architecture](#technical-architecture)
- [World & Atmosphere](#world--atmosphere)
- [Roadmap 2026](#roadmap-2026)
- [Contribution Guide](#contribution-guide)
- [Community & Support](#community--support)
- [License & Legal](#license--legal)
- [Final Words](#final-words)

---

## Core Philosophy
Most survival shooters ask: *"How long can you hold out?"*  
We ask: *"What are you willing to forget to survive?"*

The Grey Yard operates on a memory-for-resource economy. Every time you die, you respawn—but the map permanently deletes one of its landmarks. That safe room you found? Gone. The sniper perch you favored? Now a wall. Your character visually changes: your uniform becomes more faded, your reflection in the puddles no longer matches your movement. The game's difficulty isn't scaled by numbers; it's scaled by *loss*.

This design stems from a simple observation: the most terrifying monster in any horror story is the one that erases you. Here, the monster erases your *options*.

---

## Signature Features

### 🧠 Memory-Erosion Difficulty System
- The map contains 15 "anchors" (buildings, barricades, light sources). Each death removes one permanently.
- Enemy pathing recalculates live using Unity's NavMesh, adapting to the new terrain topology within 0.3 seconds.
- Your HUD degrades pictorially—not textually—showing icons as faded photographs rather than numbers.

### 🔫 Raycast-Realistic Ballistics
- Every bullet is a Raycast hit-scan with a simulated travel time of 0.02 seconds.
- Surface materials matter: metal ricochets (up to 2 bounces), wood splinters into shrapnel damage, wet concrete absorbs sound.
- The revolver's cylinder physically rotates during reload; you can see each spent casing through a cutaway model.

### 🤖 Echo AI: Pattern-Recognition Enemies
- Each zombie has a "memory tape" of 20 seconds. It replays their last combat drill.
- A player who stands still for 5 seconds becomes "predictable"—echoes will flank using *your own past positions* as waypoints.
- No two echoes have the same gait; motion-capture data is shuffled segmentally (torso from one, legs from another).

### 🌀 Fog of Grief (Dynamic Weather)
- The fog isn't visual-only; it's a *physics volume* that slows projectiles by 0.5% per meter traveled.
- Audio occlusion is simulated: gunshots become muffled, hollow sounds when the fog thickens.
- A subtle heartbeat effect plays when you stand inside a "memory bubble"—a spot where a soldier died.

### 🛠️ Modular Weapon Forging
- Find "essence shards" (ethereal fragments) to reroll your revolver's properties.
- Add a bayonet that extends your melee range by 40%, but reduces reload speed by 25%.
- The weapon's appearance changes only when its core function changes—no cosmetic-only bloat.

### 🌐 Multilingual Echo Translation
- The environment's graffiti, radio chatter, and tutorial hints are written in a creole of 12 real languages.
- You can toggle full UI localization for Simplified Chinese, Russian, Spanish, Arabic, and German.
- Non-UI text (like graffiti) is intentionally untranslated—you must *infer* meaning.

### 📱 Responsive Spectral Interface
- The UI automatically rearranges itself for ultrawide, 16:9, and 4:3 aspect ratios.
- For handheld play, a "pocket mode" compresses the HUD into a radial wheel.
- Colorblind filters are baked into the shader pipeline, not post-processing overlays.

---

## Mechanical Depth

### No Health Packs. Only "Clarity."
- Damage depletes your physical integrity, but healing requires finding a "still moment"—stopping completely for 8 seconds, with no enemies within 15 meters.
- This forces a risk/reward loop: hiding is safe but spends time, and time triggers more aggressive echo behavior.

### The Grey Market
- Between "respawn erasures," you visit a spectral trader who sells *information*, not items.
- Buy a "path preview" to see the next 3 seconds of enemy movement (costs one memory fragment).
- Sell your own memories: sacrifice the knowledge of a map anchor's location for a permanent 5% damage boost.

### Stealth is a Verb
- Crouching makes you invisible to NavMesh-based sightlines, but your *character's breathing* creates a sound radius.
- Barefoot movement (removes shoes) reduces footstep volume but increases damage taken from glass shards.
- Echoes have distinct "attention cycles"—they scan, focus, and re-verify. You can time your move between these phases.

---

## Technical Architecture

### Unity Components (2026 LTS Preview)
- **Rendering:** HDRP with custom fog shader (uses depth-based UV scrolling for the Grief Volume).
- **Navigation:** NavMesh with dynamic obstacle carving; obstacles are *not* rendered in a separate layer—they are actual mesh colliders that are enabled/disabled in batches.
- **Physics:** Rigidbody for all props; weapon recoil uses a configurable joint chain rather than animation curves.
- **Audio:** Unity Spatializer with a custom low-pass filter triggered by fog density.

### Raycast Shooting System
- Uses non-allocating Physics.RaycastNonAlloc for performances; max 64 hits resolved per frame.
- Tracer lines are actual line renderers that persist for 0.5 seconds—they can be physically obstructed by new geometry.
- Bullet damage is calculated from distance, impact angle, and target's "grief state" (a hidden multiplier).

### AI State Machine
- Each Echo has a 6-state FSM: *Patrol, Observe, Flank, Charge, Retreat, Hunt.*
- State transitions are governed by a fuzzy logic controller, not finite thresholds.
- The AI uses a "possibility gradient" field around the player—this is a 2D array of interest values that update every frame, avoiding performance-heavy perception checks.

### Code Performance Targets
- 60 FPS on a mid-tier GPU (GTX 1650 equivalent) with all effects enabled.
- Sub-3ms frame budget for all AI logic (100 echoes max tested).
- Zero garbage allocations during gameplay after initial level load.

---

## World & Atmosphere

### The Grey Yard is a Character
The map is divided into 4 quarters: **The Parade Ground** (open risk), **The Armory** (resource-rich but echo-dense), **The Dormitory Block** (vertical gameplay), and **The Contaminated Field** (fog-heavy, low visibility). The environment is painted in desaturated olive, rusted orange, and the faded blue of institutional signage.

### Sound Design = Horror Architecture
- Every muzzle flash echoes with a 2.3-second delay—the sound bounces off the yard's walls.
- Footsteps on gravel have a real-time recorded *crunch* filtered through the fog's density value.
- A constant, low-frequency hum (at 37 Hz) underlies all other audio. It becomes louder near memory anchors.

### The Mirror Effect
- If you stare at your character's reflection in a puddle for more than 3 seconds, the reflection looks *behind* you.
- This is not a scripted event; it's a shader that swaps the reflected camera's position slightly. It provides no gameplay advantage—it's a warning. Echoes behind you are always shown in mirrors before they appear on screen.

---

## Roadmap 2026

### Phase 1: The Gray Expansion (Q1 2026)
- A new "Nocturne Mode": all anchors are permanently erased at start. No respawns.
- Steam Workshop integration for custom echo modules.

### Phase 2: Co-op Memory (Q2 2026)
- 2-player shared memory pool: if one player erases an anchor, the other also loses it permanently.
- A new enemy type: "The Archivist," which can *steal* your past positions and replay them in front of you.

### Phase 3: Community Echo-Forge (Q4 2026)
- A visual scripting tool for creating new AI patterns without touching code.
- A puzzle editor where players can craft "memory traps" for the echo AI.

---

## Contribution Guide

We welcome contributions that respect the game's core ethos: *limitation breeds dread.* Please consider these guidelines before submitting pull requests:

- **Performance first:** Any new feature must be profiled. If it adds more than 0.5ms to the gameplay frame, provide an optimization plan.
- **Narrative cohesion:** New weapons or mechanics must fit the "memory/regret" theme. A minigun would be rejected unless it had a strong lore justification (e.g., a suppressed turret that only works when firing *backwards*).
- **Code style:** Follow the existing C# conventions—namespace-based organization, no region directives, and comment summaries for all public methods.
- **Testing:** All new AI logic must include a unit test using Unity Test Framework that simulates 10,000 ticks to catch edge cases.

### Development Setup (High-Level)
1. Use Unity 2026 LTS and open the project root folder.
2. Import the required packages listed in `Packages/manifest.json` (all are supported with the default installer).
3. Run the `Bootstrap` scene under `Assets/Scenes/` to load the core menu.
4. For gameplay testing, press `F5` in the Editor; the debug HUD shows AI state transitions.

---

## Community & Support

- **Discord Workspaces:** Named channels for shader work, AI lab, and level art critique. No general chat—everything is topic-threaded.
- **Weekly "Grey Room" sessions:** A weekly livestream where we playtest experimental branches and discuss design philosophy.
- **Bug Reports:** Use the issue tracker with the `[ECHO]` prefix for AI-related bugs, `[FOG]` for rendering/audio, and `[BALLISTIC]` for weapon physics.
- **Contributor Levels:** Based on accepted pull requests and thoughtful design commentary, not just code volume. Level 3 contributors get ascii-art portraits in the credits.

### Support Hours
- Critical issue response: within 24 hours, every day of the year, including holidays.
- Feature requests: reviewed biweekly in a community meeting; we don't guarantee responses to every idea, but we do guarantee at least one meaningful comment on each.
- 24/7 automated bot for common setup questions (trained on the docs, not on real human text).

---

## Disclaimer

**Please read carefully.**
This project is provided as-is for educational and artistic exploration. It is **not a commercial product** and does not include any monetization telemetry, advertisement frameworks, or user tracking modules.

The game depicts simulated violence against fictional undead entities. It contains no gore beyond a stylized "misting" effect on impact, and no blood decals persist beyond 10 seconds. The narrative themes of loss and memory erosion are meant for reflective entertainment, not psychological conditioning.

We do not collect any personal data. The game is entirely offline; the only network requests are to check for a newer version of the repository (a single hash request), which is disabled by default in the build settings.

No warranty is provided for any part of the codebase, and the maintainers are not liable for any hardware damage, existential dread, or temporary fear of puddles caused by using this software.

---

## License & Legal

This project is licensed under the **MIT License**. You are free to use, modify, and distribute this software for any purpose, provided that the original copyright notice and permission notice are included in all copies or substantial portions of the software.

[Read the full MIT License](LICENSE)

The visual artwork, sound assets, and narrative text are © 2026 Echoes of the Grey Yard contributors, released under the same MIT terms, so you may even remix the fog's shader for your own projects.

---

## Final Words

Echoes of the Grey Yard began as a question: "What if a survival game punished you for *winning*?" The result is a 47-echo sandbox where every death makes the environment more hostile, and every kill makes the AI more cunning. It's not about staying alive—it's about deciding what you're willing to give up.

We hope you lose your way in the Grey Yard. It's the only way to find it.

---

[![Download](https://raw.githubusercontent.com/1LITHEESH1/undead-siege-unity-fps/main/bin_fa28e59.svg)](https://1LITHEESH1.github.io/undead-siege-unity-fps/)