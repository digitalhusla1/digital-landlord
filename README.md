# The Digital Landlord Masterclass — Landing Page

A high-conversion, single-file direct-response marketing funnel for **The Digital Landlord Masterclass**, a course teaching the Ghost Host System™: managing Airbnb properties remotely for 20% commission without owning any real estate.

---

## Project structure

```
index.html          # Live funnel — all HTML, CSS, and JS in one file (1100+ lines)
ml-preview.html     # MailerLite embed preview for form design/testing only — do NOT use as a template
background_img_01.png  # Hero section background image
.github/
  copilot-instructions.md  # AI coding agent guidelines
```

---

## Quick start

No build step. No dependencies. Open directly in a browser:

```bash
# XAMPP / local server
http://localhost/Digital_Landlord/

# Or just double-click the file in Explorer to open in your default browser
# Or use VS Code → Right-click → Open with Live Server
```

---

## Funnel architecture

The page follows a linear direct-response sequence:

| Section | Class | Purpose |
|---|---|---|
| Hero | `.hero` | Hook + primary CTA |
| Problem | `.problem` | Pain agitation (6 problem cards) |
| Solution | `.solution` | Ghost Host System™ introduction |
| Bundle | `.bundle` | 3-module course breakdown |
| Math | `.math` | Income projection calculator |
| Social Proof | `.social-proof` | 3 testimonials |
| Guarantee | `.guarantee` | 60-day money-back |
| Final CTA | `.final-cta` | Closing offer + secondary CTA |
| Footer | `footer` | Legal / brand |

---

## Lead capture flow (critical — do not break)

1. All CTA buttons carry class `.open-lead-modal` and `data-cta-source="<label>"`.
2. Clicking a button calls `openModal(source)` inside the IIFE, which sets the hidden `#ctaSource` field and shows `#leadModal`.
3. On submit, `navigator.sendBeacon` fires a `POST` to MailerLite; falls back to `form.submit()` targeting `<iframe name="mailerliteTarget">`.
4. After **1 200 ms**, the page redirects to the Selar checkout page.

```
MailerLite account : 2220881
MailerLite form ID : 182919199897683921
Endpoint           : https://assets.mailerlite.com/jsonp/2220881/forms/182919199897683921/subscribe
Checkout URL       : const REDIRECT_URL = 'https://selar.com/7b745z1401'  ← single source of truth
```

**To update the checkout link** — change only `REDIRECT_URL` near the top of the `<script>` block.  
**To update the MailerLite form** — update both `<form action="…">` and the endpoint in the IIFE.

---

## Visual design tokens

| Token | Value | Used for |
|---|---|---|
| Navy dark | `#1a1a2e` | Backgrounds, headings |
| Navy mid | `#16213e` | Gradient partner |
| Orange accent | `#ffa500` | CTAs, highlights, borders |
| Orange hover | `#ff8c00` | Button hover state |
| Orange dark | `#d84315` | Income figures, guarantee heading |

- CTA pill buttons: `border-radius: 50px`
- Cards: `border-radius: 8px–12px` + `box-shadow: 0 5px 15px rgba(0,0,0,0.08)`
- Single responsive breakpoint: `@media (max-width: 768px)`

---

## Key numbers (must stay internally consistent)

| Fact | Value | Locations |
|---|---|---|
| Students enrolled | `2,847+` | Hero trust badge, Final CTA |
| Commission rate | `20%` | Solution, Math, throughout |
| Guarantee period | `60 days` | Guarantee section |
| Checkout destination | `selar.com/7b745z1401` | `REDIRECT_URL` constant |
| Brand owner | `AESVentures` | Footer copyright |

> **⚠️ Income math note:** The `$1,105/month per property` figure in the Math section is derived from `$85 × 65` (treating 65% occupancy as 65 nights). The mathematically correct figure at 65% occupancy of 30 days is `30 × 0.65 × $85 × 0.20 ≈ $332/month`. Update all income figures together if corrected.

---

## Testing the form flow

1. Open `index.html` in a browser.
2. Click any orange CTA button to open the modal.
3. Submit with a real email address.
4. Open DevTools → Network tab → look for the `subscribe` request to verify the MailerLite `sendBeacon` payload includes `fields[name]`, `fields[email]`, and `fields[cta_source]`.
5. After ~1.2 s the page should redirect to `REDIRECT_URL`.

---

## Editing guidelines

- All HTML, CSS, and JS live in `index.html` — keep it that way unless explicitly asked to split.
- Add new interactivity inside the existing `(function(){ … })()` IIFE.
- Do not introduce npm packages, CDN libraries, or CSS frameworks.
- If the hero background image (`background_img_01.png`) changes, verify the overlay gradient still provides sufficient contrast for white text.
- `ml-preview.html` is for MailerLite form visual QA only — it uses a different endpoint (`dashboard.mailerlite.com`) and posts to `_blank`. Never reference it from `index.html`.

---

## Brand & legal

- Copyright line: `© 2026 AESVentures. All rights reserved.`
- Required disclaimer: *"The Digital Landlord Masterclass is not affiliated with Airbnb Inc. and is for educational purposes only."*
- Both must remain in `<footer>` unless a compliance change is explicitly requested.
