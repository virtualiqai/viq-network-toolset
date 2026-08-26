# Changelog

All notable changes to **Virtual IQ AI NetOps Toolset** are documented here.
Format follows [Keep a Changelog](https://keepachangelog.com/en/1.0.0/) and adheres to [Semantic Versioning](https://semver.org/).

---

## [3.0.3] — 2026-08-26

Promoted from the `3.0.3-beta.1` and `3.0.3-beta.2` betas. **Everyone on 3.0.2 or
earlier should move to this build** — it carries the 3.0.x security remediation.

> **Upgrading on Windows:** close VIQ Engineer Toolset before running the
> installer. The application locks its own program file while running, and the
> installer will stop with "Error opening file for writing" if it is still open.
> A running-process check is coming in the next release.

### 🔐 Security hardening

This release is the remediation build from the 3.0.x security audit. It tightens
the localhost API boundary, credential handling, file-transfer containment,
subprocess argument safety, report and export rendering, and the build/release
pipeline.

- **Localhost API isolation.** The backend now enforces a loopback `Host`
  allowlist, rejects null-origin requests, and requires CSRF tokens on
  state-changing routes — so a page open in your browser cannot reach the
  toolset's local API. (VIQ-SEC-001, 001b, 002, 013)
- **Credential vault.** Vault unlock attempts are rate-limited, and the update
  download URL is restricted to a known destination. (VIQ-SEC-026, 035)
- **Logging and data at rest.** Request bodies are redacted *before* truncation
  rather than after, the redacted field set was broadened, vault and switch
  bodies are skipped entirely, non-JSON and oversized bodies are never stored
  raw, and the data directory is created `0700`. (VIQ-SEC-008, 022, 023)
- **File-transfer containment.** The SCP/SFTP server resolves symlinks before
  enforcing its root boundary, and inbound connections are throttled against
  brute force. (VIQ-SEC-004, 012)
- **Command and argument safety.** Option-like usernames and `--` separators are
  rejected in the SSH fallback path, and WHOIS targets are validated before use.
  (VIQ-SEC-014, 024)
- **Resource bounds.** Ping-sweep and port-scan ranges are bounded before
  expansion, so a large input can no longer exhaust memory. (VIQ-SEC-011)
- **Reports and exports.** Report print popups use a nonce-based
  Content-Security-Policy with attribute-safe escaping, and the Sweep and Roam
  CSV exporters neutralize spreadsheet formula injection. (VIQ-SEC-003, 010,
  028, 029)
- **Config Audit PDF export.** Changed configuration lines are now redacted
  before they reach the exported report: `enable secret`, `username secret`,
  SNMP communities, TACACS/RADIUS keys, NTP authentication keys, HSRP/VRRP/GLBP
  authentication, IPsec pre-shared keys, and NX-OS `snmp-server user` auth/priv
  values no longer leave your machine in a PDF. The on-screen side-by-side view
  is unchanged.
- **Privacy.** Inter and JetBrains Mono are now vendored locally — the
  application no longer contacts Google Fonts. (VIQ-SEC-009)
- **Windows installer.** The installer's finish page launches the application
  de-elevated instead of inheriting installer privileges. (VIQ-SEC-017)
- **Dependencies and build pipeline.** CVE-flagged dependencies were floored to
  safe versions, CI workflow tokens reduced to least privilege, and all GitHub
  Actions pinned to commit SHAs. (VIQ-SEC-018, 020, 021)

### 🎯 Verdict accuracy — independent network-architect review

- **Nine misreported results corrected** across tools — including SSL grading
  that failed healthy STARTTLS servers on a transient handshake hiccup, AP-uplink
  status, DHCP scope matching, and SockPerf sample counting.
- **Wi-Fi 6E** channel-width reporting is disambiguated, co-channel interference
  is judged **per band**, and an unknown channel width is disclosed instead of
  guessed.
- **Windows tracert** parsing reads all three probes per hop, so partial loss at
  a hop is visible instead of silently dropped.
- **IPv6-only hosts** are reported as "IPv6-only — not tested" rather than
  falsely DOWN.
- **SSL Check** grades an unreachable endpoint **"unknown"** instead of **F** —
  no evidence, no verdict. F still means a real TLS failure.
- **MTR / Trace:** silent hops (routers that answer no trace probes) are no
  longer counted as 100% loss — they are disclosed separately, so "Max Loss"
  reflects only responding hops and agrees with the "Destination reached"
  verdict.
- **MTR per-hop loss on macOS:** a hop that dropped some — but not all — of its
  probes reported 0% loss, and its best/avg/worst ignored every reply after the
  gap. Partial loss is now counted on macOS and Linux exactly as it already was
  on Windows.
- **MTR report header** now states the probes per hop actually sent (3) instead
  of the number requested.

### 🔎 Tools and reports

- **SNMP Port Mapper:** the Errors column now shows an error **rate**
  (errors/min between your last two scans) instead of the cumulative
  since-boot total, so a port repaired months ago no longer reads as degraded
  forever. Counter wrap and device reboots are detected (shown as
  reset/baseline, never fabricated). The lifetime total remains in the cell
  tooltip. *The first scan after upgrading establishes the baseline.*
- **Switch Health:** modular chassis fan trays (Catalyst 9400/9600 "Fantray")
  are now detected — these chassis previously reported "no fans".
- **SSH session import:** the tool tiles (SecureCRT, PuTTY, MobaXterm, ~/.ssh,
  CSV, JSON) are clickable and pre-filter the file picker to that tool's format.
- **WHOIS** now honors its "Domain or IP" label: IP lookups parse registry
  answers (Net Range, CIDR, Organization, Abuse Contact, …) and a Raw WHOIS
  Excerpt panel was added.
- **ASN Lookup** accepts real BGP ASNs only — asplain (`13335`) or asdot
  (`1.2`), 2-byte and 4-byte ranges — with clear guidance on invalid input.
- **Ping report** now includes the Latency Over Time graph the live view
  already showed.
- **Report pagination:** sections no longer strand a near-blank page before
  their content in printed/PDF reports.

### Fixed

- The macOS app bundle stamps its real version (Finder previously showed
  `0.0.0`; the running app was unaffected).
- The in-app **User Guide** stamps its version from the build and defaults its
  channel to match the running build, instead of showing a stale value.

## [3.0.2] — 2026-08-04

### 🔎 BGP ASN Lookup — Registry-Neutral Data Source

- ASN lookups now query the RIPE NCC's official public **RIPEstat** API
  (`stat.ripe.net`) for holder, registry, country, and announced-prefix
  counts, replacing a third-party security-tool API. Results were validated
  against independent sources across all five regional registries (ARIN,
  RIPE NCC, APNIC, LACNIC, AFRINIC); org and abuse contacts continue to come
  from official registry RDAP.

## [3.0.1] — 2026-08-04

### 🎯 Measurement Accuracy — Internet Diagnostic Engine v2.2

- Throughput now uses the same trimmed-mean methodology as leading speed-test
  services, with adaptive test windows and probe-all/pick-fastest server
  selection for both download and upload. Results validated side-by-side
  against Ookla on the same links (download within ~0.1%, upload within ~2%
  against an adjacent on-net server).
- Real-time call-conditions verdicts (VoIP / video / remote desktop) come from
  a TCP connect-RTT train instead of idle ICMP probes. This eliminates the
  false POOR verdicts caused by Wi-Fi power-save wake latency — the report now
  names that artifact explicitly when it is detected.
- Gauges stream live samples from the first seconds of the test; a Stop button
  was added to Ping Sweep with partial results.

### 🩺 Honesty & Windows Fixes

- **WLAN on Wi-Fi-less Windows machines:** Investigator and Wi-Fi Snapshot now
  report "Wi-Fi cannot be assessed" (adapter absent or the WLAN AutoConfig
  service stopped) instead of a false HEALTHY verdict scored from Ethernet.
- **User Guide on Windows:** the guide is now bundled in the installer — the
  Help button and nav item no longer 404.
- **Port Map:** trunk/access Mode is now read from the correct SNMP trunking
  status (it was inverted on IOS/IOS-XE switches whose trunks use a hardened
  native VLAN); sessions left idle for hours self-heal their security token.
- Every tool page shows a **"Collected \<date time\>"** stamp so results
  reopened later are never mistaken for fresh data.

## [3.0.0] — 2026-08-01

### 🚀 Major Release

- **WLAN Investigator suite:** one-button client-to-WAN Wi-Fi investigation
  with a 9-layer verdict ladder, roaming logger, DHCP/RADIUS probes, AP
  uplink checks, RF reference, and case history with exportable reports.
- **Verdict-led PDF reporting** across every tool, with a standardized layout,
  report IDs, and confidential markings.
- **SSH terminal makeover:** profile grid with import from common clients,
  tabbed sessions, and an encrypted credential vault.
- **Internet Diagnostic overhaul**, standards-based jitter analysis (RFC 5481
  PDV, ITU-T Y.1541 classes), and a full field-test hardening round on
  Windows and macOS.
- Full Windows CI test gate: the complete backend + UI suite now runs on
  Windows before any installer is published.

## [2.6.20] — 2026-05-22

### 🔌 Windows SCP Push From Cisco IOS Now Works End-to-End

Cisco devices running `copy running-config scp:` to a Windows VIQ Toolset host previously failed with `%Error writing scp://… (Unknown error -1)` while the same client succeeded against a macOS host. The root cause was a silent Kerberos-setup failure: asyncssh constructs a `GSSServer` on every connection by default, which on Windows pulls in `sspi.ServerAuth('Kerberos', …)` → `sspi.py` lazy-imports `win32timezone` (a `pywin32` submodule that PyInstaller's static analysis did not detect because nothing in our code imports it directly). The `ModuleNotFoundError` escaped the connection factory, asyncio logged it, and the SSH handshake aborted before a clean error could be sent to the client — leaving Cisco IOS with no specific SCP error to report and rendering the generic "Unknown error -1."

The fix is platform-neutral and surgical: the asyncssh listener now passes `gss_host=None`, which makes the `if options.gss_host:` guard in asyncssh's connection setup short-circuit and skip GSSAPI/Kerberos entirely. No Kerberos code path runs on any platform. The SFTP server only ever offered password and pubkey authentication, so no functionality is lost — and the same change protects Linux hosts where the equivalent failure mode would have been `python-gssapi`/`libkrb5` not being installed.

### ✨ UI — Glow Survives `prefers-reduced-motion`

Windows 11's Accessibility → Visual Effects → "Animation effects" setting (often Off on managed installs) makes Chrome/Edge advertise `prefers-reduced-motion: reduce`. Our existing media query honors that by stripping every `animation:` rule — but the brand mark (top-left of the header) and the About page avatar derived their entire sapphire glow from the `kGlow` keyframe's `box-shadow`, with no base shadow on the static class. Animation off therefore meant glow off.

A static `box-shadow` fallback now lives inside the same media query so the brand identity stays visible even when animations are disabled by the OS.

---

## [2.6.19] — 2026-05-22

### 🪵 `netops.log` Now Lands Where Users Can Find It

The previous build's diagnostic logging never produced a file on Windows because the rotating file handler was writing into `%PROGRAMFILES%\VIQ Engineer Toolset\assets\` — an admin-only path. Python's `RotatingFileHandler` silently swallows `PermissionError` on first open, so the log appeared to be working with no on-disk evidence to debug from.

v2.6.19 routes the log (and future runtime state) through a per-platform user-writable data directory:

- **Windows:** `%LOCALAPPDATA%\VIQ Engineer Toolset\netops.log`
- **macOS:** `~/Library/Application Support/VIQ Engineer Toolset/netops.log`
- **Linux:** `$XDG_DATA_HOME/viq-engineer-toolset/netops.log` (default `~/.local/share/…`)
- Override available via the `NETOPS_DATA_DIR` environment variable.
- Falls back to the original install-bundle path if the user dir can't be created.

The resolved log path is now emitted on startup and surfaced through `/api/health` alongside the existing `data_dir` and `base_dir` so it's discoverable from the UI. The diagnostic wrappers around the SCP file path (`open` / `seek` / `read` / `write` / `close`) added in v2.6.18 finally have somewhere to write to.

### ✨ UI — Glow Survives `prefers-reduced-motion: reduce`

Windows 11's Accessibility → Visual Effects → "Animation effects" setting (often Off on managed installs) makes Chrome/Edge advertise `prefers-reduced-motion: reduce`. The toolset's media query honors that by stripping every `animation:` rule — but the brand mark (header top-left) and the About page avatar derived their entire sapphire glow from the `kGlow` keyframe's `box-shadow`, with no base shadow on the static class. Animation off therefore meant glow off.

A static `box-shadow` fallback now lives inside the same media query so the brand identity stays visible even when animations are disabled by the OS.

---

## [2.6.18] — 2026-05-22

### 🩺 Diagnostic Build — Windows SCP Write Path

A targeted observability release for an open issue where Cisco IOS `copy running-config scp:` to a Windows VIQ Toolset host fails with `%Error writing scp://… (Unknown error -1)` even though the same client uploads successfully to a macOS VIQ host. asyncssh's SCP sink catches only `(OSError, SFTPError)` — any other exception propagates as a generic fatal error and Cisco renders it as `Unknown error -1`, with no server-side trace.

This build adds:

- `asyncssh` logger raised to **INFO** at startup so its SCP/SFTP events flow into the existing `netops.log` rotating file at the install root.
- Explicit `log.exception()` wrappers around the SFTP server's `open()` and the tracked-transfer file's `seek` / `read` / `write` / `close` so a full traceback (with `local_path`, `flags`, `pflags`, `parent_writable`, `bytes_so_far`) lands in `netops.log` regardless of which step throws.
- A new explicit `seek()` method on the tracked file wrapper — previously `seek` slipped through `__getattr__`, hiding any seek-stage failure from the existing error-marking path.

No behavior changes for users whose SCP/SFTP already works. After reproducing the Cisco error on Windows, send the most recent `netops.log` from `%PROGRAMFILES%\VIQ Engineer Toolset\netops.log` for diagnosis.

---

## [2.6.17] — 2026-05-21

### 🔧 SSH Stack Restored for Legacy Network Gear

Older Cisco, Arista, Juniper, Huawei, and MikroTik devices became unreachable in v2.6.16 because the upstream `paramiko` 4.x line removed the `DSSKey` class and tightened the default preferred-algorithm lists (ssh-rsa, ssh-dss, SHA-1 DH KEX). v2.6.17 pins `paramiko>=3.4,<4` so legacy host-key types, KEX algorithms, and ciphers are back in the default negotiation set without monkey-patching. SSH Terminal, Switch Health, and Config Audit now work against the same hardware they did in v2.6.1.

### 🧪 SSH Compatibility Test Suite

A new `tests/test_ssh_tools.py` pytest suite spins up an in-process paramiko mock SSH server (RSA + Ed25519 host keys) and exercises `_ssh_open_temp_shell`, `api_ssh_connect`, and the `/api/health` dependency diagnostic end-to-end. CI now runs this suite on both macOS and Windows runners *before* the PyInstaller build — if SSH is broken on either platform, the release is blocked.

### 🩺 `/api/health` Dependency Diagnostic

The health endpoint now reports a `dependencies` map indicating which Python libraries loaded successfully (`paramiko`, `asyncssh`, `cryptography`, `pysnmp`, …) and which feature blocks are affected when any are missing. SSH-related errors are also enriched with the offered algorithm lists from the failing handshake, making "Incompatible ssh peer" failures actionable instead of opaque.

---

## [2.6.16] — 2026-05-22

### Highlights

This release consolidates ~15 iterative beta builds (v2.6.2 through v2.6.16) into the new production line. The headline improvements are in **SCP/SFTP**, **Config Audit**, and **Switch Health**, plus a number of UX and reliability fixes the user community asked for.

### 🔐 SCP/SFTP Server

- **Port-22 forwarding via `pfctl` rdr** on macOS. The embedded SCP/SFTP listener still binds an unprivileged port (default 2222) so no part of the toolset runs as root continuously. Clicking "Install Port Forwarding" pops a single macOS admin-password prompt, after which a pf rdr rule redirects external port-22 traffic to the listener — letting network devices push to the standard SCP port without disabling macOS Remote Login, and without `sudo`-launching the whole app. Rule targets the live interface IP (via the pf `(iface)` macro) so packets stay routable; an earlier 127.0.0.1 redirect hit BSD's "martian packet" anti-spoofing drop. Removal is a single click that restores `/etc/pf.conf`.
- **Listener Host dropdown** — a Wireshark-style interface picker. The SCP/SFTP page enumerates every live IPv4 interface (Wi-Fi, Ethernet, USB tether, VPN tunnel, bridge) and lets you pick which one(s) to bind on. Port forwarding rules are installed only on the selected interface, so an active corporate VPN tunnel isn't accidentally exposed.
- **Native folder picker** for the Root Directory field (`📂 Browse…` button opens macOS's standard Finder folder-selection dialog). Plus a "Quick pick" dropdown for common paths (`~/Documents/VIQ-SFTP-Root`, `~/Downloads`, etc.). The text input still accepts any absolute path manually.
- **Default Root Directory** moved from inside the .app bundle to `~/Documents/VIQ-SFTP-Root` so files survive reinstalls and aren't hidden inside the application package.
- "Forwarding port 22 → 2222 on: en0 (10.93.139.213)" status line surfaces which interface is actually forwarded, so you don't have to drop to `sudo pfctl -s nat` to check.

### 🔎 Config Audit

- **Side-by-side aligned diff** replaces the bottom unified-diff dump. Startup config left, running config right, scroll-synced, with green/red/amber row colors and word-level highlighting on changed lines. New "Added / Removed / Changed" tile counts.
- **Whitespace-agnostic comparison** — config lines are normalized for the diff alignment (collapsed internal whitespace, stripped trailing whitespace) but the original device formatting is preserved in the display panes. False-positive diffs from pager artifacts and spacing inconsistencies are gone.
- **`--More--` pager handling** — when a Cisco device pauses output with `--More--`, the SSH read loop now sends a space character to advance. Earlier versions only stripped the literal text and let the device sit at the pager indefinitely, so `show running-config` returned ~60 lines (one PTY page) instead of the full config.
- **Boilerplate filtering** — lines like `Building configuration…`, timestamp comments, and bare `!` separators are excluded from the alignment so the diff focuses on real configuration drift.
- **Drift Status tile** shows the running count of differences (`14 diffs` instead of the previous "Difference Found" that didn't fit).

### 📡 Switch Health

- **SSE streaming with a 7-phase progress stepper** — connect, prep, version & inventory, CPU & resources, power · fan · temperature, interface errors, score health. Each phase reports completion text as it happens instead of one big spinner-and-wait.
- **Cancel button** appears on running operations; converts to "Dismiss" after failure so the post-failure stepper can be cleared.

### 🛡️ SSH connectivity

- **Legacy host-key + KEX algorithms** re-enabled for older Cisco / Arista / MikroTik / Huawei gear (`ssh-rsa`, `ssh-dss`, `diffie-hellman-group14-sha1`, group1-sha1, group-exchange-sha1, `aes128-cbc`, `3des-cbc`, `hmac-sha1`). Paramiko 3.x disables these by default, which produced "Incompatible ssh peer (no acceptable host key)" against the kind of equipment most engineers actually have to work with.
- Same legacy treatment applied to the system-`ssh` fallback path via `HostKeyAlgorithms=+ssh-rsa,ssh-dss`, `KexAlgorithms=+...`, `Ciphers=+aes128-cbc,3des-cbc`.

### 🪟 Windows

- **PyInstaller spec now uses `collect_all()`** for `paramiko`, `cryptography`, `cffi`, `bcrypt`, `nacl`, `asyncssh`, `pysnmp`, `pyasn1` — pulls in the full native-extension and DLL trees that the static heuristic analysis was missing. Earlier Windows builds silently shipped without working SSH; v2.6.16 fixes Config Audit, Switch Health, and SSH Terminal on Windows.

### 🔔 Error log + diagnostics

- **Bell icon in the header** opens a session-scoped error log panel showing recent errors with timestamps, sources (`SCP/SFTP`, `Switch Health`, `Config Audit`, `About`, etc.), and detail strings. Red badge with the unread count. Clear button at the top of the panel.
- **`/api/health` exposes a dependency map** showing which optional native deps loaded successfully and what features each blocks if it fails. Future Windows-vs-macOS bugs diagnose in 5 seconds.

### ℹ️ About / Version

- **GitHub link** in the Developer card now actually points at the public release repo.
- **"Check for Updates"** now re-fetches the upstream `version.json` on each click instead of returning a startup-cached value. Uses proper numeric version comparison (so `2.6.10 > 2.6.9` is correctly recognized as an update — string compare previously got it wrong because `'9' > '1'` as characters).
- About page shows "Last Checked" timestamp and any check error inline.

### 🔧 CI / build pipeline

- **Workflow refactored to upload directly to `viq_eng_toolset`** — no intermediate GitHub Actions artifact storage. The free-tier 500 MB artifact quota was exhausted during the v2.6.x rapid-iteration cycle, blocking releases until the cache recalculated. The new pipeline has four jobs (`pre-release` → `build-macos` / `build-windows` in parallel → `finalize`) and is immune to the quota cliff.
- **SHA-256 checksums** are now generated in the `finalize` step from the actual uploaded artifacts (not intermediates), so they always match what users actually download.

### 🐛 Bug fixes / polish

- Stepper UI no longer auto-hides on failure; cancel/dismiss button persists so users can read the error before clearing.
- Per-tool error attribution in the bell log (errors show as "SCP/SFTP" or "Switch Health" instead of generic "Toolset").
- Activity-log middleware redacts credentials (community strings, passwords, SSH keys) before persistence — no-op on a public release but the protection has been there since v2.4.2 and applies to this release.

---

## [2.6.1] — 2026-05-21

### Brand Identity v1.0

This release introduces the official Virtual IQ AI brand identity across the application and repository.

- **New app icon.** The Windows installer, macOS DMG, taskbar/Dock icon, and system-tray glyph all use the new IQ ligature mark.
- **In-app header brand.** The dashboard header now displays the IQ mark inside a small dark card with the same pulsing sapphire glow used on the About avatar, alongside the "Virtual IQ AI" wordmark and "by Muhammad Kashif (KASH)" author attribution.
- **About / Version page.** Developer card now uses the brand mark in place of the previous lettermark; About copy updated to "Virtual IQ AI · USA".
- **Web favicons.** Full favicon set added — SVG, ICO multi-res, 16/32/192/512 PNGs, and Apple touch icon — so browser tabs and bookmarks reflect the new brand.
- **Theme color.** Added `theme-color` meta tag (`#0A0E1A`) so mobile and desktop chrome blend with the dark UI.
- **README banner.** GitHub repo home page now leads with the production brand banner instead of the placeholder Shields.io badge.

No functional changes to any tool. All read-only safety guarantees, rate limits, credential redaction, and SHA-256 checksum publishing carry over unchanged from v2.5.x.

---

## [2.5.1] — 2026-05-21

### UI Polish

- **Header attribution.** Split the dim single-line "Built by KASH" into three legible stacked lines so the author name is fully visible. Brand color and developer card refreshed with full bio and certifications (CCNP Enterprise / Data Center / Security, Microsoft AZ-700).
- Patch over v2.5.0 — no functional or security changes.

---

## [2.5.0] — 2026-05-21

### Verifiable Release Artifacts

This release introduces published download integrity verification.

- **SHA-256 checksums published with every release.** A `SHA256SUMS.txt` file is now attached to each GitHub release alongside the DMG and Setup.exe. Users can verify the integrity of downloaded artifacts before installation:
  - macOS / Linux: `shasum -a 256 -c SHA256SUMS.txt`
  - Windows: `Get-FileHash -Algorithm SHA256 VIQ-Engineer-Toolset-Setup.exe`
- All other functionality unchanged from v2.4.2.

---

## [2.4.2] — 2026-05-21

### Initial Public Release

This is the first publicly available release of Virtual IQ AI NetOps Toolset. The application has been under continuous development against production enterprise infrastructure prior to this public availability. Version 2.4.2 reflects the application's internal release history; the product debuts publicly at this version.

### 🚀 Application Platform

- Cross-platform desktop application packaged as native installers for **Windows (NSIS)** and **macOS (DMG)**
- Bundled **Python 3.13** runtime — no separate Python installation required
- **Flask + flask-cors** backend bound to loopback only (`127.0.0.1`); never exposed externally
- Automatic port selection: tries `5001`, `7421`, `8765`, `9080`, `9090` in order, with ephemeral fallback
- Automatic browser launch on startup
- **Windows system-tray icon** with Quit menu (pystray)
- **Graceful shutdown** via in-app Shutdown button, system-tray menu, or SIGINT/SIGTERM
- **AES-256-GCM source-code protection** with native loader extension
- **Per-IP, per-tool rate limiting** with hard host and port caps to prevent accidental overrun:
  - SNMP: 6 rpm / 1 concurrent
  - Sweep: 3 rpm / 1 concurrent, max 254 hosts
  - Port scan: 5 rpm / 1 concurrent, max 100 ports
  - NetDiag: 4 rpm / 1 concurrent
  - fping: max 20 hosts
- **Loopback-only API** with optional PBKDF2-SHA256 (600k iterations) local-user authentication

### 🔐 Privacy & Credential Protection

- **No telemetry, no analytics, no crash reporting, no usage tracking**
- Only outbound HTTPS request on startup is the GitHub version check
- Third-party lookups (IP Info, BGP ASN) are strictly opt-in per tool invocation
- **Device credentials are never persisted between runs** — no master credential vault exists
- **Activity log automatically redacts credential fields before insert.** A walker traverses each JSON request body and replaces values for approximately 20 credential-related field names (SNMP communities, SSH passwords, private keys, auth tokens) with `***REDACTED***`. Matching is case-insensitive and recursive into nested objects/arrays
- **`/api/ssh/*` and `/api/filetransfer/user/*` request bodies are excluded from activity logging entirely** as defense in depth
- Six unit tests cover the redaction logic across SNMP, password, private-key, nested-object, non-JSON, empty, and case-insensitivity scenarios

### 🔍 Discovery Tools

- **SNMP Port Mapper** — Walks IF-MIB, BRIDGE-MIB, Q-BRIDGE-MIB, IP-MIB, LLDP-MIB, and CDP-MIB; correlates port-to-MAC-to-IP-to-neighbor mappings with optional L3 router correlation
- **Ping Sweep** — ICMP sweep across a CIDR
- **Nmap Scanner** — Wraps the locally installed nmap binary
- **DNS Lookup** — Full record-type support (A, AAAA, CNAME, MX, NS, TXT, SOA, PTR) via dnspython
- **WHOIS** — Domain and IP WHOIS lookups against registrar whois servers
- **BGP ASN Lookup** — ASN ownership and announced prefixes via RDAP (ARIN/RIPE/APNIC)
- **MAC Address Lookup** — Offline OUI vendor identification

### 🛠️ Troubleshooting Tools

- **NetDiag Report** — Streaming combined diagnostic bundle (DNS + ping + MTR + port scan + MSS probe + SSL) over Server-Sent Events for a single host
- **Ping** — Single-host ICMP probe
- **MTR / Traceroute** — Hop-by-hop latency and loss analysis
- **fping** — Parallel multi-host ICMP
- **Port Scanner** — TCP connect with optional banner grab
- **Switch Health** — Vendor-neutral CPU/temperature/fan/PSU/uptime parsing over SSH. **Strict allowlist** enforced in code — blocks `configure`, `reload`, `clear`, `copy`, `write`, `erase`, and other write commands

### 🔒 Security Tools

- **SSL Inspector** — TLS certificate chain, validity, ciphers, and SAN inspection
- **Config Audit** — Local-only static audit of pasted device configurations for weak SNMP, telnet, default credentials, and missing logging
- **Ncat / Netcat** — Command builder and reference UI
- **SSH Terminal** — In-browser xterm.js terminal session via paramiko (with system-ssh fallback). Supports password, RSA, Ed25519, ECDSA, and DSS authentication
- **SCP / SFTP Server** — Local SFTP/SCP server (asyncssh) for inbound configuration pushes from devices

### 📊 Performance Tools

- **SockPerf** — TCP/UDP latency and throughput probe with built-in listener mode
- **Bandwidth Calculator** — Throughput, transfer-time, and utilization math
- **MSS Calculator** — Dual-probe path MTU/MSS (jumbo 9000 + standard 1500, DF-bit) with jumbo/standard/tunnel/fragmented classification

### 🧰 Utilities

- **Subnet Calculator** — CIDR, broadcast, usable range, wildcard mask
- **IP Converter** — Base and format conversions for IPv4 and IPv6
- **IP Info** — Public IPv4 autodetect, geolocation, and PTR resolution
- **Wake-on-LAN** — Magic packet sender (UDP broadcast or unicast)

### 📚 Reference

- **Port Reference** — Offline searchable port-to-service mapping
- **CIDR Table** — Offline CIDR-to-netmask reference

### 🧪 Developer Tools

- **Activity Log** — Local SQLite audit log of every API call with 14-day auto-purge and automatic credential redaction
- **Wishlist** — Local feature-request submission
- **About / Version** — Build info and update check

### ⚠️ Known Limitations

- Windows installer is not yet code-signed; SmartScreen may warn on first launch
- macOS DMG signing depends on CI build secrets; verify with `spctl --assess` on your downloaded DMG
- No SHA-256 checksums published yet
- No Linux build available
- Switch Health output parser supports Cisco IOS / IOS-XE / NX-OS syntax only
- Nmap, MTR, and fping tools require the corresponding binary installed locally — they are not bundled
- IPv6 end-to-end support is partial — host validators accept IPv6 but several tools assume IPv4 paths
- Banner-grab in Port Scanner may trigger IDS/IPS on hardened networks

---

## Upcoming Releases

Tracked, in no particular order:

- **NetAI Platform** *(Q4 2026)* — Flagship AI-augmented network operations platform from Virtual IQ AI. Predictive incident detection, autonomous Level-1 alert triage, and natural-language network querying. Currently in active development.
- **Signed Windows installer** (Authenticode)
- **Notarized macOS application** (Apple Developer ID + notarization)
- **Encrypted credential vault** for frequently accessed targets
- **Linux build** (deb / AppImage)
- **Multi-vendor Switch Health** parsers (Arista EOS, Juniper Junos, PAN-OS)
- **Full IPv6 parity** across all tools
- **Light/dark theme toggle** (currently dark only)

---

*© 2026 Virtual IQ AI LLC. All rights reserved.*
