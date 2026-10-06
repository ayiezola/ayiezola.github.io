---
layout: post
title: WhatsApp "Linked Devices" Account Takeover Phishing Campaign
subtitle: Phishing & Account Takeover (ATO)
tags: [phishing, whatsapp, account takeover]
comments: false
author: Ayiezola
---

# 🚨 Phishing Analysis: WhatsApp "Linked Devices" Account Takeover Campaign

> **Date:** October 2026
> **Target:** Malaysian WhatsApp Users (generic mobile users)
> **Author:** Mr Ayiezola

---

## 1. Executive Summary
This report documents a live, professionally-operated phishing campaign that impersonates the **WhatsApp Security Center** to hijack WhatsApp accounts through the app's legitimate **"Linked Devices"** feature. Victims receive an SMS/WhatsApp lure claiming their account has been **flagged for a policy violation** and must verify **within 2 hours**. The link opens a fake "WhatsApp Security Center" fronted by a rotating shortener, behind which sits a **live human operator chat** that walks the victim through linking **the attacker's** device using WhatsApp's **8-digit linking code** — resulting in **full account takeover**.

<div style="background-color: #ffe6e6; border-left: 6px solid #ff4d4d; padding: 15px; margin: 20px 0;">
  <strong>⚠️ DANGER:</strong> The domains <code>wsappcenter.com</code> / <code>apwscenter.com</code> and the lure <code>hxxps://avvf[.]me/pltjd</code> are confirmed <strong>MALICIOUS</strong>. Do not enter real data.
</div>

---

<p align="center">
  <img src="https://raw.githubusercontent.com/ayiezola/ayiezola.github.io/master/assets/phishing-whatsapp-ato/wa-ato-009.png" alt="WhatsApp Security Center Phishing Kit" width="900px" style="border: 1px solid #ddd;"/>
  <br><em>Full Chain Phishing ATO — WhatsApp Linked Devices takeover.</em>
</p>

---

## 2. Threat Intelligence & Infrastructure
This section outlines the core technical indicators identified during triage.

| Entity | Intelligence Detail |
| :--- | :--- |
| **Lure URL** | <code style="color: #d73a49;">hxxps://avvf[.]me/pltjd</code> (rotating shortener) |
| **Phishing Kit Hosts** | `wsappcenter.com` (suspended) · `apwscenter.com` (active) |
| **Origin Server (exposed)** | <code style="color: #d73a49;">47.128.213.130</code> (AWS EC2, Singapore) |
| **Target Region** | 🇲🇾/🌏 Generic (kit default country code +86) |
| **Impersonated Brand** | WhatsApp (Meta) — "WhatsApp 安全中心 / Security Center" |
| **Attack Vector** | SMS / WhatsApp message → fake link → live-chat social engineering |
| **Objective** | WhatsApp Account Takeover via **Linked Devices 8-digit code** |
| **Threat Status** | <span style="color: white; background-color: #d73a49; padding: 2px 8px; border-radius: 4px; font-weight: bold;">ACTIVE / MALICIOUS</span> |

**Sample lure (verbatim):**
> "you whatsapp account has been flagged for a policy violation! Please verify your identity within 2 hours to avoid account suspension: https://avvf[.]me/pltjd"

### 🚩 Infrastructure Red Flags
> [!IMPORTANT]
> **Fresh, throwaway infrastructure:** all three domains were registered **19–22 September 2026** (NameCheap / GNAME), all proxied behind **Cloudflare**, and the kit was deployed and issuing TLS certificates within days. The domains have no legitimate purpose, no history, and no relation to WhatsApp or Meta.

---

## 3. Visual Analysis & Proofs

### Delivery Method & Social Engineering
The threat actor (TA) delivers the lure by SMS/WhatsApp with an urgent "account will be suspended" hook, then redirects the victim through a **rotating short link** to a look-alike "WhatsApp Security Center".

<p align="center">
  <img src="https://raw.githubusercontent.com/ayiezola/ayiezola.github.io/master/assets/phishing-whatsapp-ato/wa-ato-001.png" alt="Lure SMS" width="500px" style="border: 1px solid #ddd;"/>
  <br><em>Figure 1: Lure message received — account "flagged", verify within 2 hours.</em>
</p>

