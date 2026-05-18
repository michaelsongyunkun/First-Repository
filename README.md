# Web Crawler Skill

A small, polite web-crawling skill for Codex. It inspects list pages, suggests selectors, extracts structured records, optionally enriches records from detail pages, cleans common field types, and exports CSV, Excel, JSON, and a crawl report.

## Features

- Heuristic list, detail, and pagination detection
- Optional JSON or simple YAML selector configs
- Starter config generation with `--suggest-config`
- One-page diagnostics with `--inspect`
- Custom fields such as `image`, `date`, `tags`, `brand`, `author`, and `category`
- Field cleaning for URLs, dates, lists, whitespace, site suffixes, replacements, and money text
- Same-host detail-page enrichment
- Robots.txt checks, retry, timeout, delay, and a polite user agent by default
- CSV, XLSX, JSON, and machine-readable report output

## Quick Start

```bash
python scripts/crawl_site.py "https://example.com/products" --output ./crawl-output --max-pages 5 --delay 1
```

Preview extraction quality:

```bash
python scripts/crawl_site.py "https://example.com/products" --inspect
```

Generate a starter config:

```bash
python scripts/crawl_site.py "https://example.com/products" --suggest-config
```

Run with a config:

```bash
python scripts/crawl_site.py "https://example.com/products" --config examples/crawler-config.json --output ./products
```

## Config Example

```json
{
  "selectors": {
    "list_item": ".product-card",
    "title": "h2, .title",
    "link": "a[href]",
    "price": ".price, [itemprop=price]",
    "summary": ".description",
    "next_page": "a[rel=next], .pagination .next"
  },
  "fields": {
    "image": {
      "selector": "img",
      "attr": "src",
      "type": "url"
    },
    "tags": {
      "selector": ".tag, .category",
      "type": "list",
      "multiple": true
    },
    "date": {
      "selector": "time",
      "attr": "datetime",
      "type": "date"
    }
  },
  "cleaning": {
    "title": ["remove_site_suffix"],
    "price": ["normalize_whitespace"]
  },
  "detail": {
    "enabled": false,
    "max_pages": 20,
    "selectors": {
      "brand": ".brand",
      "summary": ".product-description"
    }
  }
}
```

## Compliance

Crawling must follow applicable law, website terms, robots.txt, and data rights. Do not use this project to bypass authentication, CAPTCHAs, paywalls, IP bans, or other access controls. Keep crawl volume modest and use delays.

## Test

```bash
python scripts/self_test.py
```

## License

MIT License. See `LICENSE`.

## Repository Layout

- `SKILL.md`: Codex skill instructions
- `scripts/crawl_site.py`: crawler implementation
- `scripts/self_test.py`: self-checks
- `references/extraction-strategy.md`: extraction and cleaning guidance
- `examples/crawler-config.json`: starter config
