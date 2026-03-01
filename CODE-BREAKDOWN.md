# BSEC Conference 2026 — Full Code Breakdown

Use this document in a Claude (or other AI) chat when asking for updates to the website. It provides structure, class names, IDs, and conventions so changes stay consistent.

---

## 1. Project overview

| Item | Detail |
|------|--------|
| **Purpose** | Marketing site for 20th Annual Babson Sustainability & Energy Conference — "Entrepreneurs for Impact" (April 24, 2026) |
| **Stack** | Vanilla HTML, CSS, JavaScript. No build, no framework. |
| **Files** | Two pages: main conference page + team gallery page. All styles and scripts are inline in each file. |
| **Design** | Dark/cream palette, teal/gold accents, Bebas Neue + Cormorant Garamond + Outfit. Scroll-reveal and fixed nav. |

---

## 2. File structure

```
2026 BSEC Conference Website/
├── 2026 BSEC Conference Website.html   # Main homepage (~1,770 lines)
├── bsec-team.html                       # Leadership team gallery (~690 lines)
├── assets/
│   └── babson-student-led-event.png     # Footer compliance logo (Babson "A Student-Led Event")
├── CODE-BREAKDOWN.md                    # This file
└── CODEBASE-SUMMARY.md                  # Higher-level summary (optional reference)
```

- **No shared CSS/JS:** Each HTML file has its own `<style>` and `<script>`. Keep design tokens and nav behavior in sync across both files when changing globally.
- **Links between pages:** Main page "Meet the Team" → `bsec-team.html`. Team page nav links → `2026 BSEC Conference Website.html#section-id` (hash anchors).

---

## 3. Design system (CSS variables)

Defined in `:root` in **both** files. Change in both places if you update the design.

```css
/* Colors */
--g-deep:    #050608;   /* page dark bg */
--g-mid:     #10141E;
--g-light:   #25C7B8;   /* teal accent */
--g-pale:    #0F2528;
--cream:     #F9FAFB;   /* light sections */
--cream-dk:  #E5E7EB;
--gold:      #25C7B8;   /* same as g-light in practice */
--gold-lt:   #5FF7E8;
--gold-pale: #D1FAF5;
--white:     #FFFFFF;
--ink:       #020617;
--ink-mid:   #4B5563;
--ink-soft:  #6B7280;
--accent-cyan: #43F1E8;
--accent-red:  #FF4C65;

/* Typography */
--serif:   'Cormorant Garamond', Georgia, serif;
--sans:    'Outfit', system-ui, sans-serif;
--display: 'Bebas Neue', system-ui, sans-serif;

/* Motion */
--ease: cubic-bezier(0.25, 0.46, 0.45, 0.94);
```

**Layout:** `.container` = max-width 1140px, padding 0 2rem. Section backgrounds alternate dark (`--g-deep`) and light (`--cream` / `--white`).

---

## 4. Main page: `2026 BSEC Conference Website.html`

### 4.1 Section order and IDs

| Order | Section ID | Purpose |
|-------|------------|---------|
| 1 | `#hero` | Full-viewport hero, headline, CTAs |
| 2 | `#glance` | At a glance strip (date, location, attendees, “20th Annual”) |
| 3 | `#about` | About + leadership photo + “Meet the Team” link |
| 4 | `#program` | Program cards (keynote, tracks, showcase, award) |
| 5 | `#speakers` | Speaker grid |
| 6 | `#agenda` | Conference agenda timeline |
| 7 | `#sponsors` | Sponsor tiers + CTA box |
| 8 | `#tickets` | Early bird pricing + countdown + CTA |
| 9 | `footer` | Brand, previous conferences, quick links, copyright, compliance logo |

### 4.2 Nav (`#nav`)

- **Behavior:** `position: fixed`. Gets class `.solid` when `scrollY > 60` (dark bg, blur, shadow). JS: `nav.classList.toggle('solid', window.scrollY > 60)`.
- **Structure:**
  - `.nav-brand` → link to `#hero`, contains `.nav-brand-top` ("BSEC Conference 2026") and `.nav-brand-sub` ("Entrepreneurs for Impact").
  - `#nav-links` → ul with links: `#about`, `#program`, `#speakers`, `#agenda`, `#sponsors`, and one external `.nav-cta` ("Get Tickets").
  - `#hamburger` → button for mobile; toggles `.open` on `#nav-links`.
- **Mobile:** At 640px, `.nav-links` hidden; hamburger shows; `.nav-links.open` = full-screen overlay. Same pattern on team page.

### 4.3 Hero (`#hero`)

- **Layout:** `min-height: 100vh`, flex center. Contains:
  - `#hero-canvas` — 2D canvas particle mesh (dots + lines), animated in JS.
  - `.hero-grain` — SVG noise overlay.
  - `.hero-overlay` — gradient overlay.
  - `.hero-content` — badge, eyebrow, `.hero-title` ("Entrepreneurs" / "for Impact"), `.hero-sub`, `.hero-meta` (date, venue, location), `.hero-ctas` (Get Your Ticket, Become a Sponsor).
  - `.hero-scroll-hint` — “Scroll” at bottom.
