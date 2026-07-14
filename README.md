# LxveAce Tag Studio

An industrial label and tag designer that prints to almost anything. **Design once. Export everywhere. Nothing leaves your machine.**

[![Release](https://img.shields.io/github/v/release/LxveAce/tag-studio?label=release&color=blue)](https://github.com/LxveAce/tag-studio/releases)
[![Downloads](https://img.shields.io/github/downloads/LxveAce/tag-studio/total?label=downloads&color=success)](https://github.com/LxveAce/tag-studio/releases)
[![License](https://img.shields.io/badge/license-MIT-green)](#license)
[![Platform](https://img.shields.io/badge/Windows-10%2B-0078D4)](#system-requirements)

**[Download](#downloads)** · **[Website](https://experttags.com)** · **[Templates](#templates)** · **[Security](./SECURITY.md)** · **[Discord](https://discord.gg/lxvelabs)**

> Provided **as is**, without warranty; you use it at your own risk. See [DISCLAIMER.md](DISCLAIMER.md).

---

Draw a label once on a drag-and-drop canvas, then send it straight to the machine that cuts, prints, or engraves it: Trotec lasers, Panduit thermal printers, Roland vinyl cutters, and any ZPL-compatible thermal printer. Or export plain PDF, SVG, and PNG that most other hardware can read directly. No round-tripping a design through three other programs to get it onto a tag.

This repo is the public release home for Tag Studio: the Windows installer (on [Releases](https://github.com/LxveAce/tag-studio/releases)), the bundled template library, the license, and the security policy. Tag Studio is **free and MIT-licensed**.

Project site: **[experttags.com](https://experttags.com)**

<!-- STATUS-ROADMAP:START -->
## Status & roadmap

**Latest release:** v0.3.1 (2026-06-09). The build ships as a Windows installer with an in-app update pipeline (`latest.yml`). Release notes live on the [Releases](https://github.com/LxveAce/tag-studio/releases) page.

**Being tidied up:**
- Making sure [experttags.com](https://experttags.com) reflects the **MIT / free** license consistently with the installer and this repo.
- Verifying the Windows installer trust chain — the Authenticode signature plus the published `latest.yml` hash — and making that a standing check before every release.

**Next:**
- A release-sync flow from the private source repo so pending print-correctness fixes (landscape clipping, dropped-PDF rotation, SVG text sizing) reach released builds.
- Auto-stamp `version.json` from the build so the manifest can't drift from what actually shipped.
- A per-release template-sync checklist so the bundled library stays in step with the app.
<!-- STATUS-ROADMAP:END -->

## Key features

### Canvas & design
- **Drag-and-drop editor** — place, move, rotate, and resize text, barcodes, images, and shapes
- **Multi-select & marquee** — shift-click or rubber-band a group and move it as one
- **Smart alignment guides** — snap to other elements' edges and centers with live guide lines
- **Align & distribute** — align a selection and space it evenly, horizontally or vertically
- **Element library** — text with system fonts, Code 128B / Code 39 / QR barcodes, images, and geometric shapes
- **Data binding** — link elements to CSV or Excel columns for batch generation
- **Editing workflow** — undo/redo, zoom, nudge, and keyboard shortcuts

### Export & output
- **Standard formats** — PDF, SVG, and PNG (high-DPI raster)
- **Per-machine output:**
  - **Trotec Ruby** — vector import (PDF/SVG)
  - **Panduit Easy-Mark Plus** — CSV thermal format
  - **Roland BN-20A** — VersaWorks bridge (PDF/EPS)
  - **Zebra / generic thermal** — ZPL II jobs for any ZPL-compatible printer
- **N-up sheet layout** — tile a label across Letter/A4/Legal/Tabloid with margins and gutters, paginated automatically
- **Batch / data merge** — combine a template with CSV or Excel rows to generate many labels in one pass

### Barcodes
- **Code 128B** — full ASCII 32–126 (inventory, shipping)
- **Code 39** — alphanumeric
- **QR** — any URL or text
- Configurable quiet zone and optional human-readable text

### Privacy & credentials
- **OS credential storage** — machine authentication tokens live in the Windows credential store (DPAPI / Credential Manager), never in plaintext config files
- **No telemetry** — no analytics, crash reporting, or usage tracking
- **Offline by design** — designs, templates, and fonts are processed locally; the app only reaches out to check for updates or when you export to a machine you've authorized

## Supported machines

| Vendor | Target | Format | Status |
|--------|--------|--------|--------|
| **Trotec** | Ruby | Vector import (PDF/SVG) | Stable |
| **Panduit** | Easy-Mark Plus | CSV (thermal) | Stable |
| **Roland** | BN-20A | VersaWorks (vector/raster) | Stable |
| **Zebra / generic** | ZPL-compatible thermal | ZPL II job | Beta (verify on device) |

Not on the list? Most printers and cutters read standard PDF, SVG, PNG, or ZPL directly, so you can usually feed them one of the exports above.

## Downloads

Installers are published on every release. Grab the current build from the **[GitHub Releases](https://github.com/LxveAce/tag-studio/releases)** page.

- **Windows:** `LxveAce-Tag-Studio-0.3.1-Windows-Setup.exe`

Each release ships the Windows installer plus a `latest.yml` manifest for in-app updates. On first run, Windows may flag an unrecognized publisher — check the installer's signature/publisher before you run it (see [SECURITY.md](./SECURITY.md)).

### System requirements

- Windows 10 (21H2) or later, 64-bit
- 4 GB RAM minimum (8 GB recommended)
- ~250 MB free disk space

## Getting started

**1. Install.** Download the latest installer from [Releases](https://github.com/LxveAce/tag-studio/releases) and run it. The app checks for updates and can install new versions in the background.

**2. Start from a template.** Open a bundled template (Asset, Inventory, Shipping, Cable, Badge, and more) or start a blank label, then save your own as a `.json` document.

**3. Design.** Add Text, Barcode, Image, or Shape elements to the canvas. Drag to move, use the handles to rotate and resize, and set fonts, colors, and barcode symbology in the properties panel. Bind a field to data for batch mode (for example, link a barcode to an `AssetID` column).

**4. Export.**
- **Single label** — Export, then pick a format (PDF, SVG, PNG, or a machine format).
- **Batch / data merge** — Export with Data, load a CSV or Excel file, select rows, export.
- **N-up sheets** — tile the label across a paper size with margins and gutters.

## Templates

The [`templates/`](./templates) folder ships ready-to-use layouts:

| Template | Size (in) | Category |
|----------|-----------|----------|
| Asset Tag | 2 × 1 | Asset |
| Industrial Asset | 3 × 1 | Asset |
| QR Asset | 2 × 2 | Asset |
| Inventory Bin | 2.6 × 1 | Inventory |
| Shipping Label | 4 × 2 | Shipping |
| Shipping Box | 4 × 6 | Shipping |
| Cable / Wire Marker | 1.5 × 0.5 | Cable |
| Barcode Strip | 2.6 × 0.6 | Barcode |
| Product / Price | 2 × 1 | Retail |
| File Folder | 3.4 × 0.6 | Filing |
| Name Badge | 3 × 2 | Badge |
| Name Badge | 3.5 × 2.25 | ID Badge |
| Fonts & Barcodes Showcase | 4 × 3 | Demo |

Each template is a `.json` file: top-level `name`, `description`, `category`, and `machineCompat`, plus a `doc` object holding `schemaVersion`, `page` (`widthIn` / `heightIn`), `dpi`, `background`, and an `elements` array. Duplicate one and edit it to make your own.

## FAQ

**Can I use a machine that isn't listed?** Tag Studio exports standard PDF, SVG, PNG, and ZPL, which many printers and cutters can consume directly. Native adapters cover Trotec, Panduit, Roland, and ZPL-compatible thermal printers.

**How do I generate many labels from a spreadsheet?** Open a template, choose **Export with Data**, load your CSV or Excel file, map columns to elements, select rows, and export a multi-page PDF or machine format with one label per row.

**Are my designs and data private?** Yes. Documents are JSON files stored on your computer — Tag Studio doesn't upload them. Machine credentials go in the OS credential store, never in plaintext.

**Does it work offline?** Yes — editing, templates, fonts, and export all run locally. The app reaches out only to check for updates or when you export to a machine you've authorized.

**How do I update?** Tag Studio checks for new releases and can install updates in the background. You can also pull the latest installer from the Releases page any time.

**I found a security issue — who do I contact?** See [SECURITY.md](./SECURITY.md) for the responsible-disclosure process. Email LxveLabs@proton.me — don't open a public issue.

## Security

Designs and data stay on your machine: no telemetry, no cloud upload, nothing phoned home. Machine credentials live in the Windows credential store (DPAPI), never in plaintext. The full posture — credential handling, code integrity, the IPC boundary, and the disclosure timeline — is in [SECURITY.md](./SECURITY.md). Report vulnerabilities privately to LxveLabs@proton.me rather than a public issue.

## Learn more

- **Product info & downloads:** [experttags.com](https://experttags.com)
- **Label & tag engineering notes:** [`docs/LABEL-TAG-ENGINEERING.md`](./docs/LABEL-TAG-ENGINEERING.md) — reusable lessons on print-correct barcode/label output (units, quiet zones, actual-size printing)

## License

LxveAce Tag Studio is released under the [MIT License](./LICENSE). **It's free to use** — no license key, payment, account, subscription, or cloud dependency, and MIT-permissive (use, modify, and redistribute freely). Product info lives at [experttags.com](https://experttags.com).

## Built with

- [Electron](https://www.electronjs.org/) — cross-platform desktop runtime
- [React](https://react.dev/) — UI
- [Konva](https://konvajs.org/) — canvas rendering and interaction

## Connect

- **Website:** [experttags.com](https://experttags.com)
- **Discord:** [discord.gg/lxvelabs](https://discord.gg/lxvelabs) — questions, help, or to talk through a labeling workflow
- **GitHub:** [@LxveAce](https://github.com/LxveAce)
- **Email:** LxveLabs@proton.me (business) · lxveace@proton.me (direct)

---

Built by LxveAce · an ExpertTags product · [experttags.com](https://experttags.com)
