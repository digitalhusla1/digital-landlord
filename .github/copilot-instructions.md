# Copilot Instructions for Digital_Landlord

## Project overview
- Two-file repo: `landing-page.html` is the live marketing funnel (HTML + CSS + JS, 1100+ lines); `ml-preview.html` is a standalone MailerLite embed preview used for form design/testing only—do not treat it as a template.
- No framework, bundler, package manager, test suite, or backend. Keep all changes in-file unless explicitly asked to split assets.
- The page sells one offer, **The Digital Landlord Masterclass**, so edits must preserve the direct-response funnel flow unless a redesign is explicitly requested.

## Architecture and data flow
- Linear funnel order: `hero` → `problem` → `solution` → `bundle` → `math` → `social-proof` → `guarantee` → `final-cta` → `footer`.
- Layout is driven by section-scoped classes plus a shared `.container`; extend existing patterns rather than introducing a new layout system.
- **Lead-capture flow (critical — do not break):**
  1. Any `.open-lead-modal` button fires `openModal(button.dataset.ctaSource)`, which sets hidden `#ctaSource` (`fields[cta_source]` in MailerLite) and shows `#leadModal`.
  2. On submit, `navigator.sendBeacon` posts to `https://assets.mailerlite.com/jsonp/2220881/forms/182919199897683921/subscribe` (MailerLite account `2220881`, form `182919199897683921`); falls back to `form.submit()` into the hidden `<iframe name="mailerliteTarget">`.
  3. After 1 200 ms, the page redirects to `const REDIRECT_URL = 'https://selar.com/7b745z1401'` (the Selar checkout). **This constant is the single source of truth for the purchase URL.**
  4. Form field names are MailerLite-namespaced: `fields[name]`, `fields[email]`, `fields[cta_source]`.

## Editing conventions
- Keep HTML, CSS, and JS colocated in `landing-page.html`: `<style>` in `<head>`, one closing `<script>` block wrapping a single IIFE `(function(){ … })()`.
- Visual language: dark navy gradients (`#1a1a2e`, `#16213e`), orange accent (`#ffa500`), 50 px pill buttons, rounded cards, subtle box shadows, section-scoped class names like `.problem-card` and `.bundle-item`.
- Responsive strategy: one `@media (max-width: 768px)` block in the `<style>` tag plus small targeted overrides—do not introduce additional breakpoints or a CSS framework.
- New interactivity belongs inside the existing IIFE; do not introduce libraries or global variables.
- Modal accessibility attributes (`aria-hidden`, `role="dialog"`, `aria-modal`, `aria-labelledby`, `aria-live`) must be kept in sync if the modal markup changes.

## Copy and content guardrails
- Numbers that appear in multiple places (student count `2,847+`/`2,800+`, commission rate `20%`, guarantee period `60 days`) must stay internally consistent—update all occurrences together.
- Preserve the persuasion sequence: pain points → solution → proof → guarantee → CTA.
- Keep the footer's `© 2026 AESVentures` brand line and the Airbnb non-affiliation disclaimer unless a compliance change is requested.

## Workflow notes
- Preview by opening `landing-page.html` directly in a browser or the VS Code Simple Browser—there is no build step.
- To test the full form flow, submit the modal form; on success the page redirects to `REDIRECT_URL`. Use browser DevTools Network tab to verify the MailerLite `sendBeacon` payload.
- Do not add npm, frameworks, or external CDN links for routine edits.
- Favor minimal, surgical changes. DOM hooks and readable inline content are intentional; do not abstract them away.

## Key references in `landing-page.html`
- Hero background image: `background_img_01.png` — verify overlay contrast after any hero styling changes.
- Checkout destination: `const REDIRECT_URL` near the top of the `<script>` block.
- MailerLite form endpoint and IDs are hardcoded in both the `<form action="…">` and the IIFE — update both if the form changes.
