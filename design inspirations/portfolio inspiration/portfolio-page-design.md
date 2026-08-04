# Design Language Report — "Technical Blueprint" SaaS Landing Page Style

A reference for building pages in this visual system. Describes principles and tokens, not literal content — apply the *logic*, not the specific copy, product name, or diagram content shown in the source image.

## 1. Core Character
A single-hue, developer-facing marketing page that reads like engineering documentation crossed with a product spec sheet. Confident, technical, restrained — sells credibility through precision and whitespace, not imagery or color variety.

**Keywords:** monochrome, blueprint, mono-label, boxed-grid, dev-tool-native.

## 2. Color Palette
- **Effectively one hue** (a strong royal/electric blue) plus black, white, and off-white. No secondary accent color anywhere.
- Value is created through **tint steps of the single hue**: near-black-blue for the top utility bar, saturated mid-blue for the hero block, pale blue/white for body sections, and blue-on-white text for links/highlighted words.
- White space (near-white background, #FAFAFA-ish) dominates every section below the hero — the hero is the one saturated, high-contrast block on the page.
- Text color logic: **dark navy/black headlines on white sections**, **white headlines on the blue hero**. Body copy is muted gray-blue, never pure black — keeps hierarchy between headline and paragraph clear at a glance.

## 3. Layout System
- **Full-bleed sections stacked vertically**, each with generous internal padding and a consistent max-width content column — no sidebar, no multi-column app chrome (this is a marketing page, not a workspace).
- A **thin dotted-line border** frames the entire page and often re-appears as a subtle inner border around cards/sections — a signature "blueprint/schematic" cue rather than solid strokes.
- Grid-based feature/step blocks: **evenly spaced boxed cards** (2, 3, or 4 across) with thin solid borders, consistent internal padding, and matching corner treatment (sharp or barely-rounded — not pill-rounded). Cards are outlined, not filled — the outline itself is the visual weight.
- Section rhythm: **eyebrow tag → large headline → short supporting paragraph → grid of cards**, repeated section after section. This tag+headline+grid triad is the core repeating unit of the whole page.
- Hero section breaks the grid pattern with an **illustrative technical diagram** (boxed "message" mockups connected implicitly by layout) rendered in outline/wireframe style, matching the blueprint theme rather than a photo or screenshot.

## 4. Typography
- **Two-font pairing carries all the character:**
- A **monospace font** for small utility text: eyebrow labels/tags, step numbers, button microcopy, nav bar — signals "developer tool" instantly and in small doses.
- A **humanist sans-serif** for headlines and body copy, mixing upright and italic within the same headline to add emphasis to one key phrase (e.g. one word/phrase in italic, rest upright) — this italic-emphasis trick is a repeatable device for headline treatment.
- Headlines are large, bold, tight line-height, left-aligned; never centered.
- All-caps mono labels are small, letterspaced, and often sit inside a small tag/pill or bracket-style marker (small square icon or arrow glyph before the label).

## 5. Components & Patterns
- **Eyebrow tag:** small rectangular chip, solid-color fill (dark on light sections, white on the hero), mono uppercase text — always precedes a headline, always top-left of its section.
- **Arrow-prefixed micro-labels:** a small "→" glyph before short mono labels (used for card categories, nav items) — a recurring bullet/marker style instead of dots or numbers.
- **Bordered feature card:** thin single-pixel border, no fill, no shadow; internal structure = small icon/glyph in a bordered square → mono eyebrow label → bold short title → 1–2 line gray description. Repeats identically across "steps" and "features" grids.
- **Step numbering:** "STEP 01 / 02 / 03" mono labels rather than large numerals — numbering is treated as metadata, not decoration.
- **Primary CTA button:** solid-fill rectangular (sharp-cornered) button, high contrast (white-on-blue or blue-on-white depending on section), mono or semi-bold label, paired inline with an email input field in the hero — classic "waitlist" pattern.
- **Wireframe diagram illustration:** hero-only device — abstracted UI mockups (message bubbles/cards) drawn as thin outlined rectangles with placeholder line-text, arranged to imply a product flow without showing a literal screenshot.
- **Corner accents:** small L-shaped bracket marks in card/section corners (echoing technical drawings/blueprints) — a subtle detail reinforcing the "spec sheet" motif.
- **Nav bar:** dark, full-width, minimal — product name/logo left, single CTA button right, nothing else.

## 6. Iconography
Extremely minimal — small single-color line/solid glyphs (roughly 16–20px) inside thin bordered squares, one per feature card. Icons are functional labels, not illustrations; never more than one per card, never colorful.

## 7. Voice/Content Shape (structural, not literal)
- Headlines follow a **short-punch + italic-emphasis** formula ("The [italic emphasis] [plain rest]").
- Section headers follow **eyebrow tag → bold statement, often two-part with one clause in italic/accent color** (e.g., plain clause + colored/italic clause in the same sentence).
- Card copy is terse: a 2–4 word bold title + one short descriptive sentence, no more.

## 8. Principles to Carry Forward
1. **One hue, many tints.** Resist adding a second accent color — create depth by shifting the single hue's saturation/lightness, not by introducing new colors.
2. **Mono for metadata, humanist for message.** Reserve the monospace font strictly for labels/eyebrows/utility text; keep headlines and body in the humanist sans — the contrast between the two fonts does the "technical but readable" work.
3. **Outline over fill.** Cards, buttons-on-light-sections, and diagrams should default to thin borders and white space rather than solid color fills or shadows — fills are reserved for the hero and primary CTAs only.
4. **Dotted/thin borders as a motif.** A light dotted outer frame and hairline card borders create the "blueprint" feel — use consistently, not just once.
5. **Repeat one section grammar.** Tag → headline → subtext → card grid is the page's atomic unit; vary the grid count (2/3/4 columns) but keep the sequence identical across sections for scannability.
6. **Emphasis via italics, not color, inside headlines.** Use italic styling on a key phrase within an otherwise plain headline as the primary rhetorical/visual emphasis tool, saving color for tags and CTAs.
7. **Diagrams over photography.** Any hero visual should be an abstracted, outlined product/system diagram — never a photo, never a literal full-fidelity screenshot.

---
*Use this as a style brief: match the single-hue system, mono/humanist font pairing, outlined card grammar, and section rhythm described above — do not reuse the specific product name, copy, diagram content, or icon set from the reference image.*