# Eternal Vigilance — VTES Tournament Winning Decks

Data repository for [VTES](https://www.vekn.net/) Tournament Winning Decks (TWD) in YAML format.

Managed by [channel-ten](https://github.com/gurchon-hall/channel-ten) (scraper, validator, publisher).

## Structure

```
YYYY/MM/<event_id>.yaml   — published tournament decks, organized by date
changes_required/          — decks flagged for manual review (merged forum icon)
errors/
├── illegal_crypt/         — decks with invalid crypt cards
├── illegal_library/       — decks with invalid library cards
├── incoherent_date/       — decks with mismatched dates
├── too_few_players/       — tournaments below minimum player threshold
└── unconfirmed_winner/    — decks with unresolved winner identity
publish/YYYY/MM/<date>.md  — weekly publish reports (PRs sent to GiottoVerducci/TWD)
```

## Data format

Each file is named `<event_id>.yaml` where `event_id` comes from the VEKN event calendar URL.

See [channel-ten README](https://github.com/gurchon-hall/channel-ten#data-format) for the full YAML schema and examples.

## Usage

This repository is automatically updated by GitHub Actions workflows in `channel-ten`:

- **Daily scrape** — new tournament decks are added
- **Weekly validation** — existing decks are re-validated and enriched
- **Weekly publish** — validated decks are published to [GiottoVerducci/TWD](https://github.com/GiottoVerducci/TWD)
