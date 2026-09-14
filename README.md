# War1gus NX (Nintendo Switch Port)

[![Support me on Ko-fi](https://img.shields.io/badge/Ko--fi-Support%20me-FF5E5B?logo=kofi&logoColor=white)](https://ko-fi.com/thorhax)
[![GitHub Release](https://img.shields.io/github/v/release/Thorhax/War1gus-NX-Modern?include_prereleases&color=blue)](https://github.com/Thorhax/War1gus-NX-Modern/releases)

Modern Nintendo Switch homebrew port of **War1gus** (*Warcraft: Orcs & Humans*) running on the Stratagus RTS engine.

Based on **Stratagus v3.3.3** with handheld controller and touchscreen support, optimized for Nintendo Switch using devkitA64 and libnx.

---

## Features

- **Full Warcraft: Orcs & Humans Experience**: Complete Orc and Human single-player campaigns, skirmish maps, custom scenarios, and classic RTS gameplay.
- **Embedded RomFS**: All War1gus Lua logic, campaign definitions, all 196 single-player mission scenarios, maps, shaders, UI assets, and full 45-track high quality OGG soundtrack are embedded directly into `war1gus.nro`. Works completely self-contained out-of-the-box!
- **Optimized Handheld Controls**:
  - Dual analog control (Left stick moves cursor, Right stick scrolls map).
  - Touchscreen controls (tap to select, drag selection box, 2-finger tap for right-click).
  - Fast cursor toggle (hold `R`).
  - D-Pad control groups with `L` modifier to assign groups.
  - Dedicated RTS action hotkeys (Attack, Stop, Patrol, Build).
- **Native Video & Audio**:
  - Dynamic 720p (Handheld) / 1080p (Docked) bilinear scaling.
  - OGG/MIDI music and digital sound effects via Tremor / SDL2_mixer.
- **Robust Error Handling**:
  - Missing-data detection with on-screen guidance.
  - Detailed execution logs written to `sdmc:/switch/war1gus/war1gus.log`.

---

## Installation & Setup

War1gus requires original game data extracted from a retail CD, DOS floppy, or GOG release of **Warcraft: Orcs & Humans**.

### Step 1: Extract Game Data

You can extract data using the provided `war1tool` binary (Linux) or via the official War1gus installer on PC.

#### Using `war1tool`:
```bash
# Run war1tool with -m (MIDI music) and -v (video cutscenes):
./war1tool -m -v /path/to/warcraft1_cd/ war1gus_data/
```
The output directory `war1gus_data/` will contain:
- `graphics/`
- `sounds/`
- `music/`
- `videos/` (if extracted)

*(If you already play War1gus on PC or PS Vita, you can directly copy your existing extracted folders!)*

### Step 2: Copy to Nintendo Switch SD Card

1. Copy `war1gus.nro` to:
   ```
   sdmc:/switch/war1gus/war1gus.nro
   ```
2. Copy the extracted data folders into `sdmc:/switch/war1gus/`:
   ```
   sdmc:/switch/war1gus/
   ├── war1gus.nro
   ├── graphics/
   ├── sounds/
   ├── music/
   └── videos/           (optional)
   ```
   *(Note: `campaigns/`, `maps/`, and `scripts/` are already bundled inside the NRO's RomFS. If you place custom maps or campaigns in this directory, they will override the built-in ones.)*

### Step 3: Launch

Launch **War1gus** from the Homebrew Menu.  
> **Tip:** Launching via Title Override (hold **R** while launching any installed Switch game) is recommended for full RAM access.

---

## Controls

| Switch Button | Action |
| --- | --- |
| **Left Stick** | Move Cursor / Pointer |
| **Right Stick** | Scroll Map View |
| **B** | Left Mouse Button (Select unit/building, confirm order, click UI) |
| **A** | Right Mouse Button (Cancel order, right-click move/attack) |
| **Y** | Attack Order shortcut (`A`) |
| **X** | Stop Order shortcut (`S`) |
| **ZL** | Patrol Order shortcut (`P`) |
| **ZR** | Build / Harvest Order shortcut (`B`) |
| **D-Pad Up / Right / Down / Left** | Select Control Group 1, 2, 3, 4 |
| **L + D-Pad** | Assign Control Group 1, 2, 3, 4 |
| **R (Hold)** | Fast Cursor Movement (2x speed) / Shift modifier |
| **Plus (+)** | Escape / Main Game Menu |
| **Minus (-)** | F10 / In-Game Options Menu |
| **Touchscreen Tap** | Left Click / Select |
| **Touchscreen Drag** | Drag-Select multiple units |
| **Touchscreen Two-Finger Tap** | Right Click / Move order |

---

## Saves & Logs

- **Save Games**: Saved to `sdmc:/switch/war1gus/wc1/save/`
- **Preferences**: Saved to `sdmc:/switch/war1gus/wc1/preferences.lua`
- **Log File**: Written to `sdmc:/switch/war1gus/war1gus.log`

---

## Building from Source

The port is compiled using devkitPro (`devkitA64` and `switch-portlibs`).

```bash
docker run --rm -v $(pwd)/stratagus:/work -w /work/build devkitpro/devkita64:latest bash -c "
    cmake .. -DCMAKE_TOOLCHAIN_FILE=/opt/devkitpro/cmake/Switch.cmake \
             -DCMAKE_BUILD_TYPE=Release \
             -DENABLE_STATIC=ON \
             -DENABLE_USEGAMEDIR=ON && \
    make -j\$(nproc)
"
```
The resulting homebrew binary `war1gus.nro` will be generated in `build/`.

---

## Credits & License

- **Stratagus & War1gus Team**: [Wargus Project](https://github.com/Wargus)
- **PS Vita Port Controls**: [Northfear](https://github.com/Northfear/stratagus-vita)
- **Nintendo Switch Port**: Thorhax
- **Warcraft: Orcs & Humans**: Blizzard Entertainment

Licensed under the **GNU General Public License v2.0 (GPLv2)**.
