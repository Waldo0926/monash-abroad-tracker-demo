# Monash Exchange Tracker — Public Demo

**English** · [简体中文](README.zh-CN.md)

[**Public demo**](https://waldo0926.github.io/monash-abroad-tracker-demo/) · [Current demo dataset](./data/current.json) · [Fictional previous-round baseline](./data/comparison.json)

A portfolio-safe public demonstration of my **Monash Abroad exchange-program tracker**.

The full tracker collects Monash program-search data, normalises program details, compares runs and application rounds, records field-level changes, and publishes a searchable viewer. The production repository remains private while the project is associated with active university coursework.

This repository demonstrates the viewer and comparison workflow without publishing production data or scraper internals.

> **Demo-data notice:** every institution, country, URL, score, availability value, previous-round value and place count in this repository is fictional. No real Monash exchange-program record is included.

## What this demo shows

- English / 简体中文 interface switching
- Search by university or country
- Production-style sidebar filtering with checkbox facets for region, program terms, duration, eligible campus, faculties/schools, application status and exchange availability
- OR matching within a facet and AND matching across different facets
- **Dynamic facet counts** that react to the other active filters
- Filter-aware summary cards for total results and Green / Yellow / Red / Closed availability
- Interactive table-column sorting
- Previous-round cut-offs, minimum results and anticipated places
- **Automatic comparison against a fictional previous-round baseline**, producing change chips such as availability ↑/↓, places ↑/↓, cut-off ↑/↓, opened/closed and new
- Search/detail consistency warnings
- The same canonical JSON field structure used by the private tracker for the demonstrated current records
- Responsive layout that collapses the two-column viewer cleanly on smaller screens

The public page intentionally describes itself as a **static portfolio dataset**, rather than a live production feed.

## How the comparison demo works

`data/current.json` contains the fictional current round. `data/comparison.json` contains a separate fictional previous-round baseline. The viewer derives its change chips at runtime by comparing the two datasets rather than hard-coding labels into the HTML.

The direction semantics mirror the production viewer: a higher cut-off or more competitive availability is treated as an adverse change, while more places, a lower comparable cut-off, or reopening is treated as favourable.

## Public demo vs production project

| Component | Production tracker | This public demo |
| --- | --- | --- |
| Scraper / crawler | Included in private repository | Not included |
| Current-record schema | Canonical tracker schema | Same structure for demonstrated fields |
| Data | Real collected records | 18 fictional records |
| Previous-round comparison | Real frozen baseline | Separate fictional baseline |
| Viewer | Production viewer | Portfolio-safe viewer using the same interaction and comparison concepts |
| Automated collection | Scheduled workflow | Not included |
| Historical change tracking | Full append-only history and snapshots | Static fictional comparison demonstration only |

## Repository structure

```text
.
├── index.html              # Search, facets, comparison chips, bilingual UI
└── data/
    ├── current.json        # Fictional current dataset using the tracker schema
    └── comparison.json     # Fictional previous-round baseline for change detection
```

## Run locally

Because the page loads JSON with `fetch()`, serve the repository through a local HTTP server rather than opening `index.html` directly:

```bash
python3 -m http.server 8000
```

Then open:

```text
http://localhost:8000/
```

## Why the production repository is private

The production tracker contains the actual data-collection implementation and real collected exchange-program data. Keeping it private prevents assessment-related source code and production data from being exposed while still allowing the interface, data model and comparison workflow to be demonstrated publicly.

## Disclaimer

This is an independent portfolio project and is **not an official Monash University service**. The fictional demo records must not be used for exchange planning, eligibility decisions, or application advice.
