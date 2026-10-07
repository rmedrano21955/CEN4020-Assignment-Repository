# Snake Clone: Requirements Backlog

**Primary goal:** Build a snake clone with added features.

**Status:** Elicitation pass 2 (revised). Platform is desktop. Reference game is Google Snake. Prototype is the target scope.

**Revision summary:** Mouse controls were removed. Shooting is a keyboard-only feature. Local multiplayer on the same computer is now in scope (this reverses the earlier "solo only" decision).

### Tagging scheme

| Tag | Values |
|---|---|
| **Type** | Goal, Constraint, Functional (F), Non-functional (NF), Enabler (E), Spike (S) |
| **Priority** | MoSCoW: Must, Should, Could, Won't |
| **Source** | Stated (said by owner), Confirmed (owner approved), Inferred (implied by "snake clone"), Assumed (needs validation) |
| **Effort** | S (hours), M (a day or two), L (several days) |
| **Depends on** | IDs that must exist or be decided first |

### Suggested GitHub labels

`type:functional`, `type:nfr`, `type:enabler`, `type:spike`, `prio:must`, `prio:should`, `prio:could`, `prio:wont`, `effort:S`, `effort:M`, `effort:L`, `source:assumed`, `area:core`, `area:feel`, `area:fun`, `area:shooting`, `area:multiplayer`, `area:meta`, `area:ui`, `area:tech`

## Decision log

| ID | Decision | Date/Pass | Notes |
|---|---|---|---|
| DEC-01 | Platform is desktop. | Pass 2 | Browser vs. native still open (DISC-01). |
| DEC-02 | Google Snake is the quality and feature reference. | Pass 2 | |
| DEC-03 | Fun driver is high-score chasing. | Pass 2 | Drives META-01 to Must. |
| DEC-04 | Done means a playable prototype. | Pass 2 | See "Prototype definition". |
| DEC-05 | Add a shooting feature. | Pass 3 | |
| DEC-06 | Local multiplayer on the same computer is in scope. | Pass 3 | Reverses the earlier "solo only" answer. Replaces former OOS-03. |

### Out of scope

| ID | Excluded | Reason |
|---|---|---|
| OOS-01 | Unlockable skins or themes | Owner decision |
| OOS-02 | Online leaderboards or accounts | Owner decision |
| OOS-03 | Mouse controls (aiming, steering, or menus) | Owner decision |
| OOS-04 | Online or networked multiplayer | Local same-computer only |
| OOS-05 | AI-controlled snakes or bots | Not requested |
| OOS-06 | Gamepad and touch input | Desktop, keyboard only |

## Goals and constraints

| ID | Requirement | Type | Priority | Source | Effort | Depends on |
|---|---|---|---|---|---|---|
| GOAL-01 | Success is judged by the owner's own enjoyment, not market appeal. | Goal | Must | Stated | – | – |
| CON-01 | The game remains recognizably a snake clone (grid, growing snake, food). Shooting and multiplayer are variations inside that frame. | Constraint | Must | Stated | – | – |
| CON-02 | Personal, single-developer project. Scope stays small and shippable in increments. | Constraint | Must | Inferred | – | – |
| CON-03 | All input is keyboard only. | Constraint | Must | Stated | – | – |

## Core gameplay

| ID | Requirement | Type | Priority | Source | Effort | Depends on |
|---|---|---|---|---|---|---|
| CORE-01 | Render a grid-based play field. | F | Must | Confirmed | S | TECH-01 |
| CORE-02 | Snake advances one cell per game tick in its current direction. | F | Must | Confirmed | S | CORE-01, TECH-02 |
| CORE-03 | Player changes direction via keyboard. A direct 180° reversal is disallowed. | F | Must | Confirmed | S | CORE-02, TECH-03 |
| CORE-04 | Food spawns on a random empty cell. | F | Must | Confirmed | S | CORE-01 |
| CORE-05 | Eating food grows the snake and increases score. | F | Must | Confirmed | S | CORE-02, CORE-04 |
| CORE-06 | Colliding with the snake's own body ends that snake's run. | F | Must | Confirmed | S | CORE-02 |
| CORE-07 | Colliding with a wall ends that snake's run (default behavior). | F | Must | Confirmed | S | CORE-02 |
| CORE-08 | Game states: ready → playing → paused → game over, with quick restart. | F | Must | Confirmed | S | CORE-06 |
| CORE-09 | Input buffering: queue 1 to 2 direction inputs per tick so fast turns aren't dropped. | F | Must | Confirmed | S | CORE-03 |

