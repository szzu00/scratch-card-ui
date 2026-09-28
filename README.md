# Scratch Card UI

FiveM NUI for a Los Santos Rubels scratch card. Frontend only — HTML, CSS, and Vue 3. No Lua backend and no framework dependency.

**Author:** szzu00  
**Contact:** Discord `szzu00`

---

## Framework support

| ESX | QBCore | Other frameworks |
|-----|--------|------------------|
| Not required | Not required | UI only — wire it to your own resource |

This package is the UI layer. Show / hide it and pass data from your own client script via NUI messages.

---

## Requirements

- [FiveM](https://fivem.net/) (for in-game use)
- Modern browser (for local preview)
- Vue 3 via CDN (already linked in `script.js`)

---

## Installation

1. Put the `scratch-card-ui` folder into your resource (or copy `frontend/` into an existing resource).
2. Point your `fxmanifest.lua` `ui_page` at the HTML entry:
   ```lua
   ui_page 'frontend/index.html'

   files {
       'frontend/index.html',
       'frontend/assets/**/*'
   }
   ```
3. Open / close the NUI from your client script (`SetNuiFocus`, `SendNUIMessage`, etc.).
4. Restart the resource / server.

### Local preview

Open `frontend/index.html` in a browser (or serve the `frontend/` folder). No build step.

---

## How it works

1. `#app` mounts a Vue 3 app (`assets/js/script.js`).
2. Tile look is controlled by CSS state classes on `.tile` (`unopen`, `win`, `lose`).
3. When the board has `.done`, the result overlay (`.result`) is shown.
4. Result variant is set with `.result.win` or `.result.lose`.

Scaling uses `html { font-size: 1vh }` so all `rem` sizes scale with viewport height.

---

## Changing colors (`frontend/assets/css/colors.css`)

All accent colors come from CSS variables. Edit this file only — components already use `var(--green*)`.

```css
:root {
    --green: #53FFA9;
    --green-dark: #329965;
    --green-a50: rgba(83, 255, 169, 0.5);
    --green-dark-a50: rgba(50, 153, 101, 0.5);
    --green-a25: rgba(83, 255, 169, 0.25);
    --green-dark-a25: rgba(50, 153, 101, 0.25);
    --green-dark-a20: rgba(50, 153, 101, 0.2);

    --green-hue: 150;
}
```

| Variable | Used for |
|----------|----------|
| `--green` | Gradients, borders, win text, result ring |
| `--green-dark` | Gradient end |
| `--green-a50` / `--green-dark-a50` | Win result disc |
| `--green-a25` / `--green-dark-a25` | Win tile background, panel glow |
| `--green-dark-a20` | Panel inset shadow |
| `--green-hue` | Character `hue-rotate` (match the accent hue) |

Lose state is fixed red (`#ED474A` / `#87282A`) in `base.css`, not via these variables.

Example — switch accent to red for testing:

```css
--green: #FF5353;
--green-dark: #993232;
--green-a50: rgba(255, 83, 83, 0.5);
/* …same pattern for a25 / a20… */
--green-hue: 0;
```

---

## State classes

### Tiles

| Class | Role |
|-------|------|
| `.tile.unopen` | Closed tile |
| `.tile.win` | Revealed win |
| `.tile.lose` | Revealed lose |

Interactive tiles also use `.btn.press`.

### Result overlay

| Class | Role |
|-------|------|
| `.board.done` | Shows the result overlay |
| `.result.win` | Win styling |
| `.result.lose` | Lose styling (hides the amount) |

---

## Vue 3 (`assets/js/script.js`)

Mounted via CDN ES module. Composition API with `setup()`:

```js
import { createApp } from 'https://cdn.jsdelivr.net/npm/vue@3.5.13/dist/vue.esm-browser.js'

createApp({
    setup() {
        return { }
    }
}).mount('#app')
```

- Root uses `v-cloak` (hidden until Vue is ready).
- Script tag must stay `type="module"`.
- Extend `setup()` for open/scratch logic and NUI message handlers when you add the backend.

---

## File structure

```
scratch-card-ui/
├── README.md
└── frontend/
    ├── index.html
    └── assets/
        ├── css/
        │   ├── fonts.css
        │   ├── colors.css
        │   ├── base.css
        │   ├── utilities.css
        │   └── animations.css
        ├── fonts/
        ├── img/
        └── js/
            └── script.js
```

---

## Notes

- UI-only package — no `fxmanifest.lua` / Lua config shipped; add your own resource wrapper.
- Preview texts and tile outcomes are static in HTML until wired to Vue / NUI.
- Provided as-is for use on your own FiveM server.
- Questions / support: Discord **szzu00**
