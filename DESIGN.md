# Design System — palo.mx

## Product Context
- **What this is:** Sports betting odds tracker for Mexican and US leagues (Liga MX, MLB, NBA, NFL, La Liga, Champions League, Premier League)
- **Who it's for:** Spanish-speaking bettors who want prop lines, player stats, and edge analysis in one place
- **Space/industry:** Sports betting / fantasy / data tools
- **Project type:** Single-page web app (data-dense, real-time, mobile-friendly)

---

## Aesthetic Direction
- **Direction:** Retro-Futuristic Pixel Arcade Fiesta
- **Decoration level:** Intentional — pixel art logo, step-end blink animation, monospace data; decoration earns its place
- **Mood:** The inside of a Mexican arcade cabinet at midnight. Dark, electric, a little chaotic. Data-dense but never clinical. The kind of app you actually want to keep open.
- **Anti-patterns avoided:** No purple gradients, no card grids with icon circles, no centered hero sections, no bubbly border-radius everywhere

---

## The Piñata Palette

The logo is a 12×12 pixel piñata. Its seven colors ARE the design system. Every CSS variable maps back to a stripe on that piñata.

```css
:root {
  /* ── background stack ── */
  --bg:        #0a0509;   /* near-black maroon — the void */
  --surface:   #150d10;   /* card / panel background */
  --surface-2: #1a0d12;   /* hover state, expanded rows */
  --border:    #2a1520;   /* dividers, row separators */
  --border-2:  #3a1525;   /* stronger borders, inactive controls */

  /* ── piñata palette (all 7 colors, each with a semantic role) ── */
  --magenta:   #f5146e;   /* negative odds, active UI state */
  --gold:      #f5c518;   /* brand, INSERT COIN, highlights */
  --green:     #1ed760;   /* positive odds, statistical edge */
  --orange:    #f47c1a;   /* hot market, value flag (reserved) */
  --red:       #cc1a1a;   /* danger: heavy juice −200+, warnings */
  --blue:      #4a90d9;   /* BA stats, neutral data, links */

  /* ── text ── */
  --muted:     #6b4f58;   /* de-emphasized text, labels */
  --text:      #f0e6ea;   /* primary body text — warm off-white */
}
```

### Semantic usage

| Variable | Role | Example |
|---|---|---|
| `--magenta` | Negative odds, active state, brand CTA | `−115` odds, active tab underline, match pill |
| `--gold` | Brand, INSERT COIN tagline, highlights | Logo dot, blink animation |
| `--green` | Positive odds, edge %, filter hit | `+130` odds, hit rate bars |
| `--orange` | Hot market, promo line (reserved for future) | Trending props |
| `--red` | Danger — heavy juice −200+, errors | `−200` filter button when active |
| `--blue` | Neutral stats — BA, AB, non-directional data | `.312` batting average |
| `--muted` | De-emphasized — labels, inactive, secondary | Position, team ID, filter status |
| `--text` | Primary body text | Player names, odds values |

---

## Typography

Three fonts, three jobs. No overlap.

- **Pixel / Logo:** `Press Start 2P` — logo wordmark, INSERT COIN tagline, and any Easter eggs. Never use for body text or data. Sizes: 11px (logo), 6–7px (tagline/micro labels).
- **Data / Mono:** `Geist Mono` — all numeric data (odds, hit rates, BA, AB, edge %). Always set with `font-variant-numeric: tabular-nums` so columns align.
- **UI / Body:** `DM Sans` — player names, team names, filter labels, any prose. Weights: 400 (body), 500 (emphasis), 600 (headings).

**Loading:** Google Fonts CDN — `Press Start 2P`, `Geist Mono:wght@400;500;600`, `DM Sans:wght@400;500;600`

**Scale:**
| Role | Font | Size |
|---|---|---|
| Logo wordmark | Press Start 2P | 11px |
| Micro labels (INSERT COIN, sport tabs) | Press Start 2P | 6–7px |
| Odds values | Geist Mono | 13px / 600 |
| Stat data (BA, hit rate, edge) | Geist Mono | 9–10px |
| Player names | DM Sans | 14px / 500 |
| Filter labels, secondary | DM Sans / Geist Mono | 10–11px |

---

## Layout

- **Approach:** Grid-disciplined — everything inside `max-width: 960px`, centered, `px-4` gutters
- **Header:** Sticky, blurred, 3-zone (logo left / bet amount center / controls right)
- **Content:** Sport tabs → match pills → filter bar → market tables. Vertical flow, no sidebars.
- **Tables:** 4-column (player | stats | line | odds). Player cell uses `rowspan` for multi-row props.
- **Border radius:** `2px` everywhere — sharp, arcade-grid feel. No rounded-xl anywhere.
- **Max content width:** 960px

---

## Spacing

- **Base unit:** 4px
- **Density:** Compact — this is a data tool, not a landing page
- **Row padding:** `7px 10px` (table cells)
- **Section gaps:** `space-y-3` between market groups
- **Control padding:** `4–5px` vertical, `8–10px` horizontal

---

## Motion

- **Approach:** Minimal-functional with one expressive exception
- **Transitions:** `0.1–0.2s` on border-color and color changes (hover states, filter buttons)
- **Row expand:** instant — no animation, data appears immediately
- **INSERT COIN blink:** `step-end` animation at 1.1s — deliberately choppy, arcade-authentic. This is the one place where motion is personality, not utility.

---

## Component Patterns

### Odds coloring
```
positive (> 0)  → var(--green)
negative (< 0)  → var(--magenta)
null / missing  → var(--muted)
```

### Filter buttons
```
inactive  → border: --border-2, bg: --surface-2, color: --muted
active    → border: --magenta, bg: --magenta, color: #fff
```

### −200 heavy line button
```
inactive  → border: --border-2, bg: --surface-2, color: --muted
active    → border: --red, bg: --red, color: #fff
(red signals danger/warning, distinct from magenta filter state)
```

### Row states
```
default   → bg: transparent
hover     → bg: var(--surface-2)
expanded  → bg: var(--surface-2) persistent (class: row-expanded)
hover on expanded → bg: var(--border) (slightly brighter)
```

### BA / batting average stat
```
color: var(--blue) — neutral, non-directional data
```

---

## Decisions Log

| Date | Decision | Rationale |
|---|---|---|
| 2026-04-02 | Initial design system — "pixel arcade fiesta" | Retro-futuristic aesthetic matching pixel piñata logo and palo.mx brand |
| 2026-04-02 | Added --orange, --red, --blue | Piñata has 7 colors; design system only used 3. All 7 now named and semantic. |
| 2026-04-02 | −200 button uses --red, not --magenta | Red = danger/warning (heavy juice). Magenta = active filter. Separate semantic roles. |
| 2026-04-02 | BA stat color #5eafc6 → var(--blue) | Unified hardcoded blue into the named palette. |
| 2026-04-02 | Expanded rows keep hover color persistent | Visual affordance — shows which player is selected without losing context on mouseout. |
| 2026-04-02 | "no pierdas el tino" footer uses --muted | Was --border-2 (#3a1525) — essentially invisible. Now readable but still de-emphasized. |