## Game feel

| ID | Requirement | Type | Priority | Source | Effort | Depends on |
|---|---|---|---|---|---|---|
| FEEL-01 | Input response feels immediate (no perceptible lag between keypress and turn). | NF | Must | Confirmed | S | CORE-03, TECH-02 |
| FEEL-02 | Sound effects for eating, dying, and starting. | F | Should | Confirmed | S | CORE-05, CORE-06 |
| FEEL-03 | Visual feedback ("juice"): eat pulse, death effect, light screen shake. | F | Should | Confirmed | M | CORE-05, CORE-06 |
| FEEL-04 | Smooth interpolated movement between cells (snake and projectiles). | F | Should | Confirmed | M | CORE-02, TECH-02 |
| FEEL-05 | Background music with a mute toggle. | F | Could | Confirmed | S | UI-02 |

## Fun and variety

| ID | Requirement | Type | Priority | Source | Effort | Depends on |
|---|---|---|---|---|---|---|
| FUN-01 | Speed increases as score or length grows. | F | Should | Confirmed | S | CORE-05 |
| FUN-02 | All gameplay tuning values (speed, growth per food, grid size, shooting and multiplayer settings) live in a config, not in code. | E | Should | Confirmed | S | CORE-02 |
| FUN-03 | Alternate wall behavior: wrap-around, selectable as an option. | F | Should | Confirmed | S | CORE-07, FUN-02 |
| FUN-04 | Food variety: bonus food that expires, slow-down food, shrink food. | F | Could | Confirmed | M | CORE-05, FUN-02 |
| FUN-05 | Temporary power-ups (ghost through self, score multiplier, rapid fire). | F | Could | Confirmed | M | FUN-04 |
| FUN-06 | Static obstacles or preset level layouts (indestructible). | F | Could | Confirmed | M | CORE-01, CORE-07 |
| FUN-07 | Multiple game modes (Classic, Wrap, Obstacles, Time Attack). | F | Could | Confirmed | M | FUN-03, FUN-06, UI-02 |
| FUN-08 | Selectable difficulty presets. | F | Could | Confirmed | S | FUN-01, FUN-02 |
| FUN-09 | More than one food item on the field at once (configurable count). | F | Could | Confirmed | S | CORE-04, FUN-02 |

## Shooting (keyboard only)

Shots are fired in the direction the snake's head is facing. There is no free aiming.

| ID | Requirement | Type | Priority | Source | Effort | Depends on |
|---|---|---|---|---|---|---|
| SHOOT-01 | A dedicated fire key launches a projectile from the head, in the head's facing direction. | F | Must | Stated | S | CORE-02, TECH-03 |
| SHOOT-02 | Projectile travels in a straight line, faster than the snake, until it hits something or leaves the field. | F | Must | Stated | M | SHOOT-01, TECH-02 |
| SHOOT-03 | Shooting is limited so it can't be spammed. Mechanism is open (cooldown, ammo, or length cost). Default assumption: a short cooldown plus a small length cost. | F | Must | Stated | S | SHOOT-01, SPIKE-02 |
| SHOOT-04 | A shot that hits the opponent snake has an effect (see SPIKE-03 for the exact rule). | F | Must | Stated | M | SHOOT-02, MP-01, SPIKE-03 |
| SHOOT-05 | A snake's own projectiles never harm its own snake. | F | Should | Stated | S | SHOOT-02 |
| SHOOT-06 | Projectiles do not destroy food. | F | Should | Stated | S | SHOOT-02 |
| SHOOT-07 | Sound and visual feedback for firing and hits (extends FEEL-02 and FEEL-03). | F | Should | Stated | S | SHOOT-02, FEEL-02, FEEL-03 |
| SHOOT-08 | Shooting parameters (projectile speed, cooldown, length cost, damage) live in the config. | E | Should | Stated | S | FUN-02, SHOOT-03 |
| SHOOT-09 | Shooting can be switched off for a pure-classic mode. | F | Could | Stated | S | SHOOT-01, FUN-07 |
| TARGET-01 | Solo mode: destructible obstacles spawn on empty cells and are removed when hit, awarding score. Gives the solo player something to shoot. | F | Should | Stated | M | SHOOT-02, CORE-04 |
| TARGET-02 | Moving or hostile targets. Conflicts with OOS-05 unless that is revisited. | F | Won't | Stated | L | TARGET-01 |

