# LxveAce Tag Studio

[![License](https://img.shields.io/badge/License-Proprietary-red.svg)](#license)
[![Release](https://img.shields.io/badge/Release-0.2.0-blue.svg)](#downloads)
[![Windows](https://img.shields.io/badge/Windows-10%2B-0078D4.svg)](#system-requirements)
[![macOS](https://img.shields.io/badge/macOS-10.15%2B-999999.svg)](#system-requirements)
[![Linux](https://img.shields.io/badge/Linux-Ubuntu%2020%2B-E95420.svg)](#system-requirements)

**The industrial-grade label & tag design studio that prints to anything.**

LxveAce Tag Studio combines an intuitive drag-and-drop canvas with powerful multi-machine export. Design once, print to Trotec laser engravers, Panduit thermal printers, Roland vinyl cutters, and more — without leaving the editor.

## Key Features

### Canvas & Design
- **Drag-drop editor**: Place, rotate, and snap text, barcodes, images, and shapes to a pixel-perfect grid
- **Rich element library**: Text with 20+ system fonts, Code128B/Code39/QR barcodes, images, and geometric shapes
- **Smart data binding**: Link elements to CSV columns for instant batch label generation
- **Undo/redo & keyboard shortcuts**: Professional-grade editing workflow

### Export & Integration
- **Multi-format output**:
  - **PDF**: Universal vector export (machine-independent)
  - **Trotec**: Native Ruby import (Speedy series)
  - **Panduit Easy-Mark Plus**: Proprietary CSV thermal format
  - **Roland BN-20A**: VersaWorks bridge export
- **Batch mode**: Merge templates with data rows for 10s–1000s of labels in one run

### Barcode Support
- **Code 128B**: Full ASCII 32–126 support (inventory, shipping)
- **Code 39**: Alphanumeric, check digit optional
- **QR**: Dynamic data encoding, any URL or text
- Smart quiet zones and customizable text rendering

### Templates & Presets
- **Bundled library**: Industry-standard sizes (2x1, 4x6, asset tags, shipping labels)
- **Category-tagged**: Asset, Inventory, Shipping, Custom
- **Machine hints**: Templates flag compatible machines
- **Extend**: Add custom templates via the templates folder

### Security & Credentials
- **OS credential storage**: Machine authentication tokens stored in Windows DPAPI, macOS Keychain, Linux Secret Service — never in config files
- **No telemetry**: Complete data privacy
- **Signed installers**: Code integrity verified on every install

## Supported Machines

| Vendor | Model | Format | Status |
|--------|-------|--------|--------|
| **Trotec** | Speedy 100/360 | Ruby import (vector) | Stable |
| **Panduit** | Easy-Mark Plus | Proprietary CSV (thermal) | Stable |
| **Roland** | BN-20A | VersaWorks (vector/raster) | Stable |

*Need a different machine? The adapter system is extensible — contact us for custom support.*

## Downloads

### Latest Version: 0.2.0 (Stable)

Installers and templates are published by CI on every release. Visit our **[GitHub Releases](https://github.com/LxveAce/tag-studio/releases)** page to download:

- **Windows**: `LxveAce-Tag-Studio-0.2.0-Setup.exe`
- **macOS**: `LxveAce-Tag-Studio-0.2.0-arm64.dmg` (Apple Silicon) / `LxveAce-Tag-Studio-0.2.0-x64.dmg`
- **Linux**: `lxveace-tag-studio-0.2.0.AppImage` or `.deb` / `.rpm`

### System Requirements

#### Windows
- Windows 10 (21H2) or later
- 4 GB RAM minimum (8 GB recommended)
- 250 MB free disk space
- .NET Framework 4.7+ (installer bundles if missing)

#### macOS
- macOS 10.15 (Catalina) or later
- 4 GB RAM minimum (8 GB recommended)
- 250 MB free disk space
- Intel or Apple Silicon support

#### Linux
- Ubuntu 20.04 LTS or equivalent (Debian-based)
- 4 GB RAM minimum (8 GB recommended)
- 250 MB free disk space
- GTK 3.0+ development libraries (auto-installed on Debian/Ubuntu)

## Getting Started

### 1. Install
Download the installer for your platform and run it. On Windows & macOS, the app will auto-update when new versions are available.

### 2. Open a Template
Launch Tag Studio → **File** → **New from Template**. Browse the bundled library (Asset, Inventory, Shipping, etc.) or import your own `.tagstudio` document.

### 3. Design
- Click the canvas to add elements (Text, Barcode, Image, Shapes)
- Drag to move, rotate handles for angles, corner handles to resize
- Use the properties panel on the right to style fonts, colors, and barcode symbols
- Bind data fields for batch mode (e.g., link barcode to "AssetID" column in CSV)

### 4. Export
- **Single label**: File → Export → choose format (PDF, native machine format)
- **Batch mode**: File → Export with Data → upload CSV → select rows → export
- Choose output folder; Tag Studio handles the rest

## Frequently Asked Questions

### Can I use Tag Studio with a machine not in the supported list?

Tag Studio's adapter system is extensible. We support Trotec, Panduit, and Roland out of the box, but we can add support for your hardware. Contact our support team with the machine model and API docs.

### How do I merge a template with 1000 shipping labels from a CSV?

Open the template → **File** → **Export with Data** → upload your CSV (columns: Tracking ID, Recipient, Barcode) → select all rows → click Export. Tag Studio will generate a multi-page PDF or native format with one label per row.

### Are my designs and data encrypted?

Documents are JSON files stored on your computer — tag studio does not upload them anywhere. Machine credentials (auth tokens, API keys) are stored in your OS credential store (Windows DPAPI, macOS Keychain, Linux Secret Service), never in plain text.

### What fonts are available?

Tag Studio includes 20+ system fonts: Arial, Helvetica, Segoe UI, Times New Roman, Georgia, Courier New, Consolas, Impact, and more. All render natively in the canvas and export to PDF/machine formats.

### Can I rotate or skew elements?

Yes. Every element (text, barcode, image, shape) supports free rotation about its center (in degrees). Skew is not yet supported but is on the roadmap.

### Does Tag Studio work offline?

Yes. The app is entirely offline except when exporting to cloud-connected machines (future roadmap). Templates, fonts, and editors run locally.

### How do I update to a new version?

On Windows & macOS, Tag Studio will notify you of updates and install them in the background. On Linux, use your package manager or download the latest `.AppImage` / `.deb` / `.rpm`.

### I found a security issue. Who do I contact?

Please see [SECURITY.md](./SECURITY.md) for responsible disclosure instructions.

## Support & Feedback

- **Documentation**: See the [docs/](./docs/) folder for user guides and API reference
- **Issues**: Report bugs on our [GitHub Issues](https://github.com/LxveAce/tag-studio/issues) page
- **Email**: support@lxveace.com

## License

LxveAce Tag Studio is a proprietary, commercial product. Use of the software is governed by the License Agreement included in this distribution. A 30-day evaluation license is included; after evaluation, a commercial license is required for continued use.

See [LICENSE](./LICENSE) for full terms.

## Credits

LxveAce Tag Studio is built with:
- [Electron](https://www.electronjs.org/) — Cross-platform desktop apps
- [React](https://react.dev/) — UI framework
- [Konva](https://konvajs.org/) — Canvas rendering & interaction
- [jsPDF](https://github.com/parallelize/jsPDF) — PDF generation

---

**[Download Now](https://github.com/LxveAce/tag-studio/releases)** | **[Documentation](./docs/)** | **[License](./LICENSE)**