- **Animations:** Elements use `.reveal` and fade-up keyframes; `.hero-title` has optional glow/glitch (respects `prefers-reduced-motion`).
- **CTAs:** Ticket link + `mailto:gradsustainability@babson.edu` for sponsor.

### 4.4 At a glance (`#glance`)

- **Structure:** `.divider` (SVG) then `.container` > `.glance-grid` (4 columns).
- **Tiles:** `.glance-tile` with `.glance-icon`, `.glance-value`, `.glance-label`. Content: April 24, Babson College, 150–200 attendees, 20th Annual.
- **Responsive:** 2 columns at 900px, 2 at 640px.

### 4.5 About (`#about`)

- **Layout:** `.about-grid` (2 columns: quote side + body).
- **Quote side:** `.about-quote-side` → `.about-photo` (img + `.about-photo-caption` + `.about-meet-team-link` to **bsec-team.html**), `.about-quote-mark`, `.about-quote`, `.about-attribution`.
- **Body:** `.about-body` → `.tag`, h2, paragraphs, `.about-history`.
- **Link:** "Meet the Team" uses `href="bsec-team.html"` (or `bsec-team` depending on server). Ensure it points to the team page.

### 4.6 Program (`#program`)

- **Structure:** `.container` > `.tag`, h2, `.section-sub`, `.program-grid` (2×2 grid of `.program-card`).
- **Cards:** `.program-card` has `.program-number`, `.program-icon`, h3, p, and optionally `.tracks-list` or `.award-badge`.
- **Content:** 01 Morning Keynote, 02 Four Tracks, 03 Venture Showcase, 04 Entrepreneur for Impact Award. Comments: "REPLACE: add confirmed keynote speaker name and title when announced."

### 4.7 Speakers (`#speakers`)

- **Structure:** `.container` > `.tag`, h2, optional `.speakers-notice`, `.speakers-grid`.
- **Cards:** `.speaker-card` with `.speaker-avatar` (circle; use inline `style="background-image:url(...)"` for headshots), `.speaker-name`, `.speaker-role`, `.speaker-org`, `.speaker-links` (e.g. LinkedIn).
- **Comments in code:** "REPLACE: replace each speaker-card with real speaker data" and "set background-image: url('headshot.jpg') on .speaker-avatar". Several cards still have `.speaker-org` "TBD". Confirmed speakers include Stephen Spinelli Jr., Leila Lamoureux, John Chaimanis (with headshots and LinkedIn).

### 4.8 Agenda (`#agenda`)

- **Structure:** `.container` > `.tag`, h2, `.agenda-note`, `.agenda-timeline`.
- **Items:** Each is `.agenda-item` with:
  - `.agenda-time` (e.g. "8:00 AM")
  - `.agenda-node` (dot; add `.major` for key sessions)
  - `.agenda-content` → h4 (session title), p (description), optional `.agenda-speaker` and/or `.agenda-tracks` (pills).
- **Speaker block (for a session):**
  - `.agenda-speaker` → `.agenda-speaker-photo` (circle; inline `background-image`), `.agenda-speaker-info` → `.agenda-speaker-name` (can contain `<a>` to LinkedIn), `.agenda-speaker-designation` (e.g. "MBA '92, PhD"), `.agenda-speaker-title` (e.g. "President, Babson College").
- **Current times:** 8:00, 8:45 (Welcome Remarks from President Spinelli — with speaker block), 9:00, 9:30, 10:30, 12:00, 1:30, 4:00, 5:00, 5:30.
- **Comments:** "REPLACE: update agenda items with final times, session titles, and speakers" and "update times, session titles, and speaker names when finalized."

### 4.9 Sponsors (`#sponsors`)

- **Structure:** `.container` > `.tag`, h2, `.sponsors-sub`, `.sponsor-tiers` (multiple `.sponsor-tier`), then `.sponsor-cta-box`.
- **Tier:** `.sponsor-tier` → `.tier-header` (`.tier-name` + `.tier-line`), `.tier-logos` (`.sponsor-logo-box` or `.sponsor-logo-box.placeholder`).
- **Tiers in markup:** Presenting, Gold, Silver, Bronze (with Kendall + Babson Greenworks logos), In-Kind. Many are `.placeholder` "TBD".
- **CTA box:** "Join our sponsor community" + email + "Request Sponsor Deck" (mailto). Comment: "REPLACE: update href to actual sponsor deck PDF link when available."

### 4.10 Tickets (`#tickets`)

- **Structure:** `.tickets-inner` (max-width 780px, centered) → `.tag`, h2, `.ticket-prices` (e.g. $25 / $40), `.tickets-copy`, `#early-bird-countdown`, CTA button.
- **Countdown:** JS targets `#early-bird-countdown`, deadline `2026-03-24T23:59:59-04:00`, updates every 60s. Comment: "REPLACE: update urgency copy and deadline when early bird window closes."

