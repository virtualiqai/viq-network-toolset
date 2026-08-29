# Third-Party Notices & Open-Source Component Acknowledgments

**Product:** VIQ Network Toolset
**Publisher:** Virtual IQ AI · USA

VIQ Network Toolset is a free, proprietary desktop application that **bundles** open-source software, publicly available data, and third-party brand icons. Each bundled component is used under the terms of its own license, summarized below. Where a license requires the full license text, that text is included with the distributed application and is also available from the upstream project. **For any component, the original upstream license text takes precedence over the summary in this document.**

Versions below reflect the components actually bundled in the shipped (frozen) build unless marked as a build/declared dependency. If you believe a component is missing or misattributed, contact **info@virtualiqai.com** (subject `[LICENSE ATTRIBUTION]`).

---

## 1. Bundled runtime libraries (Python)

| Component | Version | License | Copyright / Attribution | Upstream |
|------|-----|-----|-------------|-----|
| **Flask** | 3.1.3 | BSD-3-Clause | © Pallets | https://palletsprojects.com/p/flask/ |
| **Werkzeug** | 3.1.8 | BSD-3-Clause | © Pallets | https://palletsprojects.com/p/werkzeug/ |
| **Jinja2** | 3.1.6 | BSD-3-Clause | © Pallets | https://palletsprojects.com/p/jinja/ |
| **MarkupSafe** | 3.0.3 | BSD-3-Clause | © Pallets | https://palletsprojects.com/p/markupsafe/ |
| **Click** | 8.3.2 | BSD-3-Clause | © Pallets | https://palletsprojects.com/p/click/ |
| **itsdangerous** | 2.2.0 | BSD-3-Clause | © Pallets | https://palletsprojects.com/p/itsdangerous/ |
| **blinker** | 1.9.0 | MIT | © Jason Kirtland & contributors | https://github.com/pallets-eco/blinker |
| **Flask-Cors** | 6.0.2 | MIT | © Cory Dolphin | https://github.com/corydolphin/flask-cors |
| **pysnmp** | 7.1.27 | BSD-2-Clause | © LeXtudio Inc.; original © Ilya Etingof | https://github.com/lextudio/pysnmp |
| **pyasn1** | 0.6.3 | BSD-2-Clause | © Ilya Etingof & pyasn1 contributors | https://github.com/pyasn1/pyasn1 |
| **psutil** | 7.2.2 | BSD-3-Clause | © Giampaolo Rodolà | https://github.com/giampaolo/psutil |
| **dnspython** | 2.8.0 | ISC | © Bob Halley & dnspython contributors | https://www.dnspython.org/ |
| **requests** | 2.33.1 | Apache-2.0 | © Kenneth Reitz / Python Software Foundation | https://github.com/psf/requests |
| **certifi** | 2026.2.25 | MPL-2.0 | © Kenneth Reitz | https://github.com/certifi/python-certifi |
| **charset-normalizer** | 3.4.7 | MIT | © Ahmed R. TAHRI | https://github.com/jawah/charset_normalizer |
| **idna** | 3.11 | BSD-3-Clause | © Kim Davies & contributors | https://github.com/kjd/idna |
| **urllib3** | 2.7.0 | MIT | © Andrey Petrov & contributors | https://github.com/urllib3/urllib3 |
| **paramiko** | 3.5.1 | LGPL-2.1 | © Jeff Forcier & contributors | https://www.paramiko.org/ |
| **bcrypt** | 5.0.0 | Apache-2.0 | © The Python Cryptographic Authority (PyCA) | https://github.com/pyca/bcrypt |
| **PyNaCl** | 1.6.2 | Apache-2.0 | © The PyNaCl developers (PyCA) | https://github.com/pyca/pynacl |
| **cryptography** | 50.0.1 | Apache-2.0 **OR** BSD-3-Clause (dual; recipient may choose) | © The Python Cryptographic Authority (PyCA) | https://cryptography.io/ |
| **cffi** *(native `_cffi_backend`)* | bundled | MIT | © Armin Rigo, Maciej Fijałkowski & contributors | https://github.com/python-cffi/cffi |
| **asyncssh** | 2.24.0 | EPL-2.0 **OR** GPL-2.0-or-later (dual) - **used under EPL-2.0 election** | © Ron Frederick | https://github.com/ronf/asyncssh |
| **openpyxl** | 3.1.5 | MIT | © openpyxl authors (Eric Gazoni, Charlie Clark) | https://openpyxl.readthedocs.io/ |
| **et_xmlfile** | 2.0.0 | MIT | © openpyxl authors | https://foss.heptapod.net/openpyxl/et_xmlfile |
| **Pillow (PIL)** | 12.3.0 | MIT-CMU / HPND | © Jeffrey A. Clark & contributors; original PIL © 1997-2011 Secret Labs AB / Fredrik Lundh | https://python-pillow.org/ |
| **defusedxml** | 0.7.1 | PSF-2.0 (Python Software Foundation License) | © Christian Heimes | https://github.com/tiran/defusedxml |
| **truststore** | 0.10.4 | MIT | © Seth Michael Larson | https://github.com/sethmlarson/truststore |
| **importlib_metadata** | 8.7.1 | Apache-2.0 | © Jason R. Coombs | https://github.com/python/importlib_metadata |
| **invoke** | 3.0.3 | BSD-2-Clause | © Jeff Forcier & contributors | https://www.pyinvoke.org/ |
| **pyobjc-core + pyobjc-framework-Cocoa / -CoreLocation / -CoreWLAN** *(macOS only)* | 12.2.1 | MIT | © Ronald Oussoren & contributors | https://github.com/ronaldoussoren/pyobjc |
| **pystray** *(declared dependency - verify per-platform bundling †)* | ≥ 0.19 | LGPL-3.0 | © Moses Palmér | https://github.com/moses-palmer/pystray |
| **pywin32** *(Windows only - build/runtime dependency)* | build dep | PSF-2.0 | © Mark Hammond & contributors | https://github.com/mhammond/pywin32 |

