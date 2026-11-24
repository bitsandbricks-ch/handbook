# UI, tokens and brand

## Brand

- This handbook holds the **generic** guide: the BitsAndBricks master brand, how an app gets its sub-brand (own accent colour and logo on the shared tokens), logo rules, tone and wording. TODO: write `brand/README.md` with Mike.
- Each app keeps its own assets in `brand/` in its repo (Hubra and Seatbit already do). One shared script generates PNG, ICO, banners and social previews from SVG.
- Apps share the family look through tokens and differ through their accent and logo, so they read as siblings, not the same app twice.

## Tokens

CSS variables for greys, spacing, radii, fonts, status colours (ok, warn, error) and backgrounds,
light and dark. Mantine apps load them through their theme; Vue apps through their stylesheet.
Each app sets `--accent`.

## UI rules

- Main buttons in the app's accent only for the actions that plan, approve or apply; everything else plain.
- Every list has an empty state (icon, one line, the next action) and a loading state.
- Long lists page at 50 rows with one shared pager.
- Text sizes scale with a user setting; contrast meets WCAG AA in both themes.
- Wording: plain words, sentence case, no jargon the user didn't bring.

> **Avoiding conflicts:** styles live with their component; tokens are versioned in one package.
