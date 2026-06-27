# Label & Tag Generation — Engineering Notes

General, reusable lessons for building barcode label/tag generators (Python + ReportLab +
Code 128). Project-agnostic — no job/customer specifics. Distilled from real production use.

## Output format
- **Vector PDF** (ReportLab) renders text and bars as true vectors → crisp at any print size.
- **One label per page = one physical tag.** Plays well with thermal roll stock + contour/cut.
- Tell users to **print at Actual size / 100%** (never "fit to page"), or every measurement drifts.
- For thermal printers (Panduit, Zebra, etc.) also consider a **CSV export** for their own software
  (e.g. Easy-Mark Plus) in addition to direct PDF — different shops want different inputs.

## Coordinates & layout
- Work in **inches**, convert to points (`×72`) only at the drawing edge.
- **Center-anchored layout** (offsets measured from the page center) is predictable and template-
  friendly: `x>0` = right, `y>0` = up, `0` = centered.
- Vertically centering a cap-height string: shift the baseline **down ~0.35 × font_size**. If you
  have both a live preview and a PDF path, they MUST share the exact same baseline formula or they
  visibly diverge.

## Fonts
- Prefer the **14 standard PostScript fonts** (Helvetica / Helvetica-Bold …) — guaranteed rendering,
  no embedding surprises. **Validate** a requested font and fall back gracefully (don't crash on
  "Arial"); map spec wording like "Sans, Black, Bold" to the closest standard face.
- Helvetica **cap height ≈ 0.717 × font_size** — handy for sizing a background band/box around text.

## Code 128 / 128B
- **Charset is ASCII 32–127 only.** Validate at **data-load** time (not just at export) and report the
  offending row/value so the user can fix the source.
- 128B uses **11 modules per symbol char**; total bar width = `modules × module_width`.
- To hit a **fixed target bar width** regardless of text length, compute
  `module_width = target_width / module_count` **per barcode** — don't hardcode a module width.
- **Quiet zones are additive** (extra whitespace OUTSIDE the bars), not part of the bar width.
  Minimum 0.5" (or ≥ 10 × module width). `full_width = quiet + bars + quiet`.
- In ReportLab, `Code128(quiet=…)` is a **boolean flag**; set `lquiet`/`rquiet` (in points) for exact
  control. Read the rendered width from `bc.width` and center via `x = center − bc.width/2`.

## The #1 gotcha: fill-color bleed
- ReportLab barcodes are drawn with the **current canvas fill color**. If you set a colored fill for a
  caption and then draw the barcode, **the bars inherit that color** (e.g. navy bars instead of black).
  Always `setFillColor(black)` immediately **before** drawing the bars. This is an easy, invisible bug —
  the PDF "looks fine" on screen but the symbology contrast/intent is wrong.

## Background bands, boxes & borders
- Draw filled **background bands first** (so text/bars sit on top); draw the **border/box last** (so it
  stays crisp on top of everything).
- For a border that is exactly `W × H` at the trim edge, **inset the stroke path by half the line width**
  (ReportLab strokes are centered on the path, so a path at the page edge gets half-clipped).
- **Assert** a background band's rect does not overlap the barcode rect before shipping.
- **Sample exact colors from a reference image** (median of the region) instead of eyeballing a hex.

## Data import
- Excel/CSV: strip whitespace, skip blank/NaN (use `pandas.isna`, not `== "nan"`), stringify non-text
  cells (Excel hands back ints/floats/dates).
- **Reset all column mappings + cached data when a new file is loaded** — stale mappings from the last
  file are a classic silent corruption.

## Batch output & verification
- Split big batches into multiple files; put the **count in the filename** and write a **CONTENTS
  manifest** listing every value + source range.
- **Verify three independent ways before calling it done:**
  1. **Geometry** — parse the PDF (e.g. PyMuPDF) for page size, element bounding boxes, colors, fonts.
  2. **Scan-back** — rasterize each page and decode with an **independent** reader (e.g. zxing-cpp);
     the decoded string must equal the human-readable caption on the page.
  3. **Re-derive** the expected set straight from the source data, **independent** of the generator's
     own intermediate files, and diff.
- The bar is **"zero barcodes that fail to scan,"** not "it looks right."

## Packaging
- PyInstaller one-file works, but add **hidden imports** for dynamically-loaded submodules
  (e.g. `reportlab.graphics.barcode.code128`) or the exe crashes at runtime with a missing-module error.
- Keep label specs as **JSON templates** (one per device/label type), auto-discovered at startup —
  specs out of code means non-developers can add/adjust label types.
