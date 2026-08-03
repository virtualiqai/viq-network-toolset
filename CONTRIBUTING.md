# Contributing & Feedback

Virtual IQ AI NetOps Toolset is proprietary software. The source code is not publicly available and pull requests with code changes are not accepted.

However, the project benefits enormously from real-world feedback. The list below is how you can contribute meaningfully.

---

## 🐛 Reporting a Bug

Found something broken? Please file a report so it can be reproduced and fixed.

**Where to file:**
- **Public bug:** [GitHub Issues](https://github.com/virtualiqai/viq_eng_toolset/issues/new)
- **Security vulnerability:** Do **not** file publicly. See [SECURITY.md](./SECURITY.md) for the private disclosure process.

**What to include:**

1. **Version** — Find it in **Developer → About / Version** inside the app
2. **Operating system** — Windows 10 1809 / Windows 11 / macOS 14 / etc.
3. **Tool affected** — e.g., "SNMP Port Mapper" or "NetDiag Report"
4. **Steps to reproduce** — what you did, in order
5. **Expected behavior** — what you thought would happen
6. **Actual behavior** — what actually happened
7. **Error message** — full text or screenshot, if any
8. **Sanitized data** — if the issue involves a specific device, please redact hostnames, IPs, and credentials before sharing

---

## 💡 Requesting a Feature

Have a tool idea, workflow improvement, or new capability you wish existed? Two ways:

1. **In-app:** Use the **Developer → Wishlist** tool inside the application — submissions are stored locally and reviewed when you share them
2. **Public:** Open a GitHub issue labeled `feature-request`

**Good feature requests include:**
- The operational problem you're trying to solve
- What the tool or feature would do, end-to-end
- Which protocols, vendors, or use cases it would target

Roadmap decisions are driven heavily by feedback from working engineers. Real use cases beat theoretical ones every time.

---

## ✅ Sharing Compatibility Reports

The SNMP code paths use vendor-neutral standard MIBs and should work against any device implementing them. If you've tested the toolset against specific hardware — successfully or not — that information helps everyone.

**Help us build a verified compatibility matrix.** Open an issue labeled `compatibility-report` with:

- Device vendor and model
- OS / firmware version
- Which tools worked (SNMP Port Mapper, Switch Health, SSH Terminal, etc.)
- Any quirks, workarounds, or partial-support notes

**Devices the codebase is built to support** (standard MIBs + Cisco-style show output):

- Cisco Catalyst IOS / IOS-XE
- Cisco Nexus NX-OS
- Cisco ASR / ISR routers
- Arista EOS (SNMP only — Switch Health parser is Cisco-syntax)
- Juniper Junos (SNMP only — Switch Health parser is Cisco-syntax)
- HP / Aruba (SNMP)
- MikroTik (SNMP)
- Huawei (SNMP)
- Generic Linux hosts (ICMP, DNS, port scan, traceroute)

Confirmed working ≠ tested in your environment. Always validate before relying on the toolset for production-critical workflows.

---

## 🔐 Security Issues

Please do **not** file security vulnerabilities as public issues.
See [SECURITY.md](./SECURITY.md) for the responsible disclosure process.

---

## ❓ Asking a Question

For general questions or usage help:

- **GitHub Discussions** *(if enabled)* — for community Q&A
- **Email:** info@virtualiqai.com — for direct support questions
- **LinkedIn:** [linkedin.com/in/kash-muhammad](https://www.linkedin.com/in/kash-muhammad/)

---

## 📜 Support Model

Virtual IQ AI NetOps Toolset is provided **as is**, with no formal support obligation. That said:

- The author actively responds to issues and feature requests when time allows
- Bug fixes and improvements ship in subsequent releases
- Critical security issues are prioritized

For organizations needing **guaranteed response times, dedicated support, custom features, or a commercial license**, contact **info@virtualiqai.com** to discuss a paid arrangement.

---

## 🤝 Code of Conduct

This project values respectful, professional collaboration. Be kind, be technical, be specific. Personal attacks, harassment, or hostile behavior in issues or other public channels will result in immediate ban from the project's public spaces.

---

## 📬 Direct Contact

**Muhammad Kashif / Virtual IQ AI LLC**
- 📧 Email: info@virtualiqai.com
- 🔗 LinkedIn: [linkedin.com/in/kash-muhammad](https://www.linkedin.com/in/kash-muhammad/)
- 🐙 GitHub: [github.com/virtualiqai](https://github.com/virtualiqai)

---

*© 2026 Virtual IQ AI LLC. All rights reserved.*
