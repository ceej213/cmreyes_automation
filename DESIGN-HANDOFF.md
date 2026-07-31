# Handoff: CMREYES Automation — Portfolio Site

## Overview

A single-page personal portfolio for **Carlos Javier Reyes** (CMREYES Automation), an AI-automation freelancer. It presents four highlighted client builds plus five secondary builds, an about/timeline section, and a contact section. Primary business goals: get a prospect to (a) watch a Loom walkthrough of a flagship build, or (b) send an enquiry email.

The page has four scroll sections — **Hero, Work, About, Contact** — reachable from a fixed vertical icon rail. Project cards open a **screenshot lightbox** with keyboard/arrow navigation.

## About the Design Files

The files in this bundle are **design references created in HTML** — a working prototype demonstrating intended look, motion, and behavior. They are **not production code to lift directly**.

`CMREYES Portfolio v3.dc.html` is authored in a proprietary in-house component format (a `<x-dc>` template with `{{ }}` value holes plus a `Component` logic class, driven by `support.js`). **Do not try to port `support.js` or the `<x-dc>` / `<sc-for>` / `<sc-if>` / `hint-*` syntax** — it is a prototyping runtime, not a shipping framework.

The task is to **recreate this design in the target codebase's environment** using its established patterns and libraries. If no codebase exists yet, Next.js (App Router) + Tailwind is a natural fit — the design is a static marketing page with light client state.

Direct translation of the prototype's concepts:

| Prototype construct | Target equivalent |
| --- | --- |
| `<x-dc>` template + `Component` class | One page component with local state |
| `renderVals()` return object | Derived values / props / handlers |
| `<sc-for list as>` | `.map()` |
| `<sc-if value>` | Conditional render |
| `style-hover` / `style-focus` attrs | `:hover` / `:focus` CSS or Tailwind variants |
| `static GALLERIES` / `static PROJECTS` | A typed content module (`content/projects.ts`) or CMS |
| `data-props` JSON | Feature flags / config, or delete and hardcode |

**Content is the valuable part of this bundle.** The copy in `PROJECTS` and `GALLERIES` is final, client-approved, and should be transcribed exactly — em dashes, capitalisation, and all.

## Fidelity

**High-fidelity.** Colors, typography, spacing, radii, shadows, motion curves, and copy are final. Recreate pixel-accurately using the codebase's existing primitives. Every value needed is enumerated in **Design Tokens** below.

Two deliberate design decisions to preserve:
1. **Featured vs. secondary cards are visually distinct** — featured cards are taller (250px cover vs 190px), carry a "HIGHLIGHT" badge, a blue-tinted border, a larger title, and a Loom link. This hierarchy is the point of the section; don't flatten it into one uniform grid.
2. **Dark is the default theme.** Light mode is a supported alternate, persisted to `localStorage`.

## Screens / Views

Single page, four sections, plus one overlay. Max content width **1180px**, centered, with `padding: 34px 34px 34px 118px` (the large left padding clears the fixed icon rail). Sections are stacked in a flex column with **30px** gap.

### 0. Icon Rail (fixed, global)

- **Purpose**: section navigation + theme toggle.
- **Layout**: `position: fixed; left: 26px; top: 50%; translateY(-50%)`, `z-index: 60`. Vertical flex, `gap: 10px`, `padding: 14px 10px`, `border-radius: 999px`, background `--panel`, `1px solid --line`, shadow `0 0 26px rgba(59,111,224,0.28)`.
- **Items**: 5 circular 40×40 buttons — Home (`#top`), Work (`#work`), About (`#about`), Contact (`#contact`), theme toggle. Each: `border-radius: 50%`, `1px solid --line`, color `--muted`, 18×18 stroked SVG icon (`stroke-width: 1.9`, round caps/joins).
- **Active state** (driven by scroll position): background `--accent`, color `#fff`, border `--accent2`, shadow `0 6px 20px rgba(59,111,224,0.5), 0 0 0 4px rgba(59,111,224,0.14)`, `transform: scale(1.1)`; the inner SVG scales to `1.06`. Transition `.32s cubic-bezier(.22,1,.36,1)` on color/border/background, `.38s cubic-bezier(.34,1.56,.64,1)` (overshoot spring) on transform.
- **Hover** (inactive): color `--fg`, border `--line2`, background `--panel2`.
- **Active-press**: `transform: scale(1.04)`.
- **Theme toggle** glyph: `☀` in dark mode, `☾` in light mode.
- **Responsive** (≤1100px): becomes a *horizontal* pill docked bottom-center — `left: 50%; bottom: 16px; translateX(-50%); flex-direction: row; padding: 10px 14px`. At ≤620px `gap: 6px`.

