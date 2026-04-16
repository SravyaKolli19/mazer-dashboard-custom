# Mazer Dashboard — Customized (Task 3)

A customized, data-driven admin dashboard built on top of the **Mazer** Bootstrap 5 template (https://github.com/zuramai/mazer).

---

## What Was Done

### UI/UX Customization
| Change | Details |
|--------|---------|
| **Theme** | Dark-first design with CSS custom properties (`--bg`, `--surface`, `--accent`, etc.) |
| **Typography** | Replaced default fonts with **Syne** (headings) + **DM Sans** (body) from Google Fonts |
| **Color palette** | Deep navy backgrounds with accent colors: purple `#6c63ff`, teal `#00d4aa`, red `#ff6b6b`, yellow `#ffd166` |
| **Cards** | Rounded corners (`16px`), subtle borders, hover lift animation |
| **Light/Dark toggle** | Runtime theme switch with a toggle in the topbar |
| **Animations** | `fadeUp` keyframe on stat cards and activity items with staggered delays |
| **Responsive sidebar** | Burger-menu button on mobile collapses/opens the sidebar |

### New Components Added
- **Trend badges** on stat cards (↑ / ↓ with color-coded percentage)
- **Region breakdown** with proportional progress bars
- **Avatar initials** generated from names (no broken image links)
- **Message preview list** with timestamps
- **Dark/Light theme toggle** that re-renders ApexCharts to match

---

## Data Architecture

All dashboard data lives in **data.json** and is dynamically loaded into the dashboard using the Fetch API.

## Data Integration

The dashboard uses a JSON-based data layer (`data.json`) which is fetched dynamically using the Fetch API. This approach separates data from UI and allows easy migration to backend APIs in future.

### Structure

```json
{
  "user":       { "name", "handle", "initials" },
  "stats":      [ { "label", "value", "trend", "up", "icon", "color" } ],
  "visitChart": { "months": [], "data": [] },
  "regions":    [ { "name", "count", "color" } ],
  "comments":   [ { "name", "handle", "text", "time", "color" } ],
  "messages":   [ { "name", "handle", "preview", "time", "color" } ],
  "audience":   { "labels": [], "data": [] }
}
```

### Rendering flow (JS)
```
DOMContentLoaded
 ├── initDate()        → topbar subtitle
 ├── initUser()        → sidebar user card from data.user
 ├── initStats()       → 4 stat cards from data.stats
 ├── initVisitChart()  → ApexCharts area chart from data.visitChart
 ├── initRegions()     → progress-bar list from data.regions
 ├── initComments()    → activity feed from data.comments
 ├── initMessages()    → message list from data.messages
 └── initDonut()       → ApexCharts donut from data.audience
```

To swap data, edit **`data.json`** or replace the `dashboardData` object in the `<script>` tag with a `fetch('data.json')` call for real API integration.

---

## Setup & Usage

This is a **standalone HTML file** — no build step required.

```bash
# Option 1: Just open in browser
open index.html

# Option 2: Serve locally 
npx serve .

```

---

## Libraries Used

| Library | Version | Purpose |
|---------|---------|---------|
| Bootstrap 5 | 5.3.2 | Layout, grid, utility classes |
| Bootstrap Icons | 1.11.3 | Icon set |
| ApexCharts | 3.45.2 | Area chart + Donut chart |
| Google Fonts | – | Syne + DM Sans typography |

---

## Key Changes vs Original Mazer

1. **Merged Nunjucks templates** into a single self-contained `.html` — no build pipeline needed.
2. **Replaced static hard-coded values** with a `dashboardData` JSON object (all numbers, names, labels come from data).
3. **Replaced external image avatars** (which 404 without a local server) with CSS initial-based avatars.
4. **Added light/dark toggle** — the original only had a dark mode script but no runtime toggle.
5. **Added trend indicators** and responsive stat cards with animation.
6. **Redesigned sidebar** with section labels, active state indicator bar, and user profile footer.
