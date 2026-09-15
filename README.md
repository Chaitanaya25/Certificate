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
| `new coner.png` | The four frame corners, at 76px (ink renders ~58 × 59). **It is a bottom-right corner piece** — its top-left quadrant has zero ink, so the dense scrollwork hugs the page corner. Flips: BR none, BL `scaleX(-1)`, TR `scaleY(-1)`, TL `scale(-1,-1)`. Pure scrollwork with no baked-in rules, so it just layers over the frame |
| `MIDDLE THING.png` | The name-divider flourish, 54 × 30, already gold — no recolour |
| `feather new thing.png` | The quality seal — a complete wreath with both branches, all three stars and the crossed stems in one image. Used as a `mask-image` with `background-color: var(--indigo)` because the artwork is steel blue (`#0C375D`). **Never transform it** — it is used at its native orientation; any flip puts the stars at the bottom |
| `signature 1.png` / `signature 2.png` | Cropped to their ink so they sit on the rule; left unfiltered (black on white paper) |
| `LEFT SIDE LEAF THING.png` / `RIGHT SIDE LEAF THING.png` | The faint background watermark — one element each, **at native orientation with no transform**. They are drawn as a genuine pair (LEFT bows left, RIGHT bows right, both tips-up/stem-down), so no mirroring is needed or wanted; flipping either one would invert it. Recoloured by mask to `var(--watermark)`. Each gets its own `mask-size`/`mask-position` because the files are not the same scale — ink 827×1892 vs 639×1772, and RIGHT carries large transparent margins a bare `contain` would leave in |
| `ISO 9001 2015 Certification Logo.png` | **Not used** — a blue globe wordmark, while the reference shows a green certified seal, drawn as inline SVG instead |
| `Ministry_of_Corporate_Affairs_India.svg.webp` | **Not used** — superseded by `EMBLEM.png` plus HTML text. Its emblem (29.6% white fill) and "MCA" boxes (19.1% knocked-out letters) cannot be filtered white without destroying them |

## The frame

The rules stop short at every corner and the ornament owns that space. This is what
`certificate 1.jpeg` does: scanning each of its rules for contiguous gold shows the
rule absent for the corner-most ~50–62px, with only fragmented ornament ink there.

A CSS `border` cannot do that — it draws a closed rectangle straight through the
ornament — so the frame is **eight bars** (`.fr-ot/.fr-ob/.fr-ol/.fr-or` outer,
`.fr-it/.fr-ib/.fr-il/.fr-ir` inner). Rule geometry is still the reference's: outer
16px inset / 2px `#BB7434`, inner 22px / 1px `#B5885F`.

The gap size is set by our ornament, which is bigger than the reference's. At 76px
with a 15px offset its ink spans 21.6–79.6px horizontally and 22.0–80.8px vertically
from each face corner, so the horizontal bars clear 80px and the vertical bars 81px.
**Resizing or repositioning the corners means recomputing those two numbers**, or the
rules will overlap the scrollwork again.

The eight class names are deliberately distinct singles rather than compounds like
`.fr.r` — an earlier revision used that form and `.r` silently collided with another
element's class, restyling it.

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
- **Corner ornaments are larger than the reference's** (~58×59px of scrollwork against
  the reference's 34×34) and a brighter orange (`#D46805`) than the copper rules they
  sit beside. The rules themselves are reference-accurate.
- **The seal's text is smaller than the reference's.** This wreath's opening is 75px at
  the "COMMITTED TO" row, so the text is sized to the wreath actually in use.
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
