# AnyCrawler Public Crawl API

This reference keeps only the minimum contract an agent needs at runtime.

## Base setup

- Base URL: `https://api.anycrawler.com`
- API key env var: `ANYCRAWLER_API_KEY`
- Optional base URL env var: `ANYCRAWLER_BASE_URL`
- Preferred client: `scripts/anycrawler_crawl_api.py`

## Endpoint selection

| Need | Use |
| --- | --- |
| Read or extract webpage content | `POST /v1/crawl/page` |
| Capture a screenshot | `POST /v1/crawl/screenshot` |

## `page` request fields

| Field | Notes |
| --- | --- |
| `url` | Required target URL |
| `method` | `fetch` first, `render` for dynamic/incomplete pages |
| `accept_cache` | Use when freshness is not critical |
| `include_metadata` | Enables `results.metadata` |
| `include_links` | Enables `results.links` |
| `include_media` | Enables `results.media` |
| `markdown_variant` | `markdown` or `readability`; output stays in `results.markdown` |
| `browser_wait_until` | Only for `method=render` |

## `screenshot` request fields

| Field | Notes |
| --- | --- |
| `url` | Required target URL |
| `full_page` | Full-page by default |

## Response fields to care about

### Shared

- `data.ok`
- `data.error`
- `data.retryable`
- `meta.requestId`

### `page`

- `data.results.markdown`
- `data.results.metadata` when requested
- `data.results.links` when requested
- `data.results.media` when requested

### `screenshot`

- `data.results.snapshot_url`

## Error handling

| Status | Handling |
| --- | --- |
| `400` | Invalid request; fix input before retry |
| `401` | Invalid or missing API key |
| `402` | Account capacity issue; do not blind retry |
| `403` | Usually account or paid-plan field issue; remove ineligible fields or fix account state |
| `409` | Retryable after backoff |
| `429` | Retryable after backoff; also check quota/concurrency pressure |
| `502` | Retryable after backoff |
| `504` | Retryable after backoff |

## Retry rules

1. Record `meta.requestId` on every failure.
2. Check `data.retryable` before retrying.
3. Prefer changing the request for `400`, `401`, `402`, and most `403` responses.
4. Back off before retrying `409`, `429`, `502`, and `504`.

Advanced paid-plan overrides and full gateway metadata live in `maintainer.md`.

For release, billing, headers, and the full error catalog, see `maintainer.md`.
