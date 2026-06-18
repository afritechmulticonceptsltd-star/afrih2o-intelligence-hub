[README.md](https://github.com/user-attachments/files/29093984/README.md)

# AfriH₂O Intelligence Hub

A live executive dashboard for Afritech Multi Concepts Ltd's Afri-Sachet Water (AF_SACH)
production line — tracking sales, production, and machine-level efficiency in real time,
sourced directly from the company's operational spreadsheet.

Live demo: 



## Overview

This dashboard replaces static end-of-week reporting with a single-page, always-current
view of the AF_SACH line. It's built to be read by leadership in seconds: revenue and
production trends against weekly targets, customer concentration, machine-level yield and
waste, and shift-level efficiency — all pulled live, no manual export/import step.

It was built end-to-end as part of Afritech's internal data intelligence pipeline:
Google Forms feed daily sales, shift, and machine logs → an Apps Script reconciliation
layer cleans and validates the data → the cleaned tables feed both this dashboard and a
Looker Studio reporting layer.

## Architecture


Google Forms (field data entry)
        │
        ▼
Spoke Sheets (raw submissions)
        │
        ▼
Apps Script (Cleaning.gs / KpiEngine.gs)
   — validation, reconciliation, ₦ tolerance checks, KPI computation
        │
        ▼
Intelligence Hub Spreadsheet
   (SACH_SALES_CLEAN, SACH_KPI_SALES, SACH_SHIFT_CLEAN,
    SACH_KPI_PRODUCTION, SACH_MACHINE_CLEAN, TARGETS)
        │
        ├──► Looker Studio (executive reporting)
        │
        └──► Apps Script Web App (doGet) ──► JSON ──► This Dashboard


The dashboard itself is a single static HTML file with no build step. On load, it fetches
JSON from a deployed Apps Script Web App endpoint and renders everything client-side. If
the endpoint is unreachable, it falls back to bundled sample data so the UI never breaks.

## Features

- Executive summary with revenue, production, and collection-rate KPIs against live weekly targets
- Sales deep dive: 14-day revenue and bag trends, weekly performance by batch, customer revenue concentration
- Production deep dive: yield and waste tracking, zero-production day flags, per-machine output and efficiency
- Graceful degradation to demo data if the live data source is unavailable
- Zero backend to maintain — data layer is entirely Google Sheets + Apps Script

## Tech stack

- HTML, CSS, vanilla JavaScript (no frameworks, no build tooling)
- Inline SVG for charts and visualizations
- Google Apps Script for the data API (`doGet` endpoint, JSON response)
- Google Sheets as the operational data store
- GitHub Pages for static hosting

## Project structure


.
├── index.html     # the dashboard — single file, all CSS/JS inline
└── README.md


The Apps Script source (`Code.gs`) that powers the live data feed lives in the linked
Google Sheet's Apps Script project, not in this repo, since it needs direct access to the
spreadsheet to run.

## Running locally

No build step required. Clone the repo and open `index.html` directly in a browser, or
serve it with any static file server:

bash
git clone https://github.com/<your-username>/afrih2o-intelligence-hub.git
cd afrih2o-intelligence-hub
python3 -m http.server 8000


By default it runs on bundled sample data. To connect it to a live data source, set the
`API_URL` constant near the top of the `<script>` block in `index.html` to your own
deployed Apps Script Web App URL.

## Data note

This dashboard is connected to a live operational data feed for a real production line.
The version deployed at the live demo link above may be showing real business figures —
treat accordingly.

## Author

Kris Eyo
Data Intelligence Lead & Project Manager, Afritech Multi Concepts Ltd
_add LinkedIn / portfolio link here_
