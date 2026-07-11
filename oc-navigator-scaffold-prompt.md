# Prompt: Scaffold "Orange County Life Navigator" into a Full Job Search & Social Services Web App

Copy everything below into Claude Code / Claude (Max) as your starting instruction. Attach `index.html` (the current single-file prototype) alongside it.

---

## ROLE

You are a senior full-stack engineer. I have a single-file HTML/CSS/vanilla-JS prototype called **Orange County Life Navigator**. It currently does two things in one page: (1) renders a calming animated color-field canvas, and (2) lets a user filter a hardcoded array of social-services and job resources by city/need/goal.

I want you to take this prototype and scaffold it into a real, production-shaped web application codebase — not a rewrite from scratch in spirit, but a proper project structure that preserves all existing functionality and prepares the ground for the features listed below.

## SOURCE CODE

The full current prototype is in the attached `index.html`. Key things to preserve from it:
- The animated canvas "regulating color field" (whale/Coltrane-loop breathing effect) and rotating guiding phrases
- The two-panel layout: Social Services navigator (city/need/story → filtered results) and Work & Income navigator (city/goal/story → filtered results)
- The existing `socialResources` and `workResources` arrays — treat these as seed/mock data to be migrated into a real data layer
- The dark, calm visual theme (slate/blue-green palette, rounded cards, uppercase micro-labels)
- The crisis/emergency footer disclaimer (911 / 988) — this must remain visible on every page

## WHAT "SCAFFOLDING" MEANS HERE

1. Set up a proper project (Node/JS-based — Vite + React recommended, or Next.js if you think routing/SSR benefits outweigh the setup cost; pick one and justify it briefly).
2. Break the single HTML file into components: `ColorField`, `SocialServicesNavigator`, `WorkNavigator`, `ResourceCard`, `ResourceList`, `Header`, `Footer`, `CrisisBanner`.
3. Move the resource arrays into a `/data` or `/api` layer (JSON files to start, structured so they can later be swapped for a real database/CMS without changing component code).
4. Set up basic routing (Home, Social Services, Jobs, Resource Detail, About/Contact, Admin placeholder).
5. Add a `README.md` explaining project structure, how to run it, and where to plug in real county data sources.
6. Keep the animated canvas as a lightweight ambient background component — don't let it block interactivity or accessibility.
7. Set up environment config (`.env.example`) for anything that will eventually need API keys (maps, job board APIs, etc.).
8. Add basic linting/formatting (ESLint + Prettier) and a simple test setup (Vitest or Jest) with at least one test per core component.

## KEY FEATURES TO BUILD TOWARD (minimum 10 — implement scaffolding/stubs for all, fully build the first 3–4 if time allows)

1. **Guided Resource Finder** — evolve the existing city/need/goal filters into a multi-step questionnaire (housing, food, healthcare, legal, childcare, employment, benefits) that routes users to matched county and nonprofit resources, expanding on the current `socialResources`/`workResources` filtering logic.
2. **Job Search Board** — a real job listing feed (start with mock/seed data matching the current `workResources` shape, structured so it can later pull from CalJOBS, Indeed API, or county job postings) with search, filters (city, category, entry-level vs. skilled, full/part-time), and saved listings.
3. **Resource Directory with Map View** — plot social-service locations (food banks, shelters, One-Stop centers, libraries) on an interactive map by city, with hours, contact info, and eligibility notes.
4. **User Accounts & Saved Progress** — optional lightweight auth so a user can save their intake story, bookmarked resources, and job applications across visits (localStorage-first is fine for scaffolding; note where real auth/DB would plug in).
5. **Multilingual Support** — i18n scaffolding (English/Spanish/Vietnamese at minimum, reflecting OC demographics) with a language switcher in the header.
6. **Application/Intake Form Builder** — reusable form component (like the existing "Tell your situation" textarea) that can be extended into structured intake forms for benefits pre-screening or job applications.
7. **Resume & Skills Helper** — a simple tool/stub page for resume upload or basic resume-builder guidance, linking out to library/One-Stop resources already in the data.
8. **Crisis & Emergency Quick-Access Bar** — always-visible, one-tap access to 911, 988, and 2-1-1, expanding the current footer disclaimer into a persistent, accessible component (not just footer text).
9. **Admin/Content Management Stub** — a protected route/page structure (auth-gated placeholder) where county staff could eventually add/edit/retire resource listings without a code deploy.
10. **Notifications / Follow-Up Reminders** — stub a notification system (email or in-app) for things like benefit renewal deadlines or job application follow-ups.
11. **Accessibility & Low-Bandwidth Mode** — ensure the app meets WCAG AA basics (keyboard nav, contrast, reduced-motion toggle for the animated canvas) and works on low-end devices/slow connections common among target users.
12. **Analytics/Impact Dashboard (internal)** — stub a simple internal page tracking anonymized usage (searches by need type, city, resource click-throughs) to help the county understand demand.

## CONSTRAINTS & PRIORITIES

- Prioritize a clean, extensible file structure over building every feature fully right now — this is a scaffolding pass.
- Do not lose any existing behavior from `index.html` (color field animation, phrase rotation, filtering logic) during the migration — port it faithfully into components first, then improve.
- Keep the visual identity (dark calming theme, rounded cards, gradient buttons) unless there's a strong accessibility reason to change something (e.g., contrast ratios).
- Add a `reduced motion` check for the canvas animation (`prefers-reduced-motion`) since this is a stress-sensitive user base.
- Comment the code where you're stubbing something intentionally for a future real backend/API integration, so it's obvious what's mock data vs. production-ready.
- After scaffolding, give me a short summary of the file tree and a prioritized list of what to build next.

## DELIVERABLE

A working local dev environment (`npm install && npm run dev`) with the migrated prototype running, the file/component structure above in place, stubs for all 12 features, and a README covering setup, structure, and next steps.
