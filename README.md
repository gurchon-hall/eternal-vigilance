# Eternal Vigilance — VTES Tournament Decks

Data repository for [VTES](https://www.vekn.net/) tournament decks in YAML format, holding two
independent, differently-shaped datasets — see below before assuming a path is TWD.

Managed by [channel-ten](https://github.com/gurchon-hall/channel-ten) (scraper, validator, publisher).

## Structure

```
YYYY/MM/<event_id>.yaml         — TWD: the winning deck of each tournament, organized by date
tda/YYYY/MM/<event_id>/
  └── <author_id>.yaml          — TDA: every participant's deck, one file per deck
changes_required/                — TWD decks flagged for manual review (merged forum icon)
errors/
├── illegal_crypt/               — TWD decks with invalid crypt cards
├── illegal_library/             — TWD decks with invalid library cards
├── incoherent_date/             — TWD decks with mismatched dates
├── too_few_players/             — TWD tournaments below minimum player threshold
├── unconfirmed_name/            — TWD decks whose name could not be confirmed against the VEKN event calendar
└── unconfirmed_winner/          — TWD decks with unresolved winner identity
publish/YYYY/MM/<date>.md        — weekly TWD publish reports (PRs sent to GiottoVerducci/TWD)
```

`tda/` has its own `errors/<first_error>/<event_id>_<author_id>.yaml` subtree (same error-type
names, TDA-specific validation) — it is separate from the top-level `errors/`, which is TWD-only.

## Data format

### TWD (Tournament Winning Deck)

Each file is named `<event_id>.yaml` where `event_id` comes from the VEKN event calendar URL —
**one deck per tournament** (the winner's).

See [channel-ten README](https://github.com/gurchon-hall/channel-ten#data-format) for the full YAML schema and examples.

### TDA (Tournament Deck Archive)

`tda/YYYY/MM/<event_id>/<author_id>.yaml` — **every participant's deck** for a tournament, not
just the winner's, sourced from [smeea/vdb](https://github.com/smeea/vdb) rather than the VEKN
forum. `event_id` is that source's archive id: numeric when the event has a VEKN calendar id, or
a short slug (e.g. `online1`) for recurring online events that never got one. `author_id` is the
deck author's VEKN member number when resolvable, or a slug of the raw author string otherwise.

TDA files share the event-level fields (`name`, `location`, `date_start`, `rounds_format`,
`players_count`, `winner`) but each file additionally carries its own `author` (the deck's
owner, not necessarily the tournament winner) and `deck`. See
[channel-ten's TDA pipeline doc](https://github.com/gurchon-hall/channel-ten/blob/main/docs/tda_pipeline.md)
for the full schema and source-format details.

**TWD and TDA are never merged into the same file or directory** — a TWD YAML has exactly one
deck (the winner's); a TDA event directory can have dozens. Downstream consumers (e.g.
`tabriz-assembly`'s database import) keep them in separate tables for the same reason.

## Usage

This repository is automatically updated by GitHub Actions workflows in `channel-ten`:

- **Daily scrape** — new TWD tournament decks are added
- **Monthly TDA scrape** — new TDA tournament archives (every participant's deck) are added
- **Weekly validation** — existing TWD decks are re-validated and enriched
- **Weekly publish** — validated TWD decks are published to [GiottoVerducci/TWD](https://github.com/GiottoVerducci/TWD)
