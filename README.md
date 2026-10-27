# Daggers Edge Games

A growing collection of small browser games, vibe-coded with Claude — built through conversation, playtested, and refined one feature at a time.

Every game is a **single self-contained HTML file**. No installs, no build step, no internet connection needed: download a file and open it in any modern browser on desktop or phone.

---

## The games

| Game | Genre | Status | Play |
|---|---|---|---|
| **Rogue Dagger** | Pixel-art bullet-hell boss rush | Playable | [`Rogue Dagger/rogue-dagger.html`](Rogue%20Dagger/rogue-dagger.html) |
| **Cozy Garden** | Cozy farming with light idle automation | Playable | [`Cozy Garden/cozy-garden.html`](Cozy%20Garden/cozy-garden.html) |
| **Voltfront** | Grid-based mech auto-battler | Playable | [`Voltfront/voltfront.html`](Voltfront/voltfront.html) |
| *Next game* | — | Coming soon | — |

### Rogue Dagger
Fight through three themed rushes of three bosses each, as a dagger-throwing Rogue or a charge-blasting Wizard. Each boss has a themed entrance, three escalating phases and growing attack patterns. Includes Easy, Standard and one-life Hard modes, artifacts earned by clearing rushes, rush select, and chiptune music (or your own songs).
**Controls:** left stick or WASD to move · hold on the screen or the mouse to attack · DASH or Space · USE or E for your artifact.

### Cozy Garden
Plant, water and harvest clay crops on a tilted toy-diorama plot, then grow it from a 3×3 patch into an automated 8×8 garden. Fill villagers' orders, chase rare golden crops, collect 12 Curios with side effects, and build sprinklers, auto-planters and tractors. A soft rain keeps the garden growing for up to 8 hours while you're away, and a daily gift rewards your visit streak.
**Controls:** tap or click soil to plant · tap a plant to water it · tap a glowing plant to harvest.

### Voltfront
Build a squad of energy-powered mechs under a 12-power reactor, place them on a 6×8 grid, and watch the battle play out on its own. Every attack is telegraphed onto a tile before it fires, so a shove or a pull can turn an enemy's shot onto its own side. A live round-1 forecast during deployment shows exactly where everyone will move and strike. 12 mechs, 4 squad synergies, and a Clash Mini–style hangar with an Inscryption P03 machine-room look.
**Controls:** tap cards to build your squad · tap or drag to place mechs · Engage, then watch.

<!-- Copy this block for each new game:
### Game Name
One or two sentences on what it is and what makes it fun.
**Controls:** …
-->

---

## Playing

1. Open the game's folder.
2. Open its `.html` file in a browser. On a phone, save the file and open it from your Files app or browser.

That's it. Progress and settings save in your browser, on your device only.

## Repository layout

```
Games/
├── README.md                  ← this file
├── Rogue Dagger/
│   ├── rogue-dagger.html      ← the whole game
│   ├── RogueDagger-iOS.zip    ← iOS app wrapper
│   └── README.md              ← features, controls, how the code is organized
├── Cozy Garden/
│   ├── cozy-garden.html       ← the whole game
│   └── README.md
├── Voltfront/
│   ├── voltfront.html         ← the whole game
│   └── README.md
└── <Next Game>/
    ├── <next-game>.html
    └── README.md
```

Each game gets its own folder with its own README covering features, controls and notes for working on the code.

---

## How these are made

These games are built by **vibe coding**: describing what I want to make with Claude, playing the result, and coming back with what to change.

- **Start small.** First get a playable core loop working, then add content, then systems, then polish.
- **Playtest every change.** Most bugs and feel problems only show up once you actually play.
- **Ask for small changes.** Asking for the smallest edit that does the job keeps working features working.
- **Keep it in one file.** One HTML file is easy to share, archive and hand back to Claude for the next round of changes.

### Standards every game follows
- Works on desktop (keyboard and mouse) and mobile (touch controls), in portrait and landscape
- Game speed is the same on every screen. Logic runs at a fixed 60 updates per second, so 120/144 Hz monitors don't speed it up.
- One tap counts as one input. Phones fire a touch event and a fake mouse click for the same tap, so the duplicate is filtered out.
- Sound starts on the player's first tap, as browsers require
- No tracking, ads or network requests
- Original art and sound only: art drawn in code, music generated in code or supplied by the player

### Starting a new game
When starting a new game with Claude:
1. Describe the genre, an inspiration, the art style and the core mechanic in a few sentences.
2. Ask for a single-file HTML game with touch and keyboard controls.
3. Once it's playable, add its folder here, write its README, and add a row to the table above.

---

## License

© 2026 Daggers Edge Games. All rights reserved unless a game's folder says otherwise.
