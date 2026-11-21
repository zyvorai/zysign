# ZySign — macOS DSC Toolkit for MCA21 V3

> Sign MCA eForms directly from your Mac using a USB DSC token — no eMudhra emBridge required.

**All signing is local. PINs never leave your machine. No uploads. No cloud.**

---

## What is ZySign?

The MCA21 V3 portal (`efiling.mca.gov.in`) requires a local signing daemon to
communicate with your USB DSC token. ZySign replaces the proprietary eMudhra
emBridge daemon entirely, with two modes:

- **ZySign** — desktop app for signing PDFs directly, no browser needed
- **ZySign Bridge** — background service that lets the MCA portal sign through your browser, exactly as before

---

## Requirements

- macOS 13 Ventura or later (Apple Silicon or Intel)
- A USB DSC token with its driver installed (see [Token Setup](#token-setup) below)

No Python, Xcode, or Homebrew required. ZySign bundles everything it needs.

---

## Install

### DMG (drag to Applications)

1. Download the latest `ZySign-*.dmg` from the [Releases](https://github.com/zyvorai/zysign/releases) page
2. Open the DMG and drag **ZySign** to `/Applications`
3. If Gatekeeper warns, allow it in **System Settings › Privacy & Security**
4. Open `ZySign.app`

---

## Token Setup

Install the PKCS#11 driver for your token **before** launching ZySign:

| Token | Driver file | Download |
|-------|-------------|----------|
| **mToken CryptoID** | `libcryptoid_pkcs11.dylib` | [MCA21 DSC driver page](https://efiling.mca.gov.in/efilingapp/assets/downloads/dscdriver.htm) — use the Apple Silicon build on M-series Macs |
| **ePass 2003** (Feitian) | `libePass2003.dylib` | [ftsafe.com/downloads](https://www.ftsafe.com/en/downloads) → macOS PKI Client |
| **ProxKey / Watchdata** | `libwdpkcs.dylib` | [proxkey.in](https://proxkey.in) → Downloads → macOS |
| **Generic (OpenSC)** | `opensc-pkcs11.so` | `brew install opensc` |

After installing, verify the driver is in place:

```bash
# mToken example — adjust the filename for your token
ls /usr/local/lib/libcryptoid_pkcs11.dylib
```

---

## Using ZySign (Desktop PDF Signing)

1. Plug in your USB token
2. Open `ZySign.app`
3. **Step 1 — Token:** your token name appears once detected
4. **Step 2 — PDF:** click *Open PDF* and select your MCA eFiling form
5. **Step 3 — Sign:** enter your PIN and click *Sign*

The signed PDF is saved alongside the original as `<filename>_MCA_SIGNED.pdf`.

---

## Using ZySign Bridge (Browser-based MCA Signing)

ZySign Bridge replaces the eMudhra emBridge daemon so the MCA portal works in
your browser without any changes to your workflow.

1. Open `ZySign.app` and navigate to **Bridge** in the sidebar
2. Click **Start Bridge** — it stops any running emBridge automatically
3. Visit `efiling.mca.gov.in` and sign as normal

The bridge listens on `https://localhost.emudhra.com:26769`, the same address the
MCA portal expects.

---

## Security

| Property | Guarantee |
|----------|-----------|
| PIN | Never stored, never logged, discarded immediately after signing |
| Private key | Never extracted from hardware — signing is delegated to the token via PKCS#11 `C_Sign` |
| Document data | Never sent off-device |
| PIN transport | AES-256-CBC encrypted over localhost HTTPS only |

---

## Pricing and trial

| Plan | INR incl. 18% GST (India) | US dollars excl. tax (elsewhere) |
|------|---------------------------|----------------------------------|
| Per user, per year | 999 | about 10 |
| Firm Starter (up to 5 Macs), per year | 3,499 | about 35 |
| Firm Practice (up to 25 Macs), per year | 14,999 | about 151 |

Free 30-day trial: write to [sales@zyvor.dev](mailto:sales@zyvor.dev) for a trial key. No data is lost when it ends; a commercial license is required to keep using it. Buy: [zyvor.dev/buy](https://zyvor.dev/buy#zysign). Prices: INR incl. 18% GST in India; US dollars excl. tax elsewhere (VAT, GST or sales tax added where applicable). Standard orders: pay online with Razorpay (UPI, card, netbanking). Bigger orders, team and volume orders, and customers outside India: we send a US-dollar invoice and you pay through Skydo (international bank transfer, or card/ACH through a secure Skydo link). Custom pricing: contact sales@zyvor.dev.

---

## Uninstall

```bash
# Remove the app
sudo rm -rf /Applications/ZySign.app

# Remove data files
rm -rf ~/Library/Application\ Support/com.zyvor.zysign
rm -f  ~/Library/Preferences/com.zyvor.zysign.plist
sudo rm -rf /Library/ZySignBridge
```

---

## License

Proprietary — Zyvor. All rights reserved.
