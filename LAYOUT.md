# Project Layout & Structure

This document is the single reference for how a project in this repository is laid out and structured. Follow it so every project stays consistent, predictable, and easy to reason about.

## 📁 Standard Layout

Every project follows this skeleton:

```
<project-name>/
├── index.html          # The only HTML entry point
├── manifest.json       # Central settings (optional — only if the project needs settings)
└── src/
    ├── app.js          # Main entry script
    ├── style.css       # Entry stylesheet (imports only)
    ├── css/            # Any additional CSS files
    │   ├── ...
    │   └── ...
    └── js/             # Any additional JS files
        ├── ...
        └── ...
```

- `index.html` — the entry point. Always at the project root.
- `manifest.json` — the central settings file. Present **only** if the project needs settings.
- `src/app.js` — the main script. Kept at `src/` level, not inside `src/js/`.
- `src/style.css` — the entry stylesheet. Contains only `@import`s.
- `src/css/` — all other CSS files.
- `src/js/` — all other JavaScript files.

## ⚙️ Settings via `manifest.json`

Settings are kept in **one central place**: `manifest.json`.

```json
{
  "title": "My Project",
  "theme": "dark",
  "features": {
    "enableX": true,
    "size": 12
  },
  "extraConfig": "config/extra.json"
}
```

Key:value pairs in `manifest.json` hold the project's own settings.

### Declaring extra setting files

If a project needs additional setting files, it **must declare them in `manifest.json`**. No setting file may be loaded unless it is declared there first. Declarations use a `name: relative/path/another.json` form:

```json
{
  "extraConfig": "config/extra.json",
  "localeFile": "config/locale-en.json"
}
```

- Paths are **relative** to the project root.
- The key is the name the project refers to it by; the value is the relative path.
- A setting file that isn't declared in `manifest.json` shouldn't exist as far as the project is concerned.

## 🎨 CSS — One Import, Many Files

`index.html` imports exactly **one** stylesheet: `src/style.css`. That file is the **entry stylesheet** and contains no rules of its own — only `@import` statements pulling in the rest:

```css
/* src/style.css */
@import url("./css/reset.css");
@import url("./css/layout.css");
@import url("./css/theme.css");
```

- Paths inside `@import` are relative to `style.css` itself, so `./css/...` resolves to `src/css/...`.
- All other CSS files live in `src/css/`.
- `@import` rules must appear before any other rule in the file. Keep `style.css` as a pure index so this is never an issue.
- Order is explicit: imports cascade top to bottom, so load order is readable in one place.

## 🧩 JavaScript — One Import, Many Files

`index.html` imports exactly **one** script: `src/app.js`. That file is the **entry script** and imports everything else:

```js
// src/app.js
import { initSomething } from "./js/something.js";
import { setupThing } from "./js/thing.js";
```

- `src/app.js` is loaded as a module so ES imports work.
- All other JS files live in `src/js/`.
- Paths are relative to `app.js`, so `./js/...` resolves to `src/js/...`.

## 🚫 No Inline CSS or JavaScript

- **No inline `<style>` blocks.**
- **No inline `style="..."` attributes.**
- **No inline `<script>` blocks.**
- **No inline event handlers** (`onclick="..."`, `onload="..."`, etc.).

All CSS lives under `src/css/`, all JavaScript under `src/js/` (plus the two entry files `src/style.css` and `src/app.js`).

## 🔗 HTML Imports — At Most Two

`index.html` may reference **only two** files:

1. **The entry stylesheet** — `src/style.css`
2. **The entry script** — `src/app.js`

```html
<link rel="stylesheet" href="./src/style.css" />
<script type="module" src="./src/app.js"></script>
```

That's it. Nothing else gets imported from `index.html`. Everything else is pulled in from these two entry points.

## ✅ Rules at a glance

| Rule | Detail |
| --- | --- |
| Entry HTML | Exactly one — `index.html` at the project root |
| Settings | Centralised in `manifest.json` (only if needed) |
| Extra setting files | Must be declared in `manifest.json` as `name: relative/path/another.json` |
| Entry stylesheet | One file — `src/style.css`, containing only `@import`s |
| Other CSS files | Live in `src/css/`, imported by `src/style.css` |
| Entry script | One file — `src/app.js` |
| Other JS files | Live in `src/js/`, imported by `src/app.js` |
| Inline CSS | ❌ Not allowed |
| Inline JS | ❌ Not allowed |
| HTML imports | At most two — `src/style.css` and `src/app.js` |

## 🧪 Minimal example

```
my-project/
├── index.html
├── manifest.json
└── src/
    ├── app.js
    ├── style.css
    ├── css/
    │   ├── reset.css
    │   └── theme.css
    └── js/
        └── helper.js
```

**index.html**

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <title>My Project</title>
  <link rel="stylesheet" href="./src/style.css" />
</head>
<body>
  <main id="app"></main>
  <script type="module" src="./src/app.js"></script>
</body>
</html>
```

**manifest.json**

```json
{
  "title": "My Project",
  "extraConfig": "config/extra.json"
}
```

**src/style.css**

```css
@import url("./css/reset.css");
@import url("./css/theme.css");
```

**src/css/reset.css**

```css
* { box-sizing: border-box; margin: 0; }
```

**src/css/theme.css**

```css
:root { --bg: #111; --fg: #eee; }
body { background: var(--bg); color: var(--fg); }
```

**src/app.js**

```js
import { boot } from "./js/helper.js";
boot();
```

**src/js/helper.js**

```js
export function boot() {
  document.getElementById("app").textContent = "Hello.";
}
```