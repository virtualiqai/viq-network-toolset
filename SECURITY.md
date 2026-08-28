# Security & Privacy

This document describes the security posture, privacy behavior, and data handling of VIQ Network Toolset. We aim to be transparent about every outbound connection the software makes and every file it writes to disk.

The behavior described here applies to both release channels - the [stable channel](https://github.com/virtualiqai/viq-network-toolset) and the [beta channel](https://github.com/virtualiqai/viq-network-toolset-beta). Beta and stable builds are produced from the same source tree by the same CI pipeline and ship with the same encryption, redaction, and rate-limit guarantees. The only difference is maturity.

---

## The Short Version

- **Runs locally.** The application binds its web UI to `127.0.0.1` (loopback) only - nothing on your network can reach it.
- **No account, no cloud.** There is no sign-in, no cloud service, and no data sync.
- **No telemetry.** No analytics, no crash reporting, no usage tracking.
- **Read-only by default** against target devices (SSH Terminal and the optional SCP/SFTP Server are the only operator-initiated exceptions).
- **Credentials stay on your machine.** One-off tool credentials are not persisted and are redacted from the local activity log; any SSH credentials you choose to save are held in an OS-protected, AES-256-GCM encrypted vault - never in plaintext, never in the cloud.
- **Report a vulnerability** privately to **security@virtualiqai.com** - please do not open a public issue.

---

## Beta Channel Caveats

Beta builds carry an additional class of risk that does not apply to stable builds:

- **Functional regressions may be present.** A feature that worked in the previous stable release may not work in the current beta. Keep a known-good stable build available for fallback.
- **Behavior may change between beta iterations.** A `v3.0.0-beta.1` and a `v3.0.0-beta.13` are not guaranteed to be compatible - keyboard shortcuts, UI layout, log formats, and configuration file shapes may shift while we iterate.
- **Beta builds are not intended for production-adjacent use.** Use them on the same workstations and against the same devices as stable builds, but expect to roll back if needed.
- **Beta-specific diagnostics may be enabled.** Some beta builds raise log verbosity, add tracebacks, or expose internal state through `/api/health` that the stable channel does not. Treat the beta `netops.log` as a debugging artifact; do not retain or share it without redacting any device identifiers it may have captured.

The security guarantees listed in the rest of this document - credential redaction, AES-256-GCM source encryption, loopback-only listener, read-only-by-default operations against target devices, no telemetry - apply equally to beta and stable.

---

## Supported Versions

| Channel | Repository | Status |
|-----|------|----|
| **Stable** | [`virtualiqai/viq-network-toolset`](https://github.com/virtualiqai/viq-network-toolset) | ✅ Active - production line |
| **Beta** | [`virtualiqai/viq-network-toolset-beta`](https://github.com/virtualiqai/viq-network-toolset-beta) | ⚠️ Pre-release - for testing only |

Only the most recent build on each channel is considered supported. The beta release page retains up to three recent betas for downgrade comparisons, but security and bug fixes only land in the latest. Always download from the official release page - do not run binaries obtained from third-party mirrors or unofficial sources.

---

## Code Signing & Download Integrity

### Current Status

- **Windows installer** - **not currently code-signed.** Microsoft SmartScreen may display a warning on first launch. See *First-Run Warnings* below.
- **macOS application** - built with conditional Apple Developer ID signing in the CI pipeline. Verification of the published DMG can be performed by running:
  ```bash
  spctl --assess --type execute --verbose /Applications/VIQ\ Engineer\ Toolset.app
  codesign -dv /Applications/VIQ\ Engineer\ Toolset.app
  ```
- **SHA-256 checksums** - **published with every release.** A `SHA256SUMS.txt` file is attached to each GitHub release alongside the DMG and Setup.exe. Verify with:
  ```bash
  # macOS / Linux
  shasum -a 256 -c SHA256SUMS.txt

  # Windows (PowerShell)
  Get-FileHash -Algorithm SHA256 VIQ-Network-Toolset-Setup.exe
  ```

### Roadmap

A future release will include an Authenticode-signed Windows installer and a fully notarized macOS application (Apple Developer ID + notarization stapled to the DMG). Until then, please verify you are downloading directly from the official GitHub release page.

---

## First-Run Warnings

Independent software publishers without long-standing reputation will encounter operating-system security warnings on first launch. This is expected behavior and does not indicate the software is malicious. Detailed step-by-step instructions are in the README under *First-Run Security Warnings*.

---

## Privacy & Telemetry

The application performs **no telemetry, no analytics, no crash reporting, and no usage tracking.** It does not contact any server other than those listed below.

### Always-On Outbound Connections

| When | Destination | Purpose | Data Sent |
|---|-------|-----|------|
| Application startup | `raw.githubusercontent.com/virtualiqai/viq-network-toolset/main/version.json` (stable) **OR** `raw.githubusercontent.com/virtualiqai/viq-network-toolset-beta/main/version.json` (beta), depending on the channel selected on the About page (HTTPS) | Check whether a newer release is available on the active channel | None - only the HTTP request itself. No system information, IP address, machine identifier, or user identifier is transmitted in the request body. Standard HTTP headers are sent by the underlying OS/HTTP library. |

The version check happens once at startup with a short timeout. If the request fails, the application continues normally.

### Opt-In Outbound Connections

These connections only happen when you invoke the specific tool:

| Tool | Destination | Purpose |
|---|-------|-----|
| **IP Info** | `api.ipify.org` (HTTPS) | Detect your public IPv4 address - only triggered if you leave the IP field blank |
| **IP Info** | `ipwho.is` (HTTPS) | Geolocation of an IP address |
| **BGP ASN Lookup** | `rdap.arin.net`, `rdap.db.ripe.net`, `rdap.apnic.net` (HTTPS) | Retrieve ASN ownership and announced prefixes via RDAP |

If you never use these tools, these connections never occur.

### Targeted Device Traffic

All other network traffic - SNMP, SSH, ICMP, DNS, WHOIS, DHCP, RADIUS, and TCP probes - goes directly from your workstation to the device or host you point the tool at. This traffic is not proxied, mirrored, or observed by Virtual IQ AI · USA.

---

## Credential Handling

### One-Off Tool Credentials Are Not Persisted

Credentials entered for a single tool run - **SNMP community strings, SSH passwords or keys for ad-hoc connections, and API/auth tokens** - are **not saved between runs**. They live only in the browser form fields, are passed per-request in the JSON body to the local backend, and are discarded after use.

### Saved SSH Sessions & the Encrypted Credential Vault

The SSH Terminal lets you **optionally save** hosts, groups, and connection settings for reuse. When you choose to save a credential (an SSH/SFTP username and password, or a key passphrase), it is placed in an **encrypted credential vault** - never stored in plaintext:

- **Saved hosts** live in `ssh-store.json` in your local data directory and reference a stored secret only by an **opaque `credential_id`**. This file contains **no plaintext passwords**.
- **The vault** (`vault.enc`) is encrypted with **AES-256-GCM** (a fresh nonce per write). Its master key is protected by your operating system's keystore - the **macOS login Keychain** (via `/usr/bin/security`) or **Windows DPAPI** (`CryptProtectData`, user scope) - or, optionally, by a **passphrase you set** (key derived with **scrypt**) for shared machines or CI.
- Saved secrets are decrypted **only in memory**, resolved **server-side at connect time**, and are **never returned to the browser UI, included in session exports, or written into any report**.
- Everything stays on your machine; no saved credential is ever transmitted to Virtual IQ AI or to any cloud service.

### Activity Log - Automatic Credential Redaction

The application maintains a local audit log of API calls (path, method, status, duration, partial request body up to 2 KB) in `netops_activity.db`. **Request bodies persisted to this log are automatically redacted before insert.**

The redaction logic walks each JSON request body and replaces values for approximately 20 credential-related field names - including `community`, `password`, `private_key`, `privateKey`, `auth_password`, `priv_password`, `snmp_community`, `authKey`, `privKey`, `token`, and others - with the literal string `***REDACTED***`. Matching is case-insensitive on keys and recursive into nested objects and arrays.

In addition, the entire request bodies of **`/api/ssh/*`** and **`/api/filetransfer/user/*`** endpoints are excluded from activity logging entirely as a defense-in-depth measure.

To clear the activity log on demand, open the application, navigate to **Activity Log**, and click **Clear Log**. The same operation is available via the API: `DELETE /api/activity-log`.

---

## Local Data Storage

The application stores the following files on your local machine in a per-platform, user-writable data directory (Windows: `%LOCALAPPDATA%\VIQ Network Toolset\`; macOS: `~/Library/Application Support/VIQ Network Toolset/`; override via `NETOPS_DATA_DIR`):

| File | Purpose | Retention |
|---|-----|------|
| `netops_activity.db` | Local SQLite audit log of every API call (method, path, status, duration, client IP, **redacted** request body up to 2 KB) | Auto-purged after **14 days** |
| `port_mapper.db` | History of SNMP port-mapper scans, by switch (interface tables, MAC tables) | Until manually deleted |
| `ssh-store.json` | Saved SSH hosts, groups, snippets, and settings. Secrets are referenced by `credential_id` only - **no plaintext passwords** | Until manually deleted |
| `vault.enc` | Encrypted credential vault (AES-256-GCM; key in the OS keystore or your passphrase) holding saved SSH/SFTP secrets | Until manually deleted |
| `known_hosts` | SSH host-key fingerprints for hosts you've connected to | Until manually deleted |
| `netops-filetransfer.json` | Configuration for the SCP/SFTP server (only if you start that feature) | Until manually deleted |
| `netops-sftp-host.key*` | SSH host keys used by the SCP/SFTP server (only if you start that feature) | Until manually deleted |
| `netops.log` | Rotating diagnostic log | Bounded by rotation |

None of these files are transmitted off your machine.

---

## SSH Authentication

The SSH Terminal, Switch Health, and Config Audit tools support three authentication modes:

- Username and password
- RSA, Ed25519, ECDSA, or DSS private key (pasted into the UI)
- System SSH fallback (uses your OS's ssh client if paramiko cannot connect)

SSH host keys are stored in a local `known_hosts` file for future verification, following the standard SSH model.

---

## Optional Local-User Authentication

The application supports an optional local username/password gate for the web UI itself (separate from device credentials). This is activated by setting the environment variable `NETOPS_REQUIRE_AUTH=true` or by creating a `netops-users.json` file. Passwords for this local gate are hashed using **PBKDF2-SHA256 with 600,000 iterations** before storage - they are never persisted in plaintext.

By default this feature is **disabled** and the loopback-only listener provides the security boundary.

---

## Read-Only Operations Against Target Devices

The toolset is **read-only by default** with respect to target devices:

- **SNMP** - GET / GETBULK / WALK only. The application never issues SNMP SET.
- **Switch Health** - Uses a strict allowlist of `show` commands. The strings `configure`, `reload`, `clear`, `copy`, `write`, and `erase` are hard-blocked in the command path.
- **Config Audit** - Operates on text you paste into the UI or a read-only configuration retrieved over SSH; it never writes to the device.
- **WLAN tools** - Query the operating system's Wi-Fi interfaces and issue benign, standards-based probes (DHCP, RADIUS reachability, gateway RTT). They do not alter device or AP configuration.

The only ways the toolset writes to a device are:

- **SSH Terminal** - whatever the operator types in the terminal session
- **SCP / SFTP Server** - accepts inbound file pushes initiated by a target device

Both of these are explicitly operator-initiated.

---

## Rate Limiting

The application enforces per-client-IP, per-tool rate limits to prevent the toolset from being weaponized to flood target devices:

| Tool | Rate | Concurrency | Hard Cap |
|---|---|-------|-----|
| SNMP | 6 RPM | 1 | - |
| Sweep | 3 RPM | 1 | 254 hosts |
| Port Scan | 5 RPM | 1 | 100 ports |
| NetDiag | 4 RPM | 1 | - |
| fping | - | - | 20 hosts |

Ten rate-limit violations within a minute trigger a 60-second lockout for that client IP.

---

## Network Listener Posture

The application binds its web server to `127.0.0.1` (loopback) on a high port. **It never accepts connections from any other interface.** Other devices on your network cannot reach the application; only software running on the same machine can reach `localhost`.

The application tries the following ports in order until it finds one free: `5001`, `7421`, `8765`, `9080`, `9090`. If none are available, it falls back to an ephemeral port.

---

## Source Code Protection

The application's Python source code is encrypted at build time using **AES-256-GCM** (via the `cryptography` library) with a randomly generated 32-byte key. The key is embedded in a native loader extension (`.so` on macOS, `.pyd` on Windows). This is intended to discourage casual reverse engineering of the proprietary source; it is not a defense against determined analysis of the compiled binary itself.

---

## Reporting a Vulnerability

If you discover a security vulnerability, please report it privately. **Do not open a public GitHub issue** for security matters.

**Preferred channels:**
- **Email:** security@virtualiqai.com (use the subject line `[SECURITY]`)
- **GitHub:** Open a private security advisory on [the stable repo](https://github.com/virtualiqai/viq-network-toolset/security/advisories/new) or [the beta repo](https://github.com/virtualiqai/viq-network-toolset-beta/security/advisories/new) - whichever channel the issue applies to

**Please include:**
1. A description of the vulnerability
2. Steps to reproduce
3. Affected version(s)
4. Potential impact assessment
5. Your suggested remediation, if any

We aim to acknowledge reports within **72 hours** and to publish a fix or mitigation guidance promptly. Researchers who responsibly disclose will be credited in the changelog (with consent). We ask that you give us a reasonable opportunity to remediate before any public disclosure, and that your testing be limited to systems you are authorized to test.

---

## Operational Security Recommendations

When deploying this toolset in a corporate or sensitive environment:

1. **Run on a dedicated jump host or management workstation.** Don't install it on a general-purpose end-user PC that handles unrelated workloads.
2. **Use scoped credentials.** Create read-only SNMP communities (v2c) or SNMP v3 users with the minimum privileges required.
3. **Prefer SSH keys over passwords** for all device authentication.
4. **Enable disk encryption** on any workstation running the toolset (BitLocker on Windows, FileVault on macOS).
5. **Restrict outbound firewall rules** from the host to only the protocols and destinations you need.
6. **Verify the download** by checking the GitHub release page directly. Do not trust links from email, chat, or third-party blogs.
7. **For regulated environments** (HIPAA, PCI-DSS, SOX, FedRAMP, etc.), evaluate whether this software fits your existing endpoint, vulnerability, and audit policies before deployment. The software is not certified to any compliance framework.

---

*© 2026 Virtual IQ AI · USA. All rights reserved.*