### A. Landing Page Impersonation
The page uses WhatsApp branding and bilingual copy (Chinese default, English option) to create a false sense of authority.

<p align="center">
  <img src="https://raw.githubusercontent.com/ayiezola/ayiezola.github.io/master/assets/phishing-whatsapp-ato/wa-ato-002.png" alt="Fake WhatsApp Security Center" width="800px" style="border: 1px solid #ddd;"/>
  <br><em>Figure 2: Main landing page (Chinese default).</em>
</p>

<p align="center">
  <img src="https://raw.githubusercontent.com/ayiezola/ayiezola.github.io/master/assets/phishing-whatsapp-ato/wa-ato-003.jpg" alt="Fake WhatsApp Security Center (EN)" width="800px" style="border: 1px solid #ddd;"/>
  <br><em>Figure 3: English variant of the same kit.</em>
</p>

### B. Live-Chat Account Takeover Flow
The phishing flow transitions from simple brand impersonation to an **active account hijacking attempt**:

1. **Device Fingerprinting:** The kit first asks the visitor to choose **Android / iPhone** and serves a device- and language-specific skin.
2. **Live "Support Agent":** A **real-time chat** opens with a human operator ("REALTIME TALK" panel backend) who builds trust and guides the victim.
3. **Linked-Devices Hijack:** The operator instructs the victim to open WhatsApp → **Linked Devices** → *Link with phone number* → and enter the **8-digit code the operator provides**.

<div style="background-color: #fff3cd; border-left: 6px solid #ffecb5; padding: 15px; margin: 20px 0; color: #856404;">
  <strong>Note:</strong> The victim is tricked into linking the <strong>attacker's</strong> device. Entering the 8-digit code does not "verify" the account — it hands the attacker full access to the victim's chats, contacts, and any <strong>OTP / 2FA codes</strong> delivered over WhatsApp.
</div>

<p align="center">
  <img src="https://raw.githubusercontent.com/ayiezola/ayiezola.github.io/master/assets/phishing-whatsapp-ato/wa-ato-004.jpg" alt="Kit on iPhone" width="700px" style="border: 1px solid #ddd;"/>
  <br><em>Figure 4: Device-specific kit (iPhone).</em>
</p>

<p align="center">
  <img src="https://raw.githubusercontent.com/ayiezola/ayiezola.github.io/master/assets/phishing-whatsapp-ato/wa-ato-005.jpg" alt="Kit on Android" width="700px" style="border: 1px solid #ddd;"/>
  <br><em>Figure 5: Device-specific kit (Android).</em>
</p>

### Indicators of Compromise (IoCs)
* **URL:** `hxxps://avvf[.]me/pltjd`
* **Name:** `[Insert Name]`
* **WhatsApp Number:** `[Insert Phone Number]`
* **8-digit Linking Code:** `[Insert Code]`

---

## 4. Technical Findings & Data Exfiltration

### A. Infrastructure Recon
Investigation of the kit domains revealed several technical red flags:
* **Registrars:** NameCheap (`wsappcenter.com`, `apwscenter.com`) and GNAME (`avvf.me`).
* **Registration window:** 19–22 September 2026 — brand-new, mass-abuse infrastructure.
* **Localization:** Kit shipped with **zh-CN, zh-TW, en-US and es-ES** skins; default country code **+86**.
* **Certificate Transparency:** Let's Encrypt wildcard certs first issued 19–22 Sep 2026, re-issued 1 Oct 2026.

### B. Server Misconfigurations (Content Exposure)
Due to poor server hardening, several internal resources were exposed:
* **Directory Indexing:** `/assets/` and `/assets/verification/` returned open directory listings (Go `http.FileServer` misconfiguration), leaking `notify-bak.mp3` and additional locale files.
* **Operator panel frontend:** `/admin-login.html`, `/admin.js`, `/console.js`, `/sites.js`, `/templates.js` were publicly reachable (auth-gated API, but the full panel UI was downloadable).

<p align="center">
  <img src="https://raw.githubusercontent.com/ayiezola/ayiezola.github.io/master/assets/phishing-whatsapp-ato/wa-ato-011.png" alt="Operator panel login" width="800px" style="border: 1px solid #ddd;"/>
  <br><em>Figure 6: Exposed operator panel login ("REALTIME TALK" agent workstation).</em>
