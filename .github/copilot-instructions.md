# Copilot Instructions for Digital_Landlord

## Big picture
- This is a static, single-page marketing funnel with no backend, build step, or package manager.
- Primary app surface is `index.html` (HTML + CSS + JS in one file).
- `ml-preview.html` is MailerLite visual/embed preview only; do not use it as implementation source.

## Core architecture and data flow
- Funnel order is intentional and should stay linear: `hero` → `problem` → `solution` → `bundle` → `math` → `social-proof` → `guarantee` → `final-cta` → `footer`.
- Styling is section-scoped with a shared `.container`; extend existing classes before introducing new structural patterns.
- Interactivity lives in one IIFE at the end of `index.html`; keep new JS inside that scope.

## Lead capture integration (critical)
- CTA buttons use `.open-lead-modal` + `data-cta-source`; `openModal()` writes that value into hidden `#ctaSource` (`fields[cta_source]`).
- Form posts to MailerLite endpoint `https://assets.mailerlite.com/jsonp/2220881/forms/182919199897683921/subscribe`.
- Submit flow: `navigator.sendBeacon(form.action, new FormData(form))` with fallback to `form.submit()` targeting hidden `<iframe name="mailerliteTarget">`.
- Successful submit redirects after 1200 ms via `const REDIRECT_URL = 'https://selar.com/7b745z1401'` (single source of truth for checkout URL).
- If MailerLite IDs/endpoints change, update both the form `action` and JS assumptions together.

## Project-specific conventions
- Keep HTML, CSS, and JS colocated in `index.html` unless explicitly asked to split files.
- Preserve visual system: navy gradient base (`#1a1a2e`, `#16213e`), orange accent (`#ffa500`), pill CTAs (`border-radius: 50px`), rounded cards + soft shadows.
- Responsive approach is a single primary breakpoint at `@media (max-width: 768px)`; avoid adding framework-style breakpoint systems.
- Maintain modal accessibility wiring (`aria-hidden`, `role="dialog"`, `aria-modal`, `aria-labelledby`, `aria-live`) when editing modal markup.

## Content and consistency guardrails
- Keep cross-page marketing numbers internally consistent (e.g., `20%`, `60 days`, `2,847+`).
- Preserve persuasive sequence (pain → solution → proof → guarantee → CTA) unless user asks for strategic rewrite.
- Keep footer legal/brand text: `© 2026 AESVentures` and Airbnb non-affiliation disclaimer unless explicitly requested.

## Developer workflow and deployment
- Local preview: open `index.html` directly in browser or serve from local web root.
- Form QA: submit modal and confirm MailerLite request payload includes `fields[name]`, `fields[email]`, `fields[cta_source]`, then redirect to `REDIRECT_URL`.
- Netlify behavior (`netlify.toml`): no build command, publish root — `index.html` is served automatically at `/`.

## Reference files
- `index.html`: source of truth for layout, styles, JS behavior, and lead flow.
- `ml-preview.html`: MailerLite form preview sandbox only (uses different endpoint/target behavior).
- `netlify.toml`: static hosting + SPA-style redirect setup.
