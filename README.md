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
| `MSME India Logo.png` | Cropped to the wordmark block (the emblem above it is trimmed), filtered white |
| `DPIIT #startupindia logo.png` | Referenced as `DPIIT%20%23startupindia%20logo.png` — the `#` **must** stay percent-encoded as `%23` or the browser reads it as a URL fragment and the image silently fails to load |
| `signature 1.png` / `signature 2.png` | Cropped to their ink so they sit on the rule; left unfiltered (black on white paper) |
| `ISO 9001 2015 Certification Logo.png` | **Not used** — it is a blue globe wordmark, while the reference shows a green certified seal, which is drawn as inline SVG instead |

Known placeholders in the current artwork: `GG.png` is a plain black monogram
rather than the reference's purple graduation-cap mark, and `signature 1.png` is
literally the word "Signature". Both are positioned and sized correctly, so real
artwork drops straight in.

Everything decorative — corner flourishes, the double gold frame, the subtitle
ornaments, the name divider, the laurel watermark, the quality seal and the green
ISO seal — is inline SVG, so it stays crisp at any print resolution.

## Things deliberately different from the reference JPEGs

- **Page size.** The references are 1536×1024 (ratio 1.51); this is true A4
  landscape (1.414). Type is scaled to match the references' line widths, so line
  breaks are identical and the extra height becomes slightly more generous leading.
- **Ministry of Corporate Affairs block** is text only — no Ashoka emblem asset exists.
- **ISO badge** is drawn as SVG rather than using the supplied blue PNG.

## Local preview

```bash
python -m http.server 8777 --directory .
```

Then open <http://localhost:8777/certificate.html>. Opening the file directly with
`file://` also works.
