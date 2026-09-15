# GradGenious Certificate Generator

One template, both certificate variants: `certificate.html`.

## Issue a certificate

Open `certificate.html` and edit the six fields at the top of the file:

```js
const CERT = {
  certificateType: "TRAINING",              // "TRAINING" | "INTERNSHIP"
  recipientName:   "Your Name",
  programTitle:    "",                      // blank -> shows the bracketed placeholder
  startDate:       "START DATE",
  endDate:         "END DATE",
  diceId:          "GGED-3025-00012345",
};
```

Switching `certificateType` swaps four strings automatically:

| | `TRAINING` | `INTERNSHIP` |
|---|---|---|
| subtitle | OF TRAINING COMPLETION | OF INTERNSHIP COMPLETION |
| lead-in | …the **training** program on | …the **internship** program on |
| title placeholder | `[ TRAINING PROGRAM TITLE ]` | `[ COURSE / PROJECT TITLE ]` |
| paragraph | During the **training**, … | During the **internship**, … |

Dates are auto-wrapped in `[ ]` unless you already include brackets.

## Batch issuance without editing the file

Every field can be overridden from the URL — useful for generating many
certificates from a script:

```
certificate.html?type=internship&name=Asha%20Ramachandran&title=Full%20Stack%20Web%20Development&start=01%20Jun%202026&end=31%20Aug%202026&dice=GGED-3025-00048217
```

Keys: `type`, `name`, `title`, `start`, `end`, `dice`.

## Export to PDF

Ctrl+P → **Destination:** Save as PDF → **Layout:** Landscape → **Paper:** A4 →
**Margins:** None → **Background graphics:** ON.

Background graphics must be on or the indigo panel and gold frame will not print.
The sheet is exactly one A4 landscape page.

## Assets

Images live in `./Images/`. To change artwork, drop a replacement in at the same
path — no code change needed.

| Path | Notes |
|---|---|
| `GG.png` | Cropped to its ink and filtered to white for the indigo panel |
| `EMBLEM.png` | Ashoka Lion Capital beside the Ministry text. Rendered with `filter:invert(1)` + `mix-blend-mode:screen`, **not** `brightness(0) invert(1)` — it is black line art with 41.2% opaque white fill, so the plain filter would flatten it to a featureless white blob. invert turns the lines white and the fill black; screen then drops the black to transparent |
| `MSME India Logo.png` | Cropped to the wordmark block (the emblem above it is trimmed), filtered white |
| `DPIIT #startupindia logo.png` | Referenced as `DPIIT%20%23startupindia%20logo.png` — the `#` **must** stay percent-encoded as `%23` or the browser reads it as a URL fragment and the image silently fails to load. Also cropped: the ink occupies only x 723–2941 of a 3664px canvas |
| `SIDE FINAL PART.png` | The four frame corners, at 89px. **It is a bottom-right corner piece** — its top-left quadrant has zero ink, its double tabs cross the top edge (continuing as the right-hand rules) and the left edge (continuing as the bottom rules). Flips: BR none, BL `scaleX(-1)`, TR `scaleY(-1)`, TL `scale(-1,-1)` |
| `MIDDLE THING.png` | The name-divider flourish, 54 × 30, already gold — no recolour |
| `LEFT SIDE LEAF THING.png` | The quality seal, used **twice** — once plain, once `scaleX(-1)` — as a `mask-image` with `background-color: var(--indigo)`, since the artwork is navy. No stars: none is baked into the asset and none is fabricated |
| `RIGHT SIDE LEAF THING.png` | **Not used.** It is not a clean mirror of the left branch (ink 639×1772 vs 827×1892, aspects 0.361 vs 0.437, 17.9% silhouette disagreement), so pairing the two gave a visibly lopsided wreath. Mirroring the left branch gives a symmetric one |
| `signature 1.png` / `signature 2.png` | Cropped to their ink so they sit on the rule; left unfiltered (black on white paper) |
| `side corner.png` | **Superseded** by `SIDE FINAL PART.png` |
| `feather and star.png` | **Superseded** by the two-branch seal |
| `ISO 9001 2015 Certification Logo.png` | **Not used** — a blue globe wordmark, while the reference shows a green certified seal, drawn as inline SVG instead |
| `Ministry_of_Corporate_Affairs_India.svg.webp` | **Not used** — superseded by `EMBLEM.png` plus HTML text. Its emblem (29.6% white fill) and "MCA" boxes (19.1% knocked-out letters) cannot be filtered white without destroying them |

## How the frame meets the corners

`SIDE FINAL PART.png` carries the frame's double rules as baked-in tabs, so the
straight rules are **eight absolutely-positioned bars** (`.fr`), not two bordered
boxes — a border draws a closed rectangle and would run straight through the
ornaments. Each bar stops 89px from its edge, exactly where the corner image begins,
and the ornament's own tab continues the line.

At 89px the tabs land 16.0 / 22.1px (vertical) and 16.0 / 21.4px (horizontal) from
the `.face` edge, which is where `certificate 1.jpeg` has its rules. The bars are
`#CE6B0C` — **sampled from the asset's own tabs**; the reference's copper
`#BB7434` / `#B5885F` would leave a visible colour seam at every junction.

If you ever resize the corners, the bar offsets and thicknesses must be recomputed
from the tab geometry, or the junction breaks.

Known placeholders in the current artwork: `GG.png` is a plain black monogram rather
than the reference's purple graduation-cap mark, and `signature 1.png` is literally
the word "Signature". Both are positioned and sized correctly, so real artwork drops
straight in.

## Things deliberately different from the reference JPEGs

- **Page size.** The references are 1536×1024 (ratio 1.51); this is true A4
  landscape (1.414). Type is scaled to match the references' line widths, so line
  breaks are identical and the extra height becomes slightly more generous leading.
- **Ministry of Corporate Affairs block** is text only (see the asset table above).
- **ISO badge** is drawn as SVG rather than using the supplied blue PNG.
- **The frame is a brighter orange than the reference's copper.** The corner asset's
  baked tabs are `#CE6B0C`; the rules must match them exactly or every junction shows
  a seam, so the whole frame follows the asset rather than the reference.
- **Corner ornaments are larger than the reference's** (~57×61px of scrollwork against
  the reference's 34×34), and the new artwork replaces the reference's plain mitred
  corner with scrollwork.
- **The seal is an open V, not a closed wreath.** Two single-branch assets with no
  crossing element leave the stem bases ~27px apart, where the reference's branches
  cross at the bottom.
- **The divider ornament is narrower and taller than the reference's** (54×30 against
  90×22) — the supplied flourish is aspect 1.79 where the reference's is 4.1 — and it
  is a brighter yellow-gold than the tan hairline it sits on.
- **The tagline reads "DREAM JOBS".** The reference JPEGs themselves read "DREAM JOES";
  this is a deliberate typo correction.

## Local preview

```bash
python -m http.server 8777 --directory .
```

Then open <http://localhost:8777/certificate.html>. Opening the file directly with
`file://` also works.
