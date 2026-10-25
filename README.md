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

The package is intentionally site-agnostic: scenario values are supplied by the challenge run, while the agent contains generic navigation, extraction schemas, validation, and pricing logic.

## Quick start

Requirements: Python 3.12+, `uv`, and Chromium via Playwright.

```bash
uv sync
uv run playwright install chromium

# Local scouting environment supplied by the Atira CLI
eval "$(atira scout --env)"
atira models proxy

uv run --no-sync python main.py --dry-run --out runs/local.json
uv run --no-sync pytest -q
```

`--dry-run` prepares the workflow without making a binding submission. Runtime credentials and model keys must be supplied through environment variables; never commit them.

## Validation

The latest package was validated in the Atira challenge with a quality-100 result, including the recorded `release/best-6m22` leaderboard submission. The current package revision is preserved from the project's latest `main` branch.

## Collaboration

Built collaboratively by **Kian** and **[Matin22](https://github.com/Matin22)**, with a strong focus on reliability, evidence provenance, deterministic calculations, and safe browser automation.

## License

See the upstream project for licensing and challenge-specific terms.
