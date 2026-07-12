# Prompt: Scaffold "OC Job + Social Services Navigator" into a Full Web App

Copy everything below into Claude Code / Claude (Max) as your starting instruction. Attach `oc_job_social_navigator.html` (the current single-file prototype) alongside it.

---

## ROLE

You are a senior full-stack engineer. I have a single-file HTML/CSS/vanilla-JS prototype called **OC Job + Social Services Navigator**. It's a black-and-white, monospace ("Consolas terminal") styled site that lets a user search and filter a hardcoded array of Orange County jobs/training and social-services resources by keyword, city, urgency, goal, and personal barriers, then generate a lightweight "action plan."

I want you to take this prototype and scaffold it into a real, production-shaped web application codebase — not a rewrite from scratch in spirit, but a proper project structure that preserves all existing functionality and prepares the ground for the features listed below.

## SOURCE CODE

The full current prototype is in the attached `oc_job_social_navigator.html`. Key things to preserve exactly as-is in behavior and feel:
- The monochrome, monospace "terminal" visual system (black background, white text, Consolas/mono font, `.terminal` boot-sequence panel, sticky header, skip link)
- The hero section with live stat counters (e.g. `totalResources`)
- The three-way tab switch: **Jobs + Training / Social Services / All Resources**
- The quick search bar: keyword + city + urgency ("Need help today" / "This week" / "Planning ahead")
- The filters sidebar: primary goal, barrier to account for (no car, no computer, kids, documents, language), free-text situation notes, and one-click "quick chips" (resume help, training, food, housing)
- The results/cards rendering, "Apply" / "Reset" behavior, and the **Copy plan** button
- The Action Plan panel: the "Next 48 hours" checklist and the "Safety note" / immediate-help box (911, 988, 2-1-1) — this must remain visible and prominent
- The existing `resources` array and its schema — treat this as seed/mock data to migrate into a real data layer, and preserve its exact shape:
  ```js
  {
    category: "jobs" | "services",
    title: string,
    tags: string[],
    cities: string[],
    goals: string[],       // e.g. "first-job", "training", "food", "housing", "legal"
    barriers: string[],    // e.g. "no-car", "no-computer", "kids", "documents", "language"
    urgency: string[],     // "today" | "week" | "plan"
    meta: string,
    details: string,
    steps: string,
    url: string
  }
  ```
- Accessibility already in place: skip link, `aria-live` results region, `role="tablist"`/`role="tab"`, `prefers-reduced-motion` handling — keep and extend, don't regress any of it.

## WHAT "SCAFFOLDING" MEANS HERE

1. Set up a proper project (Node/JS-based — Vite + React recommended, or Next.js if you think routing/SSR benefits outweigh the setup cost; pick one and justify it briefly).
2. Break the single HTML file into components: `Header/Topbar`, `Hero` (with `TerminalPanel` and live `StatusGrid`), `TabSwitcher`, `QuickSearchBar`, `FiltersSidebar` (including `QuickChips`), `ResourceCard`, `ResultsList`, `ActionPlanner` (checklist + copy-plan logic), `SafetyNoteBanner`, `Footer`.
3. Move the `resources` array and `cities` list into a `/data` or `/api` layer (JSON files to start, matching the schema above exactly), structured so it can later be swapped for a real database/CMS without changing component code.
4. Set up basic routing (Home/Navigator, Jobs, Social Services, Resource Detail, Action Plan, About/Contact, Admin placeholder).
5. Add a `README.md` explaining project structure, how to run it, and where to plug in real county data sources.
6. Set up environment config (`.env.example`) for anything that will eventually need API keys (maps, job board APIs, notification/email service, etc.).
7. Add basic linting/formatting (ESLint + Prettier) and a simple test setup (Vitest or Jest) with at least one test per core component — prioritize tests for the filter/search logic since that's the core of the app.

## KEY FEATURES TO BUILD TOWARD (minimum 10 — implement scaffolding/stubs for all, fully build the first 3–4 if time allows)

1. **Smarter Guided Search** — evolve the existing keyword + city + urgency + goal + barrier filtering into a proper query engine (combinable filters, relevance ranking, "did you mean" for typos), building directly on the current filter logic rather than replacing it.
2. **Job Search Board** — expand the "jobs" category into a real listing feed (search, sort, entry-level vs. skilled, full/part-time, save listings), structured so it can later pull from CalJOBS, Indeed API, or county job postings instead of the mock array.
3. **Resource Directory with Map View** — plot "services" category locations (food banks, shelters, One-Stop centers, libraries) on an interactive map by city, using the existing `cities` list and resource `meta`/`url` fields, with hours, contact info, and eligibility notes.
4. **Persistent Action Plans & Saved Progress** — turn the current "Copy plan" / checklist into a real saved plan: user can save their filters, situation notes, checked-off steps, and bookmarked resources across visits (localStorage-first is fine for scaffolding; note where real auth/DB would plug in).
5. **Multilingual Support** — i18n scaffolding (English/Spanish/Vietnamese at minimum, reflecting OC demographics) with a language switcher in the header, including translated goal/barrier/urgency labels.
6. **Eligibility Pre-Screener** — extend the "barrier" filter concept into a short guided questionnaire that pre-screens likely eligibility for CalFresh, housing assistance, etc., and flags which resources are the best-fit next step.
7. **Resume & Skills Helper** — a tool/stub page for resume upload or basic resume-builder guidance, linking out to the library/One-Stop resources already in the data (the "resume" goal tag).
8. **Notifications / Follow-Up Reminders** — stub a notification system (email or in-app) tied to the Action Plan checklist — e.g. remind the user about a deadline or follow-up date they logged.
9. **Admin/Content Management Stub** — a protected route/page structure (auth-gated placeholder) where county staff could add/edit/retire resources in the data layer without a code deploy.
10. **Low-Bandwidth & Offline Mode** — since the design is already lightweight/monochrome, push this further: works fully on low-end devices/slow connections, with a basic offline-cache of the resource list (service worker or simple cache-first strategy).
11. **Analytics/Impact Dashboard (internal)** — stub an internal page tracking anonymized usage (searches by goal/barrier/urgency, city, tab selected, resource click-throughs, checklist completion) to help the county understand real demand and gaps.
12. **Shareable Plan Links** — extend "Copy plan" into a shareable, read-only link (e.g. `/plan/:id`) a caseworker or family member could open to see the same recommended resources and checklist.

## CONSTRAINTS & PRIORITIES

- Prioritize a clean, extensible file structure over building every feature fully right now — this is a scaffolding pass.
- Do not lose any existing behavior from `oc_job_social_navigator.html` (tabs, quick search, filters, quick chips, copy plan, action-plan checklist, safety note) during the migration — port it faithfully into components first, then improve.
- Keep the visual identity (black/white monochrome, Consolas/mono type, terminal boot-sequence panel, sticky header) unless there's a strong accessibility reason to change something (e.g., contrast ratios — check them explicitly since this is a high-contrast but very stark palette).
- Preserve and extend the existing accessibility work (skip link, `aria-live`, tab roles, reduced-motion) — don't regress any of it.
- Comment the code where you're stubbing something intentionally for a future real backend/API integration, so it's obvious what's mock data vs. production-ready.
- After scaffolding, give me a short summary of the file tree and a prioritized list of what to build next.

## DELIVERABLE

A working local dev environment (`npm install && npm run dev`) with the migrated prototype running, the file/component structure above in place, stubs for all 12 features, and a README covering setup, structure, and next steps.