**† pystray note:** declared in `requirements.txt` and listed as a PyInstaller hidden import, but a `pystray` package directory was not observed at the top level of the current frozen macOS bundle (the macOS tray may be served directly via PyObjC). Confirm whether pystray is actually linked into each platform's build; keep the LGPL-3.0 entry and the replacement notice below only for the platforms where it ships.

---

## 2. Front-end libraries (vendored, served to the local browser UI)

Located under `assets/vendor/xterm/`.

| Component | Version | License | Copyright / Attribution | Upstream |
|------|-----|-----|-------------|-----|
| **xterm.js** (`xterm.js`, `xterm.css`) | *verify - not embedded in the minified build* | MIT | © The xterm.js authors; portions © 2012-2013 Christopher Jeffrey (term.js) | https://xtermjs.org/ |
| **xterm addon: search** (`xterm-addon-search.js`) | *verify* | MIT | © The xterm.js authors | https://github.com/xtermjs/xterm.js |

> Record the exact bundled xterm.js version (it is not exposed in the minified asset). The bundled `xterm.css` header confirms: *"Copyright (c) 2014 The xterm.js authors. All rights reserved. Copyright (c) 2012-2013, Christopher Jeffrey (MIT License). @license MIT."*

---

## 3. Bundled data

| Component | Snapshot | Terms | Attribution | Source |
|------|-----|----|-------|----|
| **IEEE OUI / MA-L registry** (`oui.tsv.gz`) | 2026-07-28 | Publicly available for download from the IEEE Registration Authority; redistributed for MAC-vendor lookup. IEEE requests attribution and that the data not be represented as an IEEE-endorsed product. | IEEE Registration Authority | https://standards-oui.ieee.org/ |

See *Notices* below for the IEEE attribution statement.

---

## 4. Icon & brand-mark assets

### 4a. Vendored icon sets (`assets/vendor/`)