### 4.11 Footer

- **Structure:** `footer` > `.container` > `.footer-grid` (brand + 2 `.footer-col`), `.footer-bottom` (copyright line only; "Hosted by..." line was removed), `.footer-compliance`.
- **Compliance:** `.footer-compliance` contains single `<img src="assets/babson-student-led-event.png" alt="Babson College, A Student-Led Event" loading="lazy" />`. Subtle (opacity 0.85, height ~20px, 18px on mobile). No link.

---

## 5. Team page: `bsec-team.html`

- **Entry:** Linked from main page About section ("Meet the Team").
- **Nav:** Same visual and behavior as main (fixed, `.solid` on scroll). Links go to `2026 BSEC Conference Website.html#about`, `#program`, etc., plus "Get Tickets" to ticket URL. Brand links to `2026 BSEC Conference Website.html#hero`.
- **Hero:** `#team-hero` — short "Meet the Entrepreneurs for Impact Team" block, dark gradient.
- **Gallery:** `#team-gallery` → `.container` > `.tag`, h2, `.team-sub`, `.team-grid` (responsive grid of `.team-card`).
- **Team card:** `.team-card` → `.team-photo` (inline `background-image`; aspect ~85% padding), `.team-body` → `.team-name`, `.team-role`, `.team-year`, `.team-bio`, `.team-links` (LinkedIn). Data came from leadership onboarding CSV; headshots from Babson SharePoint (may require auth in some contexts).
- **Footer:** Minimal: copyright, "Back to conference page" link, same `.footer-compliance` logo as main page.
- **Scripts:** Nav scroll + hamburger only (no scroll reveal, no canvas).

---

## 6. Scripts (main page)

| Script | Purpose |
|--------|--------|
| Nav scroll | `window.scroll` → `nav.classList.toggle('solid', window.scrollY > 60)` |
| Mobile menu | `#hamburger` click → toggle `.open` on `#nav-links`, animate icon (X) |
| Scroll reveal | `IntersectionObserver` on `.reveal` → add `.visible` when in view |
| Hero canvas | IIFE: resize, init nodes, `requestAnimationFrame` draw (particles + lines) |
| Early bird countdown | `#early-bird-countdown`, deadline March 24 2026, update every 60s |

Team page: nav scroll + hamburger only.

---

## 7. External URLs and assets

| Use | URL or path |
|-----|-------------|
| Tickets | `https://cglink.me/22L/r375910` |
| Contact | `mailto:gradsustainability@babson.edu` |
| Sponsor deck | mailto (no PDF link in code yet) |
| Fonts | Google Fonts: Bebas Neue, Cormorant Garamond, Outfit |
| Icons | Font Awesome 6.5.0 (CDN) |
| Compliance logo | `assets/babson-student-led-event.png` |
| Leadership photo (about) | `https://i.imgur.com/voPrG7V.jpeg` |
| Speaker/agenda headshots | Imgur URLs + Babson SharePoint for team page |

---

## 8. REPLACE / TBD checklist (for updates)

- **Speakers:** Replace placeholder speaker cards and TBD orgs; set `.speaker-avatar` background-image for each.
- **Program:** Add confirmed keynote name/title when announced.
- **Agenda:** Update times, session titles, speaker names when finalized; add more `.agenda-speaker` blocks if needed.
- **Sponsors:** Replace placeholder tiers (Presenting, Gold, Silver, In-Kind); add sponsor deck PDF link when available.
- **Tickets:** Update early bird deadline and urgency copy when it changes (JS deadline + text near `#early-bird-countdown`).

---

## 9. Conventions for edits

- **Two files:** Nav, footer compliance, and design tokens exist in both HTML files. Change both when updating global behavior or branding.
- **Reveal:** Add `.reveal` (and optional `.reveal-delay-1` …) to new sections if they should animate in on scroll (main page only; team page has no reveal).
- **Agenda speaker:** Copy the 8:45 AM “Welcome Remarks from President Spinelli” block to add another session speaker: same `.agenda-speaker` structure, swap photo URL, name link, designation, title.
- **Links:** Internal = hash (`#about`). Cross-page = `bsec-team.html` or `2026 BSEC Conference Website.html#section`. External = `target="_blank" rel="noopener noreferrer"`.
- **Apostrophes in copy:** Use `&rsquo;` (e.g. MBA &rsquo;27, &rsquo;92) for typographic apostrophes in HTML.

---

## 10. Responsive breakpoints

- **900px:** Glance 2-col, about 1-col, program 1-col, footer 2-col.
- **640px:** Nav → hamburger; hero/container padding reduced; footer 1-col; agenda time column narrower; footer compliance centered, logo 18px.
- **480px:** Nav/hero further reduced; hero CTAs stack; buttons full-width.

Use these same breakpoints when adding new sections so the site stays consistent.
