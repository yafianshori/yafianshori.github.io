# Personal Website — Yafi Anshori — Design Spec

**Date:** 2026-05-30
**Status:** Approved (design phase)
**Author:** Yafi Anshori (with Claude)

## 1. Purpose

A single-page personal website that functions as a **professional landing page + portfolio**. Two audiences:
1. Potential freelance clients who might hire Yafi for projects.
2. People who want to get to know Yafi.

Primary goal: convince a visitor — within the first 3 seconds ("golden time") — that Yafi solves real operational problems and is worth a premium rate. Primary CTA: **Hire / Contact**.

## 2. Positioning

**Problem-solver, results-first.** Not framed primarily as a technical coder.

- Headline emphasizes outcomes: *"Mengubah masalah operasional menjadi sistem yang menghemat miliaran."*
- Three equal pillars of expertise, shown as roles: **Business Analyst · Scrum Master · Builder**.
- Scrum Master & BA presented as **practical experience** (no certification claims — honest).
- Entire site framed as **ROI / investment**: paying Yafi returns more than it costs.

## 3. Verified Facts & Proof (content source of truth)

All claims below are backed by national media / third-party validation. Do not invent beyond these.

### Identity
- Yafi Anshori, S.Tr.Kom — Politeknik Negeri Jember (Polije).
- Business Analyst at PT Petrokimia Gresik (BUMN, agro-industry).
- Self-described: perfectionist, fast learner, hard worker.
- GitHub: `yafianshori`. LinkedIn: `in/yafianshori`. Email: bangapli.sc@gmail.com (confirm before publishing).

### Background / journey
- Microsoft Student Partner (MSP) — trainer & speaker.
- INAICTA 2013 nominee. Microsoft Windows 8 DreamSpark & Ucrew.
- Chairman of GameKita Community (game dev community).
- Desktop app developer at Berca Sportindo.
- Now: BA + innovator + builder at Petrokimia Gresik.

### Products Yafi brought / built (portfolio core)
- **SISTRO** (Sistem Scheduling Truk Online): web truck scheduling for subsidized fertilizer distribution.
  - **TKMPN 2019 (XXIII) — top award.** Featured at **Hannover Messe 2023** (attributed to the PRODUCT, not Yafi personally).
  - Impact: late-truck arrivals dropped 1.361 → 553 events/month; dwell time 2–3 days → max 1 day.
  - Scale: ~10.000 trucks, ~70 transport partners, 5,4 million tons/year.
- **DTMS** (Digital Transport Management System): real-time inter-warehouse material transfer + driver performance monitoring.
  - Impact: potential savings **Rp 12,9 billion/year**.
- **2CE / WMS 2CE** (Customer Centric Excellence): web WMS for non-subsidized fertilizer distribution, 50+ distributors; digitized PPB; integrated with Google Maps + SISTRO.
  - Part of "The Best Performer Warehouse Operation" — Indonesia Logistics Awards (ILA) 2024.
- **EXCELLO** (Excellence in Customer Safety and Loyalty): part of the Smart Warehouse Ecosystem (WMS, DTMS, 2CE/CCE, WMMS, WBMS).
  - Ecosystem won "Warehouse & Bagging Transformation for Supply Chain Excellence" — BILA 2025. Total warehouse efficiency Rp 14,8 billion.

### Hero metrics (animated count-up)
- `Rp 14,8 M+` efficiency delivered.
- `5+` apps that won TKMPN / national recognition.
- `5,4 jt ton/thn` fertilizer managed by systems he built.

### Tech & skills
- BA & Agile/Scrum practice.
- Flutter/Dart, PHP/Laravel, React/Recharts.
- AI tooling: Claude Code, n8n, MCP. Docker, monorepo (pnpm).
- Legacy: .NET, C#, VB, CodeIgniter, Android.

## 4. Visual Direction (explicitly anti "AI-slop")

