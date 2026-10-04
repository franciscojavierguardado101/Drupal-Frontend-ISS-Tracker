# Drupal + Frontend ISS Live Tracker

Full-stack implementation of the ISS Live Tracker feature, built for a headless Drupal 11 + Next.js architecture. This repo contains both the Drupal backend module and the Next.js frontend component together.

## Structure

```
drupal/     Drupal 11 custom module (francisco_iss_tracker)
frontend/   Next.js component, Leaflet map, and API proxy route
```

---

## Drupal module (`drupal/`)

Provides the `iss_tracker` paragraph type so editors can place the tracker on any page via the CMS.

**What it does:**
- Registers an `iss_tracker` paragraph type via the Paragraphs module
- Provides an optional label field (`field_iss_label`) for editors to customize the section heading
- Delivers component data to the Next.js frontend via JSON:API — no custom endpoint needed

**Installation:**
```bash
cp -r drupal /path/to/web/modules/custom/francisco_iss_tracker
drush en francisco_iss_tracker -y
drush cr
```

Requires Drupal 10 or 11 with the `paragraphs` module.

---

## Next.js frontend (`frontend/`)

**What it does:**
- Live Leaflet map with the ISS position updated every 5 seconds
- Ground track polyline of the last 60 recorded positions
- Footprint circle showing the ISS visibility area on Earth's surface
- Antimeridian handling so the track never draws a line across the entire globe
- Follow / Free toggle to lock or unlock the map camera on the ISS
- Live crew list fetched on mount from a public space station API
- Clicking a crew name loads a Wikipedia bio panel with photo, summary, and article link

**APIs used (no keys required):**

| Data | Source |
|------|--------|
| ISS position | `https://api.wheretheiss.at/v1/satellites/25544` |
| Crew list | `https://corquaid.github.io/international-space-station-APIs/JSON/people-in-space.json` |
| Crew bios | Wikipedia REST API |

**File breakdown:**

| File | Purpose |
|------|---------|
| `frontend/src/app/api/iss-tracker/route.ts` | Next.js API proxy for position and crew |
| `frontend/src/components/paragraphs/iss-tracker/IssTracker.tsx` | Main component: polling, state, crew, wiki panel |
| `frontend/src/components/paragraphs/iss-tracker/IssMap.tsx` | Leaflet map loaded via dynamic import (ssr:false) |
| `frontend/src/components/paragraphs/iss-tracker/types.ts` | TypeScript interfaces |
| `frontend/src/components/paragraphs/iss-tracker/index.ts` | Barrel export |

---

## Stack

- Drupal 11 (headless, Pantheon)
- PHP 8.2
- Next.js 16 App Router (Vercel)
- TypeScript
- Leaflet.js
- ESRI dark gray tiles (free, no key)
