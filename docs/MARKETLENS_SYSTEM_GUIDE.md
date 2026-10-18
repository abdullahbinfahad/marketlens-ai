# MarketLens AI — Full System Guide

## What is deployed

MarketLens AI is a browser-native product-intelligence site served from GitHub Pages at `marketlens-ai.com`. The public app has a search-first home page, live public-provider normalization, transparent product scoring, a comparison workspace, profitability simulation, and a separate **Trends & Opportunities** page.

The system includes a real 12,000-record catalogue asset at `catalogue.json`. It is built from public Open Beauty Facts and Open Pet Food Facts records. It provides discovery and identity coverage, but it is not a 12,000-attribute record for every product. Missing price, review, demand, or profit fields remain unavailable instead of being invented.

## File map

- `index.html` — public search application, provider cascade, normalization, scoring, workspace views, branding, and bottom Trends & Opportunities link.
- `trends.html` — separate live-trends page. It queries DummyJSON and Fake Store API, shows returned price/rating/review fields, links to Google Trends, and explains the evidence model.
- `catalogue.json` — 12,000 public catalogue records with product identity, brand, category, description, countries, update field, provider, and source URL.
- `source_registry.js` — source directory used by the methodology view.
- `assets/marketlens-ai-logo.png` — supplied MarketLens AI logo used in the favicon, header, hero/banner, and footer.
- `.github/workflows/validate-pages.yml` — automated Pages validation.

## Search data flow

1. The user submits a product query.
2. The browser queries the managed catalogue and free public endpoints in parallel.
3. Returned records are normalized into the shared fields used by the workspace.
4. Duplicate product/brand records are merged.
5. Exact manufacturer matches are placed before related results.
6. Catalogue records are filtered by token relevance and sorted by relevance before entering the result set.
7. The interface displays source, price, rating, review count, evidence confidence, and decision state for every result.

The public cascade includes DummyJSON, Fake Store API, Open Beauty Facts, Open Food Facts, Open Library, Google Books, Wikipedia, iTunes Search, the managed catalogue, and verified manufacturer records. Providers can fail independently; the search continues with the providers that respond.

## Decision model

The core score uses the fixed transparent weighting:

```text
MarketScore = Demand × 40% + Profit × 30% − Risk × 20% + Competition Gap × 10%
```

The UI uses `GO` at 70 or above, `CAUTION` from 45 through 69.99, and `PASS` below 45. Sparse or knowledge-only records are marked `VERIFY` so a high numeric score cannot be mistaken for proven commercial evidence.

The score uses only returned fields such as rating, reviews, sales, revenue, margin, price, saturation, seller count, sentiment, and competitor gap. A missing field lowers confidence; it is never filled with an invented value.

## Trends & Opportunities page

The dedicated page has three layers:

- Live public product observations from DummyJSON and Fake Store API.
- Public catalogue depth from the 12,000-record asset.
- External research links to Google Trends and Wikimedia Analytics for search interest and reference attention.

Products are ranked by an observed-signal proxy combining returned rating, review count, and price availability. This is not a sales forecast. Google Trends provides relative search interest, not revenue. Wikimedia pageviews provide public attention, not demand. Historical marketplace sales, seller counts, country-level prices, tariffs, and review-text sentiment require authorized backend connectors.

## Adding protected AI safely

Do not put OpenAI, DeepSeek, Google, eBay, Keepa, or Shopify keys in `index.html` or `trends.html`. Browser code is public. Add a server endpoint instead:

```text
Browser → /api/analyze → server-side provider/LLM credential → structured JSON → browser
```

The endpoint should receive a normalized product ID and research scope, fetch allowed sources, return citations and timestamps, and cache responses. The response should separate observed facts, model interpretation, and unknowns. The existing protected Streamlit backend repository is the appropriate place for credential-backed connectors.

## Local validation

```bash
python3 -m http.server 8000
# open http://localhost:8000/

python3 - <<'PY'
from pathlib import Path
import json, re
html = Path('index.html').read_text()
cat = json.loads(Path('catalogue.json').read_text())
assert len(cat['records']) >= 10000
assert 'trends.html' in html
Path('/tmp/marketlens-inline.js').write_text('\n'.join(re.findall(r'<script(?: src="[^\"]+")?>(.*?)</script>', html, re.S)))
PY
node --check /tmp/marketlens-inline.js

python3 - <<'PY'
from pathlib import Path
import re
s = Path('trends.html').read_text()
Path('/tmp/marketlens-trends.js').write_text('\n'.join(re.findall(r'<script>(.*?)</script>', s, re.S)))
PY
node --check /tmp/marketlens-trends.js
```

## Deployment

Push `index.html`, `trends.html`, `catalogue.json`, `source_registry.js`, `assets/`, and the workflow to the Pages repository. GitHub Pages serves the custom domain from its `CNAME` file. After propagation, test `/`, `/trends.html`, `/catalogue.json`, and searches such as `shampoo` or `Samsung A25`.

## Data-quality policy

MarketLens can provide a large searchable catalogue and useful signals, but no free source guarantees every product, rating, country, marketplace, or current trend. Product pages should show provenance, observation time where available, and confidence or verification state. The system must never create fake ratings, fake sales, fake review counts, or fake AI research claims to fill a blank field.
