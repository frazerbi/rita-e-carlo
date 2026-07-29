## Project

Save-the-date landing page for Rita & Carlo's wedding (12 settembre 2026, Padova), built from this Figma design: https://www.figma.com/design/MGLGWCdkcOoUrAkCqUGaNL/Rita-e-Carlo---Save-the-Date-Landing-Page (root node `1:2`).

Structure:
- `src/layouts/Layout.astro` — global `<head>`, font loading, and design tokens (colors, `--font-display`/`--font-body`) as CSS custom properties on `:root`
- `src/components/` — `Hero.astro`, `PhotoStrip.astro` (reused 3x), `EventStep.astro` (reused for "La Santa Messa" and "La Festa" via a `variant` prop), `Gifts.astro`, `Footer.astro`
- `src/pages/index.astro` — assembles all sections in order
- `src/fonts/PontiffWide.otf` — the display font, self-hosted (paid/licensed font, not on Google Fonts); Poppins is loaded from Google Fonts in `Layout.astro`
- `src/img/` — real couple/venue photos; only 2 couple photos exist today, reused across 5 photo-strip slots — swap in more if provided
- `src/assets/decor/` — fig/leaf decorative SVGs downloaded from the Figma file

No Tailwind — styling is plain CSS in component-scoped `<style>` blocks, using the tokens defined in `Layout.astro`.

**Still placeholder** in `src/components/Gifts.astro`: gift registry name/link and the IBAN/account holder for the bank transfer option — replace with real details before launch.

## Development

When starting the dev server, use background mode:

```
astro dev --background
```

Manage the background server with `astro dev stop`, `astro dev status`, and `astro dev logs`.

## Documentation

Full documentation: https://docs.astro.build

Consult these guides before working on related tasks:

- [Adding pages, dynamic routes, or middleware](https://docs.astro.build/en/guides/routing/)
- [Working with Astro components](https://docs.astro.build/en/basics/astro-components/)
- [Using React, Vue, Svelte, or other framework components](https://docs.astro.build/en/guides/framework-components/)
- [Adding or managing content](https://docs.astro.build/en/guides/content-collections/)
- [Adding styles or using Tailwind](https://docs.astro.build/en/guides/styling/)
- [Supporting multiple languages](https://docs.astro.build/en/guides/internationalization/)
