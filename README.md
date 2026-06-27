# LxveAce Tag Studio

[![License](https://img.shields.io/badge/License-Proprietary-red.svg)](#license)
[![Release](https://img.shields.io/badge/Release-0.3.1-blue.svg)](#downloads)
[![Platform](https://img.shields.io/badge/Windows-10%2B-0078D4.svg)](#system-requirements)
[![Stack](https://img.shields.io/badge/Electron%20%2B%20React-2b2b2b.svg)](#built-with)

**An industrial-grade label & tag design studio that prints to anything.**

LxveAce Tag Studio pairs a drag-and-drop design canvas with multi-machine export. Design a label once, then send it to Trotec laser engravers, Panduit thermal printers, Roland vinyl cutters, and ZPL-compatible thermal printers — or export to standard PDF, SVG, and PNG — without leaving the editor.

This repository is the **public release home** for Tag Studio: signed installers (via Releases), the bundled template library, the license agreement, and the security policy. The application source is closed.

> Project site: **[experttags.com](https://experttags.com)**

## Key Features

### Canvas & Design
- **Drag-and-drop editor**: place, move, rotate, and resize text, barcodes, images, and shapes
- **Multi-select & marquee**: shift-click or rubber-band a group and move it together
- **Smart alignment guides**: snap to other elements' edges and centers with live guide lines
- **Align & distribute**: alignment plus even horizontal/vertical spacing
- **Element library**: text with system fonts, Code 128B / Code 39 / QR barcodes, images, and geometric shapes
- **Data binding**: link elements to CSV or Excel columns for batch label generation
- **Editing workflow**: undo/redo, zoom, nudge, and keyboard shortcuts

### Export & Output
- **Standard formats**: **PDF**, **SVG**, and **PNG** (high-DPI raster)
- **Per-machine output**:
  - **Trotec** — vector import (PDF/SVG) for Ruby
  - **Panduit Easy-Mark Plus** — CSV thermal format
  - **Roland BN-20A** — VersaWorks bridge (PDF/EPS)
  - **Zebra / generic thermal** — **ZPL II** jobs for ZPL-compatible printers
- **N-up sheet layout**: tile a label across Letter/A4/Legal/Tabloid with margins and gutters, paginated automatically
- **Batch / data merge**: combine a template with CSV or Excel rows to generate many labels in one pass

### Barcodes
- **Code 128B** — full ASCII 32–126 (inventory, shipping)
- **Code 39** — alphanumeric
- **QR** — any URL or text
- Configurable quiet zone and optional human-readable text

### Templates
- **Bundled library** of common label sizes shipped in [`templates/`](./templates)
- **Category-tagged**: Asset, Inventory, Shipping, Cable, Barcode, Retail, Filing, Badge / ID Badge, and a fonts-and-barcodes demo
- **JSON format**: templates are plain `.json` documents (page size in inches, DPI, and an element list) — copy and adapt them or add your own

### Privacy & Credentials
- **OS credential storage**: machine authentication tokens are kept in the OS credential store (Windows DPAPI / Credential Manager), never in plaintext config files
- **No telemetry**: no analytics, crash reporting, or usage tracking
- **Offline by design**: designs, templates, and fonts are processed locally; the app contacts external systems only when you export to an authorized machine

## Supported Machines

| Vendor | Target | Format | Status |
|--------|--------|--------|--------|
| **Trotec** | Ruby | Vector import (PDF/SVG) | Stable |
| **Panduit** | Easy-Mark Plus | CSV (thermal) | Stable |
| **Roland** | BN-20A | VersaWorks (vector/raster) | Stable |
| **Zebra / generic** | ZPL-compatible thermal | ZPL II job | Beta (verify on device) |

## Downloads

### Latest version: 0.3.1

Signed installers are published on every release. Get the current build from the **[GitHub Releases](https://github.com/LxveAce/tag-studio/releases)** page.

- **Windows**: `LxveAce-Tag-Studio-0.3.1-Windows-Setup.exe`

Releases ship a Windows installer and a `latest.yml` manifest for in-app updates.

### System Requirements

#### Windows
- Windows 10 (21H2) or later, 64-bit
- 4 GB RAM minimum (8 GB recommended)
- ~250 MB free disk space

## Getting Started

### 1. Install
Download the latest installer from [Releases](https://github.com/LxveAce/tag-studio/releases) and run it. The app checks for updates and can install new versions in the background.

### 2. Start from a template
Launch Tag Studio and open a bundled template (Asset, Inventory, Shipping, Cable, Badge, and more) or start a blank label, then save your own as a `.json` document.

### 3. Design
- Add elements to the canvas: Text, Barcode, Image, Shapes
- Drag to move, use handles to rotate and resize
- Style fonts, colors, and barcode symbology in the properties panel
- Bind a field to data for batch mode (e.g. link a barcode to an `AssetID` column)

### 4. Export
- **Single label**: Export → choose a format (PDF, SVG, PNG, or a machine format)
- **Batch / data merge**: Export with Data → load a CSV or Excel file → select rows → export
- **N-up sheets**: tile the label across a paper size with margins and gutters

## Templates

The [`templates/`](./templates) folder ships ready-to-use layouts, including:

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

Each template is a JSON document with a page size, DPI, and an `elements` array. Duplicate one and edit it to create your own.

## FAQ

### Can I use a machine that isn't listed?
Tag Studio exports standard PDF, SVG, PNG, and ZPL, which many printers and cutters can consume directly. Native adapters cover Trotec, Panduit, Roland, and ZPL-compatible thermal printers.

### How do I generate many labels from a spreadsheet?
Open a template, choose **Export with Data**, load your CSV or Excel file, map columns to elements, select rows, and export a multi-page PDF or machine format with one label per row.

### Are my designs and data private?
Yes. Documents are JSON files stored on your computer — Tag Studio does not upload them. Machine credentials are stored in the OS credential store, never in plaintext.

### Does it work offline?
Yes. Editing, templates, fonts, and export run locally. The app only reaches out when you export to a machine you've authorized.

### How do I update?
Tag Studio checks for new releases and can install updates in the background. You can also download the latest installer from the Releases page at any time.

### I found a security issue — who do I contact?
See [SECURITY.md](./SECURITY.md) for responsible-disclosure instructions.

## License

LxveAce Tag Studio is proprietary, commercial software. Use is governed by the [End-User License Agreement](./LICENSE). A 30-day evaluation is included; after the trial the app enters a limited mode where editing remains available but export and machine connectivity require a valid license key. License keys are activated in-app — see the EULA for full terms.

## Built With

- [Electron](https://www.electronjs.org/) — cross-platform desktop runtime
- [React](https://react.dev/) — UI
- [Konva](https://konvajs.org/) — canvas rendering and interaction

## Connect

- **Discord**: [discord.gg/lxveace](https://discord.gg/lxveace) — questions, help, or to talk through this project
- **GitHub**: [@LxveAce](https://github.com/LxveAce)
- **Website**: [lxveace.com](https://lxveace.com)
- **Project site**: [experttags.com](https://experttags.com)
