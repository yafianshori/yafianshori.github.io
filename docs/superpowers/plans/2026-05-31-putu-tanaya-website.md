# Putu Tanaya Personal Brand Website — Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build a single-file, bilingual (ID/EN) personal brand landing page for Putu Alicia Sarah Sativa Tanaya that positions her as a hybrid builder-marketer and attracts clients, in a "Floral Editorial" (girly + professional) visual style.

**Architecture:** One self-contained `putu/index.html` (HTML + Tailwind CDN config + inline `<script>`/`<style>`). No build step, no backend. Content is bilingual via a JS dictionary keyed by language, toggled and persisted in `localStorage`. Progressive enhancement: vanilla JS adds count-up, scroll-reveal, hero animation, and card hover — page is fully readable with JS disabled. Built section-by-section so each commit leaves a working, openable page.

**Tech Stack:** HTML5, Tailwind CSS (CDN `cdn.tailwindcss.com` with inline config), Google Fonts (Fraunces display serif + JetBrains Mono), vanilla JS, inline SVG petal motifs.

**Source of truth for ALL content:** `docs/superpowers/specs/2026-05-30-putu-tanaya-website-design.md`. Do not invent facts beyond it. Honor the attribution guardrail (§3): team/co-founded wins are never solo accolades; no fabricated campaign metrics.

**Design quality requirement (§4):** This must NOT look AI-generated. Before writing visual CSS, invoke the **ui-ux-pro-max** skill (and consult **frontend-design**) and apply its metric-based rules — intentional type scale, real spacing system, restrained palette, tasteful motion. No emoji, no purple→blue gradient, no bubblegum/childish styling.

---

## File Structure

