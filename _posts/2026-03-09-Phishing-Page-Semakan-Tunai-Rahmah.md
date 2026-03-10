# 🚨 Phishing Analysis: Fake 'Sumbangan Tunai Rahmah' (STR) Status Check Page
> **Date:** March 2026  
> **Target:** Malaysian Citizens (Sumbangan Tunai Rahmah Recipients)  
> **Author:** Mr Ayiezola

---

## 1. Executive Summary
This report documents a phishing campaign active during the **Starting 2026** season. The attackers are impersonating the official **Sumbangan Tunai Rahmah (STR)** portal to harvest personal information (Phone Number) and Telegram OTP Code.

<div style="background-color: #ffe6e6; border-left: 6px solid #ff4d4d; padding: 15px; margin: 20px 0;">
  <strong>⚠️ DANGER:</strong> The URL <code>https://bantuanstr.infopublic.my.id/e/</code> is confirmed <strong>MALICIOUS</strong>. Do not enter real data.
</div>

---

<p align="center">
  <img src="https://raw.githubusercontent.com/ayiezola/ayiezola.github.io/master/assets/phishing-str/Overview_Phishing_STR.png" alt="Phishing Landing Page" width="900px" style="border: 1px solid #ddd;"/>
  <br><em>Full Chain Phishing ATO.</em>
</p>

---

## 2. Threat Intelligence & Infrastructure
This section outlines the core technical indicators identified during the initial triage.

## Entity | Intelligence Detail |
**Primary Phishing URL** | <code style="color: #d73a49;">https://bantuanstr.infopublic.my.id/e/</code>
**Collector Domain** | `berjaya66.my.id` (C2 Backend)
**Target Region** | 🇲🇾 Malaysia (International Code: +60)
**Impersonated Theme** | Sumbangan Tunai Rahmah (STR) 2026
**Attack Vector** | Telegram Social Engineering (SE)
**Threat Status** | <span style="color: white; background-color: #d73a49; padding: 2px 8px; border-radius: 4px; font-weight: bold;">ACTIVE / MALICIOUS</span>

### 🚩 Geographic Red Flags
> [!IMPORTANT]
> **Top-Level Domain (TLD) Mismatch:** The use of the `.my.id` TLD is a definitive indicator of fraud. Official Malaysian government services strictly utilize the `.gov.my` hierarchy. The `.id` extension is assigned to Indonesia, confirming that this infrastructure is not managed by the Malaysian Ministry of Finance or LHDN.

---

## 3. Visual Analysis & Proofs

### Delivery Method & Social Engineering
The threat actor uses Telegram Group to deliver phishing link. TA also put some captions to increase the link's trustworthiness.

<p align="center">
  <img src="https://raw.githubusercontent.com/ayiezola/ayiezola.github.io/master/assets/phishing-str/bantuan-str-025.png" alt="Phishing Landing Page" width="800px" style="border: 1px solid #ddd;"/>
  <br><em>Figure 1: Threat Actor Spread Phishing Link In Telegram Group.</em>
</p>

### A. Landing Page Impersonation
The page uses the official LHDN and Malaysia Madani logos to create a false sense of authority.

<p align="center">
  <img src="https://raw.githubusercontent.com/ayiezola/ayiezola.github.io/master/assets/phishing-str/bantuan-str-001.png" alt="Phishing Landing Page" width="800px" style="border: 1px solid #ddd;"/>
  <br><em>Figure 2: Main landing page.</em>
</p>

The TA may post fake testimonials or 'success' messages in the group to convince others the link is safe

<p align="center">
  <img src="https://raw.githubusercontent.com/ayiezola/ayiezola.github.io/master/assets/phishing-str/bantuan-str-027.png" alt="Phishing Landing Page" width="800px" style="border: 1px solid #ddd;"/>
  <br><em>Figure 3: Fake Testimonials.</em>
</p>

### B. Data Harvest & Account Takeover Form
The phishing flow transitions from simple data collection to an active account hijacking attempt:

1. **Phone Number Collection:** The form first requests the victim's name and mobile phone number.
2. **Real-time Exploitation:** Once the number is submitted, the backend triggers a legitimate Telegram login request to the victim's device.
3. **OTP Interception:** The victim is redirected to a second page prompting them to enter the OTP code sent to their Telegram account.

<div style="background-color: #fff3cd; border-left: 6px solid #ffecb5; padding: 15px; margin: 20px 0; color: #856404;">
  <strong>Note:</strong> This is a classic "Man-in-the-Middle" (MitM) technique. If the victim provides the OTP, the attacker gains full access to their Telegram account, contacts, and private messages.
</div>