| Asset group | Source | License | Attribution / Notes |
|-------|----|-----|-----------|
| `vendor/logos/*.svg` - 16 cloud-service brand marks (Cloudflare, GitHub, Amazon AWS, Instagram, Zoom, Microsoft Teams, Facebook, Google, Microsoft 365, YouTube, Cisco Webex, Okta, Slack, Netflix, iCloud, TikTok) | **Simple Icons** | **CC0-1.0** (the SVG icons) | Icons are CC0 (public-domain dedication - no attribution required). **The depicted marks are trademarks of their respective owners; inclusion does not imply endorsement.** See *Notices*. https://simpleicons.org/ |
| `vendor/emoji/*.svg` - standard-emoji UI glyphs (fire, package, globe, satellite, plug, clock, shield, check, warning, info, lock/locked-key, signal, search, alarm, no-entry, etc.) | **Microsoft Fluent Emoji** *(strongly indicated - verify)* | **MIT** | © Microsoft. The "fire" glyph and colorway (`#FF6723`, 32×32) match Microsoft Fluent Emoji. https://github.com/microsoft/fluentui-emoji |
| `vendor/emoji/*.svg` - tool-specific glyphs (e.g. `snmp-mapper`, `port-scan`, `bgp-asn`, `netdiag`, `switch-health`, `sockperf`, `mss-calc`, `config-audit`, `wake-on-lan`, `traceroute`, `ping-sweep`, `subnet-calc`, `ip-converter`) | Appear **VIQ-original / derived** - **verify** | - | These have no direct emoji analogue; confirm whether any are derived from a third-party set before treating as first-party. |
| `vendor/favicons/*` | First-party | - | © Virtual IQ AI · USA (product branding). |

### 4b. SSH-import source icons (embedded as `data:` URIs in `engineer-toolset.html`, `SSH_IMPORT_TOOLS`)

Six icons identify the import sources in the "Import SSH sessions" modal:

| # | In-app label | Depicts | Source | License | Obligation |
|--|-------|-----|----|-----|------|
| 1 | `~/.ssh` | "Unofficial SSH Logo" | Wikimedia Commons - *Unofficial SSH Logo.svg* by **Jessie Kirk** (2022) | **CC BY 4.0** | **Attribution required** (see *Notices*). Community/unofficial mark - not the OpenSSH project's official logo. |
| 2 | `CSV` | Spreadsheet / CSV file | **Virtual IQ AI · USA** (first-party, VIQ-original) | First-party - no third-party license | None. Original glyph drawn for this product; no third-party obligation. |
| 3 | `PuTTY` | PuTTY application logo | **homarr-labs/dashboard-icons** | **Apache-2.0** (icon repo) | Retain Apache-2.0 license + NOTICE attribution. PuTTY software is MIT (© Simon Tatham); the **PuTTY name/logo is a trademark of Simon Tatham** - nominative use. |
| 4 | `MobaXterm` | Import-source **name** label - proprietary icon **no longer bundled** (neutral glyph used) | **Mobatek** (mobatek.net) | Trademark - name used nominatively | **Nominative/identification use only.** No affiliation. The logo artwork is not reproduced. |
| 5 | `SecureCRT` | Import-source **name** label - proprietary icon **no longer bundled** (neutral glyph used) | **VanDyke Software** | Trademark - name used nominatively | **Nominative/identification use only.** No affiliation. The logo artwork is not reproduced. |
| 6 | `VIQ JSON` | JSON logo (`<title>JSON logo</title>`) | Wikimedia Commons - *JSON vector logo.svg*; logo by **Douglas Crockford** | **Public domain** | No attribution required; credit is courteous. |

---

## 5. Build tooling (used to produce the application; not redistributed as libraries of the app's own code)

| Tool | Version | License | Upstream |
|---|-----|-----|-----|
| **Python** | 3.13 | PSF-2.0 (Python Software Foundation License) | https://www.python.org/ |
| **PyInstaller** | 6.19.0 | GPL-2.0-or-later **with bootloader exception** | https://pyinstaller.org/ |

**PyInstaller bootloader exception (reproduced):**