### 1. Hero (`#top`)

- **Purpose**: state who Carlos is and what he sells within one screen.
- **Layout**: panel card (see Panel spec below), `padding: 56px 52px`. Inner grid `1fr 360px`, `gap: 52px`, `align-items: center`. Collapses to one column ≤1100px.
- **Dot-grid overlay**: absolutely positioned, inset 0 — `radial-gradient(rgba(120,160,220,0.16) 1px, transparent 1.4px)`, `background-size: 28px 28px`, masked by `radial-gradient(700px 460px at 20% 10%, #000, transparent 78%)`, `pointer-events: none`.
- **Availability pill** (optional, flag `showAvailability`): self-start, `padding: 7px 15px 7px 12px`, `border-radius: 999px`, background `--panel2`, `1px solid --line`. Contains an 8px `#34D399` dot animating `pulseDot 2.4s ease-out infinite` (expanding 0→7px green ring), then 13px/500 `--muted` text **"Available for new projects"**.
- **H1**: Space Grotesk 700, `clamp(36px, 5vw, 60px)`, `line-height: 1.06`, `letter-spacing: -0.03em`, `text-wrap: balance`. Copy: **"Automate the busywork."** / line break / **"Keep the savings."** — the second line uses the gradient-text treatment (see Design Tokens).
- **Lede**: 18px, `line-height: 1.65`, `--muted`, `max-width: 56ch`, `text-wrap: pretty`. Exact copy:
  > I'm **Carlos Reyes**. I build AI voice agents, chatbots, and workflow automations that turn slow, manual processes into money saved — fewer staff hours spent on repetitive admin, less revenue leaking through missed calls and bookings, and processes that finish in minutes instead of days.

  ("Carlos Reyes" is `--fg`, weight 600.)
