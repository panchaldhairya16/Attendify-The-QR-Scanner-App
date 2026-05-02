<div align="center">

<img src="https://img.shields.io/badge/Attendify-QR%20Attendance-6366f1?style=for-the-badge&logo=qrcode&logoColor=white" alt="Attendify"/>

# 📋 Attendify — QR Attendance System

**Zero-backend QR attendance tracking — runs entirely in your browser**

[![HTML](https://img.shields.io/badge/HTML-Single%20File-e34c26?style=flat-square&logo=html5&logoColor=white)](./index.html)
[![JavaScript](https://img.shields.io/badge/JavaScript-Vanilla%20ES2020-f7df1e?style=flat-square&logo=javascript&logoColor=black)](./index.html)
[![No Server](https://img.shields.io/badge/Backend-None%20Required-22c55e?style=flat-square)](#)
[![License](https://img.shields.io/badge/License-MIT-8b5cf6?style=flat-square)](./LICENSE)
[![localStorage](https://img.shields.io/badge/Storage-localStorage-f59e0b?style=flat-square)](#)
[![Mobile](https://img.shields.io/badge/Mobile-Friendly-3b82f6?style=flat-square)](#)

[▶ Live Demo](#demo) · [⬇ Quick Start](#-quick-start) · [✦ Features](#-features) · [❓ FAQ](#-faq)

</div>

---

## 📑 Table of Contents

1. [Overview](#-overview)
2. [Demo Preview](#-demo-preview)
3. [Features](#-features)
4. [Supported QR Formats](#-supported-qr-formats)
5. [How It Works](#-how-it-works)
6. [Quick Start](#-quick-start)
7. [Usage Guide](#-usage-guide)
8. [File Structure](#-file-structure)
9. [Tech Stack](#-tech-stack)
10. [FAQ](#-faq)

---

## 🔍 Overview

| | |
|---|---|
| 📦 **Dependencies** | 0 (zero) |
| 📄 **Files** | 1 HTML file |
| 🔌 **Backend** | None required |
| 💾 **Storage** | Browser localStorage |
| 📱 **Mobile** | Fully responsive |
| 📥 **Export** | CSV download |

**Attendify** is a fully client-side QR-code attendance system built as a single HTML file. It requires no server, no database, and no installation. Drop it on a device, open it in a browser, and start scanning — attendee records persist via `localStorage` and can be exported as CSV at any time.

Perfect for:
- 🎪 Community meetups & hackathons (e.g. GDG, Flutter, DevFest)
- 🏫 Classrooms and workshops
- 🏢 Corporate events and conferences
- 🎟️ Any event needing quick, offline-capable check-in

---

## 🖥️ Demo Preview

```
┌─────────────────────────────────────────────────────┐
│  ● ● ●          localhost:8080/attendify             │
├─────────────────────────────────────────────────────┤
│  📋 Attendify          QR Attendance System  ● Live  │
│                                                      │
│  ┌─────────────────────────────────────────────┐    │
│  │  📷 Camera Scan │ 🖼️ Scan Image │ ✏️ Manual  │    │
│  │─────────────────────────────────────────────│    │
│  │                                             │    │
│  │      [ Show QR code to camera ]             │    │
│  │                                             │    │
│  │         ┌──────────────────┐                │    │
│  │         │  ┌─┐          ┌─┐│                │    │
│  │         │  └─┘  [ QR ]  └─┘│                │    │
│  │         │    ─────────────  │                │    │
│  │         └──────────────────┘                │    │
│  └─────────────────────────────────────────────┘    │
│                                                      │
│  Attendance Log  [3]              ⬇️ Download CSV    │
│  ┌───────────────┬────────────┬────────────────┐    │
│  │ Email         │ Group      │ Time           │    │
│  ├───────────────┼────────────┼────────────────┤    │
│  │ priya@gdg.com │ GDG Ahmd   │ 10:32:15 AM    │    │
│  │ rahul@flutter │ Flutter IN │ 10:30:02 AM    │    │
│  └───────────────┴────────────┴────────────────┘    │
└─────────────────────────────────────────────────────┘
```

---

## ✦ Features

### 📷 Live Camera Scanning
Uses the device rear camera to scan QR codes in real-time at 15fps. Includes an animated scan-line overlay, corner bracket guides, and a green flash on successful scan.

### 🖼️ Image / Screenshot Scan
Upload a PNG, JPG, or GIF containing a QR code. Useful when camera is unavailable or HTTPS isn't configured. Works on `file://` URLs too.

### ✏️ Manual Entry
Add attendees by typing email and event group directly. Supports `Enter` key shortcut and one-click demo data chips for quick testing.

### 💾 Persistent Storage
Records are auto-saved to `localStorage` on every check-in and restored on page reload — no data loss on refresh or accidental tab close.

### 🔒 Duplicate Prevention
Each email can only be checked in once per session. Duplicate scans trigger a yellow warning toast — no duplicate records are ever created.

### 📥 CSV Export
Download a properly double-quoted CSV with **Email, Group, Date, Time** columns — ready for Google Sheets or Excel. Named `attendify_YYYY-MM-DD.csv`.

---

## 📄 Supported QR Formats

Attendify auto-detects these payload formats (tried in order):

| Priority | Format | Example |
|----------|--------|---------|
| 1 | **JSON Object** | `{"email":"x@y.com","group":"Dev Team"}` |
| 2 | **Key-Value Text** | `email: user@gmail.com\ngroup: GDG Ahmedabad` |
| 3 | **Plain Email** | `user@domain.com` |
| 4 | **UPI / GPay** | `upi://pay?pa=id@upi&pn=Name` |
| 5 | **Generic URL** | `https://example.com?email=x@y.com` |
| 6 | **Raw Fallback** | Any plain text / unknown content |

> ✅ **Tip:** The recommended format is JSON — it reliably carries both email and group in one QR code.

---

## ⚡ How It Works

```
Open index.html → Load localStorage → Scan / Enter QR → Parse Content → Log Attendee → Export CSV
```

**Step-by-step flow:**

1. **QR Scanned / Manual Input** — The raw QR string is captured via the Html5Qrcode library (camera or file upload) or from the manual entry form.

2. **Content Parsed** — `parseQRContent()` tries JSON → key:value → bare email → UPI URL → raw fallback, extracting `email` and `group`.

3. **Duplicate Check** — Email is checked against existing records. If already present, a warning toast fires and a 1.5s cooldown is set.

4. **Record Added** — A timestamped record is prepended to the live table with a slide-in animation. The badge count updates and a 2.5s cooldown prevents double-scans.

5. **Persisted to localStorage** — All records are serialised as ISO date strings and saved under the `attendify-records` key on every change.

---

## 🚀 Quick Start

> Camera scanning requires a **secure context** (HTTPS or localhost). Choose one option:

### Option A — Python HTTP Server

```bash
# Clone or download the repo
git clone https://github.com/yourusername/attendify.git
cd attendify

# Start local server
python3 -m http.server 8080

# Open in browser → http://localhost:8080
```

### Option B — Node / npx

```bash
npx serve .
# → http://localhost:3000
```

### Option C — GitHub Pages *(Recommended)*

```bash
# Push to GitHub
# → Settings → Pages → Deploy from main branch
# → Live at: https://yourusername.github.io/attendify/
```

> 💡 **No camera?** Open the file directly (`file://`) and use the **🖼️ Scan Image** tab instead — it works without a server.

---

## 📖 Usage Guide

### 📷 Camera Scan Tab
1. Click **Start Camera** — your browser will request camera permission.
2. Allow camera access. The rear-facing camera opens automatically.
3. Hold a QR code in front of the camera — the attendee is logged instantly.
4. A green border flash confirms a successful scan.
5. If camera is blocked, a clear error message appears with fix instructions.

### 🖼️ Scan Image Tab
1. Click the upload area or drag-drop an image (PNG, JPG, GIF).
2. The QR code in the image is decoded automatically.
3. Use this tab on `file://` URLs or when camera access is unavailable.

### ✏️ Manual Entry Tab
1. Enter the attendee's **Email** and optionally an **Event Group** (defaults to `General`).
2. Press `Enter` in either field or click **Add Attendee**.
3. Use the demo chips (`priya@gdg.com`, `rahul@flutter.dev`, etc.) for quick test data.

### ⬇️ Exporting CSV
1. Click **Download CSV** in the Attendance Log card.
2. File is saved as `attendify_YYYY-MM-DD.csv`.
3. Columns: `Email`, `Group`, `Date`, `Time` — double-quoted for safe import.

---

## 🗂️ File Structure

```
attendify/
├── index.html       # Everything — styles, markup, and logic
├── README.md        # This file
└── LICENSE          # MIT
```

That's it. One file. No `node_modules`, no build step, no config.

---

## 🛠️ Tech Stack

| Technology | Purpose |
|-----------|---------|
| **[Html5Qrcode v2.3.8](https://github.com/mebjas/html5-qrcode)** | Camera & image QR scanning (CDN loaded) |
| **Vanilla CSS** | Glassmorphism UI, CSS variables, animations |
| **Vanilla JavaScript (ES2020)** | App logic, async camera, QR parsing, localStorage |
| **[Google Fonts](https://fonts.google.com)** | Syne (headings) · DM Sans (body) · DM Mono (code) |

**No frameworks. No build tools. No npm.**

---

## ❓ FAQ

<details>
<summary><strong>Why doesn't the camera work when I open the file directly?</strong></summary>

Browsers require a **secure context** (HTTPS or localhost) for camera access. Opening `index.html` via `file://` won't work for the camera tab.

**Fix:** Run `python3 -m http.server 8080` and visit `http://localhost:8080`.  
Alternatively, use the **🖼️ Scan Image** tab — it works without a server.

</details>

<details>
<summary><strong>Is any data sent to a server?</strong></summary>

**No.** Attendify is entirely client-side. All data stays in your browser's `localStorage`. Nothing ever leaves your device. The only network requests are loading fonts and the Html5Qrcode library from CDNs on first load.

</details>

<details>
<summary><strong>What happens if I close the browser tab?</strong></summary>

Records are automatically saved to `localStorage` on every check-in. When you reopen the page, all previous records are loaded and displayed. Data only clears if you manually clear browser site data.

</details>

<details>
<summary><strong>Can the same person check in twice?</strong></summary>

No. Attendify checks the email against existing records before adding. If a duplicate is detected, a yellow warning toast fires — but no duplicate record is created.

</details>

<details>
<summary><strong>What QR code should I give attendees?</strong></summary>

Any QR code encoding their email works. Recommended formats:

```json
{"email":"user@example.com","group":"Workshop A"}
```
```
email: user@example.com
group: Workshop A
```
```
user@example.com
```

Use any free QR generator (e.g. [qr-code-generator.com](https://www.qr-code-generator.com)) to create them.

</details>

<details>
<summary><strong>Can I customise event groups?</strong></summary>

Yes — the group field is free text. Use it to segment attendees by workshop, table, team, ticket tier, or any label you like. It appears as a pill badge in the attendance table and is included in the CSV export.

</details>

---

## 🤝 Contributing

Contributions are welcome! Feel free to:

- 🐛 [Report a bug](../../issues/new)
- 💡 [Request a feature](../../issues/new)
- 🔀 [Submit a pull request](../../pulls)

---

## 📄 License

This project is licensed under the **MIT License** — see the [LICENSE](./LICENSE) file for details.

---

<div align="center">

**Built with ♥ · No backend, no nonsense.**

⭐ **Star this repo** if Attendify helped you at your event!

</div>
