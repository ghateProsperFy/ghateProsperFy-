# Eprosperfy CRO Sections — Project Spec
**Version:** 1.0  
**Status:** Approved — Ready to Build  
**Last Updated:** 2026-04-01

---

## 1. Problem & Goal

Most Shopify themes are designed for aesthetics, not conversion. Merchants using beautiful themes still lose sales because the layout, copy hierarchy, and trust signals are not optimized for buying decisions.

**Goal:** Deliver a library of modular, configurable Shopify OS 2.0 sections that embed proven CRO methodology into any existing store — without requiring a redesign or theme change.

**Secondary goals:**
- All sections must meet or exceed ADA and WCAG 2.1 AA accessibility standards (strive for AAA where feasible)
- All sections must be distinctly Eprosperfy products — improved, enhanced, and differentiated beyond any source reference material (see Section 9: IP Differentiation)

---

## 2. Section Inventory & Prioritization

**Total scope:** 50+ sections across all page types  
**V1 scope:** Homepage sections + Product Page sections (highest demand, most cross-page reusable)

### Priority Tiers

| Priority | Criteria |
|---|---|
| P1 | Page-agnostic — works on homepage, product page, collection, landing page |
| P2 | Homepage-specific — highest merchant demand |
| P3 | Product page-specific — highest conversion impact |
| P4 | Collection page, sales page, other |

### Known Cross-Page Sections (P1 — page-agnostic)
- Trust Bar / Icon Row ← **Sprint 1 start**
- Hero Banner ← **Sprint 1 start**
- Transformation / Before & After
- Social Proof / Testimonials
- FAQ / Progressive Disclosure
- CTA Band
- Image + Text Conversion Block
- Media / Press Logos Strip

### Known Page-Specific Sections
- Sticky Add-to-Cart bar → product page only
- Product grid → collection page primary, usable on homepage
- Variant selector + gallery → product page only

---

## 3. Technical Architecture

### 3A — Theme Compatibility
**OS 2.0 only.** No OS 1.0 support.  
Compatible with: Dawn, Sense, Crave, Impact, Studio, Craft, Colorblock, Ride, and all third-party OS 2.0 themes.

---

### 3B — Design Token Strategy (Two-Layer Styling)

**Layer 1 — Inherit from theme (default)**  
Sections reference the host theme's CSS custom properties:
```css
color: var(--color-foreground, #121212);
font-family: var(--font-body-family, sans-serif);
background: var(--color-background, #ffffff);
```

**Layer 2 — Section-level overrides (merchant-controlled)**  
Every section exposes override settings in its schema. If set, they take precedence. If not, theme tokens apply. Sections look native on first install and can be fully customized without touching code.

---

### 3C — Live Shopify Data

| Data type | Mechanism | Sections affected |
|---|---|---|
| Products | `collection.products` or `product` object | Product grids, featured product, hero with product |
| Collections | `section.settings.collection` (collection picker) | Collection grids, category cards |
| Star ratings — app | Compatible app block (Loox, Judge.me, Stamped) | Social proof, product cards |
| Star ratings — metafield | `product.metafields.[namespace].rating` | Same — default/fallback source |
| Star ratings — manual | Static merchant-entered string | New stores, no reviews yet |
| Inventory | `variant.inventory_quantity` | Urgency / scarcity sections |

**Review app strategy — resolved:**  
Default: metafield + manual entry  
Upgrade path: app block (merchant drops in compatible app block)

---

### 3D — Heavy Elements

**Video backgrounds:**
- `<video autoplay muted loop playsinline>`
- Always require a fallback image setting
- Schema toggle: `show_video_on_mobile` — **off by default** (Core Web Vitals)
- Lazy-load via `IntersectionObserver`

**Carousels / Sliders:**
- **Vanilla CSS scroll snap first** — zero JS, best performance
- Progressive enhancement: JS adds prev/next controls and optional autoplay
- No Swiper, Slick, or any external carousel library
- Schema settings: `autoplay`, `autoplay_speed`, `show_arrows`, `show_dots`

---

### 3E — File Structure Per Section

Every section ships as a self-contained installable package:

