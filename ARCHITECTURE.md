# Sanitised Architecture Overview

This document describes the production architecture behind the public demo without exposing private data, historical comparison results, credentials, or production infrastructure details.

## System split

```text
Public portfolio demo
GitHub Pages
    |
    +--> synthetic current dataset
    +--> synthetic previous-round baseline
    +--> filters / sorting / comparison UI

Private production system
Scheduled collector
    |
    v
Program-search crawler
    |
    v
field normalisation + validation
    |
    +--> current canonical dataset
    |
    +--> append-only change log
    |
    +--> historical snapshots
    |
    +--> frozen round baseline
    |
    v
comparison + report generation
    |
    v
access-controlled viewer
```

## Production data pipeline

### 1. Collection

A scheduled worker retrieves semester-exchange listings and detail pages. Collection is intentionally separated from the public demo because the production repository contains real data and operational logic.

### 2. Normalisation

Search-page and detail-page fields are converted into a canonical schema so status, availability, cut-offs, places, eligibility and other fields can be compared reliably between runs.

### 3. Validation and safety guards

The production tracker validates crawl results before replacing the previous dataset. Large unexpected record-count drops and failed parses are treated as unsafe runs rather than valid updates.

### 4. Field-level change detection

Each successful run is compared against the previous known state. Changes are recorded at field level instead of treating an entire program record as simply “changed”.

### 5. Historical snapshots

Historical snapshots and append-only change records support auditability and make it possible to reconstruct how program information evolved over time.

### 6. Round-over-round comparison

A frozen previous-round baseline is compared with the current round to identify changes in application status, availability, cut-offs, place counts and selected eligibility fields.

### 7. Private reporting

Production comparison reports and the live viewer are private outputs. They are not committed to this public repository.

## Public-demo architecture

The public demo intentionally has no crawler or backend. It loads two synthetic JSON files in the browser and performs filtering, sorting and comparison client-side.

```text
current.json -----+
                  +--> browser comparison logic --> change chips / table / filters
comparison.json --+
```

This keeps the portfolio demo independently inspectable while ensuring that publishing the repository cannot expose production history.

## Privacy boundary

The public repository does not contain:

- production datasets;
- real previous-round baselines;
- historical snapshots or real change logs;
- generated customer comparison reports;
- production hostnames, credentials, passwords or access-control files;
- operational logs;
- production crawler source code.

The synthetic records are invented specifically for this repository and are not transformations of real partner records.

## Engineering themes demonstrated

Across the public demo and the private production architecture, the project demonstrates:

- scheduled data collection;
- schema normalisation;
- defensive data validation;
- field-level diffing;
- append-only history and snapshots;
- round-over-round comparison;
- report generation;
- access-controlled publication;
- bilingual filtering/search UI;
- separation of public source code from private production data.
