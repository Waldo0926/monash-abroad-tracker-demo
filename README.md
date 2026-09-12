# monash-abroad-tracker-demo
# Monash Exchange Tracker — public demo

This is a portfolio-only demo build of [monash-abroad-tracker](https://github.com/Waldo0926/monash-abroad-tracker).

The real project scrapes the Monash program-search page daily and tracks changes to
exchange programs (new/removed partners, Open/Closed flips, cut-off and place changes).
Its source repository stays private while it is being assessed as university coursework,
so the "Live viewer" badge there has nowhere public to point.

This repo exists to fix that: it ships the same static viewer (`index.html`) reading the
same JSON shape the real tracker produces, but every university, country and number in
`data/current.json` is made up. Nothing here comes from Monash or any real exchange
program.

## What's real vs fake

| | Real project | This demo |
| --- | --- | --- |
| Scraper / crawler code | private repo | not included |
| `data/current.json` schema | yes | same shape |
| `data/current.json` content | live Monash data | 18 fictional universities |
| `index.html` viewer | yes | same features, rebuilt for this repo |
| Daily updates | yes, via GitHub Actions | no, static snapshot |

## Running it locally

Any static file server works, e.g.

```bash
python3 -m http.server 8000
```

then open `http://localhost:8000/`.
ictional-data public demo of the monash-abroad-tracker viewer (portfolio use, no real Monash data)
