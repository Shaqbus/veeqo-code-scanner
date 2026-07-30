# SB Transparency Scanner — Chrome Extension

**Version:** 2.2  
**Author:** Shannon Bush, Veeqo Enterprise Partnerships  
**Last Updated:** July 30, 2026

---

## What It Does

A Chrome extension that automates Amazon Transparency and Serial Number code compliance inside Veeqo. It detects products requiring codes by reading `[TP]`, `[SN]`, or `[SN2]` identifiers in the **product title**, then provides an in-app scanning workflow and one-click flat file export for Seller Central.

### Key Capabilities

- **Auto-detection** — Reads product titles for `[TP]`, `[SN]`, `[SN2]` identifiers (not tags or metadata — the actual title text)
- **Barcode scanning** — Works with any USB/Bluetooth scanner; hands-free workflow with Enter-to-advance
- **Progress badges** — Color-coded per line item: Gray (0 scanned), Orange (partial), Green (complete)
- **Ship block** — Prevents label purchase until all required codes are captured
- **Flat file export** — One-click export with carrier mapping, ship method, split shipment support
- **Per-item tracking** — Multi-SKU orders tracked independently

---

## Installation

1. Click the **Chrome extension link** provided by your Amazon rep
2. Click "Add to Chrome" → confirm the install
3. Click the SB icon in your toolbar → paste your Veeqo API key → Save
4. Reload your Veeqo tab

> **No folder downloads, no Developer Mode.** Install is a direct Chrome extension link.

Updates are delivered automatically via Chrome — no manual file replacement needed.

---

## Product Title Setup

The extension identifies items needing codes by looking for identifiers **in the product title** (not tags, not descriptions, not SKU fields).

| If item needs... | Add to product title | Example |
|------------------|---------------------|---------|
| Transparency codes | `[TP]` | `Nexen Aria AH7 Tire (205/65R16, 95H) [TP]` |
| Serial numbers (1/unit) | `[SN]` | `DJI Mini 4 Pro Drone [SN]` |
| Serial numbers (2/unit) | `[SN2]` | `iPhone 16 Pro Max [SN2]` |

**Where:** Veeqo → Inventory → Find product → Edit → Product Title

---

## Included Documentation

| File | Purpose |
|------|---------|
| `SB-Transparency-Prep-Guide-v2.2.html` | Setup walkthrough: product titles, API key, install, scanner config |
| `SB-Transparency-Help-Guide-v2.2.html` | Full feature reference: workflow, export, carriers, troubleshooting |
| `SB-Transparency-Privacy-Policy.html` | Privacy policy for Chrome Web Store listing |
| `Veeqo Code Scanner Install Guide (1).html` | Original install guide (legacy — superseded by Prep Guide) |
| `Veeqo Code Scanner Launch Announcement (1).html` | Internal launch announcement |
| `Veeqo Code Scanner Prep Guide (1).html` | Original prep guide (legacy — superseded by v2.2) |

---

## Extension Files

| File | Role |
|------|------|
| `manifest.json` | Chrome extension manifest (permissions, content scripts) |
| `background.js` | Service worker — handles API calls to Veeqo |
| `content.js` | Injected into Veeqo UI — badges, scanning modal, export |
| `popup.html` / `popup.js` | Extension popup — API key entry |
| `styles.css` | Injected styles for badges and modals |
| `icon.png` | Extension toolbar icon |

---

## How It Works (Technical)

1. `content.js` scans the Veeqo orders page DOM for product titles containing `[TP]`, `[SN]`, or `[SN2]`
2. Injects color-coded badges showing scan progress per line item
3. On badge click → opens scanning modal → captures barcode input
4. Validates code format (AZ:/ZA: for TP, SN:/SN2: for serial numbers)
5. Stores scanned codes per order-item via Veeqo API (order notes/tags)
6. Export function queries shipped orders → generates tab-delimited flat file with carrier/method mapping

---

## Code Format Validation

| Type | Required Format | Example |
|------|----------------|---------|
| Transparency (TP) | `AZ:` or `ZA:` + 20 alphanumeric chars | `AZ:CT8TW5XJY7AVA0PTNIKA47UMGF` |
| Serial Number (SN) | `SN:` + 3 chars minimum | `SN:ABC123456` |
| Serial Number 2 (SN2) | `SN2:` + 3 chars minimum | `SN2:XYZ789` |

---

## Changelog

### v2.2 — July 2026
- **FIX:** Badge no longer marks all items complete when one code is scanned
- **FIX:** SKU correctly identified per line item
- **NEW:** Ship method mapping (Veeqo IDs → human-readable names)
- **FIX:** Per-line-item cache keys prevent cross-contamination
- **FIX:** lineItemId regex matches all Veeqo UI patterns

### v2.1 — May 2026
- **NEW:** Hands-free scanning (Enter to upload, Enter to next order)
- **FIX:** Split shipment exports use correct order-item-id per allocation
- **NEW:** 5x faster badge loading via parallel API calls
- **NEW:** Export uses shipped_at filter for date range

---

## Security & Privacy

- API key stored on-device only (Chrome local storage)
- Network calls only to `api.veeqo.com`
- Zero telemetry or analytics
- Order data never stored locally beyond session
- Exports generated client-side
- Permissions scoped to `app.veeqo.com` only

---

## Support

Contact: Shannon Bush (shaqbus@amazon.com)  
Team: CDE-BIE, Veeqo Enterprise Partnerships
