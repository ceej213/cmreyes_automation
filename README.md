# CMREYES Automation — Portfolio Site

Personal portfolio for **Carlos Javier Reyes** (CMREYES Automation) — AI voice agents, chatbots, and workflow automation.

Live: deployed as a Render static site from this repo.

## Structure

| Path | What it is |
| --- | --- |
| `index.html` | The full single-page site |
| `support.js` | Client-side runtime that renders the page's `<x-dc>` component |
| `uploads/` | Logos and project screenshots |
| `shots/` | Screenshot reference page |
| `render.yaml` | Render blueprint (static site, publish path `.`) |
| `DESIGN-HANDOFF.md` | Original design spec — tokens, layout, copy, motion curves |

## Running locally

```bash
python3 -m http.server 4123
```

Then open http://localhost:4123.

## Deploying

Render serves this repo as a static site with no build step — publish directory is the repo root. Pushes to `main` trigger an auto-deploy.

## Note on the runtime

`index.html` + `support.js` are the design prototype as authored: the page is driven by a client-side component runtime that pulls React from unpkg at load. It renders correctly as a static site. If this ever gets rebuilt as a production app (Next.js + Tailwind is the natural fit), `DESIGN-HANDOFF.md` has every token, measurement, and final copy string needed to recreate it.