## Local multiplayer (same computer)

| ID | Requirement | Type | Priority | Source | Effort | Depends on |
|---|---|---|---|---|---|---|
| MP-01 | Two snakes share one play field and play simultaneously on one computer. | F | Must | Stated | M | CORE-02, TECH-03, TECH-08 |
| MP-02 | Each player has a separate keyboard layout: P1 on arrow keys with a fire key, P2 on WASD with a fire key. Exact keys TBD. | F | Must | Assumed | S | MP-01, TECH-03, SPIKE-04 |
| MP-03 | Mode select lets you start 1-player or 2-player. | F | Must | Assumed | S | MP-01, CORE-08 |
| MP-04 | Collision rules between snakes: head into other snake's body is a loss for the mover; head-to-head is a draw or both lose. | F | Must | Assumed | S | MP-01, CORE-06 |
| MP-05 | Round win condition: last snake alive wins the round. | F | Must | Assumed | S | MP-04 |
| MP-06 | Best-of-N rounds with a running score for each player and a match winner screen. N is configurable. | F | Should | Assumed | M | MP-05, FUN-02 |
| MP-07 | Fair food and spawn placement: both snakes start symmetrically and food spawns away from both heads. | F | Should | Assumed | S | MP-01, CORE-04 |
| MP-08 | Each player has their own color and a score display. | F | Should | Assumed | S | MP-01, UI-01 |
| MP-09 | Quick rematch from the results screen. | F | Should | Assumed | S | MP-05, CORE-08 |
| MP-10 | Handicap options (starting length, shot limit) for uneven skill levels. | F | Could | Assumed | S | MP-01, FUN-02 |
| MP-11 | Scoring variant for versus play: score from food and from hits, not just survival. | F | Could | Assumed | S | MP-06, SHOOT-04 |

## Meta and progression

| ID | Requirement | Type | Priority | Source | Effort | Depends on |
|---|---|---|---|---|---|---|
| META-01 | Persist the high score locally across sessions (solo mode). | F | Must | Confirmed | S | CORE-05, CORE-08 |
| META-02 | Track basic stats (games played, longest snake, total food eaten, shots hit). | F | Could | Confirmed | S | META-01 |
| META-03 | Keep a separate high score per mode (classic vs. shooting, wrap vs. walls). | F | Should | Assumed | S | META-01, FUN-07 |
| META-04 | Persist the multiplayer win tally between sessions (P1 vs. P2). | F | Could | Assumed | S | MP-06, META-01 |

## UI and accessibility

