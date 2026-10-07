---
layout: post
title: From Radio Waves to WhatsApp Account Takeover
subtitle: Phishing & Account Takeover (ATO)
tags: [phishing, whatsapp, account takeover]
comments: false
author: Ayiezola
---

# 🚨 Phishing Analysis: From Radio Waves to WhatsApp Takeover: The Fake BTS → Linked Devices Kill Chain

> **Date:** October 2026
> **Target:** Malaysian WhatsApp users
> **Author:** Mr Ayiezola

---

## 1. Executive Summary
There's a live, well-built phishing campaign out there wearing a "WhatsApp **Security Center**" mask — and what it's after isn't your password. It's your **entire WhatsApp account**. The hook is a text telling you your account got **flagged for a policy violation** and you've got **two hours** to "verify" or it's gone. And the way that first text even reaches you is nastier than a normal SMS blast: the crew pushes it from a **Fake BTS (rogue cell tower)** to slip straight past your telco's filters. Click through and you land on a fake WhatsApp page — except there's a **real person on the other end** of a chat, slowly talking you into linking **their** device with WhatsApp's own **8-digit linking code**. Hand that code over and it's game over: **full account takeover**.

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
Here's the core of what we pulled during triage.

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

**The lure, word for word:**
> "you whatsapp account has been flagged for a policy violation! Please verify your identity within 2 hours to avoid account suspension: https://avvf[.]me/pltjd"

### 🚩 Red Flags
> [!IMPORTANT]
> **All three domains are brand new.** They were registered between **19–22 September 2026** (NameCheap / GNAME), all parked behind **Cloudflare**, and the kit was live with fresh TLS certs within days. No history, no legitimate purpose, nothing to do with WhatsApp or Meta — that combo alone tells you everything.

---

## 3. Visual Analysis & Proofs

### How it lands — Fake BTS first, then a short link
The campaign has **two delivery hops**. The first one is the interesting part: a **Fake BTS**, the technique the TA originally used to get the lure onto victims' phones.

#### Method 1 — Fake BTS (False Base Station) SMS

<p align="center">
  <img src="https://raw.githubusercontent.com/ayiezola/ayiezola.github.io/master/assets/phishing-whatsapp-ato/fake-bts-diagram.png" alt="Fake BTS" width="500px" style="border: 1px solid #ddd;"/>
  <br><em>Diagram of Fake BTS. This is AI generated image</em>
</p>

A **Fake BTS** (a.k.a. false base station, cell-site simulator, or "IMSI catcher") is exactly what it sounds like: a **fake mobile tower** that stands between the victim's phone and the real network — a man-in-the-middle for mobile traffic. The reason it works is a known GSM design gap: the handset has to prove itself to the network, but the **network never proves itself to the phone**. So a rogue tower can pull nearby handsets onto it — often by **forcing phones down from 4G/5G to 2G**, which has no mutual authentication. Once the phones are connect to the fake tower, the operator can:

* **Spoof the sender ID** — the SMS can show whatever name or number they want (e.g. a "WhatsApp" alert), so it looks completely official.
* **Bypass every telco filter** — the message never touches the carrier's SMS gateway, so spam/scam filtering simply never sees it.
* **Hit everyone in range at once** — no need to know a victim's number; every handset nearby gets the text. No SIM farm, no per-SMS cost.
* **Leave almost no trace** — no carrier logs tying the blast to the sender.

For the victim the tell is subtle: the phone may briefly **drop to "2G / EDGE" or "No Service"** just before an odd SMS lands from a sender that has no business texting you.

<div style="background-color: #fff3cd; border-left: 6px solid #ffecb5; padding: 15px; margin: 20px 0; color: #856404;">
  <strong>Why it matters:</strong> Fake BTS is how the SMS slips past carrier spam filters and lands looking 100% legit — that's the entire "first hop" of the attack. Everything that follows (the short link, the kit, the live chat) is essentially ordinary phishing once you click.
</div>

<p align="center">
  <img src="https://raw.githubusercontent.com/ayiezola/ayiezola.github.io/master/assets/phishing-whatsapp-ato/wa-ato-001.jpeg" alt="Lure SMS" width="500px" style="border: 1px solid #ddd;"/>
  <br><em>Figure 1: Lure message received — account "flagged", verify within 2 hours.</em>
</p>

This message popped up on my phone right after we finished breakfast at a famous mamak restaurant around Jalan Semarak. Can you guess where? 😄

#### Fake BTS in the wild — Malaysia (2026)
This isn't theoretical. Malaysian enforcers are chasing it right now:

