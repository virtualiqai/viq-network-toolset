# Terms of Use - Plain English

*Applies whenever you download or use the Software. Free to use, as is, entirely at your own risk.*

This is a plain-English summary of how you may use VIQ Network Toolset. The formal legal agreement is in [LICENSE](./LICENSE). If anything here conflicts with the LICENSE, the LICENSE controls.

We wrote this because you deserve to understand what you're agreeing to, in language that doesn't require a law degree.

---

## Eligibility

You may use this software only if you can form a binding agreement with us and are not barred from doing so under any applicable law. If you are using it on behalf of an organization, you represent that you are authorized to accept these terms for that organization. The software is not directed to children.

---

## What You Can Do

You can install and use the toolset **for free** if you are using it for:

- **Personal projects** at home, in a home lab, or on networks you personally own
- **Learning and education** - students, certification candidates, and self-learners are welcome
- **Non-profit organizations** for their own internal operations
- **Evaluation** - try it out, see if it fits your workflow

You can install it on as many of your own devices as you want.

---

## What Requires a Commercial License

You need a separate commercial license from us if you are using the toolset for:

- **Your for-profit employer's day-to-day operations** - even internal use at a paying job is commercial
- **Consulting work for clients** - using it to diagnose or manage a paying client's network
- **Managed-service-provider work** - using it as part of an MSP/MSSP service offering
- **Reselling, repackaging, or redistributing** the software in any form

If any of those describe you, please contact **info@virtualiqai.com** to discuss licensing. We're reasonable about pricing for small teams.

---

## What You Cannot Do, Ever

Some uses are completely off-limits regardless of license:

### Don't Scan or Connect to Systems You Don't Own or Aren't Authorized to Test

This is the most important rule. **Only point this tool at networks, devices, and systems you own or have explicit written permission to access, scan, connect to, or test.**

- ✅ Your home network - fine
- ✅ Your employer's network, with your IT manager's approval - fine
- ✅ A client's network, with a signed work order describing the scope - fine
- ❌ A neighbor's Wi-Fi - not fine
- ❌ A coffee shop's network - not fine
- ❌ Any network, host, or device you don't have explicit authorization to access - not fine

Unauthorized network scanning is illegal in most countries. In the United States it can violate the **Computer Fraud and Abuse Act (18 U.S.C. § 1030)**, which carries serious criminal and civil penalties. If you misuse this tool, the consequences are entirely yours. We have no way to know what network you're pointing it at, and we will not be responsible for your misuse.

### Don't Use It for Unauthorized Penetration Testing

Penetration testing requires a written, signed agreement from the target organization explicitly authorizing the activity. Don't do "drive-by" testing of public targets, "research" against systems you don't own, or bug-bounty work outside a program's rules of engagement.

### Don't Try to Reverse Engineer the Software

The binary is intentionally protected and encrypted (AES-256-GCM source-code encryption). Don't try to extract the source code, deobfuscate the protections, defeat the encryption, or build a clone. If you want to know how something works, ask - we're often happy to share at a high level.

### Don't Redistribute

Don't repackage, mirror, host on file-sharing sites, or share the binary outside the official GitHub release page. Always direct people to the official download link so they get the genuine, current version.

### Don't Use It in Sanctioned Countries

U.S. law prohibits exporting this software to OFAC-sanctioned countries (currently including Cuba, Iran, North Korea, Syria, and the Crimea/Donetsk/Luhansk regions of Ukraine) or to anyone on U.S. denied-parties lists.

### Don't Use It to Break the Law

We trust this is obvious - if it's illegal in your jurisdiction, don't use this tool to do it.

---

## What You're Responsible For

When you use this software, **you take on full responsibility** for:

1. **Authorization.** You confirm you have permission to interact with every device, host, or network you target, scan, connect to, or test.
2. **Lawful use.** You confirm your use complies with all applicable laws, regulations, and contracts (including your employer's acceptable-use policies).
3. **Production impact.** Network probing carries risk. Test in lab environments first. If you disrupt a production switch with a Switch Health probe, that's on you - not us.
4. **Your local data.** Things you enter into the tool - IP addresses, hostnames, credentials, configurations - stay on your local machine. Securing your own machine is your responsibility. The tool keeps a local activity log; see [SECURITY.md](./SECURITY.md) for details and how to clear it.
5. **Backups.** Always have current backups of any device configuration before running interactive tools like the SSH Terminal.

---

## Third-Party Services

A few tools reach out to third-party services **only when you invoke them** - for example, IP geolocation (IP Info) and RDAP lookups (BGP ASN Lookup). Those services are operated by third parties under their own terms and privacy policies; we don't control them and aren't responsible for them. See [SECURITY.md](./SECURITY.md) for the full list of outbound connections.

---

## Third-Party Components and Trademarks

The software bundles open-source components and data sets, each under its own license - see [THIRD_PARTY_LICENSES.md](./THIRD_PARTY_LICENSES.md). Third-party product names and logos (including the import-source application icons) belong to their respective owners and are used for identification only, with no claim of affiliation or endorsement - see [NOTICE.md](./NOTICE.md).

---

## What We're Responsible For

To be straightforward: **as little as the law allows us to be.**

This software is free. It comes "as is." We do not promise it will work in your environment, will not have bugs, will not crash, or will produce accurate results in every situation. **To the maximum extent permitted by law, we are not liable for any damages, outages, data loss, fines, or other harm arising from your use of the software** (see the full **Limitation of Liability** in [LICENSE](./LICENSE) Section 8), and **you agree to defend and indemnify us** for claims arising from your use or misuse (LICENSE Section 9).

This is standard practice for free software. Major providers of free network tools (Wireshark, the Python ecosystem, and even paid products' free editions) operate under similar terms. If a free toolset came with a full enterprise warranty, it would not be free.

**The LICENSE document goes into the full legal detail.** Read it before relying on the software in any meaningful way.

---

## What Happens If You Misuse the Software

If you violate these terms - for example, by scanning a network without authorization, or by using the tool for unauthorized penetration testing - several things may happen:

1. **Your license to use the software ends immediately.** You must uninstall it.
2. **You take on legal responsibility.** If a third party sues us because of something you did with the tool, you agree to defend us and cover the costs (this is called *indemnification*; see [LICENSE](./LICENSE) Section 9).
3. **You may face criminal or civil liability** under federal law (CFAA), state computer-crime laws, or other applicable laws.

We do not monitor your usage. The toolset has no telemetry. But that does not mean misuse is invisible to your network owner, your ISP, or law enforcement.

---

## Reporting Misuse or Vulnerabilities

If you discover a security vulnerability in the software, please report it privately. See [SECURITY.md](./SECURITY.md) for the responsible-disclosure process.

If you become aware of someone misusing the software, please report it to **info@virtualiqai.com**.

---

## Changes to These Terms

We may update these Terms of Use from time to time. Material changes will be announced via a release note. By continuing to use the software after an update, you accept the updated terms.

---

## Termination

You can stop using the software at any time by uninstalling it. We can suspend or end your license if you breach these terms, as described in [LICENSE](./LICENSE) Section 13. The obligations that by their nature should survive - including authorization, restrictions, disclaimers, limitation of liability, and indemnification - continue after termination.

---

## Questions?

Email **info@virtualiqai.com**. We read everything.

---

*© 2026 Virtual IQ AI · USA. All rights reserved.*