| ID | Requirement | Type | Priority | Source | Effort | Depends on |
|---|---|---|---|---|---|---|
| UI-01 | Show current score during play (per player in multiplayer). | F | Must | Confirmed | S | CORE-05 |
| UI-02 | Main menu and settings screen (mode, players, volume, controls help). Keyboard navigation only. | F | Should | Confirmed | M | CORE-08 |
| UI-03 | Pause and resume via a single key. | F | Should | Confirmed | S | CORE-08 |
| UI-04 | Cohesive visual theme or art style. Placeholder look for now. | F | Should | Confirmed | M | CORE-01, DISC-03 |
| UI-05 | Remappable controls (including fire keys) and a colorblind-friendly palette. | F | Could | Confirmed | M | TECH-03, UI-02 |
| UI-06 | Show the high score next to the current score during play. | F | Should | Assumed | S | META-01, UI-01 |
| UI-07 | On-screen controls reminder for both players on the start screen. | F | Should | Assumed | S | MP-02, UI-02 |
| UI-08 | Show ammo or cooldown status for the fire action. | F | Should | Assumed | S | SHOOT-03, UI-01 |

## Technical and quality

| ID | Requirement | Type | Priority | Source | Effort | Depends on |
|---|---|---|---|---|---|---|
| TECH-01 | Decide target stack: browser vs. native desktop, language and library. | S | Must | Confirmed | S | DISC-01 |
| TECH-02 | Fixed-timestep game loop decoupled from rendering. Projectiles update faster than the snake tick (sub-steps or a separate rate). | E | Must | Confirmed | S | TECH-01 |
| TECH-03 | Keyboard input only: arrows, WASD, and fire keys. | F | Must | Confirmed | S | TECH-01 |
| TECH-04 | Game logic is separated from rendering so it can be unit-tested. | E | Should | Confirmed | S | TECH-01 |
| TECH-05 | Smooth rendering (target 60 fps) on the owner's own hardware. | NF | Should | Confirmed | S | TECH-02 |
| TECH-06 | Seedable RNG for reproducible tests and bug reports. | E | Could | Confirmed | S | CORE-04, TECH-04 |
| TECH-07 | Handle simultaneous key presses from two players reliably (key rollover limits on common keyboards). | NF | Must | Assumed | M | TECH-03, SPIKE-04 |
| TECH-08 | Game state supports N snakes, each with its own input, projectiles, and score, rather than hard-coding one snake. | E | Must | Assumed | M | TECH-04 |

## Discovery items and spikes

| ID | Question or spike | Type | Priority | Status | Blocks |
|---|---|---|---|---|---|
| DISC-01 | Browser or native desktop? Language and tool preference? | S | Must | **Open** (platform resolved: desktop) | TECH-01 |
| DISC-02 | What made Google Snake feel good, and what felt bad? | S | Must | Partly answered (reference only) | FUN-\*, FEEL-\* |
| DISC-03 | Preferred aesthetic and audio style. | S | Should | Open (TBD) | UI-04, FEEL-02, FEEL-05 |
| DISC-04 | Desired session length. | S | Should | Open (TBD) | FUN-01, FUN-07 |
| DISC-05 | Fun driver. | S | Must | **Resolved:** high-score chasing | – |
| DISC-06 | Solo vs. multiplayer. | S | Must | **Resolved:** local multiplayer on one computer, plus solo | MP-\* |
| DISC-07 | Definition of done. | S | Should | **Resolved:** playable prototype | – |
| DISC-08 | In multiplayer, is the goal to survive, to score, or to eliminate the other player? Does high-score chasing still apply in 2P? | S | Must | **Open** | MP-05, MP-06, MP-11 |
| SPIKE-02 | What limits shooting: cooldown, ammo pickups, or a length cost? | S | Must | **Open** | SHOOT-03 |
| SPIKE-03 | What does a hit on the opponent do: kill instantly, shrink by N segments, or stun? | S | Must | **Open** | SHOOT-04, MP-04 |
| SPIKE-04 | Which key layout works for two players on one keyboard, and does it suffer from key ghosting? | S | Must | **Open** | MP-02, TECH-07 |
| SPIKE-05 | Does shooting a food item or another projectile do anything? Do projectiles collide with each other? | S | Could | Open | SHOOT-06 |

## Dependency summary