<p align="center">
  <img src="https://raw.githubusercontent.com/ayiezola/ayiezola.github.io/master/assets/phishing-str/bantuan-str-002.png" alt="Data Capture Form" width="800px" style="border: 1px solid #ddd;"/>
  <br><em>Figure 4: Form capturing victim's name and phone number.</em>
</p>

<p align="center">
  <img src="https://raw.githubusercontent.com/ayiezola/ayiezola.github.io/master/assets/phishing-str/bantuan-str-023.png" alt="OTP Capture" width="800px" style="border: 1px solid #ddd;"/>
  <br><em>Figure 5: Form capturing victim's Telegram OTP.</em>
</p>

### Indicators of Compromise (IoCs)
* **URL:** `https://bantuanstr.infopublic.my.id/e/`
* **Name:** `[Insert Name]`
* **Phone Number:** `[Insert Phone Number]`
* **OTP:** `[Insert OTP]`

---

## 4. Technical Findings & Data Exfiltration

### A. Infrastructure Recon
Investigation of the domain `infopublic.my.id` revealed several technical red flags:
* **Registrar:** PT Digital Registra Indonesia.
* **Localization Errors:** Figure 6 shows the use of "Opsional," confirming an Indonesian origin for the phishing kit.
* **Favicon Spoofing:** Figure 7 shows the TA pulling the official favicon directly from `hasil.gov.my` to enhance visual trust.

### B. Server Misconfigurations (Content Exposure)
Due to poor server hardening, several internal directories were exposed:
* **Directory Indexing:** Figure 12 shows a full list of the phishing kit's files.
* **CPanel & Robots.txt:** Access to these files (Figures 10 & 11) helped map the attacker's infrastructure.

### C. The "Smoking Gun": Telegram Bot Interception
The most significant find was the exposure of the **Telegram Bot Token** (Figure 19). Analysis of the bot traffic revealed:
1. **Name & Phone Numbers:** Real-time harvesting of Malaysian citizen data.
2. **OTP Interception:** Active Man-in-the-Middle attacks on Telegram accounts.
3. **Collateral Damage:** The bot was also used to harvest **TikTok credentials** (Figure 26) and **private images** (Figure 23), suggesting a broader identity theft operation.

### Code Review

<p align="center">
  <img src="https://raw.githubusercontent.com/ayiezola/ayiezola.github.io/master/assets/phishing-str/bantuan-str-003.png" alt="OTP Capture" width="800px" style="border: 1px solid #ddd;"/>
  <br><em>Figure 6: Snippet of the source code showing "Opsional" (Indonesian spelling) instead of the Malaysian "Optional".</em>
</p>

**Analysis:** This suggests the phishing kit was either developed by an Indonesian-speaking threat actor or repurposed from a template originally targeting Indonesian banking/aid portals.

<p align="center">
  <img src="https://raw.githubusercontent.com/ayiezola/ayiezola.github.io/master/assets/phishing-str/bantuan-str-012.png" alt="OTP Capture" width="800px" style="border: 1px solid #ddd;"/>
  <br><em>Figure 7: TA Use Valid Favicon Icon From Official Domain (hasil.gov.my).</em>
</p>

<p align="center">
  <img src="https://raw.githubusercontent.com/ayiezola/ayiezola.github.io/master/assets/phishing-str/bantuan-str-004.png" alt="OTP Capture" width="800px" style="border: 1px solid #ddd;"/>
  <br><em>Figure 8: Error Configure Landing Page.</em>
</p>

<p align="center">
  <img src="https://raw.githubusercontent.com/ayiezola/ayiezola.github.io/master/assets/phishing-str/bantuan-str-005.png" alt="OTP Capture" width="800px" style="border: 1px solid #ddd;"/>
  <br><em>Figure 9: List of available path.</em>
</p>

<p align="center">
  <img src="https://raw.githubusercontent.com/ayiezola/ayiezola.github.io/master/assets/phishing-str/bantuan-str-006.png" alt="OTP Capture" width="800px" style="border: 1px solid #ddd;"/>
  <br><em>Figure 10: CPanel Login.</em>
</p>

<p align="center">
  <img src="https://raw.githubusercontent.com/ayiezola/ayiezola.github.io/master/assets/phishing-str/bantuan-str-007.png" alt="OTP Capture" width="800px" style="border: 1px solid #ddd;"/>
  <br><em>Figure 11: Robots.txt.</em>
</p>

<p align="center">
  <img src="https://raw.githubusercontent.com/ayiezola/ayiezola.github.io/master/assets/phishing-str/bantuan-str-008.png" alt="OTP Capture" width="800px" style="border: 1px solid #ddd;"/>
  <br><em>Figure 12: Content Expose via Page Indexing.</em>
</p>

