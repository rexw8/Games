# Rogue Dagger

A pixel-art bullet-hell boss rush in the PICO-8 style. Fight through three themed rushes of three bosses each, as a dagger-throwing Rogue or a charge-blasting Wizard.

The whole game is one self-contained HTML file (`www/index.html`) with no external dependencies. It runs in any modern browser, and this repo also wraps it as an iOS app with [Capacitor](https://capacitorjs.com).

---

## Features

- **3 boss rushes, 9 bosses:** Jungle Depths, Sunbaked Wastes and Frostbound Spire. Each rush ends in a finale boss that combines the first two.
- **Escalating fights:** every boss has a themed entrance, 3 HP-based phases, and a rotating pool of attack patterns that grows with each rush (2 → 3 → 4 patterns in the final phase).
- **2 characters**
  - **Rogue:** hold to auto-throw a steady stream of daggers.
  - **Wizard:** hold to charge (~1.2s), release to fire an orb. Damage and size scale with charge time.
- **Artifacts:** the Cloak of the Ancients is always available. Clearing a rush unlocks its reward (Mirage Veil for the desert, Rimefrost Ward for ice), and unlocks are kept forever. Artifacts recharge once per boss phase.
- **3 difficulties**
  | | Bullet speed | Boss HP | Player HP | Lives | Checkpoints |
  |---|---|---|---|---|---|
  | Easy | 65% | 100% | 6 | 3 | Retry the current boss |
  | Standard | 100% | 100% | 6 | 3 | Restart the rush |
  | Hard | 135% | 130% | 4 | 1 | None |
- **Rush select:** start from any rush.
- **Music:** three procedural chiptune tracks (Web Audio API), or load your own audio file.
- **Inscryption-style opening cutscene.**
- **Saves locally:** unlocked artifacts and all settings persist on the device.
- **Mobile-ready:** touch controls, portrait and landscape layouts, notch/home-bar safe areas, and auto-pause when the app is backgrounded.

## Controls

| Action | Touch | Keyboard / mouse |
|---|---|---|
| Move | Left stick | WASD / arrow keys |
| Attack / aim | Hold on the play area (aims at your finger) | Hold left mouse (aims at cursor) |
| Dash (brief invulnerability) | DASH | Space |
| Use artifact | USE | E |
| Menus | Tap | Click |

---

## Project structure

```
RogueDagger/
├── www/index.html          ← the entire game (edit this)
├── capacitor.config.json   ← app name, bundle ID, web folder
├── package.json            ← Capacitor CLI dependencies
├── ios/                    ← generated Xcode project (Capacitor 8, Swift Package Manager)
│   └── App/App/
│       ├── Info.plist      ← orientations, status bar, export-compliance flag
│       ├── Assets.xcassets ← app icon (1024×1024) and launch screen
│       └── public/         ← copy of www/ made by `cap sync` (don't edit)
├── APP_STORE_GUIDE.md      ← step-by-step build & submission instructions
├── APP_STORE_LISTING.md    ← drafted App Store name, description, keywords
└── PRIVACY_POLICY.md       ← privacy policy text to host publicly
```

## Running it

**In a browser:** open `www/index.html`. No build step or server needed.

**On iOS** (needs a Mac with Xcode):
```bash
npm install          # first time only
npx cap sync ios     # copy www/ into the Xcode project
npx cap open ios     # open in Xcode, then press ▶
```
See [`APP_STORE_GUIDE.md`](APP_STORE_GUIDE.md) for signing, screenshots, and submitting to the App Store.

**After changing the game:** run `npx cap sync ios` again before building, and bump the version/build number in Xcode before each upload.

---

## How the code is organized

`www/index.html` is written like a small fantasy-console cart: one `update()` and one `draw()` driven by a state machine, with each system in its own section of the script.

### Game states
`STATE` holds every screen: `INTRO → MENU → (SETTINGS | RUSH_SELECT | MUSIC_SELECT | CHARACTER_SELECT) → ARTIFACT_SELECT → BOSS_INTRO → PLAYING → PHASE_TRANSITION / RUSH_COMPLETE / GAMEOVER / VICTORY`. `update()` and `draw()` each branch on `gameState`.

### Adding content

**A new boss:** write a factory that builds on `newBossBase(name, hp, color)` and sets:
- `b.entrance`: `{ type, startX/startY, homeX, homeY }`, where type is one of `slither`, `fall`, `burrow`, `teleportIn`, `materialize`, `crash`, `descend`.
- `b.p1`, `b.p2`, `b.p3`: arrays of attack functions for each phase. `cyclePattern(b, b.pN)` rotates through them.
- `b.update()` and `b.draw()`.

Build attacks from the shared bullet helpers: `spiralShot`, `ringShot`, `aimedShot`, `fanShot`, `crossShot`, `rainDrop`. They automatically apply the difficulty's bullet-speed multiplier.

**A new rush:** write a `make…Rush()` returning `{ id, name, bgColor, accentColor, artifactReward, bosses:[f1, f2, f3] }` and `RUSHES.push()` it. The background, rush select and rewards pick it up automatically.

**A new artifact:** add an entry to `ARTIFACTS` with `id, name, desc, color, use()`. Make it a rush's `artifactReward` so players can earn it.

**A new character:** add it to `CHARACTERS`, then branch on `selectedCharacter` in `updatePlayer()` (attack) and `drawPlayer()` (look).

**A new music track:** add an entry to `MUSIC_TRACKS` with a waveform and a list of `{f: frequencyHz, d: seconds}` notes.

### Save data
Stored in `localStorage` under `rogueDagger.save.v1`: unlocked artifacts, difficulty, rush, character and music track. Every storage access is wrapped in `try/catch`, so the game still runs where storage is blocked. Bump the key's version if the save format changes incompatibly.

---

## Technical notes (hard-won — keep these intact)

- **Fixed timestep:** `update()` runs at a locked 60 Hz through an accumulator in `loop()`, while `draw()` runs every display frame. All timers count frames, so tying logic directly to `requestAnimationFrame` makes the game run 2× faster on 120/144 Hz screens.
- **One tap, one click:** every tap and click goes through `registerClick()`. It has a 200 ms debounce, and `touchstart` calls `preventDefault()` so phones don't also fire a ghost mouse click that skips a screen.
- **Browser gesture rules**
  - The music file picker must open synchronously inside `touchend` or `mousedown` (see `tryOpenMusicPicker`). Browsers silently block it from `touchstart` or from the game loop.
  - The audio context is unlocked on the player's first tap. Uploaded songs are decoded and looped through Web Audio rather than an `<audio>` element, which iOS refuses to start outside a tap.
- **State transitions must route through `BOSS_INTRO`.** Skipping it skips the boss entrance animation.
- **Backgrounding:** a `visibilitychange` listener stops the music and freezes gameplay behind a "tap to resume" screen.

## License & privacy

The game collects no data and makes no network requests. See [`PRIVACY_POLICY.md`](PRIVACY_POLICY.md).
All music is generated in code, so there are no third-party audio assets.
