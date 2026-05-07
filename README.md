# PatchBoard

**Zero-install, single-file dashboard for EPO and WSUS CSV report consolidation.**

Drop your CSV exports, get anomalies and one-click mail actions instantly — no server, no installation, no dependencies.

![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)
![No dependencies](https://img.shields.io/badge/dependencies-none-brightgreen)
![Single file](https://img.shields.io/badge/delivery-single%20HTML%20file-informational)

---

## What it does

Security teams relying on EPO (McAfee Endpoint Security) and WSUS (Windows Update Services) typically receive one CSV export per site, per tool, per week. Consolidating 20+ files by hand to identify which workstations are non-compliant and notify the right users is slow and error-prone.

PatchBoard solves this in three steps:

1. Drop all your CSV files onto the dashboard (or click to browse).
2. PatchBoard detects each file's type (EPO or WSUS) and site code from the filename, then parses and merges everything automatically.
3. Review anomalies, filter by site or type, and send pre-filled notification emails with a single click.

No data ever leaves your machine. Everything runs in the browser.

---

## Features

- **Drag and drop** — load any number of EPO and WSUS CSV files at once
- **Auto-detection** — file type and site code resolved from the filename, no manual mapping
- **Merged anomaly view** — one row per workstation, EPO and WSUS status side by side
- **KPI cards** — compliance overview adapts to the active tab (Anomalies, WSUS, EPO)
- **One-click mail actions** — opens a pre-filled draft in your mail client for each non-compliant workstation
- **Filter by site, anomaly type, or free text** — workstation name or user identifier
- **Zero dependencies** — PapaParse is inlined, no CDN, no internet access required
- **Works offline** — double-click the HTML file in Chrome or Edge, that's it

---

## Getting started

No installation. No build step. No server.

```
1. Download PatchBoard.html
2. Double-click it — opens in Chrome or Edge
3. Drop your CSV exports onto the dropzone
```

Alternatively, serve it from any static web server (nginx, IIS, a simple Python HTTP server) for team-wide access.

---

## File naming

PatchBoard detects type and site code from the filename.
Files must follow this pattern:

```
EPO  : [PREFIX]-Reporting-EPO-Site-[XXX]_[YYYYMMDD].csv
WSUS : [PREFIX]-Reporting-WSUS-Site-[XXX]_[YYYYMMDD].csv
```

| Field        | Description                                   | Example          |
|--------------|-----------------------------------------------|------------------|
| `[PREFIX]`   | Any string — your organization or tool name   | `ACME`, `CORP`   |
| `[XXX]`      | Site code, 2–6 uppercase letters or digits    | `CDG`, `LYO`     |
| `[YYYYMMDD]` | Report date                                   | `20260428`       |

See [NAMING_CONVENTION.md](NAMING_CONVENTION.md) for the full specification, fallback detection rules, and required CSV column names.

---

## Required CSV columns

### EPO

`Poste` `Status` `Last Contact` `Endpoint Security Threat Prevention` `Agent` `Type` `User`

### WSUS

`Poste` `Not Installed` `Critical` `Security` `Failed` `Pending Reboot` `Last Sync Time` `Operating System` `User`

Column names are case-sensitive. `Poste` is the join key between EPO and WSUS records. `User` is used as the `mailto:` recipient — your mail client should be able to resolve it to an email address (e.g. via Active Directory autocomplete in Outlook).

---

## Adapting to your organization

PatchBoard is intentionally generic. The only thing to adapt is the filename prefix — no code change required, as the prefix is ignored during detection.

If your filenames differ significantly, the fallback detection still works as long as `WSUS` or `EPO` appears somewhere in the filename.

The mail body wording is in French by default. To localize it, edit the `W_WORDING` object and the `buildMailto` function at the top of the `<script>` block in `PatchBoard.html`.

---

## Browser compatibility

| Browser        | Support |
|----------------|---------|
| Chrome 90+     | Full    |
| Edge 90+       | Full    |
| Firefox 90+    | Full    |
| Safari 15+     | Full    |

The `mailto:` action relies on a locally configured mail client (Outlook, Thunderbird, etc.).

---

## Project structure

```
PatchBoard/
├── PatchBoard.html        # The entire application — single file, self-contained
├── NAMING_CONVENTION.md   # File naming rules and required CSV columns
├── LICENSE                # MIT
└── README.md
```

---

## License

MIT — see [LICENSE](LICENSE).

© 2026 Jolan D. ([PercyaDJ](https://github.com/PercyaDJ))
