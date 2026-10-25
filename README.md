# Monash Exchange Tracker: Public Demo

[![Live Demo](https://img.shields.io/badge/Live_Demo-waldo0926.github.io-2563eb?style=for-the-badge)](https://waldo0926.github.io/monash-abroad-tracker-demo/)
[![Type](https://img.shields.io/badge/Type-Portfolio_Demo-7c3aed?style=for-the-badge)](#)
[![Data](https://img.shields.io/badge/Data-Synthetic_Only-475569?style=for-the-badge)](#privacy-boundary)

**English** · [简体中文](README.zh-CN.md)

[**Public demo**](https://waldo0926.github.io/monash-abroad-tracker-demo/) · [Architecture](ARCHITECTURE.md) · [Synthetic current dataset](./data/current.json) · [Synthetic comparison baseline](./data/comparison.json) · [Synthetic change feed](./data/changes.json)

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
- **Sortable QS rank column** with fictional ranks: overall ranks, ranges, a fallback to the previous year's table, specialist schools shown by their best subject placing (all subjects on hover or tap), and "Not listed" kept distinct from a poor rank
- **Collapsible change-summary panel**: ten tiles count cut-off, place, competition and status changes plus new programs against the previous round, follow the sidebar filters, and filter the table in one click
- **Clickable summary counters** that toggle the matching facet, and a row of removable filter chips above the table
- **Phone layout**: the sidebar becomes a drawer and the table becomes one card per program
- Light / dark toggle shared with the sign-in preview, and a comfortable / compact row-density switch
- **Day-by-day change feed** next to the tiles; clicking a university searches the table for it
- **Sign-in page preview** that reproduces the production design (bilingual, mobile and dark mode) without checking any credentials
- A reduced version of the production JSON schema for demonstrated fields
- Responsive layout for desktop and smaller screens

The public page intentionally presents itself as a **static portfolio demo**, not a live Monash data feed.

## How the comparison demo works

`data/current.json` contains a fully synthetic current round and `data/comparison.json` contains a separate synthetic previous-round baseline. The browser derives change chips at runtime by comparing the two datasets; the displayed changes are not hard-coded into the HTML.

The comparison semantics mirror the production viewer: for example, fewer places or a higher comparable cut-off is treated as an adverse change, while more places, a lower comparable cut-off, improved availability or reopening is favourable.

## Tech stack

| Part | Built with |
| --- | --- |
| Crawler and pipeline | Python 3, `requests`, BeautifulSoup with `lxml`, Playwright (headless Chromium, only when the site refuses plain HTTP) |
| Reports and rankings | `openpyxl` for the Excel comparison reports and the QS ranking build |
| Tests | pytest with trimmed HTML fixtures and a stub fetcher |
| Scheduling | systemd timers on a home Debian machine, which pushes the refreshed data to GitHub |
| Viewer | Plain HTML, CSS and JavaScript, no framework; deployed with GitHub Actions and GitHub Pages |
| Sign-in | A small standard-library Python service with HMAC-signed cookies, checked by Nginx `auth_request` |

## Production system highlights

The private system has been running daily since August 2026. Without publishing its code or data, these are the engineering decisions behind it:

- **Polite, self-checking collection.** It discovers all ~164 semester-exchange programs from the search page instead of a hard-coded list, crawls one request at a time, reads `robots.txt` on every run and honours its 30-second crawl delay. A failed detail page keeps the previous values rather than deleting them, a failed search page aborts the run with the dataset untouched, and a drop of more than 20% in discovered programs trips a safety guard that writes nothing.
- **Raw and normalised values side by side.** Every field keeps the page's original text next to a normalised value, so a change can always be traced back to what the site actually said.
- **A comparison that stays correct.** The previous application round is frozen as a baseline and the comparison is regenerated as a pure function of (baseline, current data) rather than edited in place, so a wrong row cannot accumulate.
- **QS rankings without silent mismatches.** A sortable QS column covers 150 of 164 partners from the 2027 and 2026 overall tables, falling back to subject rankings for specialist schools (shown as their best subject placing). Matching uses a hand-checked alias table, because an automatic first attempt paired the University of Hong Kong with Hong Kong Metropolitan (#781 instead of #11); the build fails while any partner is unresolved.
- **A viewer people actually use.** A collapsible change-summary panel counts cut-off, place, competition and status changes against the previous round, filters the table in one click, and lists the daily change feed. Access goes through a dedicated bilingual sign-in page with signed session cookies, per-IP rate limiting, and sessions that end as soon as a password changes or an account is removed.
- **Reliability fixes from real operation.** Running unattended surfaced bugs worth knowing about: git conflict markers committed into generated data (fixed by syncing before crawling and refusing to commit on conflict), a read-modify-write race that overwrote back-filled dates, `set -o pipefail` treating a `grep -q` match as failure through SIGPIPE, and a macOS launchd job that never ran because of privacy protection on the Desktop folder.
- **Scheduled on hardware the site accepts.** GitHub-hosted runners get HTTP 403 from the Monash site, so the crawl runs on a small always-on Debian machine at home: one run every evening at 19:00, plus two-hourly retries that only crawl if the last scheduled run is more than 20 hours old. A manual crawl never uses up the evening one.
- **Offline tests.** Parser tests run against trimmed HTML fixtures, and a pipeline test drives whole runs through a stub fetcher: first run, unchanged re-run, real change, removal, guard trip, search failure and detail failure.

### Sign-in design

The viewer first sat behind the web server's Basic Auth, which shows the browser's own credentials dialog. Replacing that dialog was a usability change, so the goal was to change **how** people sign in without changing **who** can get in. You can see the result in the [sign-in page preview](./signin.html).

- **Reuse password checking instead of rewriting it.** The existing account file stays the single source of truth, and the web server, which already verifies its apr1 and bcrypt hashes, still does the checking. A small sign-in service asks a loopback-only endpoint of the web server whether a username and password pass the same Basic Auth check, so no password-hashing code was written at all.
- **Every request is checked at the edge.** Before serving anything, the web server asks the sign-in service whether the session cookie is valid, and sends anyone without a valid session to the sign-in page.
- **Stateless sessions that can still be revoked.** The cookie carries the user, an expiry, and an HMAC over both plus a fingerprint of that user's entry in the account file. There is no session store, yet changing a password or removing a user changes the fingerprint and ends their sessions immediately, with no restart. Rotating the signing secret signs everyone out.
- **Sensible defaults.** "Keep me signed in" lasts 30 days; without it, the session ends when the browser closes and never lasts more than 12 hours. Sign-in attempts are limited to 6 a minute per address, and the page says to wait rather than failing silently.
- **Nothing new to operate.** Accounts are added, changed and removed with exactly the same commands as before. The service runs sandboxed with no account of its own, and the page template is re-read on every request, so wording changes go live with the regular code update.
- **Small things that matter to users.** An expired session returns to the sign-in page instead of showing a JSON parse error, the language chosen on the sign-in page carries over to the table, and the page works in both languages, on phones and in dark mode, with a sign-out button in the viewer.

## Public demo vs. private production system

| Component | Private production system | This public demo |
| --- | --- | --- |
| Scraper / crawler | Daily scheduled collection on a self-hosted server, honouring `robots.txt` | Not published |
| Normalisation | Full production pipeline | Demonstrated through synthetic schema |
| Data | Real collected records | 12 synthetic records |
| Historical tracking | Append-only changes + snapshots | Not published |
| Round comparison | Real frozen baseline | Synthetic baseline |
| Comparison reports | Private Excel reports against the frozen previous round | Not published |
| Viewer | Access-controlled viewer with a sign-in page, change-summary panel and QS ranking column | Public portfolio-safe viewer |
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
├── signin.html             # Preview of the production sign-in page (no authentication)
├── ARCHITECTURE.md         # Sanitised production-system architecture
├── README.md
├── README.zh-CN.md
└── data/
    ├── current.json        # Synthetic current dataset
    ├── comparison.json     # Synthetic previous-round baseline
    └── changes.json        # Synthetic day-by-day change feed
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
