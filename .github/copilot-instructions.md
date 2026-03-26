# Copilot Instructions for Digital_Landlord

## Big picture
- Static, single-file marketing funnel — no backend, no build step, no package manager, no CDN libraries, no CSS frameworks.
- All HTML, CSS, and JS live in `index.html` (~1 100 lines). Keep them colocated unless explicitly asked to split.
- `ml-preview.html` is a MailerLite form sandbox only — it uses a different endpoint (`dashboard.mailerlite.com`) and posts to `_blank`. Never reference it from `index.html` or use it as an implementation source.

## Funnel architecture (do not reorder)
`hero` → `problem` → `solution` → `bundle` → `math` → `comparison` → `social-proof` → `system-bio` → `guarantee` → `bonus-stack` → `faq` → `final-cta` → `footer`

Each section has a scoped CSS class (`.hero`, `.bundle`, `.social-proof`, etc.) sharing `.container`. Extend existing classes before introducing new structural patterns.

## JavaScript scope
All interactivity lives in a single IIFE at the bottom of `index.html`. Add new JS inside that scope — never add external scripts or element-level `onclick` handlers.

## Lead capture flow (critical — do not break)
1. Every CTA button needs class `.open-lead-modal` + `data-cta-source="<label>"`.
2. `openModal(source)` writes source into hidden `#ctaSource` (`name="fields[cta_source]"`).
3. Submit: `navigator.sendBeacon(form.action, new FormData(form))` → fallback `form.submit()` targeting `<iframe name="mailerliteTarget">`.
4. Redirect fires after **1 200 ms** via `const REDIRECT_URL = 'https://selar.com/7b745z1401'` — single source of truth for the checkout URL.
5. Modal accessibility wiring (`aria-hidden`, `role="dialog"`, `aria-modal`, `aria-labelledby`, `aria-live`) must be preserved on every edit.

**MailerLite IDs** — if changed, update both the `<form action="…">` attribute and the IIFE endpoint together:
- Account: `2220881` | Form: `182919199897683921`
- Endpoint: `https://assets.mailerlite.com/jsonp/2220881/forms/182919199897683921/subscribe`

## Visual design tokens
| Token | Value | Usage |
|---|---|---|
| Navy dark | `#1a1a2e` | Backgrounds, headings, modal text |
| Navy mid | `#16213e` | Gradient partner |
| Orange accent | `#ffa500` | CTAs, highlights, left borders, stars |
| Orange hover | `#ff8c00` | Button hover state |
| Orange dark | `#d84315` | Income figures, guarantee heading |

- CTA pill buttons: `border-radius: 50px`
- Cards: `border-radius: 8px–12px` + `box-shadow: 0 5px 15px rgba(0,0,0,0.08)`
- Single responsive breakpoint: `@media (max-width: 768px)` — do not add more breakpoints

## Brand voice guardrails
- ❌ Never mention `AESVentures`, `AES`, or any third-party entity name in marketing copy (testimonials, headings, body text). The footer legal line `© 2026 AESVentures` is the only exception.
- ✅ Always refer to the system/brand as **"Digital Landlord"** in copy.
- ✅ Refer to students/community as **"Digital Landlords"** (not "users" or "students").
- ✅ No income guarantees — keep claims realistic and outcome-based.

## Content consistency guardrails
Keep these values consistent across all sections:

| Fact | Value | Locations |
|---|---|---|
| Students | `2,847+` | Hero trust badge, Final CTA |
| Commission | `20%` | Solution, Math, throughout |
| Guarantee | `60 days` | Guarantee section |
| Checkout URL | `selar.com/7b745z1401` | `REDIRECT_URL` constant only |
| Brand | `AESVentures` | Footer copyright only |
| Bonus value | `$497` | Bonus stack section, final CTA button |

⚠️ **Income math warning:** The `$1,105/month per property` figure in the Math section is derived from `$85 × 65` (not a standard occupancy formula — mathematically correct at 65% of 30 nights is `≈ $332/month`). If you correct the math, update **all** income figures together.

## Developer workflow
- **Local:** open `index.html` in browser, or `http://localhost/Digital_Landlord/` on XAMPP.
- **Form QA:** DevTools → Network → verify `subscribe` beacon contains `fields[name]`, `fields[email]`, `fields[cta_source]` → confirm redirect to `REDIRECT_URL` after ~1.2 s.
- **Deploy:** push to Netlify — `netlify.toml` sets publish root to `.` with no build command; `index.html` is served at `/`.
- **Hero image:** if `background_img_01.png` changes, verify the overlay gradient (`rgba(26,26,46,0.88)`) still provides sufficient contrast for white text.

## Legal (preserve unless explicitly asked to change)
Footer must always contain both:
- `© 2026 AESVentures. All rights reserved.`
- *"The Digital Landlord Masterclass is not affiliated with Airbnb Inc. and is for educational purposes only."*
