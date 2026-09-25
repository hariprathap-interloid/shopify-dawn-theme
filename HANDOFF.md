# Project handoff: Oroskin Shopify theme (custom, built on Skeleton)

> **How to use this file:** paste its contents into a new Claude chat as the first message.
> Last updated: 2026-09-25

## Who I am

Frontend developer. Strong HTML, CSS, JavaScript and responsive design. **Beginner in Shopify, Liquid and theme development.** Explain Shopify/Liquid concepts in simple, clear sentences, and tell me *why*, not just *what*. Never assume I know Shopify jargon.

## What the project is

Build a **fully custom Shopify theme** from a Figma design for a brand called Oroskin.

- The Figma design is **completely custom** and looks nothing like Dawn.
- It has **scroll-triggered animations**.
- Goal 1: an agency **showcase site** to win new clients.
- Goal 2 (later, optional): list the theme on the **Shopify Theme Store**.

## Decisions already made (do not re-litigate)

1. **Start from Shopify's Skeleton theme**, not Dawn, and not an empty folder.
   Reason: Shopify's Theme Store requirements state that themes *"built on or derived from Dawn or Horizon are not eligible"* and that *"Shopify's Skeleton Theme is the only approved codebase for Theme Store development."* Stripping Dawn would also mean fighting its shared `base.css` / `global.js` on every page.
2. **Dawn stays as a read-only reference** at `d:\shopify-oroskin\dawn`. Rule: **read, don't copy.** Using the same Shopify APIs is fine; copying Dawn's files, markup or JS classes is not, because Theme Store code must be original.
3. **Two phases.** Phase 1 = the showcase site. Phase 2 = Theme Store hardening, started only after Phase 1 is live and stable.
4. **Animations** use native `IntersectionObserver` + CSS `transform`/`opacity`, inside a small web component. Merchant settings turn them on and off, `prefers-reduced-motion` is respected, the hero/LCP content is never hidden, and content stays visible without JS. GSAP only if an effect truly needs it, bundled into `assets/`, never from a CDN.

## Read this first

`d:\shopify-oroskin\dawn\SHOPIFY-LEARNING-ROADMAP.md` — a guide I built in a previous session. Most relevant parts:

- **Part 9.4 / 9.5** — why Skeleton, and the Theme Store eligibility rules
- **Part 19** — the full project plan: setup, architecture, the animation system with code, the milestone tables, and a "study this Dawn file" reference table
- Parts 2–7 explain Liquid, theme structure, sections/blocks/snippets, data and metafields
- Part 12 covers performance and Part 13 covers the production process

Also at `d:\shopify-oroskin\dawn\DEVELOPER-GUID.md`: CLI commands, Theme Check, Prettier, commit checklist.

## Current state

- Nothing built yet. The new theme folder does not exist.
- `d:\shopify-oroskin\dawn` is a git repo holding Dawn 15.5.0 and the guide files.
- **No development store yet.** I need to create a Shopify Partner account and a dev store as the first step.
- **Figma design is ready, but the link has not been shared with me yet.** I am waiting on it from the designer/client.

### Blocked until I get these

- [ ] Figma link (or exported screenshots) — needed before any section work
- [ ] Development store created — needed before anything can be previewed
- [ ] Brand assets: logo files, fonts (and their licences), product photography

## Where we're starting

1. Create a Shopify Partner account and a free **development store**, with realistic test data plus edge cases: no image, long title, many variants, sold out, on sale, empty collection.
2. Scaffold the theme: `cd d:/shopify-oroskin` then `shopify theme init oroskin-theme`. Confirm the folder has `assets/ blocks/ config/ layout/ locales/ sections/ snippets/ templates/`. Start a fresh git history and copy both `.md` guides into the new repo.
3. Run `shopify theme dev --store <my-dev-store>.myshopify.com`.
4. Then **Milestone 1: Foundation** — design tokens from Figma into `config/settings_schema.json` and CSS variables in `layout/theme.liquid`, plus `base.css` (reset, typography, buttons, forms) and the scroll-animation system from Part 19.4.

After the foundation, the build order is: header/footer → shared snippets and theme blocks → homepage → collection (with filters) → product (variants, add to cart, app blocks) → cart → remaining pages → QA/performance → launch.

**Steps 1–3 don't need Figma**, so I can do them while waiting for the design link.

## How I want you to work with me

- **One milestone at a time.** Don't dump the whole theme at once.
- **Explain before generating.** Tell me what we're building, which Shopify objects/APIs it uses and why, then write the code.
- **Teach me the Liquid** in what you write. I want to understand every line, not paste it blindly.
- Every section needs: `{% schema %}` settings, a **preset**, `{{ block.shopify_attributes }}` on blocks, empty states (`!= blank`), placeholder content, and locale strings instead of hardcoded text.
- Watch for these mistakes: `{% include %}` (use `render`), `img_url` (use `image_url` + `image_tag`), truthiness checks on text settings (use `!= blank`), **filters inside filter arguments** (invalid — `assign` first), hardcoded handles (use settings), lazy-loading the hero image.
- Keep performance targets in mind from the start: Lighthouse mobile ≥ 60 performance and ≥ 90 accessibility on home, collection and product.
- Ask me before assuming anything about the design. I'll paste Figma details or screenshots once I have access.
- Don't commit or push unless I ask. Never push to a live theme.

## First task

Confirm you've read `SHOPIFY-LEARNING-ROADMAP.md` (especially Part 19), then walk me through **creating the development store and scaffolding the theme**, step by step, since those don't need the Figma link. Tell me exactly what to click in the Shopify Partner Dashboard, and what test data to add.
