# LxveAce/tag-studio - Forward Plan

> Status: Shipping but inconsistent. Health: YELLOW. Last synthesized: 2026-06-27 (date placeholder — update on next pass).

## Where this stands

**What it is.** `tag-studio` (PUBLIC) is the *release home / distribution repo* for LxveAce Tag Studio — an Electron + React + Konva industrial label/tag design studio that exports to Trotec, Panduit, Roland, and Zebra/ZPL plus standard PDF/SVG/PNG. This repo intentionally contains **no application source** ("The application source is closed" — README line 12). It ships: signed Windows installer via GitHub Releases, `latest.yml` update manifest, the bundled `templates/` library (13 JSON templates), `LICENSE` (proprietary EULA), `SECURITY.md`, `docs/`, and `version.json`.

**Source of truth for code** is the PRIVATE repo `LxveAce/tag-studio-testing` (Electron 31 / React 18 / TypeScript 5.5 / Konva). The marketing/download site is `LxveAce/experttags.com` (GitHub Pages, CNAME `experttags.com`), whose `downloads.js` live-fetches the latest release from this repo.

**How to "build/run" this repo.** There is nothing to build here — it is a distribution repo. The flow is: develop in `tag-studio-testing`, build the signed installer + `latest.yml`, then publish the artifacts (plus updated `templates/`, `LICENSE`, `docs/`, `version.json`) as a GitHub Release on `tag-studio`. `experttags.com` then auto-tracks the new release via the GitHub API.

**Current state (verified).** Latest release is **v0.3.1** (2026-06-09), marked Latest, with 3 assets. The Windows installer download link RESOLVES and serves the full binary (HTTP 200, Content-Length 86,716,110 matches asset metadata exactly). `latest.yml` is internally consistent (version 0.3.1, sha512 present, size 86,716,110). The live site loads and links to the repo. The pipeline is fundamentally sound — but three consistency problems sit on top of it (see below).

## P0 - do first

1. **Reconcile the licensing/pricing story across ALL surfaces (before the next release).** Repo `LICENSE` (lines 6-23) and `README.md` (lines 12, 142) describe a **proprietary/commercial EULA**: 30-day evaluation, then export *and* machine connectivity are disabled until a paid license key is activated. Meanwhile `experttags.com` markets the product as **"Free · Open Source · no subscription"** with a "View Source" link and schema.org offer price 0; the private `tag-studio-testing` badge says "UNLICENSED / Free build." A visitor gets *opposite* messaging about cost, openness, and feature-gating. **Pick one model** and make `LICENSE`, `README.md`, `version.json` channel, and the site all agree. This is the #1 customer-facing defect.

2. **Fix the version-manifest drift.** `version.json` pins `0.2.0` while the README badge/Downloads (lines 4, 64, 68), the Latest GitHub release (v0.3.1), and the shipped `latest.yml` all say `0.3.1`. Bump `version.json` to the released version **and wire it into the release process** so it is stamped from the build automatically and can't drift again.

3. **Verify the installer trust chain (cyber-controller / installer integrity).** Confirm `LxveAce-Tag-Studio-0.3.1-Windows-Setup.exe` is **Authenticode-signed** as README/site claim ("signed installers"). `electron-updater` enforces Windows signature validation, so an unsigned or mis-signed `.exe` would silently break in-app auto-update from `latest.yml`. Also byte-verify the downloaded binary's sha512 against the value in `latest.yml`. Make both checks a release gate.

## Surface bugs found

| Title | Location | Severity | Note |
|-------|----------|----------|------|
| Site says "Free / Open Source / no subscription"; repo says proprietary paid EULA with license-key-gated export | experttags.com homepage + `C:\Users\mmrla\repos\tag-studio\LICENSE` (lines 6-23) + `README.md` (lines 12, 142) | P1 | Directly opposite customer-facing claims (cost, openness, feature-gating). Pick one model; align all surfaces. |
| `version.json` stale (0.2.0) vs actual release/README/latest.yml (0.3.1) | `C:\Users\mmrla\repos\tag-studio\version.json` | P2 | Committed manifest lags released build by a minor version. |
| Public download stuck at v0.3.1; 0.4.0 + 0.5.0 waves exist in private source but unreleased | Public: `gh release list -R LxveAce/tag-studio`. Private: `tag-studio-testing/CHANGELOG.md` ([0.5.0]+[0.4.0]) | P2 | 0.4.0 reportedly fixed landscape-clip + dropped-PDF-rotation + SVG text sizing. Public users still hit those. Release decision, not an asserted code bug (source unverified here). |
| Private testing README "Supported Machines" lists only 3 targets (omits Zebra/ZPL) | `tag-studio-testing/README.md` "Supported Machines" | P3 | Public README, thermalAdapter, zplExporter, and site all advertise 4 targets. Fix the outlier section. |

## Features to add

> **NO USER DIRECTIVES were supplied for this planning pass.** Everything below is derived strictly from recon evidence; nothing is invented. When the user provides directives, insert them here **verbatim** and promote them to the top.

- **Release-sync workflow (private -> public).** Decide and document whether to publish the 0.4.0/0.5.0 waves from `tag-studio-testing` to public `tag-studio` (this is what gets the print-correctness fixes to real users). Code stays in the private repo; only artifacts (installer, `latest.yml`, blockmap), `templates/`, `LICENSE`, and `docs/` publish to public.
- **Auto-stamp `version.json`** from the build version at release time (root-cause fix for the 0.2.0/0.3.1 drift).
- **Templates sync checklist** on every release (13 templates mirror between repos; public README table already documents all 13).
- **Single authoritative licensing/pricing statement** referenced by `LICENSE`, `README.md`, `version.json`, and the site, so one edit updates everywhere.