</p>

<p align="center">
  <img src="https://raw.githubusercontent.com/ayiezola/ayiezola.github.io/master/assets/phishing-whatsapp-ato/wa-ato-012.png" alt="Open directory listing" width="800px" style="border: 1px solid #ddd;"/>
  <br><em>Figure 7: Content exposed via open directory indexing.</em>
</p>

### C. The "Smoking Gun": Exposed Origin IP (Cloudflare Bypass)
The most significant find was a **DNS-only (grey-cloud) record** that exposed the true origin server behind Cloudflare:

* **Origin IP:** `47.128.213.130` — AWS EC2, `ap-southeast-1` (Singapore), `ec2-47-128-213-130.ap-southeast-1.compute.amazonaws.com`.
* **Stack:** nginx → Go, Debian 12. Ports **22 / 80 / 443** open (443 speaking plain HTTP).
* **Bypass:** sending `Host: whatsapp.wsappcenter.com` directly to the origin returns the **full kit and panel**, even after the domain's DNS was suspended by its registrar.

```bash
# Public recursive lookup — NXDOMAIN (registrar clientHold)
dig +short whatsapp.wsappcenter.com A

# Ask the authoritative NS directly — exposes the grey-cloud origin
dig +short @salvador.ns.cloudflare.com whatsapp.wsappcenter.com A   # -> 47.128.213.130
curl -sD- -H "Host: whatsapp.wsappcenter.com" http://47.128.213.130/ -o /dev/null   # 200, Server: nginx
```

<p align="center">
  <img src="https://raw.githubusercontent.com/ayiezola/ayiezola.github.io/master/assets/phishing-whatsapp-ato/wa-ato-013.png" alt="Origin exposure" width="800px" style="border: 1px solid #ddd;"/>
  <br><em>Figure 8: Origin-IP exposure — direct request returns the live kit (Cloudflare bypass).</em>
</p>

<p align="center">
  <img src="https://raw.githubusercontent.com/ayiezola/ayiezola.github.io/master/assets/phishing-whatsapp-ato/wa-ato-014.png" alt="Registrar clientHold" width="800px" style="border: 1px solid #ddd;"/>
  <br><em>Figure 9: Registrar status <code>clientHold</code> / NXDOMAIN after takedown.</em>
</p>

<p align="center">
  <img src="https://raw.githubusercontent.com/ayiezola/ayiezola.github.io/master/assets/phishing-whatsapp-ato/wa-ato-015.png" alt="TLS certificate" width="800px" style="border: 1px solid #ddd;"/>
  <br><em>Figure 10: Let's Encrypt wildcard certificate (SAN list).</em>
</p>

---

## 5. Kit Configuration & Backend

Deep analysis of the kit's JavaScript and network traffic revealed the "brain" of the operation.

### A. Command & Control (Backend) Endpoint
* **API:** REST + Server-Sent Events (Go `net/http`) — `/api/sessions`, `/api/conversations/{id}/events` (SSE), `/connect`.
* **Panel API:** `/api/auth/{login,logout,me}`, `/api/agent/{conversations,quick-replies}`, `/api/admin/sites` — a **multi-site, multi-agent** panel ("Realtime Talk", localStorage key `realtime-talk-agent-read`).
* **Analysis:** Victims' captured data is pushed to a **Cloudflare R2** bucket (`netblaze-images`) via **pre-signed S3 upload URLs** (`connect-src https://*.r2.cloudflarestorage.com`). Using a separate object store means the crew keeps the loot even if a front-end node is taken down.

### B. Geo-Targeting & Localization
* **Attributes:** free-text country code field; kit defaults to **+86**.
* **Analysis:** Original kit targets Chinese-speaking users; EN/ES/zh-TW skins show it is being **retooled for wider abuse**, which is why Malaysian users are now exposed.

### C. Operator Console
* **Product:** internal name **"REALTIME TALK"** (Chinese 坐席工作台) — an off-the-shelf phishing-as-a-service panel with live chat, quick replies, and multi-site tenancy.
* **Kit signature:** internal skin name **"mango"**; R2 project/bucket **"netblaze"**.

---

## 6. Deep Dive: Linked-Devices Hijack Logic

