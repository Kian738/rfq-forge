# RFQ Forge

> Evidence-backed browser automation for turning complex requests for quotation into verified bids.

RFQ Forge is a production-minded agent built for the Atira EHL challenge. It combines browser exploration, document and mailbox extraction, deterministic commercial rules, and submission-time verification into one controlled workflow.

## What it does

- Navigates tender portals, supplier portals, ERP systems, and webmail with Playwright.
- Extracts structured evidence from pages, paginated lists, PDFs, ZIPs, CSVs, and messages.
- Uses an OpenAI-compatible Qwen endpoint for planning and extraction.
- Applies pricing and business rules deterministically with locale-aware parsing and `Decimal` arithmetic.
- Handles missing evidence conservatively instead of treating uncertainty as proof.
- Validates the portal's review page field-by-field before signing.
- Retrieves the newest TAN and only reports success after the website confirms receipt.
- Defends against decoy content, stale revisions, drafts, hidden UI traps, overlays, flaky pages, and hung browser tabs.

## Architecture

```text
run contract
    ↓
parallel browser missions
    ├── tender + Q&A
    ├── current technical specification
    ├── authorized mailbox evidence
    └── ERP materials, stock, lead times, and price scales
    ↓
blackboard evidence store
    ↓
deterministic rules + pricing trace
    ↓
review validation → TAN → confirmed submission
```

The package is site-agnostic: scenario values are supplied by the challenge run, while the agent contains generic navigation, extraction schemas, validation, and pricing logic.

## Results

Built collaboratively by **Kianmehr Karimi**, **[TizianGth](https://github.com/TizianGth)**, and **[Matin22](https://github.com/Matin22)** for the Atira EHL challenge. The team reached **2nd place at the EHL Grand Finale**, while the solution ranked **#1 on the private test leaderboard**.

## Quick start

Requirements: Python 3.12+, `uv`, and Chromium via Playwright.

```bash
uv sync
uv run playwright install chromium

eval "$(atira scout --env)"
atira models proxy

uv run --no-sync python main.py --dry-run --out runs/local.json
uv run --no-sync pytest -q
```

`--dry-run` prepares the workflow without making a binding submission. Runtime credentials and model keys must be supplied through environment variables; never commit them.

## Provenance

This portfolio copy is derived from the latest `main` package in [`TizianGth/isar-labs-ehl`](https://github.com/TizianGth/isar-labs-ehl/tree/main/agent). The upstream repository contains the original challenge context and development history.