<p align="center">
  <img src="https://raw.githubusercontent.com/ayiezola/ayiezola.github.io/master/assets/phishing-str/bantuan-str-011.png" alt="OTP Capture" width="800px" style="border: 1px solid #ddd;"/>
  <br><em>Figure 13: Checking Host IP Address.</em>
</p>

<p align="center">
  <img src="https://raw.githubusercontent.com/ayiezola/ayiezola.github.io/master/assets/phishing-str/bantuan-str-009.png" alt="OTP Capture" width="800px" style="border: 1px solid #ddd;"/>
  <br><em>Figure 14: Subdomain Recon.</em>
</p>

<p align="center">
  <img src="https://raw.githubusercontent.com/ayiezola/ayiezola.github.io/master/assets/phishing-str/bantuan-str-021.png" alt="OTP Capture" width="800px" style="border: 1px solid #ddd;"/>
  <br><em>Figure 15: Nameserver Info.</em>
</p>

<p align="center">
  <img src="https://raw.githubusercontent.com/ayiezola/ayiezola.github.io/master/assets/phishing-str/bantuan-str-026.png" alt="OTP Capture" width="800px" style="border: 1px solid #ddd;"/>
  <br><em>Figure 16: Fuzzing valid path.</em>
</p>

<p align="center">
  <img src="https://raw.githubusercontent.com/ayiezola/ayiezola.github.io/master/assets/phishing-str/bantuan-str-022.png" alt="OTP Capture" width="800px" style="border: 1px solid #ddd;"/>
  <br><em>Figure 17: Path Data Expose.</em>
</p>

### 5. JavaScript Configuration in index.php
Deep analysis of the `index.php` source code revealed a `window.setting` configuration object. This script serves as the "brain" of the phishing kit, managing data flow and psychological manipulation.

#### A. Command & Control (C2) Endpoint
* **Endpoint URL:** `https://berjaya66.my.id/bot/`
* **Analysis:** This is the **primary collector**. All captured data (Name, Phone Numbers, and OTPs) is POSTed to this URL. The use of a separate domain for the backend (`berjaya66.my.id`) allows the attacker to keep harvesting data even if the front-end landing page is taken down.

#### B. Geo-Targeting (Malaysia)
* **Attributes:** `COUNTRY_CODE: '+60'`, `COUNTRY_ID: 'my'`
* **Analysis:** The kit is hardcoded to target Malaysian citizens. The script automatically applies the Malaysian international dialing code, ensuring the stolen phone numbers are ready for the attacker to use for Telegram hijacking immediately.

#### C. Man-in-the-Middle (MitM) Timer Logic
* **Attributes:** `otp: { expirationTime: 300 }`
* **Analysis:** The script implements a 300-second (5-minute) countdown.
    * **Psychological Tactic:** This creates a false sense of urgency. By mimicking a legitimate bank or Telegram security timer, it pressures the victim into entering their OTP as quickly as possible, bypassing their natural suspicion.
    * **Technical Tactic:** The `expired: false` status indicates that the backend is actively listening for an incoming OTP session from the victim's device.

<p align="center">
  <img src="https://raw.githubusercontent.com/ayiezola/ayiezola.github.io/master/assets/phishing-str/bantuan-str-030.png" alt="OTP Capture" width="800px" style="border: 1px solid #ddd;"/>
  <br><em>Figure 18: Analysis of the hardcoded window.setting configuration.</em>
</p>

### 6. Deep Dive: Telegram Hijacking Logic (`otp-controller.js`)
Analysis of the `handleOtp` function confirms a highly sophisticated **Man-in-the-Middle (MitM)** attack designed for Telegram account takeover.

#### A. Technical Features:
* **Session Pairing:** The script harvests `phone_code_hash` from the browser's storage. This value is required to complete the login process on the attacker's server.
* **2FA Detection:** The code includes logic to detect if the victim has **Two-Factor Authentication** enabled. If `needs_2fa` is returned from the C2 server, the UI dynamically switches to harvest the victim's Cloud Password.
* **Data Exfiltration:** Data is packaged into a JSON object and sent via a custom `sendMessageToTelegram` function.

**Captured Data Payload:**
```json
{
  "code": "Stolen_OTP",
  "phone_number": "Victim_Number",
  "session_id": "Session_ID",
  "phone_code_hash": "Telegram_Internal_Hash"
}
```

<p align="center">
  <img src="https://raw.githubusercontent.com/ayiezola/ayiezola.github.io/master/assets/phishing-str/bantuan-str-031.png" alt="OTP Capture" width="800px" style="border: 1px solid #ddd;"/>
  <br><em>Figure 19: Logic within otp-controller.js showing the extraction of the phone_code_hash—a critical component for bypassing Telegram security.</em>