| File | Responsibility |
|------|----------------|
| Create: `putu/index.html` | The entire site — markup, styles, content dictionary, and behavior. Single file by design (§6). |
| Reference: `docs/superpowers/specs/2026-05-30-putu-tanaya-website-design.md` | Content & design source of truth. |
| Reference: `index.html` (repo root, Yafi's site) | Pattern reference only — mirror its i18n/count-up/scroll-reveal approach for consistency. Do NOT modify it. |

The page is one file but is built in clearly-bounded sections (hero, 01 proof, 02 services, 03 process, 04 skills, 05 journey, footer) plus four JS modules (i18n, count-up, scroll-reveal, hero-accent) that are each independently understandable.

**Verification model:** No test framework exists in this repo (consistent with Yafi's site). Each task is verified by opening `putu/index.html` in a browser (or with the Playwright MCP if available) and confirming the described behavior, then committing. "Expected" describes what you must observe.

---

## Task 0: Scaffold the file and design tokens

**Files:**
- Create: `putu/index.html`

- [ ] **Step 1: Invoke design skills**

Invoke the `ui-ux-pro-max` skill and skim `frontend-design`. Lock concrete tokens before writing markup: exact type scale (e.g. display 3.5–5rem, body 1rem/1.125rem), spacing scale (4/8px base), and the rose/blush palette from spec §4. Write these down as the Tailwind config you will use.

- [ ] **Step 2: Create the HTML skeleton**

Create `putu/index.html` with: `<!DOCTYPE html>`, `<html lang="id">`, `<head>` with charset, viewport, `<title>Putu Tanaya — Product · Brand · Growth</title>`, meta description (bilingual-neutral or ID default), Google Fonts preconnect + Fraunces + JetBrains Mono, Tailwind CDN script, and an inline `tailwind.config` with the custom palette tokens:

```js
tailwind.config = {
  theme: { extend: {
    colors: {
      paper: '#FBF4F1', blush: '#FDF4F7', blush2: '#FBEFF2',
      rose: '#C9607A', rose2: '#D17A95',
      ink: '#2B1A1F', ink2: '#3A2128',
      mauve: '#9A7782', mauve2: '#6B4A55',
    },
    fontFamily: { serif: ['Fraunces','Georgia','serif'], mono: ['"JetBrains Mono"','monospace'] },
  }}
}
```

Add an inline `<style>` for: grain/blush texture, `prefers-reduced-motion` guard, base `.reveal` (opacity/translate) + `.reveal.in` states, and `scroll-behavior:smooth`. Add an empty `<body class="bg-paper text-ink font-serif">` with a `<main>` placeholder.

- [ ] **Step 3: Verify it opens**

Open `putu/index.html` in a browser.
Expected: blank cream page, no console errors, fonts load (inspect: Fraunces applied to body).

- [ ] **Step 4: Commit**

```bash
git add putu/index.html
git commit -m "Scaffold Putu site: skeleton, fonts, rose/blush Tailwind tokens"
```

---

## Task 1: i18n module + language toggle

**Files:**
- Modify: `putu/index.html`

- [ ] **Step 1: Add the content dictionary**

In an inline `<script>`, define `const I18N = { id: {...}, en: {...} }`. Seed it with the hero keys only for now (more keys added per section): `nav_cta`, `hero_eyebrow`, `hero_title_a`, `hero_title_em`, `hero_sub`. Use spec copy:
- ID `hero_title_a`: "Membangun produk —", `hero_title_em`: "dan brand yang menjualnya."
- EN `hero_title_a`: "I build products —", `hero_title_em`: "and the brands that sell them."
- CTA: ID "Kerja Sama" / EN "Work With Me".

- [ ] **Step 2: Add the apply + toggle logic**

```js
function applyLang(lang){
  document.documentElement.lang = lang;
  document.querySelectorAll('[data-i18n]').forEach(el=>{
    const k = el.getAttribute('data-i18n');
    if (I18N[lang][k] != null) el.textContent = I18N[lang][k];
  });
  localStorage.setItem('putu_lang', lang);
  document.querySelectorAll('[data-lang-btn]').forEach(b=>
    b.classList.toggle('font-bold', b.dataset.langBtn===lang));
}
const savedLang = localStorage.getItem('putu_lang') || 'id';
document.addEventListener('DOMContentLoaded', ()=>{
  applyLang(savedLang);
  document.querySelectorAll('[data-lang-btn]').forEach(b=>
    b.addEventListener('click', ()=>applyLang(b.dataset.langBtn)));
});
```

- [ ] **Step 3: Add a minimal nav with the toggle**

Add a top nav bar: left "PUTU TANAYA · CACA" (mono, mauve), right an `ID · EN` toggle (`<button data-lang-btn="id">ID</button> · <button data-lang-btn="en">EN</button>`) and a CTA button `<a data-i18n="nav_cta" href="#kontak">`. Give the hero title elements `data-i18n` attributes.

- [ ] **Step 4: Verify toggle works**

Open the page. Click EN → hero text + CTA switch to English, EN button bolds. Reload → language persists (localStorage). Click ID → switches back.
Expected: all `data-i18n` text swaps, choice persists across reload, default is ID on first visit (clear localStorage to test).

- [ ] **Step 5: Commit**

```bash
git add putu/index.html
git commit -m "Add i18n dictionary, language toggle, persistence, nav bar"
```

---

## Task 2: Hero section + count-up + hero accent

**Files:**
- Modify: `putu/index.html`

- [ ] **Step 1: Build hero markup**

Below the nav, add the hero: eyebrow (mono, rose) "PRODUCT · PROJECT · BRAND · GROWTH"; display headline using `hero_title_a` + an `<em class="text-rose italic">` for `hero_title_em`; sub-paragraph (`hero_sub`, sans/serif body, mauve2, max-width ~46ch):
- ID: "Dari IT & startup ke performance marketing. Saya pimpin proyek dari ide sampai bertumbuh — paham produknya, paham angkanya."
- EN: "From IT & startups to performance marketing. I lead projects from idea to growth — I understand the product and the numbers."

Then the CTA row (primary `Kerja Sama` pill + secondary `[LinkedIn]` mono link with the real href `https://www.linkedin.com/in/putu-alicia-sarah-sativa-tanaya-4744401a9/`, `target="_blank" rel="noopener"`) and the three proof metrics as a flex/grid row, each with a big number + mono caption:
- `Rp 13 jt` — caption ID "GEMASTIK 10 (tim)" / EN "GEMASTIK 10 (team)"
- `CEO` — caption "STARTUP BELUMNEMU"
- `Top 5` — caption ID "MAWAPRES UISI 2019" / EN "TOP STUDENT UISI 2019"

Add all new strings to `I18N`. Give metric numbers `data-countup` where numeric (`13`) and leave non-numeric (`CEO`, `Top 5`) as static text. Mark hero metric labels with `data-i18n`.

- [ ] **Step 2: Add count-up module**

```js
function countUp(el){
  const target = parseFloat(el.dataset.countup);
  const prefix = el.dataset.prefix||'', suffix = el.dataset.suffix||'';
  const dur = 1100; let start=null;
  function tick(t){ if(!start) start=t; const p=Math.min((t-start)/dur,1);
    const val = Math.round(target*p);
    el.textContent = prefix + val.toLocaleString('id-ID') + suffix;
    if(p<1) requestAnimationFrame(tick); }
  requestAnimationFrame(tick);
}
```
For `Rp 13 jt`: set `data-prefix="Rp "` `data-suffix=" jt"` `data-countup="13"`. Numbers must stay ID-formatted in both languages (spec §3).

- [ ] **Step 3: Wire count-up to IntersectionObserver (shared with reveal in Task 8)**

For now, a simple observer that calls `countUp` once when a `[data-countup]` enters view; guard with a `dataset.done` flag. If `prefers-reduced-motion`, set final value immediately without animating.

- [ ] **Step 4: Add hero floral accent**

Add a subtle decorative layer: 2–3 large low-opacity (≤0.12) inline-SVG petal/floral shapes positioned in hero corners (absolute, `pointer-events:none`, behind text via z-index). Optional gentle CSS drift animation (`@keyframes` slow translate/rotate), disabled under `prefers-reduced-motion`. No emoji.

- [ ] **Step 5: Verify hero**

Open page at 1280×800. Expected: hero fills viewport without scroll, shows name/headline/pillars/≥2 proof metrics/CTA (spec §9). `Rp 13 jt` counts up from 0 once on load/scroll-in. Petals are faint and behind text. Toggle EN → all hero copy swaps, `Rp 13 jt` stays ID-formatted. Set OS reduced-motion → no drift, number appears instantly.

- [ ] **Step 6: Commit**

```bash
git add putu/index.html
git commit -m "Add hero: hybrid headline, proof metrics, count-up, floral accent"
```

---

## Task 3: Section 01 — Yang Saya Bawa (proof cards)

**Files:**
- Modify: `putu/index.html`

- [ ] **Step 1: Build proof cards**

Add `<section id="proof">` with mono section label "01 — YANG SAYA BAWA" (EN "01 — WHAT I BRING"). A responsive grid (2-col desktop, 1-col mobile) of 4 rounded soft cards, each: title, a role badge pill, one-line description (hover reveals full detail via `max-height`/opacity transition). Content from spec §3 with honest badges:
- **BelumNemu** — badge "CEO" — ID "Startup: tag scan untuk barang hilang. Memimpin tim produk (co-founder)." Detail names Fitri Kharismawati (PM), Zeni Maliha (Designer).
- **GEMASTIK 10** — badge ID "Juara 1 · tim" / EN "1st · team" — "Idea Rush + Best Poster. Repotizen (IoT). Tim Aphrodite." Detail: Rp 13 jt, UI 2017.
- **Mawapres + Pilmapres** — badge ID "personal" — "Top 5 Mawapres UISI 2019 · Runner-up 2 Pilmapres Kopertis VII."
- **Canbeauty → Albata** — (no award badge) — ID "Marketing Mgr → Digital Marketing. Beauty & edukasi."

Add all strings to `I18N`. Give each card `class="reveal"`.

- [ ] **Step 2: Verify**

Open page. Expected: 4 cards render in rose-bordered rounded style; hover expands detail; badges read as team/personal honestly (no "I won" phrasing); EN toggle swaps all card copy; `Rp 13 jt` in detail stays ID-formatted.

- [ ] **Step 3: Commit**

```bash
git add putu/index.html
git commit -m "Add section 01: honest proof cards with role badges"
```

---

## Task 4: Section 02 — Layanan (services)

**Files:**
- Modify: `putu/index.html`

- [ ] **Step 1: Build service cards**

Add `<section id="layanan">` with label "02 — LAYANAN" (EN "02 — SERVICES"). 4 cards (4-col desktop → 2-col tablet → 1-col mobile), each with a small custom line/petal icon (inline SVG), title, one-line scope:
- **Product / PM** — ID "Ide → produk → kelola tim & roadmap."
- **Brand Strategy** — ID "Positioning, identity, messaging."
- **Performance / Paid Ads** — ID "Paid social/search, optimasi ROAS."
- **Content & Data** — ID "Konten, social, analytics & insight."

Add strings to `I18N`; `class="reveal"` on each.

- [ ] **Step 2: Verify**

Open page. Expected: 4 service cards render with icons, responsive collapse works at narrow widths, EN toggle swaps copy.

- [ ] **Step 3: Commit**

```bash
git add putu/index.html
git commit -m "Add section 02: four service cards with line icons"
```

---

## Task 5: Section 03 — Cara Saya Bekerja (process)

**Files:**
- Modify: `putu/index.html`

- [ ] **Step 1: Build process row**

Add `<section id="proses">` label "03 — CARA SAYA BEKERJA" (EN "03 — HOW I WORK"). A 5-step horizontal flow (wraps/stacks on mobile) with custom line/petal step icons and connectors: Discover → Strategy → Build → Launch → Optimize. Each step: number, label (`data-i18n`), one short line. Translate labels (e.g. ID "Riset → Strategi → Bangun → Luncurkan → Optimasi"). `class="reveal"`.

- [ ] **Step 2: Verify**

Open page. Expected: 5 steps with connectors on desktop, stacked on mobile; EN toggle swaps labels.

- [ ] **Step 3: Commit**

```bash
git add putu/index.html
git commit -m "Add section 03: five-step working process"
```

---

## Task 6: Section 04 — Keahlian & Tools (skills)

**Files:**
- Modify: `putu/index.html`

- [ ] **Step 1: Build skills grid**

Add `<section id="keahlian">` label "04 — KEAHLIAN & TOOLS" (EN "04 — SKILLS & TOOLS"). Grouped tag/grid layout: **Build** (IT/Informatika, Project Management, product), **Market** (Meta/Google Ads, GA4, content & social), **Creative/Research** (AI×human creative — note her Jurnal Sosioteknologi 2020 paper). Tags as small rounded chips. `class="reveal"`.

- [ ] **Step 2: Verify**

Open page. Expected: grouped chips render; research nod present and honestly framed (co-authored); EN toggle swaps group headings.

- [ ] **Step 3: Commit**

```bash
git add putu/index.html
git commit -m "Add section 04: grouped skills & tools with research nod"
```

---

## Task 7: Section 05 — Perjalanan (timeline) + Footer/Contact

**Files:**
- Modify: `putu/index.html`

- [ ] **Step 1: Build timeline**

Add `<section id="perjalanan">` label "05 — PERJALANAN" (EN "05 — JOURNEY"). Minimal vertical/horizontal timeline: UISI Informatika → CEO BelumNemu → Canbeauty (Marketing Mgr) → Albata (Digital Marketing). Each node: year/phase + one line. `class="reveal"`.

- [ ] **Step 2: Build footer / contact**

Add `<footer id="kontak">`: big CTA headline ID "Punya produk atau brand yang ingin bertumbuh? Mari bekerja sama." / EN equivalent; the philosophy quote (italic serif) "It's not about the title but how you uphold a responsibility."; primary `Kerja Sama` CTA; and contact links **as clearly-marked placeholders** — `mailto:EMAIL_TBD`, Instagram `@HANDLE_TBD`, LinkedIn (real URL from spec is OK: `in/putu-alicia-sarah-sativa-tanaya-4744401a9`). Add an HTML comment `<!-- BLOCKING: confirm real email + IG/WA before launch (spec §3) -->`.

- [ ] **Step 3: Verify**

Open page. Expected: timeline renders in order; footer shows CTA + quote + contact; placeholders clearly fake; LinkedIn link correct; EN toggle swaps footer copy (quote may stay English intentionally — it's her actual quote).

- [ ] **Step 4: Commit**

```bash
git add putu/index.html
git commit -m "Add section 05 timeline + footer with CTA, quote, contact placeholders"
```

---

## Task 8: Scroll-reveal, polish pass, accessibility & responsive QA

**Files:**
- Modify: `putu/index.html`

- [ ] **Step 1: Wire scroll-reveal observer**

Add one `IntersectionObserver` that adds `.in` to every `.reveal` element when it enters view (unobserve after). Under `prefers-reduced-motion`, add `.in` to all immediately (no transition). Confirm count-up shares or coexists cleanly with this observer.

- [ ] **Step 2: Design polish pass (ui-ux-pro-max)**

Re-invoke `ui-ux-pro-max` judgment: verify type scale rhythm, consistent spacing, section-number alignment, rule weights, petal-motif restraint, hover states, button/focus states. Remove anything that reads as generic/AI-template. Ensure exactly one accent family, no stray gradients, no emoji.

- [ ] **Step 3: Accessibility checks**

Verify: semantic landmarks (`nav`/`main`/`section`/`footer`), all SVG decorative petals have `aria-hidden="true"`, interactive elements are focusable with visible focus rings, body text contrast meets WCAG AA on cream (test rose `#C9607A` is used for accents/large text, not small body copy — darken to `ink2` for small text if needed), `alt`/labels present.

- [ ] **Step 4: Responsive QA**

Check 1280×800 desktop, ~768 tablet, ~375 mobile. Expected: no horizontal overflow; hero meets §9 (full hero above fold on desktop; one short scroll OK on mobile); grids collapse correctly; nav usable on mobile.

- [ ] **Step 5: Reduced-motion + offline QA**

Toggle OS reduced-motion: no petal drift, no count-up animation (final values shown), reveals appear without transition. Open the file via `file://` with network off after first load is not required (CDN needs network) — but confirm it opens directly without a build step.

- [ ] **Step 6: Final verify against success criteria**

Walk spec §9 checklist line by line and confirm each. Fix any gaps.

- [ ] **Step 7: Commit**

```bash
git add putu/index.html
git commit -m "Wire scroll-reveal; polish, accessibility & responsive QA pass"
```

---

## Done / Handoff

- [ ] Open `putu/index.html` once more; confirm full ID→EN→ID cycle, all sections, all animations, mobile + desktop.
- [ ] Leave the contact placeholders + BLOCKING comment in place for the user to fill with real details before launch.
- [ ] Report completion with the spec §9 checklist results.

## Out of scope (do NOT build — spec §8)
- No CMS/backend/blog, no testimonials, no fabricated metrics, no contact-form backend, no shared nav with Yafi's site.
