# Voltfront

A grid-based mech auto-battler. Build a squad from 12 energy-powered mechs, deploy it on a 6×8 grid, and watch the fight play out on its own. Every attack is telegraphed onto a tile before it fires, so the strategy lives in who you bring and where you put them.

It mixes the squad-building and grid positioning of **Clash Mini** with the telegraphed, puzzle-like combat of **Into the Breach**, in a dim machine-room look inspired by P03's act of **Inscryption**.

The whole game is one self-contained HTML file (`voltfront.html`). It runs in any modern browser on desktop or phone.

---

## How to play

1. **Build a squad** in the Hangar. Tap a card to add or remove it, or tap ⓘ for its full dossier. Your reactor holds **12 power** and up to **5 mechs**, and each mech draws 2–4 power.
2. **Deploy.** Press Deploy to face a random enemy squad built under the same rules. Place your mechs anywhere on your three lit rows.
3. **Read the forecast.** The board shows exactly what round 1 will do with your current placement: where each mech moves and which tiles it will strike. Move a mech and the forecast updates.
4. **Engage** and watch. Use 1×/2×/4×, Pause or Skip.
5. Afterwards, **Rematch** the same enemy with a new formation, take a **New opponent**, or go back to the Hangar.

## Features

- **12 mechs, each with a role, a strength, a limitation and one defining ability.** No mech is a strictly weaker copy of another.
- **Two-phase rounds**
  - **Maneuver:** in initiative order, every mech moves and locks an attack onto a *tile*. Locked tiles are hatched and show the damage.
  - **Strike:** in the same order, every attack fires at its tile and hits whatever stands there, friend or foe. Pushes, pulls and kills earlier in the phase change who that is.
- **Exact round-1 forecast** while deploying, for both sides.
- **4 protocols (squad synergies)** that unlock at 2 and 3 matching mechs.
- **Energy as the core resource:** a power budget when building the squad, and ability charge in battle.
- **Pushes and collisions:** a mech pushed into another mech or off the board takes 1 damage, and so does the mech it hits.
- **Win/loss record and your last squad** are saved on your device.
- **Mobile-first:** touch placement, drag to rearrange, notch and home-bar safe areas.

## Controls

| Action | Touch | Mouse / keyboard |
|---|---|---|
| Add or remove a mech | Tap its card | Click its card, or Tab to it and press Enter |
| Mech details | Tap ⓘ on the card | Click ⓘ |
| Place a mech | Tap it in the tray, then tap a lit tile | Same, with clicks |
| Move or swap placed mechs | Drag on the board, or tap two mechs | Drag, or click two mechs |
| Inspect any mech on the board | Tap it | Click it |
| Battle speed | 1× / 2× / 4× / Pause / Skip | Same |

---

## The roster

| Mech | Role | Tags | Power | Hull | Move | Init | Basic attack | Ability (energy cost) |
|---|---|---|---|---|---|---|---|---|
| Bastion | Vanguard | Plate | 3 | 12 | 1 | 2 | Melee, 2 | **Bulwark** (3): 4 shield, taunts enemies within 2 |
| Warden | Shield projector | Plate, Arc | 2 | 9 | 1 | 3 | Melee, 2 | **Aegis Field** (3): 3 shield to itself and adjacent allies |
| Titan | Reactor brawler | Plate, Surge | 4 | 12 | 1 | 2 | Melee, 3 | **Overload Slam** (3): 5 to target, 2 to every other adjacent mech |
| Lancer | Breacher | Surge, Plate | 3 | 10 | 2 | 5 | Melee, 3 | **Ram** (3): charge up to 5 tiles, 4 damage, push 1 |
| Spark | Assassin | Surge, Arc | 2 | 6 | 3 | 7 | Melee, 3 | **Blink Strike** (2): teleport behind the weakest enemy, 4 damage |
| Hook | Puller | Surge | 2 | 9 | 1 | 4 | Line 3 any direction, 3 | **Grapple** (3): hook the first mech within 5, 2 damage, drag it adjacent |
| Relay | Energy battery | Arc | 2 | 8 | 1 | 4 | Ranged 2, 2 | **Overcharge** (3): +2 energy to adjacent allies |
| Mender | Repair unit | Arc | 2 | 7 | 1 | 3 | Ranged 2, 1 | **Field Repair** (2): restore 3 hull to the most damaged ally within 2 |
| Leech | Disruptor | Arc, Volt | 2 | 7 | 2 | 5 | Ranged 2, 2, steals 1 energy | **EMP Burst** (3): enemies within 2 take 1 and lose 2 energy |
| Tesla | Chain caster | Volt | 3 | 8 | 1 | 4 | Ranged 2, 3, arcs 1 to 2 touching enemies | **Chain Storm** (3): 3 to target, arcs 2 through up to 4 connected enemies |
| Howitzer | Artillery | Volt | 3 | 6 | 1 | 1 | Artillery 2–4 tiles, 2 | **Barrage** (4): 3 to center, 1 to each neighboring tile (hits allies) |
| Rail | Lane sniper | Volt | 3 | 6 | 1 | 2 | Straight ahead, 4 to the first mech | **Piercing Rail** (3): 4 to every mech in its column (hits allies) |

