# Brand guide

The BitsAndBricks apps are one family: they share how they are built and how a brand is
written down, and each has its own name, mark and accent. This page is the generic part; each
app's own guide and files live in its repo's `brand/` folder.

## The family today

| App | Accent | Mark | Type | Guide |
| --- | --- | --- | --- | --- |
| Seatbit | Bit Green `#2EE59D` | 3×3 seat grid, a chair of bits | Geist | `seatbit/brand/BRAND.md` |
| Floorbit | Bit Green, plus Trace Cyan for "what you look at" | the same grid seen from above, an F | Geist | `floorbit/docs/brand.md` |
| Hubra | Hubra Violet (`#7441e0` light, `#9f7cf8` dark) | the Clubhouse h with a gold dot | Bricolage Grotesque wordmark | `hubra/brand/README.md` |
| Labdex | Lantern amber (`#b85a0a` light, `#f59a23` dark) | line lantern | Geist | TODO: `labdex/brand/` |
| Mason | TODO | mason mark | TODO | `mason-cli/docs/logo/` |

Seatbit and Floorbit form a sub-family: same neutrals, same grid, same green, and every name
ending in a green "bit". That is why a Geist, dark, grid-backed Labdex reads as their sibling.

## What every app's brand guide contains

1. **Name:** how it is written in text and in the wordmark; tagline; one-liner.
2. **Logo:** what the mark means; a file table (lockup light and dark, mark, mono `currentColor`, wordmark, app icon, favicon, OG image 1200×630); clear space; minimum sizes (mark 16 px).
3. **Colour:** tokens with hex and role, for dark and light. One accent with one meaning. Text colour on the accent fill named (`on-accent`). Status colours fixed and never themeable.
4. **Type:** the UI font and the mono font; the wordmark outlined to SVG so it needs no font.
5. **Shape and texture:** radii, borders, background pattern.
6. **Voice:** how the app talks; words it uses and avoids.
7. **Code:** where the tokens live in the repo and how the files are generated.

## Shared rules

- **One accent, one meaning.** The accent marks the main action (and, in Labdex, only plan, approve and apply). Status colours (ok, warn, error, unknown) are never the accent.
- **Contrast:** WCAG AA for text in both themes; text on a bright accent is dark, never white.
- **Dark and light:** both themes ship; the default follows the system.
- **Files from source:** every PNG, ICO, banner and OG image is generated from SVG by a script in the repo (`brand/generate.py` or `tools/brand.py`). Never edit an exported file.
- **Wordmarks** are outlined SVG. Marks are not recoloured, rotated, stretched or given effects.
- **No real places or people** in brand images; no Swiss cross (protected by law).
- **Naming:** the umbrella is written "BitsAndBricks"; the author appears as "Mike" only.

## Open questions for Mike

- Is there a BitsAndBricks master mark and colour (for the website, the GitHub orgs, the handbook), or is the umbrella text only?
- Should every app keep the "...bit" sub-family look, or only the event apps (Seatbit, Floorbit)? If Labdex should look less like them: keep amber and Geist but drop the dot grid, or change the type?
- Shared neutrals for all apps (Seatbit's ink scale), or neutrals per app?
- Mason: which accent and type?
