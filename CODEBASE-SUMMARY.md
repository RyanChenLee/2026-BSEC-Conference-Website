# BSEC Conference 2026 — Codebase Summary

Use this summary when discussing improvements to the site with Claude or another assistant.

---

## Project overview

- **Purpose:** Marketing site for the 20th Annual Babson Sustainability & Energy Conference (“Entrepreneurs for Impact”), April 24, 2026.
- **Stack:** Vanilla HTML, CSS, and JavaScript. No build step, no framework. Single-folder static site.
- **Hosting:** Intended to work as static files (e.g. GitHub Pages, Netlify, or direct upload). Ticket link and some assets point to external URLs.

---

## File structure

```
2026 BSEC Conference Website/
├── 2026 BSEC Conference Website.html   # Main conference homepage (~1,700 lines)
├── bsec-team.html                       # Team / leadership gallery page (~670 lines)
└── CODEBASE-SUMMARY.md                   # This file
```

- No shared CSS/JS files: each HTML file has its own `<style>` and `<script>` blocks.
- Design tokens (colors, fonts, spacing) are duplicated between the two pages but kept in sync manually.

---

## Main page: `2026 BSEC Conference Website.html`

### Sections (in order)

1. **Nav** — Fixed top bar; transparent until scroll, then solid dark with blur. Links: About, Program, Speakers, Agenda, Sponsors, Get Tickets (external). Mobile: hamburger menu.
2. **Hero** — Full-viewport hero with canvas particle mesh, headline “Entrepreneurs for Impact,” event details, and CTAs (Get Your Ticket, Become a Sponsor).
3. **At a glance** — Four-tile strip: date, location, expected attendees, “20th Annual.”
4. **About** — Two-column: quote + leadership photo (with “Meet the Team” link to `bsec-team.html`) and body copy about the conference.
5. **Program** — Four cards: Morning Keynote, Four Tracks, Venture Showcase, Entrepreneur for Impact Award.
6. **Speakers** — Grid of speaker cards (placeholder + a few with headshots/LinkedIn). “Keynote and panelists announced on a rolling basis.”
7. **Agenda** — Timeline of sessions (Registration, Welcome, Keynote, panels, lunch, breakouts, showcase, award, reception). Note in copy: “Full agenda and speaker lineup releasing March 2026.”
8. **Sponsors** — Tiers (Presenting, Gold, Silver, Bronze, In-Kind). Bronze has Kendall + Babson Greenworks logos; others are placeholder “TBD” boxes. CTA: email + “Request Sponsor Deck.”
9. **Tickets** — Early bird pricing ($25 students, $40 community), countdown to March 24, 2026, CTA to ticket URL.
10. **Footer** — Brand, previous conferences (bseclub.org), quick links, copyright, co-presidents credit.

### Design system (in-page)

- **CSS variables** in `:root`: `--g-deep`, `--g-mid`, `--g-light`, `--cream`, `--gold`, `--gold-lt`, `--serif` (Cormorant Garamond), `--sans` (Outfit), `--display` (Bebas Neue), `--accent-cyan`, `--accent-red`, `--ease`.
- **Layout:** `.container` max-width 1140px, padded. Sections alternate dark (`#050608`-style) and light (`--cream`) backgrounds.
- **Motion:** Scroll-triggered “reveal” via IntersectionObserver (`.reveal` → `.visible`). Hero title has optional glow/glitch keyframes; respects `prefers-reduced-motion`.
- **Responsive:** Breakpoints at 900px, 640px, 480px (nav collapses to hamburger at 640px; hero/buttons/typography scale down on small screens).

### Scripts (inline)

- Nav: toggles `.solid` on nav when `scrollY > 60`.
- Mobile menu: hamburger toggles `.open` on nav links and animates icon.
- Scroll reveal: IntersectionObserver on `.reveal` elements.
- Hero: Canvas 2D particle mesh (nodes + connecting lines), resizes on window resize.
- Early bird countdown: countdown to `2026-03-24T23:59:59-04:00`, updates every minute.

### External links / assets

- **Tickets:** `https://cglink.me/22L/r375910`
- **Contact:** `gradsustainability@babson.edu`
- **Fonts:** Google Fonts (Bebas Neue, Cormorant Garamond, Outfit)
- **Icons:** Font Awesome 6.5.0 (CDN)
- **Images:** Some from Imgur (e.g. leadership photo, speaker headshots); sponsor logos from Imgur; team headshots on team page use Babson SharePoint URLs (may require auth in some contexts).

### Content that’s still placeholder / REPLACE

- Speaker cards: several “TBD” / “Announcement Coming”; comments say to replace with real speaker data and headshots.
- Program: “add confirmed keynote speaker name and title when announced.”
- Agenda: “update times, session titles, and speaker names when finalized” and “Full agenda and speaker lineup releasing March 2026.”
- Sponsors: Presenting, Gold, Silver, In-Kind are “TBD” placeholder boxes; comment to add sponsor deck PDF link when available.
- Tickets: “update urgency copy and deadline when early bird window closes.”

---

## Team page: `bsec-team.html`

- **Entry point:** “Meet the Team” link in the About section of the main page.
- **Layout:** Same nav as main page (fixed, solid on scroll), but links point to `2026 BSEC Conference Website.html#section` for each section, plus “Get Tickets” to the same ticket URL.
- **Hero:** Short “Meet the Entrepreneurs for Impact Team” block with dark gradient background.
- **Gallery:** Grid of team cards (`.team-card`). Each card has:
  - Photo (CSS `background-image`; many from SharePoint).
  - Name, role line (e.g. “Director of Partnership & Sponsorship” or “Leadership Team”), “2025–2026 Leadership Team” label.
  - Short bio (from onboarding form).
  - LinkedIn link.
- **Footer:** Minimal (copyright + “Back to conference page”).
- **Scripts:** Nav scroll `.solid`; mobile hamburger (same behavior as main page). No scroll reveal or canvas.

Team data was populated from a CSV export of “BSEC Spring 2026 Leadership Onboarding Form” (name, short bio, LinkedIn URL, headshot URL). Headshots are standardized visually with a ~85% aspect-ratio box and `background-size: cover`; recommended source size is square or 4:5, e.g. 800×800 px.

---

## Cross-cutting notes

- **Accessibility:** `prefers-reduced-motion` used for hero title animation; no formal a11y audit noted in code.
- **SEO:** Single `<title>` per page; no meta descriptions or Open Graph tags in the summary.
- **Maintainability:** Duplicate CSS/JS between the two HTML files; changing nav or design system requires edits in both. No shared partials or includes.
- **URLs:** Main page is `2026 BSEC Conference Website.html` (spaces in filename). Internal link from main to team is `bsec-team.html`. Team page links back with hash anchors (e.g. `2026 BSEC Conference Website.html#about`).

---

## Quick reference for “improve the site” discussions

- **Design:** Update `:root` variables and section backgrounds in both files for consistency.
- **Content:** Search for “REPLACE” and “TBD” in the main HTML; speakers, agenda, sponsors, and ticket copy are the main placeholders.
- **Performance:** Consider inlining critical CSS, deferring Font Awesome or loading only used icons, and optimizing or lazy-loading images (including SharePoint URLs if they’re slow).
- **Structure:** Extracting shared CSS/JS into one or two files and reusing them on both pages would reduce duplication and make future design changes easier.
