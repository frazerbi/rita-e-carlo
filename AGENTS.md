## Project

Save-the-date landing page for Rita & Carlo's wedding (12 settembre 2026, Padova), built from this Figma design: https://www.figma.com/design/MGLGWCdkcOoUrAkCqUGaNL/Rita-e-Carlo---Save-the-Date-Landing-Page (root node `1:2`).

Structure:
- `src/layouts/Layout.astro` — global `<head>`, font loading, design tokens (colors, `--font-display`/`--font-body`) as CSS custom properties on `:root`, and Open Graph/Twitter Card meta tags for link-preview sharing (hardcoded to `https://ritaecarlo.it/` — update if the domain ever changes)
- `src/components/` — `Hero.astro` (includes a Don Giussani quote above the date badge; no scroll-hint button), `PhotoStrip.astro` (reused 3x), `EventStep.astro` (reused for "La Santa Messa" and "La Festa" via a `variant` prop; photo sits beside the details card, side by side — `photoPosition` prop, `"left"` for La Messa, `"right"` for La Festa, stacking below 640px; card has no description paragraph anymore — just venue name/address/photo/maps link — and stretches to match the photo's height via `align-items: stretch`, centered on mobile), `Gifts.astro`, `Footer.astro`
- `src/pages/index.astro` — assembles all sections in order
- `src/fonts/PontiffWide.otf` — the display font, self-hosted (paid/licensed font, not on Google Fonts); Lora (body font) is loaded from Google Fonts in `Layout.astro`
- `src/img/` — real couple/venue photos: `rita-carlo.jpg`, `rita-carlo-6.jpg`, `rita-e-carlo-1.jpg`, `rita-e-carlo-2.jpg`, `rita-e-carlo.jpeg`, plus venue shots (`chiesa-eremitani.jpg`, `centro-parrocchiale-AVE.jpg`). Note: in `index.astro`, the imported variable name doesn't always match the filename anymore (e.g. `ritaCarlo6` points to `rita-e-carlo-2.jpg`, while `ritaCarlo6Orig` points to the actual `rita-carlo-6.jpg`) — check the import line, not just the variable name, before reusing a photo reference.
- `src/assets/decor/` — fig/leaf decorative SVGs downloaded from the Figma file
- `public/favicon.svg` / `public/favicon.ico` — "RC" initials on a purple circle (cream on purple, inverted in dark mode), not the original fig-leaf icon
- `public/og-image.jpg` — 1200×630 static social-preview image referenced by the OG/Twitter meta tags in `Layout.astro`

No Tailwind — styling is plain CSS in component-scoped `<style>` blocks, using the tokens defined in `Layout.astro`.

**Color contrast constraint:** `--color-orange` (#e8573d) is decorative-only (connector lines, dot borders) — it fails WCAG AA as text or as a button background with cream text on top. Use `--color-orange-dark` (#b62e16) for orange text/button-backgrounds on light (cream/white) surfaces. On purple/purple-dark surfaces, use `--color-cream` for accent text (eyebrow, time-badge, footer tag, scroll hint) — no shade of orange or peach reaches AA contrast against `--color-purple`, so don't reintroduce orange/peach text there. `--color-peach` is currently unused but still defined as a token.

**Safari/WebKit `@font-face` constraint:** WebKit does not support CSS custom properties (`var()`) inside `@font-face` rules — they're silently ignored, so the font falls back to serif. The `font-family: 'Pontiff Wide'` `@font-face` in `Layout.astro` must keep its computed asset URL inlined directly into the `src: url(...)` via `set:html` (not `define:vars` + `var(--x)`), or Safari (desktop and mobile) will fail to load it while Chrome/Firefox work fine.

**Still placeholder** in `src/components/Gifts.astro`: gift registry name/link and the IBAN/account holder for the bank transfer option — replace with real details before launch.

## Deployment

Static site (no adapter) — output goes to `dist/` via `npm run build`. Deploy = upload the **contents** of `dist/` (not the folder itself) to the web server's public root. Live at `https://ritaecarlo.it/`, served by nginx, hosted at the domain root (not a subpath) — the OG-image/URL meta tags in `Layout.astro` assume this. No Node runtime or backend needed on the server.

## Development

When starting the dev server, use background mode:

```
astro dev --background
```

Manage the background server with `astro dev stop`, `astro dev status`, and `astro dev logs`.

**Known dev-server quirk:** editing a component's `<style>` block (e.g. changing how many `<style>` tags a file has) can leave the running dev server's Vite module graph stale, causing `Internal server error: No Astro CSS at index N`. Fix by restarting: `astro dev stop` then `astro dev --background` — it's a cache issue, not a code bug.

**Generating icons/images without ImageMagick:** this machine has no `rsvg-convert`/`magick`/`convert`. `sips` can rasterize SVG but silently drops `<text>` and `<style>` blocks (renders shapes only) — for SVGs with text or CSS, render via `qlmanage -t -s <size> -o <dir> file.svg` (QuickLook, WebKit-based) instead, then resize/crop the resulting PNG with `sips`. To build a multi-size `.ico` from PNGs, no tool is available either — pack it manually (ICONDIR + ICONDIRENTRY headers wrapping raw PNG bytes, valid since Windows Vista+).

## Documentation

Full documentation: https://docs.astro.build

Consult these guides before working on related tasks:

- [Adding pages, dynamic routes, or middleware](https://docs.astro.build/en/guides/routing/)
- [Working with Astro components](https://docs.astro.build/en/basics/astro-components/)
- [Using React, Vue, Svelte, or other framework components](https://docs.astro.build/en/guides/framework-components/)
- [Adding or managing content](https://docs.astro.build/en/guides/content-collections/)
- [Adding styles or using Tailwind](https://docs.astro.build/en/guides/styling/)
- [Supporting multiple languages](https://docs.astro.build/en/guides/internationalization/)
