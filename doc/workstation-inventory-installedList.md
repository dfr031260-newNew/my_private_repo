# Workstation Software Inventory & Reinstall Guide

Generated: 2026-09-13
Machine: this Windows 10 Pro workstation (user: dfr031260)

This document inventories installed software and how to get each piece back on a
new workstation. Companion file: **`winget-export.json`** (same folder) — a
machine-readable manifest of every winget-tracked package on this box.

## Fastest path to a new machine

1. Install **winget** (built into modern Windows 10/11 — `App Installer` from the
   Microsoft Store if missing) and **Chocolatey** (see below) first.
2. Copy `winget-export.json` to the new machine and run:
   ```powershell
   winget import -i winget-export.json --accept-package-agreements --accept-source-agreements
   ```
   This reinstalls every app in [Section 1](#1-reinstall-via-winget-one-command) automatically, at the pinned versions
   captured on 2026-09-13. Drop `--version` pinning by editing the JSON if you'd
   rather get latest versions instead (recommended for most apps — see note below).
3. Reinstall Chocolatey packages:
   ```powershell
   choco install claude-code terraform -y
   ```
4. Work through [Section 3](#3-manual--vendor-direct-installs-no-winget-package) for everything winget doesn't package (Adobe, Office,
   Norton, printer/OCR software, etc.) — mostly account sign-ins and vendor
   downloads.
5. Read [Section 6](#6-dont-forget-when-migrating-not-software-but-easy-to-lose) — the non-software things (keys, credentials, data) that are
   easy to forget and hard to recreate.

> **Note on `winget import`:** by default it installs the *exact pinned
> version* recorded in the JSON. For most apps you'll want current versions
> instead — either delete the `"Version"` line for each package in the JSON
> before importing, or just reinstall selectively with
> `winget install --id <Id>` from the table below.

---

## 1. Reinstall via winget (one command)

These are already tracked by Windows Package Manager. Reinstall individually with
`winget install --id <Id>` or all at once via `winget-export.json` (see above).

| Application | Winget ID | Notes |
|---|---|---|
| Adobe Acrobat Pro (64-bit) | `Adobe.Acrobat.Pro` | Needs Adobe account/subscription |
| Adobe Creative Cloud | `Adobe.CreativeCloud` | Installer/hub for Photoshop, etc.; needs Adobe account |
| Amazon Kindle | `Amazon.Kindle` | |
| Apple Application Support | `Apple.AppleApplicationSupport.x86` | Dependency for iTunes |
| Apple Software Update | `Apple.AppleSoftwareUpdate` | Dependency for iTunes |
| ASIO4ALL | `MichaelTippach.ASIO4ALL` | Universal ASIO audio driver |
| Audacity | `Audacity.Audacity` | Audio editor |
| AWS Command Line Interface v2 | `Amazon.AWSCLI` | Re-run `aws configure` after install |
| Bluetooth Battery Monitor | `LuculentSystems.BluetoothBatteryMonitor` | |
| Claude (desktop app) | `Anthropic.Claude` | |
| CutePDF Writer | `AcroSoftware.CutePDFWriter` | Virtual PDF printer |
| DBeaver Community | `DBeaver.DBeaver.Community` | Database client — re-import connection profiles |
| Dell Display and Peripheral Manager | `Dell.DisplayAndPeripheralManager` | Dell/hardware-specific; only relevant if new machine is a Dell |
| Free Alarm Clock | `ComfortSoftwareGroup.FreeAlarmClock` | |
| Git | `Git.Git` | Reconfigure `user.name`/`user.email`; restore SSH keys (see §6) |
| GitHub Desktop | `GitHub.GitHubDesktop` | Re-sign in to GitHub |
| GNU Privacy Guard (GnuPG) | `GnuPG.GnuPG` | Restore GPG keyring from backup (see §6) |
| Google Chrome | `Google.Chrome` | Sign in to Chrome/Google account to sync bookmarks/extensions |
| Google Drive | `Google.GoogleDrive` | Sign in to re-sync |
| GoTo Opener | `GoTo.GoToOpener` | GoToMeeting/GoToWebinar helper |
| Microsoft 365 Copilot | `Microsoft.365Copilot` | |
| Microsoft Edge | `Microsoft.Edge` | Usually already on Windows |
| Microsoft OneDrive | `Microsoft.OneDrive` | Sign in to re-sync |
| Microsoft Outlook (new) | `Microsoft.Outlook` | Sign in with Microsoft/Office account |
| Microsoft Teams | `Microsoft.Teams` | |
| Microsoft Visual Studio Code | `Microsoft.VisualStudioCode` | Settings Sync (turn on in-app) restores extensions/settings if you enable it before moving |
| Microsoft .NET Desktop Runtime 8 | `Microsoft.DotNet.DesktopRuntime.8` | Dependency for some apps |
| NordVPN | `NordSecurity.NordVPN` | Sign in with NordVPN account |
| Notepad++ | `Notepad++.Notepad++` | |
| paint.net | `dotPDN.PaintDotNet` | |
| PostgreSQL 15 | `PostgreSQL.PostgreSQL.15` | Installs server + pgAdmin/pgAgent/psqlODBC; databases themselves must be backed up separately (`pg_dumpall`) |
| PowerShell 7 | `Microsoft.PowerShell` | |
| PuTTY | `PuTTY.PuTTY` | Restore saved sessions from registry export if needed |
| Python 3.7 + Launcher | `Python.Python.3.7`, `Python.Launcher` | Old/EOL Python version — consider installing a current 3.x instead unless a project pins 3.7 |
| QuickTime 7 | `Apple.QuickTime` | **Discontinued/unsupported by Apple (has known unpatched vulnerabilities)** — recommend not reinstalling; use VLC instead |
| Signal | `OpenWhisperSystems.Signal` | Re-link device to phone after install |
| VLC media player | `VideoLAN.VLC` | |
| Windows PC Health Check | `Microsoft.WindowsPCHealthCheck` | |
| Zoom Workplace | `Zoom.Zoom.EXE` | |
| Zotero | `DigitalScholar.Zotero` | Sign in to Zotero account to re-sync library; local PDFs/attachments live in the Zotero data directory — back that up separately (see §6) |

**Not currently in winget's own list output but confirmed winget-installable —
worth using winget for these too on the new machine instead of their old
manual installers:**

| Application | Winget ID | Why |
|---|---|---|
| Discord | `Discord.Discord` | Installed here as a per-user app outside winget's tracking |
| WinSCP | `WinSCP.WinSCP` | Installed here via a standalone installer |
| Gpg4win | `Gpg4win.Gpg4win` | Installed here via a standalone installer (separate from the GnuPG.GnuPG entry above) |

## 2. Chocolatey packages

Chocolatey is installed on this machine with 3 packages:

```powershell
choco install claude-code -y
choco install terraform -y
```

(Chocolatey itself: `winget install --id Chocolatey.Chocolatey` or via
https://chocolatey.org/install)

## 3. Manual / vendor-direct installs (no winget package)

These were installed from vendor installers directly and aren't in winget/choco.
Reinstall from each vendor's site; items needing a license/account are flagged.

| Application | Category | Where to get it | Account/license needed? |
|---|---|---|---|
| Finale | Music notation software | MakeMusic account portal | Yes — licensed software |
| Norton 360 / Norton Security | Antivirus/security suite | norton.com, sign in to Norton account | Yes |
| WinZip | Archive utility | winzip.com | Yes — licensed |
| Spybot – Search & Destroy | Anti-malware | safer-networking.org | No (free) |
| Paltalk | Chat/video app | paltalk.com | Account |
| μTorrent | BitTorrent client | utorrent.com | No |
| iTunes | Media/device sync | apple.com/itunes or Microsoft Store | Apple ID |
| Logitech Capture | Webcam capture software | logitech.com/download | No |
| Harmony Remote software (Harmony Browser Plug-in, Harmony Remote Update, Remote Control USB Driver) | Logitech Harmony remote config | logitech.com (Harmony support) | Logitech account for remote config sync |
| LAME MP3 encoder | Audacity export codec | Audacity's own "Download LAME" instructions, or rarewares.org | No |
| LADSPA plugins for Windows | Audacity audio effects | audacity-plugins project / SourceForge | No |
| I.R.I.S. OCR (Readiris) | OCR, bundled with HP scanner software | Bundled with HP printer/scanner driver package below | No |
| Microsoft 365 (Office desktop apps, en-us) | Office suite | office.com → "Install Office", sign in | Yes — Microsoft 365 subscription |
| Adobe Flash Player 32 PPAPI | Legacy browser plugin | **Do not reinstall** — end-of-life, Adobe stopped distributing it (Jan 2021) and it's a known security risk | — |
| Microsoft Silverlight | Legacy browser plugin | **Do not reinstall** — end-of-life/unsupported | — |

## 4. Hardware/driver-specific software

These are tied to the current machine's specific hardware (Dell OEM build, Intel
integrated graphics, Realtek audio). **They generally do *not* need manual
reinstallation on a different workstation** — a new machine will pull its own
correct drivers via Windows Update, or (if it's also a Dell) via Dell Command
Update / Dell SupportAssist. Only worth tracking down manually if the new
machine is the *same* model:

- Dell Core Services / Dell Peripheral Core / Dell Display and Peripheral Manager
- Intel(R) Control Center, Intel Management Engine Components, Intel Processor
  Graphics, Intel USB 3.0 eXtensible Host Controller Driver
- Intel Graphics Command Center (Microsoft Store app)
- Realtek High Definition Audio Driver
- HP Officejet Pro 8610 printer stack (HP Enabling Services, HP PSDr NTService,
  HP Update, HP Softpaqs, "Product Improvement Study" telemetry) — if you keep
  the same HP Officejet Pro 8610 printer, get current drivers from
  support.hp.com rather than reusing these

## 5. Pre-installed Windows / Microsoft Store apps

Everything below came with Windows itself or Microsoft Store and needs **no
action** — a fresh Windows install (or Store re-sync via your Microsoft
account) restores them automatically: 3D Viewer, Copilot, Cortana, Feedback
Hub, Game Bar, Get Help, Mail and Calendar, Microsoft People, Microsoft
Photos, Solitaire & Casual Games, Snip & Sketch, Sticky Notes, Microsoft
Store, Movies & TV, MSN Weather, Windows Calculator/Camera/Clock/Maps/Media
Player/Voice Recorder, Xbox apps, Mixed Reality Portal, Phone Link, and the
various `Microsoft.VCLibs` / `WindowsAppRuntime` / `Microsoft.UI.Xaml` /
`.NET Native` runtime packages (these are silently pulled in as dependencies
by other Store apps — don't install them individually).

Likewise, the **Visual C++ Redistributables** (2005 through 2015+) listed by
winget are dependencies pulled in automatically by other installers — no need
to track or reinstall them individually.

## 6. Don't forget when migrating (not software, but easy to lose)

- **SSH keys** — `%USERPROFILE%\.ssh\` (used by Git/GitHub, PuTTY if
  converted, WinSCP). Back these up securely (not into a public repo) and
  restore to the same path.
- **GPG keyring** — export with `gpg --export-secret-keys` before wiping this
  machine; GnuPG/Gpg4win won't recreate your keys.
- **Git config** — `git config --global -l` to capture `user.name`,
  `user.email`, aliases, etc.
- **PuTTY saved sessions** — stored in the registry under
  `HKEY_CURRENT_USER\Software\SimonTatham\PuTTY`; export with `regedit` if you
  want them back.
- **AWS CLI credentials** — `%USERPROFILE%\.aws\` (already present on this
  machine at `.aws\`).
- **Zotero library data** — local attachments/PDFs directory (separate from
  the account-synced library metadata).
- **DBeaver connection profiles** — `%APPDATA%\DBeaver\`.
- **PostgreSQL databases** — back up with `pg_dumpall` / `pg_dump`; installing
  PostgreSQL fresh gives you empty databases only.
- **Browser bookmarks/extensions/passwords** — covered by signing into Chrome
  sync / Edge sync on the new machine, but verify before wiping this one.
- **License keys** — Finale, WinZip, and any other paid vendor software above;
  locate the license key/email confirmation before decommissioning this
  machine.
- **VS Code Settings Sync** — turn on Settings Sync (Accounts icon → Turn on
  Settings Sync) *before* moving, so extensions/keybindings/snippets follow
  you automatically.

---

## How this inventory was generated

- `winget list` / `winget export` — packages tracked by Windows Package Manager
  (Section 1, `winget-export.json`)
- `choco list` — Chocolatey-managed packages (Section 2)
- Windows "Programs and Features" / Apps registry entries not covered by
  winget or Chocolatey — cross-referenced manually (Sections 3–4)
- Built-in Store/Windows apps and runtime dependencies — filtered out of the
  actionable list and noted in Section 5 since they require no manual step

To regenerate this list on this machine in the future:
```powershell
winget list --accept-source-agreements
winget export -o winget-export.json --accept-source-agreements
choco list
```
