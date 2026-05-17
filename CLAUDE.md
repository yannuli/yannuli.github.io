# Yannu Portfolio — Claude Code Context

## What this project is

A full rebuild of Yannu Li's personal portfolio site. The old site lives at
`C:\Users\Administrator\Desktop\old-website` and is the content source for migration.
The new site lives at `C:\Users\Administrator\portfolio` and is what we're building.

---

## Audit of the old site (what needed modernising)

| Area | Old site | New site decision |
|---|---|---|
| Grid system | Foundation 6 (`.large-12.columns`, `.row`) | CSS Grid / Flexbox |
| JS dependencies | jQuery 3.3.1, parallax.min.js | Vanilla JS only |
| Analytics | Universal Analytics `UA-111857537-1` (deprecated) | Remove entirely (add GA4 later if needed) |
| IE compat | IE conditional comments, `browsehappy` banner | Dropped — modern browsers only |
| CSS architecture | Monolithic `main.css` with embedded normalize.css v3.0.3 | Inline `<style>` in each page; shared tokens in `styles/tokens.css` |
| Fonts | External TypeNetwork CDN + broken `http://yannuli.com/assets/Futura.ttc` | Self-hosted: Canela-Light.ttf + Helvetica-NeueLT55Roman-light.woff2 in `fonts/` |
| Color scheme | Light-only | Dark/light mode via `data-theme` on `<html>` |
| Parallax | `parallax.min.js` library | `requestAnimationFrame` vanilla JS |
| Scroll reveal | None | `IntersectionObserver` with `.reveal` / `.reveal-d1` delay classes |
| About page | Stale ("1st year HCI masters at GaTech"), old email `yannuli@gatech.edu`, 2018 copyright | Needs full rewrite |

---

## Architecture decisions

### File structure
```
portfolio/
  index.html          ← homepage (self-contained, all CSS inline)
  fonts/              ← Canela-Light.ttf, Helvetica-NeueLT55Roman-light.woff2
  assets/             ← images copied from old-website
  styles/
    tokens.css        ← design system tokens (reference only, NOT linked from index.html)
    homepage.css      ← CSS mirror of homepage styles (reference only)
  design/             ← case study pages (to be created)
    EA-do/
    EA-mobile login/
    smartdevops/
    yunnan1/
    Google-2020/
    EA-checkout/
  about/              ← about page (to be created)
```

### CSS approach
Each page is **self-contained** — all CSS lives in an inline `<style>` block in that page's
`<head>`. `tokens.css` and `homepage.css` exist as references but are **not linked** from
any page. When migrating a new page, copy the token `:root` block from `index.html` and
write page-specific CSS below it.

### Theming
- Light mode is default: `<html lang="en" data-theme="light">`
- Dark mode toggled via JS by swapping `data-theme` to `"dark"`
- CSS vars scoped: `:root` = light, `[data-theme="dark"]` = dark overrides
- Light palette: `--bg: #ffffff`, `--fg: #000000`, `--fg-muted: #555555`
- Dark palette: `--bg: #000000`, `--fg: #ffffff`, `--fg-muted: #c4c4c4`
- Logo gradient: orange (`#e38956`) in light, yellow-green (`#e3da56`) in dark
- Theme toggle button: moon icon in light mode, sun icon in dark mode

### Scroll reveal
Add class `reveal` to any element. Add `reveal-d1` through `reveal-d4` for staggered delays.
IntersectionObserver in the page `<script>` block adds `is-visible` when element enters view.

### Parallax
Hero section has three layers (`#hill-back`, `#hero-sun`, `#hill-front`) animated via
`requestAnimationFrame` reading `window.scrollY`. Depth coefficients: back hill 0.15,
sun 0.08 (upward), front hill 0.25.

---

## Homepage — DONE

`portfolio/index.html` is complete and contains real content.

### Sections
1. **Header** — fixed, goes opaque + shrinks on scroll (`is-scrolled` class), logo + About nav link + theme toggle
2. **Hero** — inline SVG hills + CSS sun glow, vanilla parallax, name + bio text
3. **Featured work** — 4 project cards
4. **Side projects** — 2 project cards
5. **Hobbies / About** — 4 hobby tiles (anchor `id="about"`)

### Project cards (all `href="#"` until case study pages are built)
| Section | Title | Image | href target |
|---|---|---|---|
| Featured | EA App Testing Tool | `assets/EA-do-0.png` | `design/EA-do/` |
| Featured | EA Mobile Login | `assets/EA-ml-0.png` | `design/EA-mobile login/` |
| Featured | DevOps Platform for Tencent | `assets/devops1.png` | `design/smartdevops/` |
| Featured | Go-Yunnan Redesign | `assets/yunnan1-1.png` | `design/yunnan1/` |
| Side | Ment | `assets/g-cover.png` | `design/Google-2020/` |
| Side | Checkout Experience for EA | `assets/EA-checkout.png` | `design/EA-checkout/` |

### Hobby tiles
| Label | Image |
|---|---|
| I'm climbing. | `assets/hobby-climbing.png` |
| I'm drawing | `assets/C4D-1.jpg` |
| I'm photographing and video editing | `assets/p-cover.png` |
| I'm reading and writing | *(no image yet — thumb is empty)* |

### Project card CSS note
Cards are `<a class="hp-project">` (anchor wrapping block content — valid HTML5).
The `.hp-project` rule must include `color: inherit; text-decoration: none;` to suppress
default anchor styling.

---

## What still needs to be built

### Case study pages (priority order matches homepage card order)
Each page should follow the same pattern as `index.html`:
- Self-contained `<style>` block with token `:root` + page-specific CSS
- No jQuery, no Foundation grid, no IE comments
- Self-hosted fonts via `@font-face`
- Dark/light theme toggle (copy the `<script>` block from `index.html`)
- Back navigation to homepage

Source content for each is in `C:\Users\Administrator\Desktop\old-website\design\<folder>\index.html`.

| Page | Old source path | New target path |
|---|---|---|
| EA App Testing Tool | `old-website/design/EA-do/index.html` | `portfolio/design/EA-do/index.html` |
| EA Mobile Login | `old-website/design/EA-mobile login/index.html` | `portfolio/design/EA-mobile login/index.html` |
| DevOps for Tencent | `old-website/design/smartdevops/index.html` | `portfolio/design/smartdevops/index.html` |
| Go-Yunnan Redesign | `old-website/design/yunnan1/index.html` | `portfolio/design/yunnan1/index.html` |
| Ment | `old-website/design/Google-2020/index.html` | `portfolio/design/Google-2020/index.html` |
| Checkout for EA | `old-website/design/EA-checkout/index.html` | `portfolio/design/EA-checkout/index.html` |

### About page
Source: `C:\Users\Administrator\Desktop\old-website\about\index.html`
Target: `portfolio/about/index.html`

The old about page content is **stale** — it describes Yannu as a 1st-year HCI masters student.
Needs a full content rewrite to reflect current role (Product designer at EA).
Old contact email `yannuli@gatech.edu` should be replaced with `yannuli.design@gmail.com`.

### Remaining homepage item
- Find or create a photo for the "I'm reading and writing" hobby tile.

---

## Old site reference paths
- Homepage: `C:\Users\Administrator\Desktop\old-website\index.html`
- Styles: `C:\Users\Administrator\Desktop\old-website\styles\main.css`
- Images: `C:\Users\Administrator\Desktop\old-website\images\`
- Case studies: `C:\Users\Administrator\Desktop\old-website\design\<name>\index.html`
- About: `C:\Users\Administrator\Desktop\old-website\about\index.html`
