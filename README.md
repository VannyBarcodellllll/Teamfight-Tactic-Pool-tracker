# TFT Comp Recommender

A self-contained, offline-ready tool for Teamfight Tactics players to manage comps, track the unit pool, scout the lobby, and get data-driven comp recommendations mid-game.

No installation. No server. Just open `index.html` in a browser.

---

## Getting Started

1. Double-click `index.html`
2. It opens in your default browser
3. Click **⚙ Import from Community Dragon** on the Units tab to auto-populate all champions for the current set
4. Add your comps on the Comps tab
5. Use Pool Tracker, Scout, and Recommendations during a game

> Requires an internet connection for the initial Community Dragon import and for loading champion portraits. Everything else works offline.

---

## Features

### Units Tab
Manage all TFT champions used by your comps.

| Action | How |
|--------|-----|
| Import current set | Click **⚙ Import from Community Dragon** → Fetch Automatically |
| Add unit manually | Click **+ Add Unit** |
| Edit / delete | Hover over a unit card → pencil or ✕ |
| Upload portrait | Upload from disk (crop tool opens) or paste an image URL |

Each unit stores: **name, cost tier, pool size, traits, portrait**.  
Units are sorted by cost (1→5) then alphabetically.

---

### Comps Tab
Define the comps you want to track and recommend.

| Action | How |
|--------|-----|
| Add comp | Click **+ Add Comp** |
| Edit / delete | Hover over a comp card → pencil or ✕ |
| Assign units | Click unit chips in the editor to select/deselect |
| Set star level | Select a unit, then click ★ ★★ or ★★★ below its portrait |
| Search units | Type in the search bar (filters by name or trait) |

**Star levels matter** — marking a unit as 3★ tells the recommendation engine you need 9 copies, so the pool availability score for that comp will drop much faster when the pool is contested.

---

### Pool Tracker Tab
Track how many copies of each unit remain in the shared pool during a game.

| Action | How |
|--------|-----|
| Decrease pool count | Click **−** (someone bought a copy) |
| Increase pool count | Click **+** (unit returned to pool) |
| Type exact count | Click the number field and type |
| Mark copies you own | Click the ★ stars below each unit (1–3 copies) |
| Reset all | Click **↺ Reset Pool** |

**Border colours:**
- 🟢 Green — >60% of pool remaining
- 🟡 Yellow — 30–60% remaining
- 🔴 Red — <30% remaining (highly contested)

**Stars (★★★)** under each card = copies you personally own. This feeds into the recommendation engine.

---

### Scout Tab
Mark what each of the 7 opponents is playing to update recommendation scores in real time.

| Action | How |
|--------|-----|
| Set opponent comp | Click the dropdown next to their player slot |
| Rename a player | Click the name field (P1–P7) and type |
| Clear all entries | Click **↺ Clear All** |

A **Contested Units** panel appears automatically when 2+ opponents are playing comps that share the same unit.

Scouting data feeds directly into the recommendation algorithm — comps your opponents are playing are penalised.

---

### Recommendations Tab
Comps ranked by a composite score calculated from five factors:

| Factor | Effect |
|--------|--------|
| Pool availability | Core score — how much of each unit is left (star-level aware) |
| Tier bonus | S +15%, A +10%, B +5% |
| My units bonus | Up to +10% for units you already own |
| Direct opponent penalty | −12% per opponent on the exact same comp |
| Unit demand penalty | Opponents competing for the same units reduce score |

Each comp card shows:
- **Score bar** (green ≥70%, yellow ≥40%, red <40%)
- **👤 X opponents** badge if anyone else is on it
- **★ X/Y owned** badge based on your Pool Tracker stars
- Per-unit chips with remaining pool count, star level, traits, and owned indicator
- Units shown in **red** when the pool is running low relative to how many copies you need

---

## Image Cropping

When you upload a portrait via file picker, a crop tool opens automatically:
- **Unit portraits** — locked to square (1:1) crop
- **Comp banners** — free crop
- Tools: Rotate Left/Right, Flip Horizontal/Vertical, Reset

---

## Data Persistence

All data is stored in your browser's `localStorage`. It persists across sessions as long as you don't clear browser data.

### Backup & Restore
Use the **↑ Export** and **↓ Import** buttons in the top-right corner of the header.

- **Export** downloads a `.json` file with all units, comps, pool state, owned stars, and scout entries
- **Import** loads a previously exported file and replaces current data

> Export regularly if you've built out a large set of comps.

---

## Tech Stack

| Piece | Details |
|-------|---------|
| Runtime | Plain HTML + vanilla JS — no framework, no build step |
| Persistence | Browser `localStorage` |
| Images | Community Dragon CDN (portraits loaded from URL, not stored locally) |
| Cropping | [Cropper.js 1.5.13](https://github.com/fengyuanchen/cropperjs) via CDN |
| Champion data | [Community Dragon](https://raw.communitydragon.org/latest/cdragon/tft/en_us.json) |

---

## Updating for a New TFT Set

When a new set releases:
1. Go to the **Units tab**
2. Click **⚙ Import from Community Dragon** → **Fetch Automatically**
3. Confirm the replacement — the tool always imports the latest set automatically
4. Re-add your comps on the **Comps tab** (or update existing ones)

Champion portraits update automatically since they load directly from Community Dragon's CDN.