- **Toolbox row**: label "TOOLBOX" (11.5px/600, `0.1em` tracking, uppercase, `--faint`), then 9 chips — `n8n, Zapier, Power Automate, PowerApps, ElevenLabs, Claude AI, WhatsApp API, Supabase, GoHighLevel`. Chip: 13px/500, `--muted`, background `--panel2`, `1px solid --line`, `border-radius: 999px`, `padding: 6px 13px`.
- **Right column**: stacked logo lockup (dark/light variants swapped by theme) with `floatY` idle animation (±10px, 
  and a caption "AI · Automation · Business Systems" at 12.5px `--faint`, `letter-spacing: 0.04em`.

### 2. Work (`#work`)

- **Purpose**: the portfolio proper. Nine projects in two tiers.
- **Header**: H2 **"Selected Work"** (Space Grotesk 700, 34px, `-0.02em`; "Work" in gradient text), then 15px `--muted` subtitle: *"Real problems, shipped solutions — click any project to view screenshots"*.
- **Tier dividers**: a row of `[label][1px flex line]` with `gap: 14px`, `margin-bottom: 20px`. Labels: **"SELECTED WORK"** (11.5px/700, `0.12em`, uppercase, `--accent2`) and **"MORE BUILDS"** (same but `--muted`). The featured grid has `margin-bottom: 46px` before the second divider.
- **Both grids**: `display: grid; grid-template-columns: 1fr 1fr; gap: 24px`. One column ≤1100px.

**Featured card** (4 of them) — `cursor: pointer`, flex column, `border-radius: 20px`, `overflow: hidden`, background `--panel2`, border `1px solid rgba(59,111,224,0.34)`, shadow `0 10px 30px rgba(3,7,18,0.35)`.
- Cover: `height: 250px`, `overflow: hidden`, `border-bottom: 1px solid --line`, background `#0A0F1A`; `<img>` at `object-fit: cover; object-position: center top`.
- **HIGHLIGHT badge**: absolute `top: 14px; right: 14px`; 10.5px/700, `0.1em` tracking, uppercase, color `#EAF1FF`, background `rgba(59,111,224,0.9)`, `border-radius: 999px`, `padding: 5px 11px`, shadow `0 6px 18px rgba(3,7,18,0.45)`.
- Body: `padding: 26px 26px 28px`, flex column, `gap: 13px`, `flex: 1`.
  - Kicker: 11.5px/700, `0.09em`, uppercase, `--accent2`.
  - Title: Space Grotesk 600, **23px**, `line-height: 1.22`, `-0.015em`.
  - Two paragraphs at 14.5px, `line-height: 1.6`, `--muted`, each prefixed by a `--fg` 600-weight run: **"Problem —"** and **"Solution —"**.
  - Stack chips: `margin-top: auto; padding-top: 6px`, flex wrap `gap: 7px`. Chip: 12px/600, `--accent2`, background `rgba(59,111,224,0.12)`, `1px solid rgba(59,111,224,0.22)`, `border-radius: 6px`, `padding: 4px 9px`.
  - **Loom row** (featured only): `border-top: 1px solid --line`, `padding-top: 16px`, `margin-top: 4px`, flex `gap: 14px`, centered. Contains a pill link — inline-flex, `gap: 9px`, 14px/600, `--accent2`, background `rgba(59,111,224,0.12)`, `1px solid rgba(59,111,224,0.3)`, `border-radius: 999px`, `padding: 9px 16px 9px 13px`, label **"Watch the walkthrough"**, preceded by a 20px `--accent2` circle containing a `▶` glyph (9px, `padding-left: 2px` to optically center). Hover: background `rgba(59,111,224,0.22)`, border `--accent2`, `translateY(-1px)`. Then 12.5px `--faint` text "Loom · opens in a new tab". `target="_blank" rel="noopener"`.
  - ⚠️ **The Loom link must `stopPropagation()` on click** — the whole card is a click target that opens the lightbox, and clicking the link must not also open it.

**Secondary card** (5 of them) — same shape, but: border `1px solid --line` (no blue tint), no card shadow, no badge, no Loom row; cover **190px**; body `padding: 22px 24px 24px`, `gap: 12px`; title **20px** / `line-height: 1.25` / `-0.01em`; paragraphs **14px**.

**Both cards — hover**: `translateY(-6px)`, border `--accent2`, shadow `0 22px 48px rgba(59,111,224,0.28)`; transition `.2s ease` on transform/shadow/border. **Press**: `scale(.99)`.

**Scroll reveal**: cards start `opacity: 0; translateY(14px)` and animate to rest over `.32s cubic-bezier(.22,1,.36,1)` as they enter the viewport (IntersectionObserver, `rootMargin: 0px 0px -8% 0px`, `threshold: 0.08`), staggered `60ms` apart capped at 5 steps. Cards already on screen at load reveal immediately with no stagger. If `IntersectionObserver` is unavailable, all cards show unanimated. **Arm the hidden state with transitions suppressed for one frame** so the initial paint doesn't animate.

### 3. About (`#about`)

- Panel, `padding: 52px`, content `max-width: 820px`, flex column `gap: 22px`.
- H2 **"About Me"** ("Me" in gradient text), 34px.
- Pull-quote line: Space Grotesk 500, 20px, `line-height: 1.45`, `--fg`, `max-width: 26ch`, `text-wrap: balance` — *"Ten years of making systems talk to each other"*.
- Two body paragraphs, 16px, `line-height: 1.7`, `--muted`, `max-width: 66ch` (verbatim in the prototype; `<em>` on "and" in the first).
- Byline row: 14px `--faint`, `gap: 10px` — **"Carlos Javier Reyes"** (Space Grotesk 600, 15px, `--fg`) · "Muntinlupa City, Philippines — works with clients worldwide".
- **Timeline** (optional, flag `showTimeline`): rows of `grid-template-columns: 140px 1fr`, `gap: 20px`, `padding: 16px 0`, separated by `1px solid --line` (also a top border on the container, `margin-top: 10px`). Years: 13.5px/600, `--accent2`, `font-variant-numeric: tabular-nums`. Title 15px/600 `--fg`; org 14px `--muted`. Collapses to a single column at ≤620px with `gap: 4px`.
  - 2024 – 2025 · Legislative Assistant II · Senate of the Philippines · Office of Senator Mark Villar
  - 2022 – 2024 · IT Head · Motiontrade Development Corporation
  - 2017 – 2022 · Business Analyst II · Insular Life Assurance Co., Ltd.
  - 2015 – 2017 · Software / Web Developer · Artise De Solution · Las Piñas City Hall

### 4. Contact (`#contact`)

- Panel, `padding: 56px 52px`, contents centered (`align-items: center; text-align: center`, `gap: 18px`).
- Decorative blur: absolute `top: -160px; right: -80px`, 420×420 circle, `radial-gradient(circle, rgba(59,111,224,0.28), transparent 65%)`, `filter: blur(30px)`.
- H2: `clamp(28px, 4vw, 40px)`, `max-width: 24ch` — **"Got a process that eats your time?"** + gradient **"Tell me about it."**
- Sub: 16.5px, `--muted`, `max-width: 48ch` — *"Send a short note about what you're trying to automate — I'll reply within a day with an honest take on whether it's worth building."*
- **Form** (`max-width: 580px`, `text-align: left`, `gap: 12px`): a 2-col grid (`gap: 12px`, 1-col ≤620px) of "Your name" / "Your email" inputs, then a 4-row "What would you like to automate?" textarea (`resize: vertical`). Field style: 15px, `--fg`, background `--panel2`, `1px solid --line`, `border-radius: 12px`, `padding: 13px 15px`, `outline: none`; **focus** → `border-color: --accent2`. Placeholders are `--faint`.
- **Submit button**: 16px/600, `#fff`, `background: linear-gradient(120deg, --accent, --accent2)`, no border, `padding: 15px 30px`, `border-radius: 999px`, shadow `0 10px 26px rgba(59,111,224,0.45)`. Hover: `translateY(-2px)`, shadow `0 16px 34px rgba(59,111,224,0.55)`. Label **"Send message"**.
- **Behavior**: builds a `mailto:` — subject `Automation inquiry from {name}` (or plain `Automation inquiry` if no name), body `{message}\n\n— {name} ({email})` (the email parenthetical omitted when blank), both `encodeURIComponent`'d, to **cmreyes@cmr-automation.com**. Helper text below: *"Opens your email app with the message pre-filled, addressed to cmreyes@cmr-automation.com"*.
  - 🔨 **Likely first change in production**: swap this for a real form POST (Resend / Formspree / a route handler) with success + error states. The prototype has neither.
- **Direct-contact pills**: `mailto:` and `tel:` buttons — 14.5px/600, `--fg`, background `--panel2`, `1px solid --line2`, `padding: 11px 22px`, `border-radius: 999px`; hover border `--accent2`. Labels "✉ cmreyes@cmr-automation.com" and "+63 917 316 3487" (phone optional, flag `showPhone`).

### 5. Footer

Flex row, `space-between`, wraps, `padding: 4px 10px 10px`, 13px `--faint`. Left: horizontal logo (44px tall; in dark mode `filter: brightness(0) invert(1); opacity: .92`) + "© 2026 Carlos Javier Reyes". Right: "n8n · Zapier · Power Platform · ElevenLabs · Claude · Supabase".

### 6. Screenshot Lightbox (overlay)

- **Trigger**: click any project card. **Dismiss**: overlay click, Close button, or `Escape`.
- **Overlay**: `position: fixed; inset: 0; z-index: 100`, `rgba(5,8,14,0.9)`, `backdrop-filter: blur(10px)`, centered, `padding: 36px` (12px ≤620px).
- **Panel**: `max-width: 1080px`, `max-height: 92vh`, flex column `gap: 14px`. Clicks inside must `stopPropagation()`.
- **Header row**: title (Space Grotesk 600, 19px, `#E9EFFA`) + counter `n / total` (13.5px/600, `rgba(233,239,250,0.55)`, tabular numerals); right-aligned **"Close ✕"** pill (14px/600, `rgba(233,239,250,0.1)` bg, `1px solid rgba(233,239,250,0.25)`, `border-radius: 999px`, `padding: 8px 18px`; hover border `rgba(233,239,250,0.7)`).
- **Image stage**: background `#0A0F1A`, `1px solid rgba(120,160,220,0.22)`, `border-radius: 16px`, `min-height: 320px`; image `width: 100%; max-height: 72vh; object-fit: contain`. On slide change the image replays `imgFade 140ms cubic-bezier(.22,1,.36,1)`.
- **Arrows** (only when the gallery has >1 shot): 44×44 circles at `left/right: 14px`, vertically centered, `rgba(10,15,26,0.7)` bg, `1px solid rgba(233,239,250,0.3)`, `#E9EFFA` `←`/`→` at 19px; hover background `--accent`. Navigation **wraps** in both directions.
- **Caption**: centered, 14.5px, `rgba(233,239,250,0.7)`.
- **Enter**: overlay `lbFade 200ms cubic-bezier(.22,1,.36,1)`; panel `lbPop 240ms cubic-bezier(.32,.72,0,1)` (from `opacity: 0, scale(.96) translateY(8px)`).
- **Exit**: a `closing` flag drives `lbFadeOut` / `lbPopOut` at **160ms**, and the component unmounts only after that timer — don't rip the node out on click or the exit animation never plays.
- **Keyboard**: `Escape` closes, `←`/`→` step. Bound on `document` at mount, removed at unmount.

## Interactions & Behavior

| Trigger | Behavior |
| --- | --- |
| Scroll | Rail active item updates to the section in view |
| Click project card | Opens lightbox at that project's gallery, slide 0 |
| Click Loom pill | Opens Loom in a new tab; **must not** open the lightbox |
| Click overlay / Close / `Esc` | Plays 160ms exit, then unmounts |
| `←` / `→` (lightbox open) | Previous / next shot, wrapping |
| Click arrows | Same, with `stopPropagation()` |
| Click theme toggle | Flips `data-theme` on the root, persists to `localStorage['cmr-theme']` |
| Card enters viewport | Reveal animation, staggered |
| Submit contact form | Builds and navigates to a `mailto:` URL |

**Reduced motion** (`@media (prefers-reduced-motion: reduce)`) — respect this; it is fully specified in the prototype: idle float and pulse animations off; lightbox in/out collapse to a 120ms fade; image fade off; card reveal keeps opacity only (no translate), 180ms, no stagger delay; rail transitions drop to 120ms and lose the scale; all `:active` press transforms off.

## State Management

Local component state only — no server state, no data fetching.

| State | Type | Notes |
| --- | --- | --- |
| `theme` | `'dark' \| 'light'` | Initialised from `localStorage['cmr-theme']`, default `'dark'` |
| `active` | `'top' \| 'work' \| 'about' \| 'contact'` | Current section, from scroll |
| `gallery` | `number \| null` | Index into `GALLERIES`; `null` = closed |
| `slide` | `number` | Current shot within the open gallery |
| `closing` | `boolean` | True during the 160ms lightbox exit |
| `ctName` / `ctEmail` / `ctMsg` | `string` | Contact form fields |

⚠️ **SSR note**: reading `localStorage` during initial state will hydration-mismatch in Next.js. Read it in an effect, or set `data-theme` from an inline pre-hydration script.

**Content model** — extract to a typed module:

```ts
type Shot   = { src: string; cap: string };
type Gallery = { title: string; shots: Shot[] };
type Project = {
  gallery: number;        // index into galleries
  featured?: boolean;
  video?: string;         // Loom share URL
  kicker: string;
  title: string;
  cover: string;
  problem: string;
  solution: string;
  stack: string[];
};
```

The prototype keys projects to galleries by numeric index (`g`). **Replace this with a string slug** — the numeric coupling is fragile and was already a source of bugs when the project list was reordered.

Nine projects, four featured:

1. ⭐ **Dapper District** — AI voice booking agent (ElevenLabs, Claude, n8n, Supabase)
2. ⭐ **Isabel Law Firm** — voice + chat intake agent (ElevenLabs, Claude, n8n, Supabase)
3. ⭐ **AI Lead Router** — n8n lead scoring/routing (n8n, Claude, Webhooks, Gmail, Sheets)
4. ⭐ **Invoice Processing Agent** — document AI for AP (n8n, Claude, OCR, Sheets)
5. Pilates Studio WhatsApp Bot (WhatsApp Cloud API, n8n, Claude, Supabase)
6. Threads Content Generator (n8n, Claude, Threads API)
7. Zapier Lead Qualification (Zapier, AI by Zapier, GoHighLevel, Typeform)
8. Zapier Ticket Triage (Zapier, AI by Zapier, Gmail, Sheets)
9. Motiontrade Application Suite (PowerApps, Power Automate, SharePoint) — 7-shot gallery

Full copy for each lives in the `static PROJECTS` array in the prototype; transcribe verbatim.

## Design Tokens

Declared on the root element and flipped by `data-theme`.

**Dark (default)**
```
--bg      #070B13      --fg      #E9EFFA
--panel   #0C121E      --muted   #9AABC4
--panel2  #111A2B      --faint   #6B7C96
--accent  #3B6FE0      --accent2 #5B9BFF
--line    rgba(120,160,220,0.16)
--line2   rgba(120,160,220,0.28)
```

**Light**
```
--bg      #EEF3FB      --fg      #131A24
--panel   #FFFFFF      --muted   #54637A
--panel2  #F4F7FD      --faint   #8593A8
--accent  #2F5FD0      --accent2 #3B6FE0
--line    rgba(19,26,36,0.10)
--line2   rgba(19,26,36,0.18)
```

Light mode also: page background `#EEF3FB`; panel shadow softens to `0 18px 50px rgba(19,26,36,0.10)`; rail shadow `0 14px 40px rgba(19,26,36,0.14)`; ambient glows drop to `opacity: .5`; logo variants swap (`.only-dark` / `.only-light`).

**Accent literals** used outside the token set: `#34D399` (available dot), `#7FB4FF` (gradient terminus), `#0A0F1A` (image wells), `#EAF1FF` (badge text).

**Typography**
- Display / headings: **Space Grotesk** 400/500/600/700
- Body / UI: **Instrument Sans** 400/500/600 + 400 italic
- Both from Google Fonts. `-webkit-font-smoothing: antialiased` on body.

| Role | Size | Weight | Tracking | Leading |
| --- | --- | --- | --- | --- |
| H1 | `clamp(36px, 5vw, 60px)` | 700 | `-0.03em` | 1.06 |
| H2 | 34px (contact: `clamp(28px,4vw,40px)`) | 700 | `-0.02em` | — |
| Featured card title | 23px | 600 | `-0.015em` | 1.22 |
| Secondary card title | 20px | 600 | `-0.01em` | 1.25 |
| Lightbox title | 19px | 600 | — | — |
| Hero lede | 18px | 400 | — | 1.65 |
| Body | 16px | 400 | — | 1.7 |
| Card body | 14.5 / 14px | 400 | — | 1.6 |
| Kicker / tier label | 11.5px | 700 | `0.09` / `0.12em` | — |
| Badge | 10.5px | 700 | `0.1em` | — |

**Gradient text** (used on the second half of every H2 and the hero's second line):
`background: linear-gradient(100deg, var(--accent2), #7FB4FF); background-clip: text; color: transparent`

**Radii**: panels 28px (20px ≤620px) · cards 20px · lightbox stage 16px · inputs 12px · image wells 16px · chips 6px · pills & buttons 999px.

**Shadows**
```
panel        0 26px 70px rgba(3,7,18,0.55)
featured     0 10px 30px rgba(3,7,18,0.35)
card hover   0 22px 48px rgba(59,111,224,0.28)
rail         0 0 26px rgba(59,111,224,0.28)
rail active  0 6px 20px rgba(59,111,224,0.5), 0 0 0 4px rgba(59,111,224,0.14)
CTA          0 10px 26px rgba(59,111,224,0.45)  → hover 0 16px 34px rgba(59,111,224,0.55)
badge        0 6px 18px rgba(3,7,18,0.45)
```

**Spacing**: section gap 30px · grid gap 24px · card body gap 12–13px · chip gap 7px · rail gap 10px. Shell padding `34px 34px 34px 118px` → `20px` sides + `96px` bottom ≤1100px → `12px` sides ≤620px.

**Motion**
| Curve | Use |
| --- | --- |
| `cubic-bezier(.22,1,.36,1)` | General ease-out — reveals, fades, presses |
| `cubic-bezier(.32,.72,0,1)` | Lightbox panel entrance |
| `cubic-bezier(.34,1.56,.64,1)` | Rail overshoot / spring |

Durations: press 140ms · image fade 140ms · lightbox out 160ms · lightbox in 200/240ms · card reveal 320ms · rail 320–380ms · theme swap 300ms.

Keyframes to port: `pulseDot` (2.4s, expanding green ring), `floatY` (±10px idle), `railGlow`, `lbFade` / `lbFadeOut`, `lbPop` / `lbPopOut`, `imgFade`.

**Breakpoints**: `1100px` (rail → bottom bar; hero and work grids → 1 column) and `620px` (panel padding/radius shrink, form → 1 column, timeline → 1 column, lightbox padding 12px).

## Assets

All under `uploads/` in this bundle; **copy them into the target project's static assets** and update paths.

**Generated graphics** (`uploads/shots/`, 2800×1760 PNG) — designed for this site to document builds with no shippable UI. Produced from `shots/portfolio-shots.html`, also included; edit and re-export from there if copy changes.
- `isabel-chat.png`, `isabel-flow.png`
- `router-leads.png`, `router-flow.png`
- `invoice-result.png`, `invoice-flow.png`

⚠️ The **lead-router figures are illustrative**, not real execution data. If the site goes public with claims attached, confirm with Carlos first.

**Client screenshots** (`uploads/`): `ai_voice.jpg`, `ai_voice_2.png`, `PilatesChatBotFlowChart.png`, `PilatestBot.png`, `ThreadsContentGenerator.png`, `Zapier-LeadQualification.png`, `Zapier-LeadQualification2.png`, `ZapierTicketTriage.png`, `MDCProjectManagement.png`, `MDCProjectManagement2.png`, `HRApplication.png`, `FleetManagement.png`, `MDCRequest.png`, `MDCRequest2.png`, `MDCRequest3.png`.

**Logos**: stacked lockup (dark + light variants) in the hero; `CMREYES-logo-horizontal-tight.png` in the footer.

**Loom walkthroughs** (external, featured projects only):
- Dapper District — `https://www.loom.com/share/8cc75f4c44204b65ad63a4b5dab55a01`
- Isabel Law Firm — `https://www.loom.com/share/b8a93a6a2b914c93868ac9f4188a6602`
- AI Lead Router — `https://www.loom.com/share/f36cab22e084416c9d8f632a1d24371d`
- Invoice Processing — `https://www.loom.com/share/9c4145e2453e438ba389cf3ffc98ff33`

## Files

| File | What it is |
| --- | --- |
| `CMREYES Portfolio v3.dc.html` | **The design of record.** Full markup, styles, content, and logic. |
| `shots/portfolio-shots.html` | Source for the six generated graphics — 1400×880 panels, captured at 2×. |
| `uploads/` | All images referenced by the design. |
| `support.js` | Prototyping runtime. **Reference only — do not port.** |

## Not in the prototype (production gaps)

Worth scoping before build:
- Real form submission with success/error states (currently `mailto:`)
- SEO: `<title>`, meta description, Open Graph image, favicon
- Analytics, and click tracking on the Loom links
- Image optimisation — the generated PNGs are 2800px wide and should be served responsively
- Accessibility audit: focus-visible rings on the rail and cards, a skip link, keyboard access to cards (they're `<article onClick>`, so they need `role="button"` + `tabIndex` + Enter/Space, or a wrapping `<a>`), and a focus trap in the lightbox
