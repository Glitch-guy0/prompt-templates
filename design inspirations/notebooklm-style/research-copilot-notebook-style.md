# Design Language Report — "Research Copilot" UI Style

A reference for building pages in this visual system. Describes principles and tokens, not literal content — apply the *logic*, not the specific labels/copy shown in the source images.

## 1. Core Character
A three-pane productivity workspace (navigation → source/list → conversational canvas → insight sidebar) built for knowledge work. Feels calm, editorial, and confident rather than dense or techy. Generous whitespace does the work that borders usually do.

**Keywords:** soft, airy, structured, glanceable, low-chrome.

## 2. Layout System
- **Multi-column workspace**, not a single chat column: a narrow icon rail (far left) → a list/source panel → a primary conversational panel → a right-hand insight panel. Each column is its own rounded "card" floating on a neutral gray background, with visible gaps between columns (not edge-to-edge).
- Panels have **large corner radii** (approx. 20–28px) and sit on a muted gray/lavender page background — this outer background is where the "cards" visually separate from each other.
- Internal content uses **generous padding** (24–32px) and vertical rhythm; sections separate with whitespace and thin hairline dividers rather than boxes-within-boxes.
- Chat bubbles for user input are **right-aligned, pill/rounded-rectangle, light gray fill**, no avatar. AI responses run full-width, left-aligned, no bubble — plain text on the panel background. This asymmetry (user = contained chip, AI = open text) is a key pattern.
- A composer/input bar is anchored at the bottom of the chat column, full-width, rounded, with a soft focus ring (colored border glow) when active, plus icon affordances (attach, mic, send) inline on the right.

## 3. Color Palette
- **Base:** near-white / off-white panel surfaces (#FAFAFA–#FFFFFF range) on a soft neutral gray page background.
- **Text:** near-black for primary text, mid-gray for secondary/meta text — no pure black.
- **Accent (sparingly used):** a warm gradient of coral-orange-to-blue/purple, reserved for brand mark, primary CTA (send button), progress rings, and one "hero" dark card. This is the single richest color note in an otherwise monochrome UI — don't overuse it.
- **One dark contrast panel** (near-black/charcoal, sometimes with a faint warm-to-cool gradient wash) is used once per screen to spotlight a key metric/summary block — a deliberate "pop" against all the light panels.
- **Data viz colors:** desaturated blue for bars, warm orange/amber for line overlays, light blue tints for secondary series. Charts stay low-saturation except for one highlighted data point/bar in full-strength blue.
- **Tags/pills:** pale blue background with darker blue text — soft, low-contrast, never loud.
- **Status/accent dots:** small red notification dot, green "connected" dot — tiny, functional, never decorative.

## 4. Typography
- Clean **geometric/humanist sans-serif** throughout (system-UI style, no serif, no display font).
- Strong but not heavy weight for headings (semi-bold), regular weight for body — size does the hierarchy work more than weight does.
- Numbers/stats are set noticeably larger and bolder than their labels (label small+gray above, value large+black below) — a "stat card" convention used repeatedly.
- Generous line-height on paragraph text; body copy reads like a document, not UI microcopy.

## 5. Components & Patterns
- **Stat/metric trio:** 3 label+value pairs shown side by side (e.g. small caption label, large bold number underneath) inside a highlighted card — use this for any "at a glance" summary.
- **Tag/chip row:** rounded pill chips, soft-tint background, used for topic/keyword clusters under a heading + count.
- **Comparison table:** minimal table with only a bottom hairline under the header row and between rows — no vertical gridlines, no cell shading, generous row padding.
- **Collapsible list rows:** folder/file rows with a leading icon, truncated label, and a chevron/expand affordance; nested items indented with a thin connecting rule.
- **Audio/media player card:** waveform visualization (bars of varying height, a played/unplayed color split), transport controls, and a speed toggle — flat, iconography-first, no skeuomorphism.
- **Loading/progress row:** circular progress indicator + filename + byte progress + time remaining, inline in a list — quiet, textual, non-blocking.
- **Floating action button:** solid circle, gradient or solid accent fill, single glyph icon — used for primary "add/send" actions only.
- **Top-right identity cluster:** small utility icons (globe/notification) + circular avatar + name/email stacked — consistent anchor point across screens.

## 6. Iconography
Thin-stroke, single-weight line icons throughout (no filled icons except status dots and the FAB). Icons are small and always paired with text labels — never icon-only for primary actions.

## 7. Motion/State Cues (implied)
- Active/focused states are shown via a colored gradient border ring around an input, not a shadow or fill change.
- Selected list items get a light gray fill + rounded corners, not a colored highlight.
- In-progress states (loading, streaming) use color (blue/red waveform segments, spinner rings) rather than motion-only cues, so state is legible in a static frame.

## 8. Principles to Carry Forward
1. **Let whitespace separate, not borders.** Reach for padding and background-color shifts before reaching for a stroke.
2. **One accent, used rarely.** Keep 95% of the UI monochrome; spend the gradient accent on exactly one focal element per screen.
3. **Contrast through darkness, not color.** The one high-emphasis card per screen should go dark/black, not saturated-colored.
4. **Numbers are heroes.** Any metric worth showing gets oversized bold type and a quiet gray label — never buried in a sentence.
5. **Conversational asymmetry.** User input = contained chip; system output = open, document-like text. This reinforces "you're reading a document that responds to you," not "you're in a chat app."
6. **Rounded everything, consistently.** One consistent large-radius rounding language across panels, buttons, chips, and inputs — no mixed radii.

---
*Use this as a style brief: match the spacing, restraint, color logic, and component grammar described above — do not reuse the specific icons, copy, chart data, or branding from the reference screens.*