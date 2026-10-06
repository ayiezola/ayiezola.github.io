# GitHub Pages publish kit — WhatsApp ATO phishing write-up

Matches the format of `2026-03-09-Phishing-Page-Semakan-Tunai-Rahmah` (Beautiful Jekyll).

## 1. Files to copy into your repo

| Local (this folder) | → Repo path |
|---|---|
| `_posts/2026-10-06-WhatsApp-Account-Takeover-Phishing.md` | `_posts/2026-10-06-WhatsApp-Account-Takeover-Phishing.md` |
| `assets/phishing-whatsapp-ato/*` | `assets/phishing-whatsapp-ato/*` |

Then:
```bash
cd ayiezola.github.io
git add _posts/2026-10-06-WhatsApp-Account-Takeover-Phishing.md assets/phishing-whatsapp-ato
git commit -m "Add WhatsApp Linked-Devices ATO phishing analysis"
git push
```
The post URL becomes:
`https://ayiezola.github.io/2026-10-06-WhatsApp-Account-Takeover-Phishing/`

> Note: your STR post had **no** front matter (relies on `_config.yml` defaults); this draft
> **includes** front matter (`layout: post`, title, tags) like your 2024 APK post. Both work —
> keep whichever you prefer.

## 2. Figure → file map

| Figure | File | What it shows | Status |
|---|---|---|---|
| 1 | `wa-ato-001.png` | Lure SMS received | ⬅️ **YOU supply** (screenshot your phone) |
| 2 | `wa-ato-002.png` | Landing page (Chinese) | ✅ |
| 3 | `wa-ato-003.jpg` | Landing page (English) | ✅ |
| 4 | `wa-ato-004.jpg` | Kit — iPhone | ✅ |
| 5 | `wa-ato-005.jpg` | Kit — Android | ✅ |
| 6 | `wa-ato-011.png` | Operator panel login (captured 2026-10-06) | ✅ |
| 7 | `wa-ato-012.png` | Open `/assets/` directory listing (2026-10-06) | ✅ |
| 8 | `wa-ato-013.png` | Origin-IP exposure / Cloudflare bypass (evidence card) | ✅ |
| 9 | `wa-ato-014.png` | Registrar clientHold / NXDOMAIN (evidence card) | ✅ |
| 10 | `wa-ato-015.png` | TLS wildcard certificate (evidence card) | ✅ |
| — | `wa-ato-006..008.jpg` | en-US / es-ES / zh-TW locale variants | ✅ |
| — | `wa-ato-009.png` | Full-page kit render (hero image) | ✅ |
| — | `wa-ato-010.jpg` | Kit asset (`mango-logo`) | ✅ |

**Only Figure 1 is missing** — everything else is in `assets/phishing-whatsapp-ato/`.

## 3. Evidence provenance
- Locale + landing screenshots: 2026-10-04 (victim-side, safe).
- Panel login + `/assets/` listing: captured 2026-10-06 15:24 UTC from analysis box (Playwright, headless).
- Origin/RDAP/cert cards: generated 2026-10-06 15:25 UTC, live queries.
- No credentials submitted; no victim data accessed.

## 4. Integrity manifest
Generate before publishing:
```bash
cd assets/phishing-whatsapp-ato
sha256sum * > ../phishing-whatsapp-ato.sha256
```
