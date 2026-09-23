# PRS Client Dashboard — How to Run & Deploy

A single self-contained web app (`index.html`) that shows PRS a clean, live view of the acquisition campaign. It reads **directly from your Google Sheet** (the "Calling Queue" tab) every time it loads — so BTN keeps updating the one CRM, and PRS just opens a URL.

---

## Try it right now (no deploy needed)
Double-click `index.html` — it opens in your browser. Enter the access code (**prs2026** by default) and you'll see the live dashboard reading from the sheet.

## What PRS sees
Executive KPIs (Calls Today, This Week, Interested, Follow-ups, Meeting Ready, Callbacks) · Acquisition Funnel · Today's Calling · Interested Firms · Google Meet Pipeline · Follow-up Pipeline · Performance (connect rate). Internal notes/phone research stay in your CRM; this is the presentation layer only.

## Deploy to a URL (Vercel + GitHub)
1. Create a free GitHub account (if needed) → **New repository** → name it `prs-dashboard`.
2. Upload `index.html` into the repo (Add file → Upload files → commit).
3. Go to **vercel.com** → sign in with GitHub → **Add New → Project** → import `prs-dashboard` → **Deploy**.
4. Vercel gives you a URL like `https://prs-dashboard.vercel.app`. Share that with PRS. Done.

Any time your team updates the sheet, the dashboard reflects it on next load / auto-refresh (every 10 min).

## Settings you can change (top of index.html, CONFIG block)
- `SHEET_ID` — already set to your sheet.
- `TAB` — the tab it reads (`Calling Queue`).
- `ACCESS_CODE` — change `prs2026` to whatever code PRS should use.
- `REFRESH_MINUTES` — auto-refresh interval.

## Mark your approved (green) firms so the dashboard counts only them
A green highlight is not readable data, so the dashboard needs a value to know which firms are approved. Do this once:
1. In the **Calling Queue**, add a new column header called **Approved** (e.g., in the first empty column).
2. Fast way to fill it: **Data → Create a filter →** click the filter arrow on any column **→ Filter by colour → Fill colour → green.** Now only your green rows show.
3. Type **Yes** in the Approved column of the top visible row, then drag/fill it down through all the visible (green) rows.
4. **Remove the filter** (Data → Remove filter). Only the green firms now say "Yes".

The dashboard automatically switches to counting only those firms. Until you do this, it shows all firms with a reminder banner.

## Requirements / limitations (please read)
- **The sheet must stay shared as "Anyone with the link – Viewer"** (it already is). If sharing is turned off, the dashboard can't read it.
- **Call Date should be entered as a real date** (e.g. 2026-09-22) for the "Today"/"This Week" numbers to be accurate. Text like `18/09/26` still shows in tables but may not count in the daily/weekly totals.
- **The access code is a light gate, not high-security login.** Anyone with the code and URL can view. For a proper secure PRS login (individual accounts), we move to the Next.js server version later — the same layout ports over, so nothing here is wasted.
- Dispositions must be typed from the standard list (`Interested`, `Meeting Agreed`, `Callback`, `No Answer`, `Busy`, `Not Interested`) — that's what the dashboard reads.

## Scaling to more cities
When you add Ahmedabad / Surat / Rajkot, either add their tabs and point a copy of the dashboard at them, or (recommended later) move to the Next.js version with a real database and per-city views — without redesigning this PRS-facing layout.
