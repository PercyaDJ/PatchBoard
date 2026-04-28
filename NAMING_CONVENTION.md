# Naming Convention

PatchBoard detects report type, site code, and date directly from the filename.
No configuration required — adapt the prefix to your organization.

---

## Pattern

```
EPO  : [PREFIX]-Reporting-EPO-Site-[XXX]_[YYYYMMDD].csv
WSUS : [PREFIX]-Reporting-WSUS-Site-[XXX]_[YYYYMMDD].csv
```

| Field        | Description                                              | Example        |
|--------------|----------------------------------------------------------|----------------|
| `[PREFIX]`   | Any alphanumeric string — your organization or tool name | `ACME`, `CORP` |
| `[XXX]`      | Site code, 2 to 6 uppercase letters or digits            | `CDG`, `LYO`   |
| `[YYYYMMDD]` | Report date in ISO format                                | `20260428`     |

---

## Examples

```
ACME-Reporting-EPO-Site-CDG_20260428.csv
ACME-Reporting-WSUS-Site-CDG_20260428.csv
CORP-Reporting-EPO-Site-LYO_20260428.csv
CORP-Reporting-WSUS-Site-MRS_20260428.csv
```

---

## Fallback detection

If a file does not match the full pattern, PatchBoard falls back to:

1. Looks for `WSUS` or `EPO` anywhere in the filename (case-insensitive).
2. Looks for `Site-[CODE]` to extract the site code.
3. If no site code is found, the file is assigned the placeholder site `SITE`.

This means non-standard filenames are accepted as long as they contain
`WSUS` or `EPO` somewhere in the name.

---

## Required CSV columns

### EPO reports

| Column                                   | Description                        |
|------------------------------------------|------------------------------------|
| `Poste`                                  | Workstation hostname               |
| `Status`                                 | Antivirus status (OK / NOK / KO)   |
| `Last Contact`                           | Last agent contact date            |
| `Endpoint Security Threat Prevention`    | EP module version                  |
| `Agent`                                  | Agent version                      |
| `Type`                                   | Computer type                      |
| `User`                                   | Global user identifier (GID)       |

### WSUS reports

| Column                  | Description                              |
|-------------------------|------------------------------------------|
| `Poste`                 | Workstation hostname                     |
| `Not Installed`         | Number of updates not installed          |
| `Critical`              | Number of critical updates pending       |
| `Security`              | Number of security updates pending       |
| `Failed`                | Number of failed installations           |
| `Pending Reboot`        | Number of updates pending a reboot       |
| `Last Sync Time`        | Last synchronisation timestamp           |
| `Operating System`      | OS version                               |
| `User`                  | Global user identifier (GID)             |

---

## Notes

- `Poste` is the join key between EPO and WSUS records. Values are compared
  case-insensitively after trimming.
- `User` (GID) is used as the `mailto:` recipient in action emails.
  Make sure your mail client can resolve GIDs to addresses via directory lookup
  (e.g. Outlook with Active Directory autocomplete).
- Column names are case-sensitive. Do not rename them in your CSV exports.
