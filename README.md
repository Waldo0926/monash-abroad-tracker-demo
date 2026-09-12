# Monash Exchange Tracker — Public Demo

**English** · [简体中文](README.zh-CN.md)

[**Public demo**](https://waldo0926.github.io/monash-abroad-tracker-demo/) · [Demo dataset](./data/current.json)

A portfolio-safe public demonstration of my **Monash Abroad exchange-program tracker**.

The full tracker collects Monash program-search data, normalises program details, compares runs, records field-level changes, and publishes a searchable viewer. The production repository remains private while the project is associated with active university coursework.

This repository demonstrates the viewer without publishing production data or scraper internals.

> **Demo-data notice:** every institution, country, URL, score, availability value and place count in `data/current.json` is fictional. No real Monash exchange-program record is included in this repository.

## What this demo shows

- Search by university or country
- Production-style sidebar filtering with checkbox facets for region, program terms, duration, eligible campus, faculties/schools, application status and exchange availability
- OR matching within a facet and AND matching across different facets
- Interactive table-column sorting
- Previous-round cut-offs and anticipated places
- Search/detail consistency warnings
- The same canonical JSON field structure used by the private tracker
- Responsive layout that collapses the two-column viewer cleanly on smaller screens

The page intentionally describes itself as a **static portfolio dataset**, rather than a live production feed.

## Public demo vs production project

| Component | Production tracker | This public demo |
| --- | --- | --- |
| Scraper / crawler | Included in private repository | Not included |
| Program-data schema | Canonical tracker schema | Same structure for demonstrated fields |
| Dataset | Real collected records | 18 fictional records |
| Viewer | Production viewer | Portfolio-safe viewer using the same filtering concepts |
| Automated collection | Scheduled workflow | Not included |
| Historical change tracking | Yes | Represented by static demo metadata only |

## Repository structure

```text
.
├── index.html          # Static searchable/filterable viewer
└── data/
    └── current.json    # Fictional dataset using the tracker schema
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

The production tracker contains the actual data-collection implementation and real collected exchange-program data. Keeping it private prevents assessment-related source code and production data from being exposed while still allowing the interface and data model to be demonstrated publicly.

## Disclaimer

This is an independent portfolio project and is **not an official Monash University service**. The fictional demo records must not be used for exchange planning, eligibility decisions, or application advice.