## Red-team / hardening

*(Public repo — responsible hardening only, no exploit recipes or license-keying internals.)*

- **Installer + update-channel integrity (ties to P0 #3):** keep `latest.yml` sha512/size generated by the build, never hand-edited, so a tampered asset can't pass `electron-updater` validation. Verify signature + hash as a release gate.
- **Site download resolution:** `experttags.com/downloads.js` already hardens link rendering with `safeUrl()` http(s)-scheme checks + HTML/attr escaping and fetches the live latest release. Keep this; do not regress to hard-coded links.
- **Document-trust boundary:** the document schema is versioned (v2) with `migrateDocument.ts` + an untrusted-document validator (`validateDocument.ts`) in the private source. Any template/schema change shipped publicly must go through migration + validation, never ad-hoc, so a crafted `.json` can't break/exploit the app.
- **Outstanding infra gap (not a repo code item, tracked for the surface):** per `session-context/SESSION.md`, `experttags.com` still lacks real HTTP security headers / HSTS (grey-cloud / DNS-only off GitHub Pages). SPF `-all` + DMARC `p=reject` + CSP `<meta>` + `security.txt` are already in place; HSTS/edge headers remain to be added at the CDN layer.
- Keep responsible-disclosure routing in `SECURITY.md`; publish no sensitive specifics.

## Dig deeper (next dedicated session)

1. **Inspect the published installer.** Download `LxveAce-Tag-Studio-0.3.1-Windows-Setup.exe`; verify Authenticode signature present/valid/chained, signer identity matches LxveAce, and sha512 of the bytes equals `latest.yml`.
2. **JS-drive `experttags.com/downloads.html`** (static fetch only shows "Loading latest release..."). Confirm the rendered button resolves to the current asset.
3. **Audit the Trotec / Panduit / Roland export pipelines** in `tag-studio-testing/src/machines` — only the thermal/Zebra internals were reviewed in recon. Ideally validate on-device (user dogfoods a Trotec CO2 + Roland BN-20A).
4. **Validate the 0.4.0 print-correctness fixes end-to-end** (landscape clipping, PDF rotation, SVG text sizing) against the bundled templates before publishing them.
5. **Reconcile the licensing model in code:** inspect the private license/keySystem/publicKey modules to confirm what the app actually enforces, so the chosen public messaging matches runtime behavior.
6. **Check older releases (v0.2.0–v0.3.0)** asset integrity/links if still user-reachable (only v0.3.1 was verified).
7. **Confirm the in-app updater is actually pointed at this repo's `latest.yml`** (client wiring is closed-source, unverified).

## Dependencies & cross-repo context

- **Private source:** `LxveAce/tag-studio-testing` (Electron 31 / React 18 / TS 5.5 / Konva). Flagship code work happens here; only artifacts publish to public `tag-studio`.
- **Site:** `LxveAce/experttags.com` (GitHub Pages, CNAME `experttags.com`); `downloads.js` live-fetches this repo's latest release, so the site auto-tracks releases.
- **Distribution:** GitHub Releases on `LxveAce/tag-studio` (installer + `latest.yml` + blockmap); electron-updater-style channel.
- **Brand/policy (user memory — lxveace-profile-rules):** commit as **LxveAce** with **NO Claude co-author**; no PII on public repos; `experttags.com` intentionally has no Discord contact.
- **Lineage (do not re-derive):** Code 128B "ported from legacy Barcode Label Gen"; center-anchored inch-offset layout + CSV/Excel batch-merge carry over from predecessors `Barcode-Tag-Creation` and Automated Tag Creator V5. **Do NOT modify** `C:\Users\mmrla\Downloads\Barcode Label Gen` (original copy).
- **Continuity notes:** `C:\Users\mmrla\repos\session-context\SESSION.md`, `C:\Users\mmrla\Projects\CLAUDE-TRANSFER.md`.

## Open questions

- **Which licensing/pricing model is the intended truth** — proprietary/commercial (repo) or free/open-source (site)? Must be a user decision before the next release.
- Is the v0.3.1 installer actually Authenticode-signed? (binary not inspected in recon)
- Does `experttags.com/downloads.html`'s JS button resolve to a valid current asset? (JS-injected; unverified)
- Should the 0.4.0/0.5.0 waves publish now, and at what private->public cadence? (no stated decision; inferred from evidence)
- Are the Trotec/Panduit/Roland export pipelines correct? (only thermal/Zebra internals reviewed)
- The CODE recon report contained only placeholder values ("test"); no static code-analysis findings were available to fold in.
- Unconfirmed: contents of private `Barcode-Tag-Creation-Source`; whether `tag-list-2`'s uncommitted files relate to Tag Studio.


<!-- DEEP-RESEARCH-NOTE (added 2026-06-27, for when Tag Studio work begins) -->
## Deep research to do BEFORE building (Tag Studio)

When we pick up Tag Studio, do deep research first into:
- **Graphic-design software** internals (layout/typography/auto-fit engines) and **industrial tag
  software** (e.g. Panduit's Easy-Mark Plus and comparable label/tag tools) to streamline tag
  generation and match how the machines expect data.
- **Batch tag generation** for large datasets: generate many tags efficiently from one dataset.
- **Variable-length data within a single batch:** rows in the same batch can have different
  character counts. Add **smart auto-fit logic** that scales/wraps/condenses each tag's text to fit
  the predetermined size/spec for its template **per tag**, while still batch-generating the whole
  set (no manual per-tag fiddling). Define the fit algorithm (max font, min font floor, condense vs
  wrap vs shrink-to-fit, and a hard "won't fit / flag for review" path) as part of the spec.