Analysis of the live-chat flow confirms a **socially-engineered account takeover** rather than a credential-stealer.

#### A. Technical Features:
* **Real-time Operator:** a human agent chats with the victim, mirroring the psychological pressure of a bank/security call.
* **Legitimate Feature Abuse:** the operator leverages WhatsApp's real **"Link with phone number"** flow. No malware, no fake APK — just a code.
* **Full Takeover:** once linked, the attacker gains message history, contacts, the ability to **message as the victim**, and **interception of OTP/2FA codes** sent over WhatsApp.

**Captured Data Payload (conceptual):**
```json
{
  "visitor_id": "session_id",
  "device": "iPhone|Android",
  "phone_number": "Victim_Number",
  "linked_device_code": "8_DIGIT_CODE",
  "locale": "zh-CN|zh-TW|en-US|es-ES"
}
```

<div style="background-color: #fff3cd; border-left: 6px solid #ffecb5; padding: 15px; margin: 20px 0; color: #856404;">
  <strong>Note:</strong> This is a <strong>Man-in-the-Middle (MitM)</strong>-style takeover: the "linking code" is the OTP-equivalent secret. Reading it to anyone hands over the account.
</div>

---

## 7. Multi-Domain / Rotating Campaign (2026)

The TA does not rely on a single URL. A **rotating shortener** (`avvf.me`) 302-redirects victims across a fleet of look-alike hosts, and only pre-provisioned campaign slugs resolve — a classic anti-blocklist design.

#### Newly Identified Assets:
* **Kit cluster A:** `whatsapp, whatsapp1–3.wsappcenter.com` — **clientHold (taken down 2026-10-04)**
* **Kit cluster B:** `ws1–ws4.apwscenter.com` — **ACTIVE**
* **Shortener:** `avvf.me` — wildcard DNS (domain now NXDOMAIN)

#### Comparative Analysis
All nodes serve **byte-identical** kit pages (same hashes), sharing the same backend and R2 exfiltration. This "mirroring" tactic provides **redundancy**: taking down one domain does not stop the campaign — as demonstrated when `wsappcenter.com` was suspended but `apwscenter.com` remained live.

---

## 8. Prevention & Reporting

**Golden Rule:** WhatsApp **never** asks you to verify your account over a link, and **never** asks you to read out or enter an **8-digit linking code** to a "support agent". A linking code is a **key to your account**.

**If you already entered a code:**
1. Open WhatsApp → **Linked Devices** → **remove any device you don't recognise**.
2. Enable **Two-Step Verification**.
3. Warn your contacts (the attacker can message them as you).

**Official / Reporting:**
* **NSRC (National Scam Response Centre):** call **997**.
* **Report the URL** via **Google Safe Browsing** and **SemakMule** (PDRM).
* **Report to MCMC** / MyCERT (Cyber999) for coordination with registrars, Cloudflare, AWS and Meta.

**Detection ideas (defensive):**
* Alert on 302s from shortener hosts to `*.wsappcenter.com` / `ws*.apwscenter.com`.
* Flag fake-brand pages whose CSP references `r2.cloudflarestorage.com`.
* High-fidelity: `<title>WhatsApp安全中心</title>` on any non-`whatsapp.com` domain.

---

**Indicators of Compromise (summary)**

```
Domains : avvf.me  wsappcenter.com  apwscenter.com
Hosts   : ws1-ws4.apwscenter.com ; whatsapp,whatsapp1-3.wsappcenter.com
Origin  : 47.128.213.130  (AWS EC2 ap-southeast-1) — nginx -> Go, Debian 12
Storage : Cloudflare R2 bucket "netblaze-images"  (account 2a055cb59af47d7e8aaa7801de56dbf6)
CF edge : 104.21.43.84 172.67.176.248  (apwscenter.com)
SHA-256 : 3093485ffb42f0088d772fbecfa563339a6453a476ae62cefbeb4b2c3eea5706  index.html
          34cd6ca4324d7890656efd1772bf48444ea4cd22bb10e61d785031c8104308a8  app.js
          209247dd28ccf82f8024906781fe057481ed217cf4062eef24419ecada5d6a71  style.css
```

---

[Back to Home](https://ayiezola.github.io/)
