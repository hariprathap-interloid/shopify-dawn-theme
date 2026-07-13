# Developer Guide — Dawn Theme Customization

This project is Shopify's free **Dawn** theme, pulled in as our starting point. We are customizing it for a client, not selling it on the Theme Store — but we still follow Theme Store-level quality rules so the final result is fast, clean, and safe to hand off.

---

## 1. One-Time Setup

1. Install the Shopify CLI (already installed on this machine — version 3.94.3):
   ```
   npm install -g @shopify/cli@latest
   ```
2. Log in and connect this folder to the client's store:
   ```
   shopify theme dev --store your-client-store.myshopify.com
   ```
   The first time, it opens a browser to log in with the client's store credentials.

**Never commit the client's `.env`, API tokens, or password into git.** If you store credentials locally, put them in a `.env` file and make sure it's listed in `.gitignore`.

---

## 2. Running the Theme Locally

Start a live preview that auto-refreshes when you save a file:
```
shopify theme dev --store your-client-store.myshopify.com
```
This gives you a local preview URL (usually `http://127.0.0.1:9292`). Keep this running while you work — every Liquid/CSS/JS change reloads automatically.

Other useful commands:
```
shopify theme push          # upload this theme to the store as a new unpublished theme
shopify theme push --live   # ⚠️ publishes directly — only do this when asked, never by default
shopify theme pull          # download the current theme FROM the store into this folder
shopify theme list          # see all themes on the store and their IDs
```

**Rule of thumb:** always work on an unpublished/draft theme (`shopify theme push` without `--live`) and only publish when the client explicitly approves it.

---

## 3. Checking Your Code (Theme Check)

Theme Check is Shopify's official Liquid linter. It catches syntax errors, deprecated tags, missing translations, accessibility issues, and performance problems — the same checks Shopify runs before accepting a Theme Store submission.

Run it from the CLI (bundled with Shopify CLI, no extra install needed):
```
shopify theme check
```

- Fix every **error** before committing.
- Warnings are worth fixing too, but use judgment — some existing Dawn warnings may already be intentionally suppressed in `.theme-check.yml`.
- If you need to ignore a specific rule for a good reason, do it narrowly (per-file or per-line), not globally.

---

## 4. Formatting Code (Prettier)

This repo already includes a Prettier config (`.prettierrc.json`) tuned for Shopify themes (handles `.liquid` files correctly).

Run Prettier with the official Shopify plugin:
```
npx prettier --write .
```
If it's not installed yet, install it once:
```
npm install --save-dev prettier @shopify/prettier-plugin-liquid
```

Run this before every commit so diffs stay clean and consistent.

---

## 5. Before Every Commit — Checklist

Run these two commands in order:
```
shopify theme check
npx prettier --write .
```
Then:
- [ ] Preview the change in `shopify theme dev` and click through the affected page(s) — homepage, product page, cart, checkout entry point.
- [ ] Check mobile view (resize browser or use device toolbar in DevTools).
- [ ] No hardcoded client secrets, API keys, or personal data in the code.
- [ ] No `console.log` or leftover debug code.
- [ ] Commit message describes *why*, not just *what* (e.g. "Add hero banner section for homepage campaign", not "update index.liquid").

---

## 6. Best Practices We Follow (Theme Store Quality Bar)

Even though we're not publishing to the Theme Store, these are the standards Shopify requires there — and they're good practice for any client project:

- **Use theme editor settings, not hardcoded content.** Text, images, and colors the client should be able to change must go through `{% schema %}` settings/blocks, not hardcoded into the Liquid file.
- **Don't break existing sections/snippets unnecessarily.** Extend Dawn's patterns (existing snippets like `snippets/`, section pattern in `sections/`) instead of duplicating logic.
- **Keep accessibility intact.** Dawn already has ARIA labels, focus states, and semantic HTML — don't strip these out when customizing markup.
- **Lazy-load images and avoid render-blocking JS/CSS.** Follow Dawn's existing patterns (`loading="lazy"`, deferred scripts) for anything new you add.
- **Respect translations.** User-facing strings should go through `locales/en.default.json` and `{{ 'key' | t }}`, not be hardcoded in English, even if we're only shipping English for now.
- **Performance budget.** Avoid adding heavy third-party scripts/fonts unless the client asked for them. Check Lighthouse/PageSpeed after major changes.
- **Test with real (or realistic) data.** Since we have the client's store credentials, pull their actual products/collections/images into the dev preview so what you build matches their real content, not placeholder Lorem Ipsum.
- **Don't touch checkout.liquid / checkout customizations** unless specifically asked — that's a separate, higher-risk surface with its own rules.
- **Keep the client's live/published theme untouched** while you work — always duplicate or work on a new unpublished theme so the live store is never at risk.

---

## 7. Quick Command Reference

| Task | Command |
|---|---|
| Live local preview | `shopify theme dev --store <store>.myshopify.com` |
| Lint the theme | `shopify theme check` |
| Format code | `npx prettier --write .` |
| Push as draft theme | `shopify theme push` |
| Push and publish live | `shopify theme push --live` (ask client first) |
| Pull current live theme | `shopify theme pull` |
| List themes on store | `shopify theme list` |