</p>

### 7. Multi-Domain Phishing Campaign (2026 Expansion)
Our investigation has uncovered that the threat actor (TA) is not relying on a single URL. A second, identical phishing page has been identified, indicating a wider coordinated campaign targeting Malaysians.

#### Newly Identified Asset:
* **Secondary URL:** `[https://umrahpercumah.infopublic.my.id/a/]`
* **Status:** Active

#### Comparative Analysis
Both sites share the same codebase, design, and exfiltration logic (Telegram-based OTP harvesting). This "mirroring" tactic is used to:
1.  **Redundancy:** Maintain uptime if the primary `infopublic.my.id`.

<p align="center">
  <img src="https://raw.githubusercontent.com/ayiezola/ayiezola.github.io/master/assets/phishing-str/bantuan-str-010.png" alt="OTP Capture" width="800px" style="border: 1px solid #ddd;"/>
  <br><em>Figure 20: Found another Phishing Page for 2026.</em>
</p>

<p align="center">
  <img src="https://raw.githubusercontent.com/ayiezola/ayiezola.github.io/refs/heads/master/assets/phishing-str/bantuan-str-028.png" alt="OTP Capture" width="800px" style="border: 1px solid #ddd;"/>
  <br><em>Figure 21: Threat Actor Telegram Token Expose.</em>
</p>

<p align="center">
  <img src="https://raw.githubusercontent.com/ayiezola/ayiezola.github.io/master/assets/phishing-str/bantuan-str-019.png" alt="OTP Capture" width="800px" style="border: 1px solid #ddd;"/>
  <br><em>Figure 22: Bot Info.</em>
</p>

<p align="center">
  <img src="https://raw.githubusercontent.com/ayiezola/ayiezola.github.io/master/assets/phishing-str/bantuan-str-020.png" alt="OTP Capture" width="800px" style="border: 1px solid #ddd;"/>
  <br><em>Figure 23: Bot Info.</em>
</p>

<p align="center">
  <img src="https://raw.githubusercontent.com/ayiezola/ayiezola.github.io/master/assets/phishing-str/bantuan-str-013.png" alt="OTP Capture" width="800px" style="border: 1px solid #ddd;"/>
  <br><em>Figure 24: Capturing Previous Log / Content From Telegram Bot Token.</em>
</p>

<p align="center">
  <img src="https://raw.githubusercontent.com/ayiezola/ayiezola.github.io/master/assets/phishing-str/bantuan-str-015.png" alt="OTP Capture" width="800px" style="border: 1px solid #ddd;"/>
  <br><em>Figure 25: Captured Image.</em>
</p>

<p align="center">
  <img src="https://raw.githubusercontent.com/ayiezola/ayiezola.github.io/master/assets/phishing-str/bantuan-str-024.png" alt="OTP Capture" width="800px" style="border: 1px solid #ddd;"/>
  <br><em>Figure 26: Image That Has Been Upload to Phishing Page.</em>
</p>

<p align="center">
  <img src="https://raw.githubusercontent.com/ayiezola/ayiezola.github.io/master/assets/phishing-str/bantuan-str-016.png" alt="OTP Capture" width="800px" style="border: 1px solid #ddd;"/>
  <br><em>Figure 27: Captured Phone Number and OTP.</em>
</p>

<p align="center">
  <img src="https://raw.githubusercontent.com/ayiezola/ayiezola.github.io/master/assets/phishing-str/bantuan-str-018.png" alt="OTP Capture" width="800px" style="border: 1px solid #ddd;"/>
  <br><em>Figure 28: Capturing Tiktok Account Credentials.</em>
</p>

<p align="center">
  <img src="https://raw.githubusercontent.com/ayiezola/ayiezola.github.io/master/assets/phishing-str/bantuan-str-017.png" alt="OTP Capture" width="800px" style="border: 1px solid #ddd;"/>
  <br><em>Figure 29: Image Captured Showing Other Phishing Page.</em>
</p>
---

## 5. Prevention & Reporting
**Official Links:**
* Official STR Portal: [https://bantuantunai.hasil.gov.my/](https://bantuantunai.hasil.gov.my/)

**How to Report:**
1. Call **NSRC (National Scam Response Centre)** at **997**.
2. Report the URL via **Google Safe Browsing**.
3. Use the **SemakMule** portal by PDRM.

<p align="center">
  <img src="https://raw.githubusercontent.com/ayiezola/ayiezola.github.io/master/assets/phishing-str/bantuan-str-029.png" alt="OTP Capture" width="800px" style="border: 1px solid #ddd;"/>
  <br><em>Figure 30: Done submit report to google safe browsing.</em>
</p>
---

[Back to Home](https://ayiezola.github.io/)
