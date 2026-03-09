# 🚨 Phishing Analysis: Fake STR Checking Page
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

## 2. Threat Intelligence
| Attribute | Details |
| :--- | :--- |
| **Phishing URL** | `https://bantuanstr.infopublic.my.id/e/` |
| **Primary Domain** | `infopublic.my.id` |
| **TLD Analysis** | `.my.id` (Indonesian TLD - a major red flag for MY gov sites) |
| **Theme** | "Semakan STR 2026" |
| **Status** | **ACTIVE** |

---

## 3. Visual Analysis & Proofs

### Delivery Method & Social Engineering
The threat actor uses Telegram Group to deliver phishing link. TA also put some captions to increase the link's trustworthiness.

<p align="center">
  <img src="https://github.com/ayiezola/ayiezola.github.io/blob/master/assets/phishing-str/bantuan-str-025.png" alt="Phishing Landing Page" width="800px" style="border: 1px solid #ddd;"/>
  <br><em>Figure 1: Threat Actor Spread Phishing Link In Telegram Group.</em>
</p>

### A. Landing Page Impersonation
The page uses the official LHDN and Malaysia Madani logos to create a false sense of authority.

<p align="center">
  <img src="https://github.com/ayiezola/ayiezola.github.io/blob/master/assets/phishing-str/bantuan-str-001.png" alt="Phishing Landing Page" width="800px" style="border: 1px solid #ddd;"/>
  <br><em>Figure 1: Main landing page.</em>
</p>

The TA may post fake testimonials or 'success' messages in the group to convince others the link is safe

<p align="center">
  <img src="https://github.com/ayiezola/ayiezola.github.io/blob/master/assets/phishing-str/bantuan-str-027.png" alt="Phishing Landing Page" width="800px" style="border: 1px solid #ddd;"/>
  <br><em>Figure 1: Fake Testimonials.</em>
</p>

### B. Data Harvest & Account Takeover Form
The phishing flow transitions from simple data collection to an active account hijacking attempt:

1. **Phone Number Collection:** The form first requests the victim's mobile phone number.
2. **Real-time Exploitation:** Once the number is submitted, the backend triggers a legitimate Telegram login request to the victim's device.
3. **OTP Interception:** The victim is redirected to a second page prompting them to enter the OTP code sent to their Telegram account.

<div style="background-color: #fff3cd; border-left: 6px solid #ffecb5; padding: 15px; margin: 20px 0; color: #856404;">
  <strong>Note:</strong> This is a classic "Man-in-the-Middle" (MitM) technique. If the victim provides the OTP, the attacker gains full access to their Telegram account, contacts, and private messages.
</div>

<p align="center">
  <img src="https://github.com/ayiezola/ayiezola.github.io/blob/master/assets/phishing-str/bantuan-str-002.png" alt="Data Capture Form" width="800px" style="border: 1px solid #ddd;"/>
  <br><em>Figure 2: Form capturing victim's name and phone number.</em>
</p>

<p align="center">
  <img src="https://github.com/ayiezola/ayiezola.github.io/blob/master/assets/phishing-str/bantuan-str-023.png" alt="OTP Capture" width="800px" style="border: 1px solid #ddd;"/>
  <br><em>Figure 3: Form capturing victim's Telegram OTP.</em>
</p>

---

## 4. Technical Findings

