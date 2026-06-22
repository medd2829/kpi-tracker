# 📊 Q3 KPI Tracker

> Personal KPI tracking app for Q3 2026 — accessible via mobile & web

[![GitHub Pages](https://img.shields.io/badge/Live-GitHub%20Pages-0ECFB3?style=flat-square&logo=github)](https://medd2829.github.io/q3-kpi-tracker/)

---

## 🎯 Overview

A lightweight daily tracking app for two Q3 2026 KPI outcomes, built for active monitoring from any device.

| Outcome | Title | Weight |
|---|---|---|
| O1 | Quality holds across every project, owned by module owners and visible to the CTO | 60% |
| O2 | Speed up defect resolution and module implementation | 40% |

---

## ✨ Features

### 📱 Dashboard
- Weighted overall progress (O1 × 60% + O2 × 40%)
- Today's pending tasks (top 3)
- Recent log entries
- Q3 days remaining countdown

### 📈 Progress
- Interactive sliders per criteria
- O1: K1 Gates · K2 Artifacts · K3 Monitoring · K4 Report
- O2: K1 Fix Ratio · K2 Guardrails · K3 Escalation · K4 Ownership · K5 Timebox
- Sync to Google Sheets

### ✅ Tasks
- 29 pre-filled template tasks grouped by week (W1–W8)
- Add custom tasks
- Weekly grouping with progress counter
- Mark done / delete

### 📝 Daily Log
- Daily notes per outcome (O1 / O2 / General)
- Auto timestamp
- Sync to Google Sheets

### ❓ Confirmations
- 10-item checklist of questions to align with manager before committing Q3 strategy
- Confirmation progress bar

---

## 🗓️ Q3 Roadmap

```
July 2026          August 2026        September 2026
─────────────────  ─────────────────  ─────────────────
W1 Foundation      W4 Build           W7 Scale
W2 Foundation      W5 Build           W8 Close & Exceed
W3 Foundation      W6 Build
```

### Key milestones:
- **W1:** Manager alignment, G2 gate template setup, fix ratio baseline
- **W2–W3:** G1–G4 enforced on active projects, artifact standard set, module 1 starts
- **W4–W5:** Sentry setup, monitoring dashboard live, weekly CTO report, module 2
- **W6–W7:** G1–G4 across all projects, module 3, push for Exceeded
- **W8:** Fix ratio ≥30%, P0/P1 comparison vs Q2, CI-gate improvement

---

## ⚙️ Setup Google Sheets (Optional but Recommended)

To sync data across devices (mobile ↔ laptop), connect to Google Sheets:

**1. Create a new Google Spreadsheet**
Name it anything, e.g. `Q3 KPI Tracker`

**2. Open Extensions → Apps Script**
Delete existing code, paste this:

```javascript
const SS = SpreadsheetApp.getActiveSpreadsheet();

function doPost(e) {
  const d = JSON.parse(e.postData.contents);
  if (d.action === 'saveProgress') saveProgress(d.data, d.timestamp);
  if (d.action === 'addTask') addRow('Tasks', d.data);
  if (d.action === 'addLog') addRow('Logs', d.data);
  return ContentService.createTextOutput('ok');
}

function doGet(e) {
  return ContentService.createTextOutput(
    JSON.stringify({status:'ok'})
  ).setMimeType(ContentService.MimeType.JSON);
}

function saveProgress(data, ts) {
  let sh = SS.getSheetByName('Progress') || SS.insertSheet('Progress');
  sh.appendRow([ts, ...Object.values(data)]);
}

function addRow(shName, data) {
  let sh = SS.getSheetByName(shName) || SS.insertSheet(shName);
  sh.appendRow([new Date().toISOString(), JSON.stringify(data)]);
}
```

**3. Deploy as Web App**
- Deploy → New Deployment → Web App
- Execute as: **Me**
- Who has access: **Anyone**
- Click Deploy → copy the URL

**4. Connect in the app**
Click **⚙ Google Sheets** button in the app → paste URL → Save

---

## 💾 Data Storage

| Data | Storage |
|---|---|
| Progress %, Tasks, Logs | Browser `localStorage` (local) |
| Cross-device sync | Google Sheets via Apps Script |
| Google Sheets URL | Browser `localStorage` (not committed to repo) |

> ⚠️ No personal data is stored in this repository.

---

## 🛠️ Tech Stack

- Pure HTML + CSS + Vanilla JS (zero dependencies)
- Google Fonts: Inter + JetBrains Mono
- Google Sheets API via Apps Script (optional)
- Hosted via GitHub Pages

---

## 📁 Structure

```
q3-kpi-tracker/
├── index.html    # Single-file app (UI + logic all in one)
└── README.md     # This documentation
```

---

## 🔄 Updating the App

To apply changes to `index.html`:
1. Edit the file locally
2. Re-upload to GitHub (Add file → Upload files → select index.html → Commit)
3. GitHub Pages auto-updates within 1–2 minutes

---

*Q3 2026 · Personal KPI Tracker*