* **Johor Bahru, 6 Aug 2026** — MCMC and PDRM dismantled a Fake BTS SMS syndicate, arresting a **65-year-old local man** caught operating a **vehicle rigged with Fake BTS gear**: two mobile phones, a SIM card, a GSM module, an antenna and the car itself. The rig beamed phishing SMS straight at **high-density commuter areas in peak hours**, specifically the **Johor Bahru–Singapore** crowd. It was one of **six Fake BTS operations** MCMC ran in 2026 (three in Johor, three in Genting Highlands). — [NST, 11 Aug 2026](https://www.nst.com.my/news/nation/2026/08/1508777/mcmc-police-bust-fake-bts-scam-syndicate-johor)
* **Dewan Negara, 23 Feb 2026** — Deputy Communications Minister **Teo Nie Ching** said the devices hide inside **vehicles or bags**, letting syndicates "move dynamically" to dodge MCMC and PDRM — and that enforcement relies on **public reports** to pin down the exact location. — [Berita Harian, 23 Feb 2026](https://www.bharian.com.my/berita/nasional/2026/02/1512786/sindiket-fake-bts-bergerak-dinamik-jadi-cabaran-penguatkuasaan)

<div style="background-color: #ffebe6; border-left: 6px solid #ff8c42; padding: 15px; margin: 20px 0;">
  <strong>Why it's relevant here:</strong> the same rogue-tower play that lands a "bank SMS" also lands a "WhatsApp suspension" text. The delivery method described in this report is exactly the tactic MCMC is tracking across Johor — the same technique, just with a different lure.
</div>

#### Method 2 — The rotating short link
From there it's the usual play: an **urgency hook** plus a **rotating short link** that bounces the victim onto a look-alike "WhatsApp Security Center".

### A. Landing Page Impersonation
WhatsApp branding, bilingual copy (Chinese by default, English on tap) — it is designed to make it look official.

<p align="center">
  <img src="https://raw.githubusercontent.com/ayiezola/ayiezola.github.io/master/assets/phishing-whatsapp-ato/wa-ato-002.png" alt="Fake WhatsApp Security Center" width="800px" style="border: 1px solid #ddd;"/>
  <br><em>Figure 2: Main landing page (Chinese default).</em>
</p>

<p align="center">
  <img src="https://raw.githubusercontent.com/ayiezola/ayiezola.github.io/master/assets/phishing-whatsapp-ato/wa-ato-003.jpg" alt="Fake WhatsApp Security Center (EN)" width="800px" style="border: 1px solid #ddd;"/>
  <br><em>Figure 3: English variant of the same kit.</em>
</p>

### B. From Fake Page to Account Takeover
It stops being a "copy-paste phishing page" real quick:

1. **It fingerprints you first:** makes you pick **Android or iPhone**, then serves a matching skin in your language.
2. **Then puts a human on the line:** a **real-time chat** opens with an operator (the "REALTIME TALK" panel backend) who enggages with you and gradually builds your trust.
3. **Then walks you into the trap:** the operator tells you to open WhatsApp → **Linked Devices** → *Link with phone number* → and key in the **8-digit code they provide**.

<div style="background-color: #fff3cd; border-left: 6px solid #ffecb5; padding: 15px; margin: 20px 0; color: #856404;">
  <strong>Note:</strong> You're actually being tricked into linking the <strong>attacker's</strong> device to your WhatsApp. Entering 8-digit code doesn't "verify" anything — it gives them your chats, your contacts, and any <strong>OTP / 2FA code</strong> that lands in your WhatsApp.
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
Digging into the domains, the usual tell-tales pop up:
* **Registrars:** NameCheap (`wsappcenter.com`, `apwscenter.com`) and GNAME (`avvf.me`).
* **Registration window:** 19–22 September 2026 — the whole thing is days old.
* **Localization:** the kit ships with **zh-CN, zh-TW, en-US and es-ES** skins, and defaults to country code **+86**.
* **Certificate Transparency:** Let's Encrypt wildcard certs first issued 19–22 Sep 2026, re-issued 1 Oct 2026 — they keep it alive.

### B. Server Misconfigurations (What They Left Wide Open)
Sloppy hardening showed us the back room:
* **Directory indexing:** `/assets/` and `/assets/verification/` hand out open directory listings (a classic Go `http.FileServer` slip), leaking `notify-bak.mp3` and extra locale files.
* **Panel out in the open:** `/admin-login.html`, `/admin.js`, `/console.js`, `/sites.js`, `/templates.js` were publicly reachable. The API's auth-gated, but the entire operator UI downloads.

<p align="center">
  <img src="https://raw.githubusercontent.com/ayiezola/ayiezola.github.io/master/assets/phishing-whatsapp-ato/wa-ato-011.png" alt="Operator panel login" width="800px" style="border: 1px solid #ddd;"/>
  <br><em>Figure 6: Exposed operator panel login ("REALTIME TALK" agent workstation).</em>
</p>

<p align="center">
  <img src="https://raw.githubusercontent.com/ayiezola/ayiezola.github.io/master/assets/phishing-whatsapp-ato/wa-ato-012.png" alt="Open directory listing" width="800px" style="border: 1px solid #ddd;"/>
  <br><em>Figure 7: Content exposed via open directory indexing.</em>
</p>

### C. The "Smoking Gun": Exposed Origin IP (Cloudflare Bypass)
The biggest discovery from the whole thing (Actually i love this part :)): a **grey-cloud (DNS-only) record** that leaked the real origin sitting behind Cloudflare.

* **Origin IP:** `47.128.213.130` — AWS EC2, `ap-southeast-1` (Singapore), `ec2-47-128-213-130.ap-southeast-1.compute.amazonaws.com`.
* **Stack:** nginx → Go, Debian 12. Ports **22 / 80 / 443** open (443 speaking plain HTTP).
* **The bypass:** fire `Host: whatsapp.wsappcenter.com` straight at the origin and it serves the **whole kit and panel** — even after the domain was suspended.

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

Pop the hood on the kit's JavaScript and traffic, and you find the "brain" of the op.

### A. Backend / Command & Control
* **API:** REST + Server-Sent Events (Go `net/http`) — `/api/sessions`, `/api/conversations/{id}/events` (SSE), `/connect`.
* **Panel API:** `/api/auth/{login,logout,me}`, `/api/agent/{conversations,quick-replies}`, `/api/admin/sites` — a **multi-site, multi-agent** panel ("Realtime Talk", localStorage key `realtime-talk-agent-read`).
* **The clever bit:** Any information uploaded by the victim upload will gets pushed to a **Cloudflare R2** bucket (`netblaze-images`) via **pre-signed S3 URLs** (`connect-src https://*.r2.cloudflarestorage.com`). Stashing it off-box means they keep the loot even if you kill a front-end node.

### B. Geo-Targeting & Localization
* **Attributes:** free-text country-code field; kit defaults to **+86**.
* **What it tells us:** the kit was specifically designed to target Chinese-speaking victims — the EN/ES/zh-TW skins mean they're now **adapting it for a wider global audience**, which is exactly why Malaysian users are being targeted.

### C. Operator Console
* **Product:** internal name **"REALTIME TALK"** (Chinese 坐席工作台) — an off-the-shelf phishing-as-a-service panel with live chat, quick replies, and multi-site tenancy.
* **Kit signature:** internal skin name **"mango"**; R2 project/bucket **"netblaze"**.

---

## 6. Deep Dive: The Linked-Devices Hijack

Read the live-chat flow and it's clear — this is **social engineering, not a credential grabber**.

#### The moving parts:
* **A human operator.** A live agent chats with the victim, running the same playbook as a fake bank/security call.
* **Abuse of a real feature.** They lean on WhatsApp's genuine **"Link with phone number"** flow. No malware, no dodgy APK — just a code.
* **Full takeover.** Once linked, they can access your message history, contacts, the ability to **message as the victim**, and **interception of OTP/2FA codes** — all through WhatsApp.

**Captured data payload (conceptual):**
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
  <strong>Note:</strong> This is a <strong>Man-in-the-Middle (MitM)</strong>-style takeover — the "linking code" is the OTP-equivalent secret. Read it to anyone and the account is theirs.
</div>

---

## 7. Multi-Domain / Rotating Campaign (2026)

They're not betting on a single URL. A **rotating shortener** (`avvf.me`) 302s victims across a fleet of look-alike hosts, and only pre-provisioned campaign slugs resolve — a textbook anti-blocklist setup.

#### What we found:
* **Cluster A:** `whatsapp, whatsapp1–3.wsappcenter.com` — **clientHold (taken down 2026-10-04)**
* **Cluster B:** `ws1–ws4.apwscenter.com` — **ACTIVE**
* **Shortener:** `avvf.me` — wildcard DNS (domain now NXDOMAIN)

#### Why it matters
Every node serves a **byte-identical** page (same hashes), all sharing the same backend and R2 exfil. That mirroring is pure **redundancy**: they killed `wsappcenter.com` and the campaign just kept humming along on `apwscenter.com`.

---

## 8. Prevention & Reporting

**The one rule to remember:** WhatsApp **never** asks you to verify your account through a link, nor does it asks you to share or enter an **8-digit linking code** to a "support agent". That code is a **key to your account** — treat it like your password.

**Already entered a code?**
1. Open WhatsApp → **Linked Devices** → **remove anything you don't recognise**.
2. Turn on **Two-Step Verification**.
3. Warn your contacts - the attacker may message them while pretending to be you.

**Official / Reporting:**
* **NSRC (National Scam Response Centre):** call **997**.
* **Report the URL** via **Google Safe Browsing** and **SemakMule** (PDRM).
* **Report to MCMC** / MyCERT (Cyber999) to get registrars, Cloudflare, AWS and Meta moving.
* **Report Fake BTS / spoofed-sender SMS to MCMC and your telco** — they can hunt the rogue tower, and it's worth asking your carrier about disabling 2G where possible.

**Detection ideas (blue team):**
* Alert on 302s from shortener hosts to `*.wsappcenter.com` / `ws*.apwscenter.com`.
* Flag fake-brand pages whose CSP references `r2.cloudflarestorage.com`.
* High-confidence: `<title>WhatsApp安全中心</title>` on any non-`whatsapp.com` domain.
* **Fake BTS:** watch for handsets dropping to **2G/EDGE** right before suspicious SMS, and treat spoofed-sender-ID blasts as a rogue-tower indicator.

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
