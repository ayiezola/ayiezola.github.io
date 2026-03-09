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

### A. Landing Page Impersonation
The page uses the official LHDN and Malaysia Madani logos to create a false sense of authority.

<p align="center">
  <img src="https://github.com/ayiezola/ayiezola.github.io/blob/master/assets/phishing-str/bantuan-str-001.png" alt="Phishing Landing Page" width="600px" style="border: 1px solid #ddd;"/>
  <br><em>Figure 1: Main landing page mimicry.</em>
</p>

### B. Data Harvest Form
The form asks for the user's NRIC (MyKad) number. Once submitted, it likely redirects to a fake banking selector or a credential harvesting page.

<p align="center">
  <img src="./images/form-submission.png" alt="Data Capture Form" width="600px" style="border: 1px solid #ddd;"/>
  <br><em>Figure 2: Form capturing NRIC data.</em>
</p>

---

## 4. Technical Findings

### Domain Infrastructure
Using `whois` and DNS lookups, we identified the following:
* **Registrar:** [Insert Registrar Name, e.g., PANDI]
* **Hosting:** [e.g., Cloudflare / Hostinger]
* **SSL Status:** Valid (Let's Encrypt), used to trick users into seeing the "Padlock" icon.

### Indicators of Compromise (IoCs)
* **URL:** `https://bantuanstr.infopublic.my.id/e/`
* **IP:** `[Insert IP Address]`

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