- **Foundation chain:** DISC-01 → TECH-01 → TECH-02 / TECH-03 / TECH-04 / TECH-08 → CORE-01 → CORE-02
- **Core loop chain:** CORE-02 → CORE-03 / CORE-04 / CORE-06 / CORE-07 → CORE-05 → CORE-08
- **Shooting chain:** SPIKE-02 → SHOOT-03; CORE-02 → SHOOT-01 → SHOOT-02 → SHOOT-04 (also needs MP-01 and SPIKE-03)
- **Multiplayer chain:** TECH-08 → MP-01 → MP-02 / MP-04 → MP-05 → MP-06 → MP-09
- **Keyboard chain:** SPIKE-04 → MP-02 and TECH-07
- **Solo targets chain:** SHOOT-02 → TARGET-01
- **Fun chain:** FUN-02 (config) → FUN-01 / FUN-03 / FUN-04 → FUN-05 → FUN-07 (modes also need FUN-06 and UI-02)
- **High-score chain:** CORE-08 → META-01 → UI-06 → META-03
- **Critical path to a playable 2-player shooting prototype:** DISC-01 → TECH-01 → TECH-02, TECH-03, TECH-08 → CORE-01 to CORE-08 → SHOOT-01 to SHOOT-04 → MP-01 to MP-05 → UI-01

## Prototype definition

**Done when** you can start a game, choose 1 or 2 players, play, shoot, die or win a round, restart, and (in solo) chase a persisted high score, all on desktop with keyboard only.

| In prototype | Deferred (post-prototype) |
|---|---|
| TECH-01 to TECH-04, TECH-07, TECH-08 | FUN-04 to FUN-09 |
| CORE-01 to CORE-09 | UI-02 (full menu and settings; use a minimal mode select) |
| SHOOT-01 to SHOOT-03, SHOOT-05 to SHOOT-08 | UI-05 (remapping, colorblind palette) |
| MP-01 to MP-05, MP-07 | MP-06, MP-08 to MP-11 |
| UI-01, UI-03, UI-06, UI-08 | META-02, META-03, META-04 |
| FUN-01, FUN-02, FUN-03 | FEEL-05 (music), TECH-06 (seedable RNG) |
| FEEL-01, FEEL-02, FEEL-03 | UI-04 (full theme; use a placeholder look) |
| META-01, TARGET-01 | SHOOT-09, TARGET-02 |
| *Stretch:* FEEL-04, MP-06 | |

## Suggested increments

1. **Increment 1, playable solo core:** TECH-01 to TECH-04, CORE-01 to CORE-08, UI-01.
2. **Increment 2, feel and persistence:** CORE-09, FEEL-01 to FEEL-03, FUN-01 to FUN-03, UI-03, META-01, UI-06.
3. **Increment 3, shooting:** resolve SPIKE-02 and SPIKE-03, then SHOOT-01 to SHOOT-03, SHOOT-05 to SHOOT-08, TARGET-01, UI-08.
4. **Increment 4, local multiplayer:** resolve SPIKE-04 and DISC-08, then TECH-07, TECH-08, MP-01 to MP-05, MP-07, and SHOOT-04.
5. **Post-prototype:** MP-06 and onward, the deferred FUN items, UI-02, UI-04, UI-05, META-02 to META-04.

Note: TECH-08 (support for N snakes) is cheaper to build in Increment 1 than to retrofit in Increment 4, so consider doing it early.

## Change log

| Pass | Changes |
|---|---|
| 1 | Initial backlog created from the brief. |
| 2 | Removed unlockable skins and online leaderboards (OOS-01, OOS-02). Core, Fun, Tech, and Feel confirmed. Meta and UI confirmed. Discovery answers recorded. Persistence and input buffering raised to Must. Prototype scope defined. |
| 3 | Mouse controls proposed, then rejected (OOS-03). Shooting added (keyboard only, fires in facing direction). Local same-computer multiplayer added, reversing the solo-only decision (DEC-06). Added MP-\*, SHOOT-\*, TARGET-\*, TECH-07, TECH-08, SPIKE-02 to SPIKE-05, DISC-08. Renumbered per-mode high scores to META-03. |