### Domain Infrastructure
Using `whois` and DNS lookups, we identified the following:
* **Registrar:** [PT Digital Registra Indonesia]
* **CDN:** [Cloudflare]
* **SSL Status:** Valid (Let's Encrypt), used to trick users into seeing the "Padlock" icon.

### Indicators of Compromise (IoCs)
* **URL:** `https://bantuanstr.infopublic.my.id/e/`
* **Name:** `[Insert Name]`
* **Phone Number:** `[Insert Phone Number]`
* **OTP:** `[Insert OTP]`

---

## 5. Prevention & Reporting
**Official Links:**
* Official STR Portal: [https://bantuantunai.hasil.gov.my/](https://bantuantunai.hasil.gov.my/)

**How to Report:**
1. Call **NSRC (National Scam Response Centre)** at **997**.
2. Report the URL via **Google Safe Browsing**.
3. Use the **SemakMule** portal by PDRM.

---

[Back to Home](https://ayiezola.github.io/)

<p align="center">
  <img src="https://github.com/ayiezola/ayiezola.github.io/blob/master/assets/phishing-str/bantuan-str-003.png" alt="OTP Capture" width="800px" style="border: 1px solid #ddd;"/>
  <br><em>Figure 3: Find Language Slank in Source Code Review.</em>
</p>

<p align="center">
  <img src="https://github.com/ayiezola/ayiezola.github.io/blob/master/assets/phishing-str/bantuan-str-004.png" alt="OTP Capture" width="800px" style="border: 1px solid #ddd;"/>
  <br><em>Figure 3: Error Configuration of Landing Page.</em>
</p>

<p align="center">
  <img src="https://github.com/ayiezola/ayiezola.github.io/blob/master/assets/phishing-str/bantuan-str-005.png" alt="OTP Capture" width="800px" style="border: 1px solid #ddd;"/>
  <br><em>Figure 3: List of available path.</em>
</p>

<p align="center">
  <img src="https://github.com/ayiezola/ayiezola.github.io/blob/master/assets/phishing-str/bantuan-str-006.png" alt="OTP Capture" width="800px" style="border: 1px solid #ddd;"/>
  <br><em>Figure 3: CPanel Login.</em>
</p>

<p align="center">
  <img src="https://github.com/ayiezola/ayiezola.github.io/blob/master/assets/phishing-str/bantuan-str-007.png" alt="OTP Capture" width="800px" style="border: 1px solid #ddd;"/>
  <br><em>Figure 3: Robots.txt.</em>
</p>

<p align="center">
  <img src="https://github.com/ayiezola/ayiezola.github.io/blob/master/assets/phishing-str/bantuan-str-008.png" alt="OTP Capture" width="800px" style="border: 1px solid #ddd;"/>
  <br><em>Figure 3: Content expose with page indexing.</em>
</p>

<p align="center">
  <img src="https://github.com/ayiezola/ayiezola.github.io/blob/master/assets/phishing-str/bantuan-str-009.png" alt="OTP Capture" width="800px" style="border: 1px solid #ddd;"/>
  <br><em>Figure 3: Subdomain Recon.</em>
</p>

<p align="center">
  <img src="https://github.com/ayiezola/ayiezola.github.io/blob/master/assets/phishing-str/bantuan-str-010.png" alt="OTP Capture" width="800px" style="border: 1px solid #ddd;"/>
  <br><em>Figure 3: Found another Phishing Page for 2026.</em>
</p>

<p align="center">
  <img src="https://github.com/ayiezola/ayiezola.github.io/blob/master/assets/phishing-str/bantuan-str-011.png" alt="OTP Capture" width="800px" style="border: 1px solid #ddd;"/>
  <br><em>Figure 3: Checking Host IP Address.</em>
</p>

<p align="center">
  <img src="https://github.com/ayiezola/ayiezola.github.io/blob/master/assets/phishing-str/bantuan-str-012.png" alt="OTP Capture" width="800px" style="border: 1px solid #ddd;"/>
  <br><em>Figure 3: Favicon icon.</em>
</p>

<p align="center">
  <img src="https://github.com/ayiezola/ayiezola.github.io/blob/master/assets/phishing-str/bantuan-str-013.png" alt="OTP Capture" width="800px" style="border: 1px solid #ddd;"/>
  <br><em>Figure 3: Capturing Previous Log / Content From Telegram Bot Token.</em>
</p>

<p align="center">
  <img src="https://github.com/ayiezola/ayiezola.github.io/blob/master/assets/phishing-str/bantuan-str-024.png" alt="OTP Capture" width="800px" style="border: 1px solid #ddd;"/>
  <br><em>Figure 3: Image That Has Been Upload to Phishing Page.</em>
</p>

<p align="center">
  <img src="https://github.com/ayiezola/ayiezola.github.io/blob/master/assets/phishing-str/bantuan-str-015.png" alt="OTP Capture" width="800px" style="border: 1px solid #ddd;"/>
  <br><em>Figure 3: Captured Image.</em>
</p>

<p align="center">
  <img src="https://github.com/ayiezola/ayiezola.github.io/blob/master/assets/phishing-str/bantuan-str-016.png" alt="OTP Capture" width="800px" style="border: 1px solid #ddd;"/>
  <br><em>Figure 3: Captured Phone Number and OTP.</em>
</p>

<p align="center">
  <img src="https://github.com/ayiezola/ayiezola.github.io/blob/master/assets/phishing-str/bantuan-str-017.png" alt="OTP Capture" width="800px" style="border: 1px solid #ddd;"/>
  <br><em>Figure 3: Image Captured Showing Other Phishing Page.</em>
</p>

<p align="center">
  <img src="https://github.com/ayiezola/ayiezola.github.io/blob/master/assets/phishing-str/bantuan-str-018.png" alt="OTP Capture" width="800px" style="border: 1px solid #ddd;"/>
  <br><em>Figure 3: Capturing Tiktok Account Credentials.</em>
</p>

<p align="center">
  <img src="https://github.com/ayiezola/ayiezola.github.io/blob/master/assets/phishing-str/bantuan-str-019.png" alt="OTP Capture" width="800px" style="border: 1px solid #ddd;"/>
  <br><em>Figure 3: Bot Info.</em>
</p>

<p align="center">
  <img src="https://github.com/ayiezola/ayiezola.github.io/blob/master/assets/phishing-str/bantuan-str-020.png" alt="OTP Capture" width="800px" style="border: 1px solid #ddd;"/>
  <br><em>Figure 3: Bot Info.</em>
</p>

<p align="center">
  <img src="https://github.com/ayiezola/ayiezola.github.io/blob/master/assets/phishing-str/bantuan-str-021.png" alt="OTP Capture" width="800px" style="border: 1px solid #ddd;"/>
  <br><em>Figure 3: Nameserver Info.</em>
</p>

<p align="center">
  <img src="https://github.com/ayiezola/ayiezola.github.io/blob/master/assets/phishing-str/bantuan-str-022.png" alt="OTP Capture" width="800px" style="border: 1px solid #ddd;"/>
  <br><em>Figure 3: Path DATA Expose.</em>
</p>
