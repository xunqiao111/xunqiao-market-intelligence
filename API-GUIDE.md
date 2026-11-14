# Xun Qiao developer API guide

**[Website](https://xunqiao.qq10.fun/) · [Product overview](README.md) · [Live API documentation](https://xunqiao.qq10.fun/api-docs)**

## Access model

The developer API is a **read-only interface for site data**. Gold membership and an authorized API key are required for calls. Free members can view the developer page and learn the interface before upgrading.

**Real-time candlestick series are excluded.** Quote snapshots and asset metrics are available according to account and source permissions. A research observation that is readable on the website may still have source-specific export restrictions.

Base URL: `https://xunqiao.qq10.fun/api/v1`

### Create and store your key

Open the API page, choose key management, give the integration a name and select the minimum read-only scopes it needs. `data.read` is the broad site-data scope. Add `workspace.read` only when the integration needs **your own** private workspace, plans or inbox.

Save a newly created secret immediately: its full value is shown once. Store it on your server in an environment variable such as `XQ_API_KEY`. Use the Authorization header. Do not put secrets in query strings, public repositories, screenshots or frontend bundles.

Revocation and membership expiry stop access. Keys are not permissions to access other users' accounts or redistribute restricted source material.

## The 26 data recipes

All routes below are relative to the base URL and use **GET**. The live documentation remains authoritative for current limits and response details.

| Recipe | Route | What to use it for |
| --- | --- | --- |
| Homepage overview | `/data/overview` | Prepared market focus, recent news and calendar; no candle arrays. |
| All news | `/data/news` | Searchable, paginated news. Supports `view`, `q`, `locale`, `limit` and `cursor`. |
| Official updates | `/data/news?view=official` | Collected first-party announcements and official updates. |
| AI selections | `/data/news?view=picks` | Reviewed selections and consolidated event sets. |
| Continuing macro impacts | `/data/news?view=macro` | Macro research within its observation window. |
| Story / event detail | `/data/stories/{id}` | Body, bilingual analysis, asset links and event-set members. |
| Instrument directory | `/data/instruments` | Search by name or ticker; filter crypto, U.S. stocks or tokenized instruments. |
| Quotes and metrics | `/data/quotes` | Quotes and key metrics for stable instrument IDs. |
| Complete asset workspace | `/data/assets/{id}` | Quote, market cap, range, volume, events, evidence, reports and reviews; no candles. |
| Related events | `/data/assets/{id}/events` | The asset's related-event ledger, with pagination and constituent counts. |
| Asset research | `/data/assets/{id}/research` | Source-specific research observations, units and timestamps. |
| AI reports | `/data/reports` | Ratings, conditions, metrics and report changes; optionally filter an asset. |
| Five-hour reviews | `/data/reviews` | Available reviews and statistics, optionally for an asset. |
| 24h association counts | `/data/relations` | Association rankings across grades and distinct event-set constituents. |
| Research records | `/data/research/records` | Search derivatives, stablecoin, DeFi, on-chain, unlock and other research records. |
| Record detail | `/data/research/records/{id}` | Inspect a research record using its returned record ID. |
| Calendar | `/data/calendar` | Supply, activity and macro dates with source time precision. |
| Source directory | `/data/sources` | Public source names, categories and status; no collector credentials. |
| My workspace | `/data/me/workspace` | Your own selected assets, watchlist, notes and preferences; needs `workspace.read`. |
| My research plans | `/data/me/plans` | Your own plans, optionally filtered by asset; needs `workspace.read`. |
| My inbox | `/data/me/alerts` | Your own announcements and alerts; needs `workspace.read`. |
| News changes | `/data/news/updates` | Incremental additions, corrections and withdrawals. |
| News metrics | `/data/news/metrics` | Hourly and daily news-supply statistics. |
| Research changes | `/data/research/changes` | Incremental research changes using an account-bound cursor. |
| Research coverage | `/data/research/status` | Public research categories, coverage and update status. |
| Tag taxonomy | `/data/taxonomy` | Tradable-asset, subgroup, sector and macro definitions. |

## Start with news

These examples read the first page of AI selections. Configure `XQ_API_KEY` privately before running them.

### cURL

```bash
curl --fail-with-body \
  -H "Authorization: Bearer $XQ_API_KEY" \
  "https://xunqiao.qq10.fun/api/v1/data/news?view=picks&limit=10"
```

### Server-side JavaScript

```javascript
const response = await fetch(
  "https://xunqiao.qq10.fun/api/v1/data/news?view=picks&limit=10",
  { headers: { Authorization: `Bearer ${process.env.XQ_API_KEY}` } }
);
if (!response.ok) throw new Error(`HTTP ${response.status}`);
console.log(await response.json());
```

### Python

```python
import json
import os
from urllib.request import Request, urlopen

request = Request(
    "https://xunqiao.qq10.fun/api/v1/data/news?view=picks&limit=10",
    headers={"Authorization": "Bearer " + os.environ["XQ_API_KEY"]},
)
with urlopen(request, timeout=30) as response:
    print(json.dumps(json.load(response), ensure_ascii=False, indent=2))
```

## Find an instrument, then query its workspace

### Search instead of guessing an identifier

```text
GET /data/instruments?q=NVIDIA&market=us&limit=10
```

Use the stable ID returned by the directory. Examples of the ID format include `us:NVDA` and `kraken:XBTUSD`. An equity, a stock token and a cryptocurrency with similar ticker letters are distinct instruments.

### Read the complete asset view

```text
GET /data/assets/us%3ANVDA
GET /data/assets/us%3ANVDA/events?limit=30
GET /data/assets/us%3ANVDA/research
```

The asset workspace combines the site's available asset information without exposing a real-time candle series. Use the event endpoint to page through related reporting, and research for observations with their source definitions intact.

### Read quotes for several instruments

```text
GET /data/quotes?ids=kraken%3AXBTUSD,us%3ANVDA
```

The catalog supports up to 40 stable IDs per `ids` request. Directory-style quote queries also support search and market pagination. Never merge values solely because their ticker strings match.

## Reports, reviews and evidence

```text
GET /data/reports?asset_id=kraken%3AXBTUSD&limit=10
GET /data/reviews?asset_id=kraken%3AXBTUSD
GET /data/research/records?category=derivatives&limit=30
GET /data/calendar?after=2026-11-15&before=2026-11-22
GET /data/taxonomy
```

Reports accept an optional `asset_id`, a `before` timestamp and up to 30 items. Reviews may be pending when the observation window has not ended or price inputs are insufficient. Use record IDs returned by the records endpoint for details; do not invent them.

The dates above are an example query window. Replace them with the dates relevant to your application.

## Pagination and incremental synchronization

### Collection pages

Follow each endpoint's returned `next_cursor` or documented pagination fields. News and association queries allow up to 100 items per request in the current catalog. Research records additionally document `offset`; report history uses `before`. Do not assume every resource uses the same cursor type.

### Changed items

For news updates, start with `after` omitted to acquire the current cursor. Send that returned value in later requests. Handle new items, corrections and withdrawals instead of treating every response as append-only.

Research changes also use an account-bound cursor. Keep independent cursor state for different accounts and data streams. On a **409**, rebuild the affected snapshot and obtain a valid cursor. On a **429**, respect **Retry-After**. Back off on temporary failures rather than retrying in a tight loop.

The current integration guide lists per-key and per-account request limits. Check the live guide before sizing an integration; access and fair-use controls can change.

## Try the interface before writing a client

On the developer page, choose a data recipe, fill an identifier or search term, adjust the page size and switch example languages. You can copy or download the request. With access enabled, use the permitted live-data control to inspect a response and download the full result.

The interface helps explain route shape, but an authenticated response is still subject to the key's scopes, account status, source permissions and actual data coverage.

## Common integration mistakes

- **Using a ticker as a global identity:** resolve the stable instrument ID first.
- **Interpreting missing as zero:** retain unavailable and pending states.
- **Mixing units:** shares, base-asset volume, USD turnover and source-specific units are different quantities.
- **Assuming a review is a trade:** reviews are hypothetical research observations.
- **Hard-coding a secret:** keep it in a private server environment and revoke it if exposed.
- **Exporting another account's data:** private routes expose only the authenticated user's authorized workspace.
- **Requesting candles externally:** real-time candlestick series are not part of this API.

**[Return to the product introduction](README.md)**
