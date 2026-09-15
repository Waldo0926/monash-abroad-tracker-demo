# Monash Exchange Tracker — Public Demo

[![Live Demo](https://img.shields.io/badge/Live_Demo-waldo0926.github.io-2563eb?style=for-the-badge)](https://waldo0926.github.io/monash-abroad-tracker-demo/)
[![Type](https://img.shields.io/badge/Type-Portfolio_Demo-7c3aed?style=for-the-badge)](#)
[![Data](https://img.shields.io/badge/Data-Synthetic_Only-475569?style=for-the-badge)](#privacy-boundary)

**English** · [简体中文](README.zh-CN.md)

[**Public demo**](https://waldo0926.github.io/monash-abroad-tracker-demo/) · [Architecture](ARCHITECTURE.md) · [Synthetic current dataset](./data/current.json) · [Synthetic comparison baseline](./data/comparison.json)

A portfolio-safe public demonstration of my **Monash semester-exchange tracking system**.

The private production system collects program-search data, normalises fields, detects field-level changes, keeps historical snapshots, compares application rounds, generates comparison reports, and publishes an access-controlled viewer. This repository demonstrates the user-facing filtering and comparison workflow without publishing production records, customer-facing comparison results, or scraper internals.

> **Synthetic-data notice:** every institution, country, URL, date, score, availability value, previous-round value and place count in this repository is fictional. The dataset is designed only to exercise the UI and change-detection presentation.

## What this demo shows

- English / 简体中文 interface switching
- Search by university or country
- Multi-facet filtering for region, program terms, duration, eligible campus, faculties/schools, application status and exchange availability
- OR matching within a facet and AND matching across different facets
- **Dynamic facet counts** that react to other active filters
- Filter-aware summary cards for total results and Green / Yellow / Red / Closed availability
- Interactive table-column sorting
- Previous-round cut-offs, minimum results and anticipated places
- **Runtime comparison against a synthetic previous-round baseline**, generating change chips for availability, places, cut-offs, opening/closing and new programs
- Search/detail consistency warnings
- A reduced version of the production JSON schema for demonstrated fields
- Responsive layout for desktop and smaller screens

The public page intentionally presents itself as a **static portfolio demo**, not a live Monash data feed.

## How the comparison demo works

`data/current.json` contains a fully synthetic current round and `data/comparison.json` contains a separate synthetic previous-round baseline. The browser derives change chips at runtime by comparing the two datasets; the displayed changes are not hard-coded into the HTML.

The comparison semantics mirror the production viewer: for example, fewer places or a higher comparable cut-off is treated as an adverse change, while more places, a lower comparable cut-off, improved availability or reopening is favourable.

## Public demo vs. private production system

| Component | Private production system | This public demo |
| --- | --- | --- |
| Scraper / crawler | Scheduled collection | Not published |
| Normalisation | Full production pipeline | Demonstrated through synthetic schema |
| Data | Real collected records | 12 synthetic records |
| Historical tracking | Append-only changes + snapshots | Not published |
| Round comparison | Real frozen baseline | Synthetic baseline |
| Comparison reports | Private customer-facing output | Not published |
| Viewer | Access-controlled production viewer | Public portfolio-safe viewer |
| Deployment | Private production environment | GitHub Pages |

See [ARCHITECTURE.md](ARCHITECTURE.md) for a sanitised overview of the complete system.

## Privacy boundary

The public repository intentionally excludes:

- real Monash exchange-program records;
- real historical snapshots or field-level change logs;
- real previous-round baselines;
- generated comparison spreadsheets/reports;
- customer-facing comparison results;
- production credentials, access-control configuration and infrastructure details;
- production scraper/crawler source code.

This split allows the engineering and interaction design to be demonstrated publicly while the live dataset and historical comparison service remain private.

## Repository structure

```text
.
├── index.html              # Search, facets, comparison chips, bilingual UI
├── ARCHITECTURE.md         # Sanitised production-system architecture
├── README.md
├── README.zh-CN.md
└── data/
    ├── current.json        # Synthetic current dataset
    └── comparison.json     # Synthetic previous-round baseline
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

## Production deployment

The real tracker and its historical data remain in a separate private repository and deployment. Production comparison results are deliberately not mirrored into this public repository.

## Disclaimer

This is an independent portfolio project and is **not an official Monash University service**. All records in this demo are synthetic and must not be used for exchange planning, eligibility decisions, or application advice.