> In addition to the permissions in the GNU General Public License, the authors give you unlimited permission to link or embed compiled bootloader and runtime files into combinations with other programs, and to distribute those combinations without any restriction coming from the use of those files.

This exception means the application's own code is not made subject to the GPL merely by being packaged with PyInstaller.

---

## Notices (required attribution text)

### IEEE OUI / MA-L registry
> Portions of this product use the IEEE OUI (MA-L) registry made publicly available by the **IEEE Registration Authority** (https://standards-oui.ieee.org/) for the purpose of mapping MAC address prefixes to vendor names. IEEE is not affiliated with, and does not endorse, VIQ Network Toolset or Virtual IQ AI · USA.

### "Unofficial SSH Logo" - CC BY 4.0
> "Unofficial SSH Logo" by **Jessie Kirk** (2022), sourced from Wikimedia Commons, licensed under **Creative Commons Attribution 4.0 International (CC BY 4.0)** - https://creativecommons.org/licenses/by/4.0/. The image is used to identify an SSH configuration import source; it may have been resized/recolored for display. This is a community logo and is not an official mark of the OpenSSH project.

### JSON logo - public domain
> The "JSON logo" (by Douglas Crockford), sourced from Wikimedia Commons (*JSON vector logo.svg*), is in the **public domain**.

### PuTTY icon - Apache-2.0 (dashboard-icons)
> The PuTTY icon is sourced from the **homarr-labs/dashboard-icons** project (https://github.com/homarr-labs/dashboard-icons), licensed under the **Apache License 2.0**. Per that project's disclaimer: all product names, trademarks, and registered trademarks are the property of their respective owners; icons are used for identification purposes only and do not imply endorsement. PuTTY is a trademark of Simon Tatham.

### Simple Icons - CC0-1.0
> Cloud-service brand icons (`vendor/logos/`) are sourced from **Simple Icons** (https://simpleicons.org/), dedicated to the public domain under **CC0-1.0**. **The depicted brand marks are trademarks of their respective owners; their inclusion identifies reachability/service targets only and does not imply any affiliation or endorsement.**

### Microsoft Fluent Emoji - MIT (verify)
> Certain UI glyphs (`vendor/emoji/`) are, or are derived from, **Microsoft Fluent Emoji** © Microsoft, licensed under the **MIT License** (https://github.com/microsoft/fluentui-emoji). *(Confirm the full set's provenance before publishing.)*

---

## LGPL components - replacement notice

This software links (dynamically, via Python's import system) to the following **LGPL**-licensed libraries: **paramiko** (LGPL-2.1) and, where bundled, **pystray** (LGPL-3.0). As required by the LGPL, you may use, copy, and modify these libraries and replace the bundled copy with your own modified version:

1. Obtain or build a modified copy of the library.
2. Locate the application's bundled libraries:
   - **Windows:** `C:\Program Files\VIQ Network Toolset\_internal\`
   - **macOS:** `/Applications/VIQ Network Toolset.app/Contents/Resources/` and `…/Contents/Frameworks/`
3. Replace the corresponding files with your modified version.
4. Relaunch the application.

Replacing bundled libraries may affect stability; you assume responsibility for any such replacement. Full LGPL texts: LGPL-2.1 https://www.gnu.org/licenses/old-licenses/lgpl-2.1.txt · LGPL-3.0 https://www.gnu.org/licenses/lgpl-3.0.txt

---

## MPL / EPL components

- **certifi** is licensed under **MPL-2.0** (file-level copyleft). It is bundled unmodified; the MPL-2.0 text is available at https://www.mozilla.org/MPL/2.0/ and the source at the upstream link above.
- **asyncssh** is dual-licensed under **EPL-2.0** and **GPL-2.0-or-later**. Virtual IQ AI · USA elects to use asyncssh under the **Eclipse Public License 2.0** - https://www.eclipse.org/legal/epl-2.0/.

---


*© 2026 Virtual IQ AI · USA. All rights reserved. This document is provided for transparency and attribution purposes.*