- **Palette:** warm near-black ink (`#0E0E0D`), paper off-white (`#F4F1EA`), single accent amber/safety-orange (industrial/warehouse reference — NOT the cliché emerald/indigo gradient).
- **Typography:** high contrast — a display serif (e.g. Fraunces / Instrument Serif) for headings + a monospace (e.g. JetBrains Mono) for labels/numbers. "Editorial × engineer" feel.
- **Layout:** asymmetric grid, thin spec-sheet rules, section numbers (01/02/03), large numbers as proof.
- **Motion:** subtle — scroll reveal, count-up, hover line draws. A calm animated hero background (flowing logistics: truck → warehouse → map, or moving technical grid). No flashy/heavy animation.
- **Texture:** faint grain + subtle grid so it doesn't read as flat template.
- Forbidden: generic purple→blue gradients, emoji-heavy copy, filler marketing fluff, lorem-style padding.

## 5. Page Structure (top → bottom)

Golden-time first, then proof. Few words, numbers do the talking.

- **Hero:** name + one sharp sentence + 3 role chips (BA · Scrum Master · Builder) + 3 count-up metrics + CTA [Hire Saya] / [LinkedIn] + ID/EN toggle + animated background.
- **01 — Impact:** strip of rupiah/outcome numbers, one context line each. ROI hero.
- **02 — Karya/Produk:** cards for SISTRO / DTMS / 2CE / EXCELLO. Each: name, one-line function, proof badges (TKMPN, Hannover Messe→on product, Rp/yr). Hover reveals detail.
- **03 — Cara Saya Bekerja:** Discovery → Analyze → Facilitate (Sprint) → Build → Deliver. Custom line icons. Justifies premium rate via certainty/process.
- **04 — Keahlian & Tools:** clean grid (BA tools, Agile, Flutter, PHP/Laravel, React, AI tooling).
- **05 — Perjalanan:** minimal timeline (MSP/INAICTA 2013 → GameKita → Berca → Petrokimia).
- **Kontak/Footer:** big CTA "Punya masalah operasional? Mari bicarakan." + email + LinkedIn + GitHub.

## 6. Technical Approach

- **Single file** `index.html`. No build step. Opens directly in browser; deployable free to GitHub Pages / Netlify drag-drop.
- Tailwind via CDN (`cdn.tailwindcss.com`) with inline config for custom palette/fonts.
- Google Fonts: one display serif + one monospace.
- Vanilla JS for: ID/EN language toggle (data-attribute driven), count-up on scroll (IntersectionObserver), scroll-reveal, hero canvas/SVG animation, card hover detail.
- **Bilingual:** all copy stored as `data-id` / `data-en` (or a JS dictionary keyed by language); toggle swaps text and persists choice in `localStorage`. Default: ID.
- Accessibility: semantic HTML, alt text, sufficient contrast, `prefers-reduced-motion` disables heavy animation.
- Performance: defer non-critical JS, lazy-friendly; target fast first paint.

## 7. Components / Units (each independently understandable)

- **i18n module** — dictionary + toggle + localStorage. Input: lang key. Output: swapped text. No dependency on other modules.
- **count-up** — animates a number from 0 to target when scrolled into view. Input: target + format. Self-contained.
- **scroll-reveal** — adds reveal class on intersection. Generic, reusable.
- **hero-bg** — canvas/SVG animation; respects reduced-motion; isolated, can be swapped without touching content.
- **product-card** — presentational; data passed via markup. Hover detail self-contained.

## 8. Out of Scope (YAGNI)

- No CMS, no backend, no blog (can be added later if site moves to Astro).
- No testimonials (none verified yet).
- No exclusivity/scarcity messaging.
- No contact form backend (mailto link only for v1).

## 9. Success Criteria

- A visitor understands "what Yafi does + that he's proven + worth premium rate" within ~3 seconds of the hero.
- Proof (rupiah savings, TKMPN, scale) is visible without scrolling far.
- Does not look like a generic AI template.
- Works as one file, opens offline, deploys free.
- ID/EN toggle works and persists.
