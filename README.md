---
license: cc0-1.0
pretty_name: "humans.top — Global ranking of influential people"
language:
  - en
  - ru
  - de
  - fr
  - it
  - es
  - tr
  - zh
  - pl
  - sr
  - ar
  - hi
  - bn
  - pt
  - ja
task_categories:
  - text-generation
  - text-classification
size_categories:
  - n<1K
annotations_creators:
  - expert-generated
source_datasets:
  - original
configs:
  - config_name: default
    data_files:
      - split: train
        path: humans.jsonl
tags:
  - tabular
  - text
  - people
  - ranking
  - influence
  - biographies
  - multilingual
  - named-entities
  - entity-linking
  - wikidata
  - knowledge-base
---

# humans.top — Global ranking of influential people (open dataset)

![Live](https://img.shields.io/badge/Live-mirrors%20the%20ranking-2ea44f)
![Lightweight](https://img.shields.io/badge/Lightweight-CSV%20%2F%20JSONL-1f6feb)
![High-quality](https://img.shields.io/badge/High--quality-15--language%20bios-8957e5)
![Frequently updated](https://img.shields.io/badge/Updated-daily-2ea44f)
![Human-checked](https://img.shields.io/badge/Human--checked-links%20%26%20accounts-d29922)

**This dataset ranks real, named living people by global influence** — e.g. #1
Donald Trump, #2 Xi Jinping, #3 Vladimir Putin, alongside figures like Elon Musk,
Narendra Modi and Lionel Messi. Every row is a **person**: their live influence
**rank**, a concise **biography in 15 languages**, and Wikidata / Wikipedia links.
Published from the website **[humans.top](https://humans.top)** (`.top` is the
domain name).

> **It is a biographical, named-entity _ranking_ dataset (text + tabular)** — *not*
> a computer-vision, image, or person/pedestrian-detection dataset. The rows are
> people's names and facts, not pixels or bounding boxes.

An open, continuously updated ranking of notable living people, ordered by a
fixed composite of **political, financial, informational and cultural
influence** computed from public statistical sources.

Each entry carries a concise, human-written **descriptor in 15 languages**, a
**Wikidata Q-ID** and **English Wikipedia** link for unambiguous entity
linking, **verified public facts** (native-language name, country of
citizenship, official website, X handle), an influence **tier**, the live
**rank**, and a canonical profile URL.

- **License:** [CC0 1.0](https://creativecommons.org/publicdomain/zero/1.0/) —
  public domain. Free for **any** use, **including AI / LLM training**. No
  attribution required (a link back to [humans.top](https://humans.top) is
  always welcome).
- **Live source of truth:** <https://humans.top/dataset>
  ([CSV](https://humans.top/dataset/humans.csv) ·
  [JSONL](https://humans.top/dataset/humans.jsonl))
- **Scope (v2):** the editorial / curated set of public figures. It contains no
  personal data of self-registered users.
- **Size:** ~200 entries (exact count + last refresh in `metadata.json`).

## Data quality & verification

The data is checked **both automatically and by hand**. Public sources such as
Wikidata are sometimes out of date — for example a stored "official website" that
has lapsed or now redirects elsewhere — so the linkage fields (**official
website**, **X / Twitter account**) are verified by a person and corrected or
removed. We prefer an **empty field over a wrong link**. This human pass is
repeated periodically, on top of the automated daily refresh.

| badge | what it means |
|---|---|
| **Live** | mirrors the current live ranking |
| **Lightweight** | tiny CSV / JSONL — loads in seconds |
| **High-quality** | curated, human-written bios in 15 languages |
| **Frequently updated** | auto-refreshed daily across every mirror |
| **Human-checked** | links & accounts reviewed by a person, not just scraped |

## Files

| file | format |
|---|---|
| `humans.csv` | the ranking — UTF-8 CSV, header row, one row per person |
| `humans.jsonl` | same rows as newline-delimited JSON (one object per line) |
| `humans-rank-history.csv` | **rank history** — one row per (day, person): `date,user_number,rank` |
| `humans-rank-history.jsonl` | same, newline-delimited JSON |
| `metadata.json` | machine-readable card: license, columns, languages, row count, `generated_at` |
| `CITATION.cff` | citation metadata (GitHub "Cite this repository") |
| `LICENSE` | CC0 1.0 Universal legal code |

This bundle holds **two datasets**: the current **ranking** (`humans.*`, rich rows
with bios/links/facts) and the daily **rank history** (`humans-rank-history.*`, a
lean time series). Join them on **`user_number`**.

## Schema

| column | meaning |
|---|---|
| `rank` | 1-based live global position (most influential = 1) |
| `user_number` | permanent public ID; appears in the profile URL, never changes |
| `name` | display name (English, canonical) |
| `tier` | influence tier from rank: `gold` (1–3), `silver` (4–10), `blue` (11+) |
| `wikidata_qid` | Wikidata entity Q-ID for disambiguation (e.g. `Q22686`); may be empty |
| `wikidata_url` | full Wikidata entity URL; may be empty |
| `wikipedia_url` | English Wikipedia article URL; may be empty |
| `profile_url` | canonical humans.top profile page |
| `avatar_url` | portrait image URL (illustrative, AI-generated); may be empty |
| `og_image_url` | Open Graph share-card image URL; may be empty |
| `birth_year` / `birth_month` / `birth_day` | date of birth (month/day may be empty) |
| `sex` | `male` or `female` |
| `native_name` | name in the person's native language/script (e.g. `习近平`); may be empty |
| `citizenships_iso` | list of the person's countries of citizenship as ISO 3166-1 alpha-2 codes, space-separated (one for most; multi-nationals list all, e.g. `CA US ZA`); may be empty |
| `official_website` | official website URL; may be empty |
| `x_handle` | X / Twitter handle without the `@`; may be empty |
| `wikidata_description` | short English factual descriptor (Wikidata entity description); may be empty |
| `viaf_id` / `isni` / `freebase_id` | cross-reference authority IDs for entity resolution; may be empty |
| `bio_en` … `bio_ja` | concise descriptor in 15 languages: `en, ru, de, fr, it, es, tr, zh-Hans, pl, sr-Cyrl, ar, hi, bn, pt, ja` |

### Rank history (`humans-rank-history.*`)

A lean daily time series of how each person's rank has moved — **history begins
2026-06-25** and grows by one snapshot per ranking change. Join to the ranking
above by `user_number` to attach names/bios.

| column | meaning |
|---|---|
| `date` | snapshot date, `YYYY-MM-DD` (UTC) |
| `user_number` | the person — **foreign key to `humans.*`** (and the `/u/{user_number}` profile) |
| `rank` | that person's 1-based global rank on that date |

```python
import pandas as pd
hist = pd.read_csv("humans-rank-history.csv")
top = pd.read_csv("humans.csv")[["user_number", "name"]]
# rank trajectory of #1 today
who = top.iloc[0]["user_number"]
print(hist[hist.user_number == who][["date", "rank"]])
```

## Usage

```python
import pandas as pd
df = pd.read_csv("https://humans.top/dataset/humans.csv")
print(df[["rank", "name", "tier", "wikidata_qid"]].head(10))
```

```python
# JSON Lines
import json, urllib.request
rows = [json.loads(l) for l in
        urllib.request.urlopen("https://humans.top/dataset/humans.jsonl").read().decode().splitlines()]
```

```sql
-- DuckDB: query the Hub's auto-converted Parquet directly, no download
INSTALL httpfs; LOAD httpfs;
SELECT rank, name, tier, wikidata_qid
FROM 'hf://datasets/dsfox/humans-top@~parquet/default/train/*.parquet'
ORDER BY rank
LIMIT 10;
```

Or **browse & SQL it online** (no setup): the [Datasette
browser](https://huggingface.co/spaces/dsfox/humans-top-datasette).

## How it's built

The ranking is generated from public statistical sources by a fixed formula and
refreshed automatically; the editorial descriptors are written and translated by
the project. This repository is a snapshot of the live endpoints above,
regenerated by [`scripts/publish_dataset.py`](https://humans.top) — re-run it to
refresh the files.

## How to cite

Cite via the DOI [10.5281/zenodo.20809207](https://doi.org/10.5281/zenodo.20809207)
(it always resolves to the latest version), or:

```bibtex
@misc{humanstop,
  title  = {humans.top — Global ranking of influential people (open dataset)},
  author = {Golubnichiy, Dmitry},
  year   = {2026},
  doi    = {10.5281/zenodo.20809207},
  url    = {https://humans.top/dataset},
  note   = {CC0 1.0 (public domain)}
}
```

## Notes & disclaimer

- Descriptors are short, deliberately wry editorial summaries — not neutral
  encyclopedia text. Use `wikidata_qid` / `wikipedia_url` for authoritative facts.
- `avatar_url` images are AI-generated illustrations, not photographs.
- Ranks change over time; `metadata.json.generated_at` records this snapshot's time.

Source & project: <https://humans.top>