```
[section-name]/
├── sections/
│   └── eprosperfy-[section-name].liquid    ← Liquid + HTML + schema
├── assets/
│   ├── eprosperfy-[section-name]-styles.css
│   └── eprosperfy-[section-name].js        ← Only if JS is needed
├── snippets/
│   └── eprosperfy-[section-name]-[part].liquid  ← Only if sub-components needed
└── README.md                                ← Installation guide
```

**Naming convention:** All files prefixed with `eprosperfy-` to avoid collisions with host theme files.

Installation: copy files to matching theme folders. No `theme.liquid` edits required.

---

### 3F — Accessibility Standards (ADA / WCAG)

**Minimum standard: WCAG 2.1 AA. Target: AAA where feasible.**

Every section must pass:

| Criterion | Requirement |
|---|---|
| Color contrast — normal text | ≥ 4.5:1 ratio |
| Color contrast — large text / UI components | ≥ 3:1 ratio |
| Keyboard navigation | All interactive elements reachable and operable via keyboard |
| Focus indicators | Visible focus ring on all focusable elements (never `outline: none` without a custom replacement) |
| Screen reader support | Semantic HTML throughout; `aria-label`, `aria-expanded`, `aria-live` where needed |
| Images | `alt` text required via schema; decorative images use `alt=""` and `role="presentation"` |
| Motion | All animations respect `prefers-reduced-motion: reduce` |
| Touch targets | Minimum 44×44px tap target size on mobile |
| Form labels | All inputs have associated `<label>` elements |
| Heading hierarchy | Logical `h1`→`h2`→`h3` structure; never skip levels |
| Video | Captions required if video has spoken content; muted autoplay is exempt |
| Color as sole indicator | Never use color alone to convey meaning — pair with icon or text |

These requirements are checked against every section in the Shopify Best Practices Checklist (in the master prompt) before a section is considered done.

---

## 4. Delivery & Packaging Model

| Product tier | Contents | Delivery | Indicative price |
|---|---|---|---|
| Individual section | 1 section package + README | Zip download | $19–$49 |
| Page kit | All sections for one page type | Zip download | $99–$199 |
| Full library | All 50+ sections | Zip download | $499+ |
| Done-for-you | Any tier + installation, styling, production setup | Service | Custom quote |
| Retainer | Updates, new sections, priority support | Monthly / annual | Custom quote |

**First sales channel:** Eprosperfy store (Shopify)  
**Future channels:** Gumroad, dedicated CRO store, Shopify App Store

---

## 5. Branding

**Code prefix:** `eprosperfy-` (used in all file names and CSS class names)  
**Public brand name:** TBD — finalize before public launch; placeholder is `Eprosperfy CRO Sections`

---

## 6. Versioning & Updates

Every section file includes a version header:
```liquid
{% comment %}
  Section: [Name] | Version: 1.0.0 | Brand: Eprosperfy CRO Sections
{% endcomment %}
```

**V1:** Buyers receive a zip. Updates require re-download and manual file replacement.  
**Future app:** App pushes updates silently; merchant approves. Section code is identical — only the delivery mechanism changes.

---

## 7. Support Model

| Tier | Included |
|---|---|
| Self-serve | README + video walkthrough |
| Install-only | One-time setup, no ongoing support |
| 30-day support | Bug fixes, configuration questions |
| Ongoing retainer | Priority support, updates, new section requests, quarterly CRO review |

---

## 8. Build Plan

### Sprint 1 — Foundation (in progress)
Establish the code pattern, CSS architecture, and schema conventions that all future sections inherit.

| Section | Type | Status |
|---|---|---|
| Trust Bar / Icon Row | P1 cross-page | In progress |
| Hero Banner | P1 cross-page | In progress |

**Deliverable:** Two fully working, ADA-compliant, production-ready sections that serve as the reference implementation for all subsequent sections.

### Sprint 2 — Homepage (P2)
All homepage wireframe sections, top to bottom.

### Sprint 3 — Product Page (P3)
All product page wireframe sections. These reference live product data.

### Sprint 4 — Collection, Sales Page, Other (P4)
Remaining wireframe pages.

---

## 9. IP Differentiation Strategy

Sections are reverse-engineered from ConversionWise training materials for agency use, then **independently improved** to create distinctly Eprosperfy products. The following enhancements ensure the sections are original works:

