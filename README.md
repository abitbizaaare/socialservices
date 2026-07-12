oc_socal_navigator_full = latest Claude version
oc-navigator-app.zip = base app ZIP for Cursor / Claude Max

#
[README (1).md](https://github.com/user-attachments/files/29932448/README.1.md)
# OC Job + Social Services Navigator — README

## Files in this project

| File | What it is |
|---|---|
| `oc_socal_navigator_full.html` | **Latest Claude-built version.** A complete, self-contained single-file prototype (HTML + CSS + JS, no build step). Open it directly in a browser to run the full app. |
| `oc-navigator-app.zip` | **Base project scaffold** — the same app broken into a real multi-file project (components, data layer, config) for continued development in Cursor or Claude Code (Max). This is the starting point for turning the prototype into a production app. |

If you're just trying the app or demoing it to stakeholders, use the `.html` file. If you're actively building the real product, unzip `oc-navigator-app.zip` into Cursor / Claude Code and work from there.

---

## What the app does

A monochrome, terminal-styled web app that helps Orange County residents find:
- Job search, training, and career-center resources
- Social services (food, housing, health, legal, youth programs)
- A guided eligibility pre-screener
- Resume help
- A personal "plan" they can save and share

It's a hash-routed single-page app — no server, no build tools, no dependencies. Everything runs client-side in the browser.

---

## Page map (hash routes)

| Route | Purpose |
|---|---|
| `#/home` | Landing page + the main search/filter navigator (keyword, city, urgency, goal, barrier, tabs for Jobs / Services / All) |
| `#/jobs` | Dedicated jobs & training board — filter by level (entry/skilled/support) and schedule |
| `#/services` | Social services grouped by need (food, housing, health, legal, youth, transport), each with a map-search shortcut |
| `#/eligibility` | 3-step guided quiz that recommends likely-relevant resources based on goal, urgency, and barriers |
| `#/resume` | Static resume-prep checklist + linked resume-help resources |
| `#/myplan` | Saved/bookmarked resources, a 48-hour action checklist, reminders, and a shareable plan link |
| `#/admin` | Staff stub — demo-PIN-gated screen to add/remove resources locally (not a real CMS — see Limitations) |
| `#/analytics` | Internal stub — local usage counts by page view and searched goal |

Routing logic lives in the `ROUTING` section of the script (`routes` array + `renderRoute()`).

---

## Code structure (inside the single HTML file)

1. **`<style>`** — all CSS, using CSS custom properties (`--bg`, `--text`, `--line`, etc.) for the black/white/monospace theme. Responsive breakpoints at 980px and 640px; respects `prefers-reduced-motion`.
2. **`<body>`** — markup for every route, each wrapped in `<section class="route-section" data-route="...">`. Only the active route's section is shown; the rest are `display: none`.
3. **`<script>`**, in order:
   - **DATA LAYER** — `cities` array and `baseResources` array (the seed/mock resource data). `getAllResources()` merges this with any admin-added resources from `localStorage`.
   - **i18n** — a dictionary (`i18n.en` / `i18n.es` / `i18n.vi`) and `applyI18n()`, which swaps text on any element with a `data-i18n` attribute. Language choice is saved to `localStorage`.
   - **LOCAL STORAGE HELPERS** — `readJSON()` / `writeJSON()` wrappers used everywhere state needs to persist.
   - **ANALYTICS** — local-only page view and goal-search counters, rendered as simple bar charts.
   - **ROUTING** — hash-based page switching.
   - **HOME / NAVIGATOR** — the original search/filter/scoring logic (keyword scoring, tab mode, filters) and card rendering.
   - **BOOKMARKS** — shared save/unsave logic used by every page's resource cards.
   - **JOBS BOARD**, **SERVICES DIRECTORY**, **RESUME HELPER** — page-specific render functions, all reading from the same resource data.
   - **ELIGIBILITY PRE-SCREENER** — quiz state machine (3 steps) and result matching.
   - **MY PLAN** — bookmarks, checklist, reminders, and the share-link encode/decode (`btoa`/`atob` of a JSON payload in the `?plan=` query param).
   - **ADMIN** — PIN gate + local add/delete of resources.
   - **INIT** — runs `applyI18n()`, `filterResources()`, and `renderRoute()` on load.

---

## Data model (`baseResources` entries)

```js
{
  category: "jobs" | "services",
  level: "entry" | "skilled" | "support",       // jobs only
  employmentType: "full-time" | "part-time" | "either" | "n/a", // jobs only
  title: string,
  tags: string[],
  cities: string[],       // subset of the `cities` array, or the full list
  goals: string[],        // e.g. "food", "housing", "training", "resume"
  barriers: string[],     // e.g. "no-car", "no-computer", "kids", "documents", "language"
  urgency: string[],      // "today" | "week" | "plan"
  meta: string,           // one-line summary shown on the card
  details: string,        // longer description
  steps: string,          // suggested next action
  url: string
}
```
This exact shape is what a real backend/CMS should target so the front end doesn't need to change when the mock data is replaced.

---

## What's stored in `localStorage`

| Key | Contents |
|---|---|
| `oc_lang` | Selected language (`en`/`es`/`vi`) |
| `oc_bookmarks` | Array of saved resource titles |
| `oc_checklist` | Checked/unchecked state of the 48-hour checklist |
| `oc_reminders` | Array of `{ text, date }` reminders |
| `oc_admin_resources` | Resources added through the Admin stub |
| `oc_analytics_pages` | Page-view counts per route |
| `oc_analytics_goals` | Search counts per goal filter |

Nothing is sent to a server — this is all per-browser, per-device state.

---

## Known limitations (by design, since this is a prototype)

- **Admin PIN (`1234`) is not real security.** It's a client-side demo gate only. A real deployment needs actual staff authentication and a backend data store.
- **Job listings are career-support organizations, not live job postings.** There's no job-board API integration yet.
- **"Map" links open Google Maps search**, not an embedded interactive map — no map API key is wired up.
- **Analytics are local-only** and reset if the browser's storage is cleared; there's no real analytics pipeline.
- **Notifications** use the in-page `Notification` API as a same-session demo; they won't fire if the tab/browser is closed. A production build needs server-side email/SMS or push notifications.
- **Translations cover interface chrome only** (nav, labels, buttons); resource descriptions themselves stay in English.

---

## Next steps (if continuing in `oc-navigator-app.zip`)

1. Replace `baseResources` with a real API/CMS-backed data layer, keeping the same schema.
2. Swap the Admin PIN gate for real authentication.
3. Replace local `localStorage` analytics with a real analytics service.
4. Add a real map integration for the Services Directory.
5. Wire up a real job-postings API (e.g. CalJOBS) for the Jobs Board.
6. Expand translations to resource content, not just UI chrome.

//
   Future expansion ideas for the Orange County navigator include adding a local job-posting forum, monetizing through carefully placed website ads, improving search engine visibility for Orange County job and social service searches, implementing full security hardening, building an API backend for new postings and live updates, tracking ordinance and policy changes, and creating dedicated resource pages for undocumented immigrants, veterans and VA support, ADA/disability services, and other specialized community needs.

}
whale.html is the original file.
