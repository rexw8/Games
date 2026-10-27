# Cozy Garden

A relaxing farming game in a single HTML file. Plant seeds, keep the soil damp, harvest glowing crops, fill villagers' orders, and grow a tiny plot into an automated 8×8 garden. The garden is a tilted clay diorama, and every crop, machine and icon is a soft-shaded clay sprite drawn in code. It plays on phones and desktop browsers.

## How to play

1. Pick a seed in the **Seeds** tab and tap empty soil to plant it.
2. Plants grow only while their soil is damp. Tap a plant to water it, or use **Water all**.
3. When a crop glows, tap it to harvest for coins and XP.
4. Deliver crops to customers in the **Journal**, level up to unlock new crops and land, and buy machines that farm for you.

Close the page whenever you like. A soft rain keeps the garden growing (at half speed) for up to 8 hours, and your machines keep working.

## Features

- **18 crops**: 6 basic seeds and 12 Curios, each with its own clay sprite and growth stages.
- **Golden crops**: any basic seed has a 4% chance to sprout golden and sell for 5×.
- **Curio Shop**: a rotating stock of 3 special seeds with side effects. Stock refreshes every 90 seconds, or reroll for 10 coins.
- **Customer orders**: 3 villagers post requests for specific crops and pay well for them, sometimes with a Curio seed as a tip. Each harvest fills one order, and a new customer arrives 40 seconds after a delivery.
- **Goals**: 17 milestones with coin rewards, from *First basket* to *Rolling acres*.
- **Almanac**: a collection book of every crop, with undiscovered crops shown as silhouettes.
- **Daily gift and streak**: a coin gift each day you visit that grows with your streak, plus a free Curio seed every third day.
- **Time away**: on return, a welcome card summarizes what grew and what your machines harvested.
- **Workshop**: sprinklers, tractors, auto-planters and land expansion.
- **Level bonus**: +2% harvest coins per level.
- **Light and dark themes**: a sunny diorama or a dusk one, following the device setting.
- **Sound**: small Web Audio chimes for planting, harvesting and level-ups, with a mute toggle.

## Crops

### Basic seeds

| Crop | Unlocks | Cost | Sells | Grow time | XP |
|---|---|---|---|---|---|
| Carrot | Lv 1 | 2 | 5 | 20s | 3 |
| Strawberry | Lv 2 | 3 | 8 | 30s | 5 |
| Tomato | Lv 3 | 4 | 11 | 40s | 8 |
| Sunflower | Lv 4 | 5 | 14 | 50s | 12 |
| Lavender | Lv 6 | 6 | 18 | 60s | 16 |
| Pumpkin | Lv 8 | 8 | 24 | 75s | 24 |

### Curios

"Neighbors" means the 8 surrounding tiles.

| Curio | Unlocks | Cost | Sells | Effect |
|---|---|---|---|---|
| Starfruit | Lv 5 | 20 | 80 | On harvest, waters the whole garden |
| Pixie Mint | Lv 5 | 22 | 90 | Neighbors stay damp twice as long |
| Goldleaf Clover | Lv 5 | 26 | 100 | 30% chance to sell for triple |
| Sleepy Cactus | Lv 6 | 24 | 95 | Keeps growing at half speed when dry |
| Dewdrop Lily | Lv 7 | 25 | 113 | Keeps itself and its neighbors watered |
| Sprinkle Poppy | Lv 8 | 27 | 110 | On harvest, sows a free seed in an empty neighbor tile |
| Honeybell | Lv 9 | 28 | 120 | On harvest, growing neighbors jump 30% closer to ripe |
| Pearl Mushroom | Lv 10 | 33 | 113 | +3 coins for every other plant in the garden |
| Ember Pepper | Lv 11 | 30 | 125 | Harvesting it also harvests every ripe neighbor |
| Thorny Rose | Lv 12 | 32 | 125 | +4 coins for every planted neighbor |
| Evergreen Blueberry | Lv 13 | 34 | 130 | Regrows once after harvest |
| Moonmelon | Lv 14 | 35 | 150 | Grows twice as fast while watered |

Curios give double XP.

## Progression

| Level | Unlocks |
|---|---|
| 1 | 3×3 plot, carrots, customer orders |
| 3 | 4×4 plot |
| 5 | Curio Shop |
| 6 | 5×5 plot |
| 8 | Workshop, Sprinkler |
| 10 | Auto-Planter |
| 11 | Tractor |
| 12 | Mega Sprinkler |

Each level-up gives a coin gift of 5 + 5 × level.

### Workshop

| Item | Cost | What it does |
|---|---|---|
| Sprinkler | 120 | Takes a tile and keeps the 8 surrounding plants watered |
| Mega Sprinkler | 600 | Takes a tile and keeps every plant within 2 tiles watered |
| Auto-Planter (max 4) | 300 | Uses no tile. Plants its chosen seed in a random empty tile every 2.5s, and you pay for each seed |
| Tractor (max 4) | 400 | Uses no tile. Drives a snake route by rows or columns and harvests every ripe crop it passes |
| Expand to 6×6 / 7×7 / 8×8 | 1,500 / 5,000 / 15,000 | Adds a row and a column of plots |

Each additional sprinkler, planter or tractor costs 35% more than the last.

## Technical notes

- **One file**: all HTML, CSS and JavaScript live in `cozy-garden.html`. Open it in any modern browser; there is no build step.
- **Procedural art**: `buildArt()` draws all 29 sprites onto 128px canvases at load with radial-gradient clay shading, a pressed highlight and a soft cast shadow, then caches them as data URLs. If canvas is unavailable, emoji are used instead.
- **Diorama**: the plot is a CSS 3D scene (`rotateX` on the board, `preserve-3d` on tiles). Crop sprites counter-rotate so they stand upright, and the tilt is set by the `--tilt` custom property.
- **Rendering**: tiles are re-rendered only when their sprite or state signature changes. Growth and water bars update in place.
- **Saving**: progress is stored in `localStorage` under `cozy-garden-v2`, with migration from older `v1` saves and 5×5 boards. The **Start a new garden** button in the Journal erases it.
- **Time away**: on load or tab return, elapsed time (capped at 8 hours) is simulated in 1–2 second steps with rain on and all pop-ups and sounds muted.
- **Fonts**: Baloo 2 and Nunito from Google Fonts, falling back to system fonts when offline.
- **Testing**: game logic was checked headlessly in Node with jsdom, using `@napi-rs/canvas` to draw the sprites.