### 9A — Accessibility as Differentiator
ConversionWise wireframes are not built to ADA/WCAG standards. Every Eprosperfy section meets WCAG 2.1 AA minimum and targets AAA. This is a substantive technical and structural difference built into every file.

### 9B — Performance as Differentiator
Sections are built to Core Web Vitals standards (LCP, CLS, INP):
- CSS scroll snap carousels instead of JS-heavy sliders
- `loading="lazy"` / `loading="eager"` image strategy
- No render-blocking external libraries
- Video off by default on mobile
- Deferred, scoped JavaScript

### 9C — Shopify-Native Integration
Sections leverage Shopify platform features ConversionWise wireframes cannot anticipate:
- Full schema configurability (metafields, product/collection pickers, color pickers)
- Shopify theme editor live preview (`shopify:section:load` events)
- Shopify CDN image transforms (`| image_url: width:`)
- OS 2.0 blocks for repeatable content

### 9D — Enhanced Merchant Control
Every section exposes significantly more configuration than any wireframe implies:
- Two-layer design token system (theme inherit + section override)
- Layout toggles, alignment options, column counts, mobile stacking behavior
- CRO-critical settings clearly labeled and explained

### 9E — CRO Improvements Beyond Source Material
During reverse engineering, actively identify and fix:
- Weak visual hierarchy
- Missing or unclear CTAs
- Accessibility gaps (color contrast, focus states, screen reader support)
- Mobile UX issues (thumb zone, tap target size, viewport overflow)
- Missing trust signals or urgency mechanics

### 9F — Extended Feature Set
Features added beyond what wireframes show:
- Dark mode support (`prefers-color-scheme: dark`)
- RTL language layout support where applicable
- Reduced motion variants
- Fallback states (no image uploaded, no reviews yet, no products in collection)

---

## 10. Out of Scope for V1

- Shopify app infrastructure
- OS 1.0 theme support
- A/B testing framework built into sections
- Analytics / tracking integration
- Custom checkout sections (Shopify Plus only)
- Headless / Hydrogen support

---

## 11. Open Decisions

All open decisions from scoping are resolved:

| # | Decision | Resolution |
|---|---|---|
| 1 | Brand name | `Eprosperfy CRO Sections` (placeholder until finalized) |
| 2 | First sections to build | Trust Bar / Icon Row + Hero Banner |
| 3 | Review app default | Metafield + manual; app block as upgrade path |
| 4 | Video mobile behavior | Off by default |
| 5 | Carousel library | Vanilla CSS scroll snap |
| 6 | Accessibility standard | WCAG 2.1 AA minimum, target AAA |
| 7 | IP differentiation | Via accessibility, performance, Shopify-native features, and enhancements (see Section 9) |

---

## 12. Definition of Done (Per Section)

A section is complete when it passes all of the following:

- [ ] All visible content driven by schema settings — no hardcoded strings
- [ ] `presets` entry present (drag-and-droppable in theme editor)
- [ ] Two-layer styling: inherits theme tokens, overridable via schema
- [ ] JS handles `shopify:section:load` for live editor preview
- [ ] Images use `loading="lazy"` (except hero: `loading="eager"`)
- [ ] All CTAs are `<a href>` or `<button type>` — no `<div>` click handlers
- [ ] WCAG 2.1 AA color contrast verified (≥ 4.5:1 normal, ≥ 3:1 large text)
- [ ] All interactive elements keyboard-navigable with visible focus ring
- [ ] `aria-label`, `aria-expanded`, `aria-live` applied where needed
- [ ] Animations respect `prefers-reduced-motion`
- [ ] Touch targets ≥ 44×44px on mobile
- [ ] Semantic HTML and logical heading hierarchy
- [ ] CSS scoped to section — no host theme collisions
- [ ] Works independently — no dependency on other sections
- [ ] Video: fallback image set, mobile off by default
- [ ] Carousel: CSS scroll snap, progressive JS enhancement
- [ ] README installation guide complete
- [ ] Productization Notes written with benefit-led copy
- [ ] Version header in Liquid file
- [ ] Tested in Dawn (reference theme) at mobile, tablet, desktop breakpoints