**Passives:** Relay gives adjacent allies +1 energy each round. Titan gains 1 energy whenever it takes damage.

## Protocols

| Protocol | 2 mechs | 3 mechs | Members |
|---|---|---|---|
| **Plate** | Plate mechs take 1 less damage per hit (min 1) | The whole squad does | Bastion, Warden, Titan, Lancer |
| **Arc** | Arc mechs start with +1 energy | Arc mechs also gain +1 energy per round | Warden, Spark, Relay, Mender, Leech |
| **Surge** | Surge mechs get +1 move and +2 initiative | Surge basic attacks deal +1 | Titan, Lancer, Spark, Hook |
| **Volt** | Volt abilities deal +1 | Volt basic attacks deal +1 too | Leech, Tesla, Howitzer, Rail |

## Energy

- Every mech gains **1 energy per round** (the pips under it). When the pips are full, its next action is its ability instead of its basic attack.
- **Grid overload (round 5):** every mech gains 2 energy per round from then on.
- **Reactor bleed (round 8):** every mech loses hull at the start of each round, rising every other round. Battles always end, usually around round 10. If bleed wipes out both squads at once, the side with more hull left before the bleed wins.

---

## How the code is organized

The file has two scripts: a **pure engine** with no DOM access, then the **UI**. The UI only reads engine state and plays back the events the engine emits, which is what makes the deploy forecast possible.

### Engine (first `<script>`)
| Section | What it holds |
|---|---|
| Protocols | `PROTOCOLS` tier table and `protocolLevels(types)` |
| Energy | The whole energy economy in one object: start energy, per-round regen, overload/bleed rounds, `gain`, `spend`, `onDamaged`, `roundStart`. Change the economy here without touching combat. |
| Mech roster | `MECHS`: stats, tags, basic attack pattern, AI style, and the ability's `plan()` or `maneuver()`. `MECH_ORDER` sets roster order. |
| AI | `chooseDest` scores every reachable tile; `basicOptions` and `targetScore` pick targets |
| Resolution | `RESOLVE` maps each action kind (`hit`, `line`, `blast`, `chain`, `ram`, `slam`, `emp`, `heal`, …) to its effect. `damage()` applies Plate, shields and deaths. |
| Round runner | `runRound(st, {planOnly})` runs one round and returns an event list. `planOnly` stops after the maneuver phase; the deploy forecast uses it. |
| Squads | `randomSquad()` and `autoFormation()` for enemies and the Auto button |

### UI (second `<script>`)
| Section | What it holds |
|---|---|
| Mech art | `drawMech(ctx, type, x, y, size, team, time)` draws every mech procedurally. Enemies are the same drawing rotated 180° in the rust palette. |
| Hangar / Codex / Protocols / Manual | DOM screens built from `MECHS` and `PROTOCOLS`, so new content shows up automatically |
| Field | Deployment input, tray, forecast |
| Rendering | Board, telegraphs, unit HUD (hull bar, shield, energy pips) |
| Battle playback | `EVH` has one handler per engine event. Each returns how long to wait before the next event, divided by the speed setting. |

### Adding content
**A new mech:** add an entry to `MECHS` with `name, role, tags, power, hp, move, speed, rank` (0 front, 1 middle, 2 back), an `ai` style (`front`, `skirm`, `ranged`, `support`, `guard`, `lane`, `artillery`), a `basic` attack, an `ability` with `cost` and either `plan(st,u)` (act after moving) or `maneuver(st,u)` (the ability *is* the movement), and the codex text fields (`strength`, `weakness`, `pairs`). Add its key to `MECH_ORDER` and a `case` in `drawMech`.

**A new action kind:** return `{ kind:'yourKind', tiles:[...], ... }` from an ability and add `RESOLVE.yourKind`. It should push an `act` event first so the UI can animate it. Mark helpful actions with `help:true` so they telegraph as friendly rings.

**A new protocol:** add it to `PROTOCOLS`, tag mechs with it, read its level with `lvl(st, team, 'KEY')` where the bonus applies, and give it a color in `PCOL`.

### Save data
`localStorage` key `voltfront.v1` stores the squad and the win/loss record. Every access is wrapped in `try/catch`, so the game still runs where storage is blocked.

---

## Technical notes

- **Determinism:** combat has no randomness. The only random choices are the enemy squad and its column order. That is why the round-1 forecast is exact.
- **Frame-rate independent:** playback waits on real elapsed time (ms), not frame counts, so 120/144 Hz screens don't speed it up.
- **Balance was checked headlessly.** The engine runs in Node (it exports itself when `module` exists). Across about 20,000 random battles, every mech wins 44–56% of the battles it appears in, battles average about 10 rounds, and a 3-tier protocol advantage lifts win rate from about 49% to about 61%. Rerun something similar after changing numbers.
- **Fonts:** headings and numbers use Tektur, Barlow Semi Condensed and IBM Plex Mono from Google Fonts. Offline, the game falls back to system fonts and plays the same.
- **No sound yet.**

## Status

Playable first version. The design is deliberately open: energy rules, protocols and the roster are expected to change.
