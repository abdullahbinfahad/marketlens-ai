# MarketLens AI — Custom Domain Application

This repository serves [marketlens-ai.com](https://marketlens-ai.com/) through GitHub Pages.

The published homepage is a browser-native, search-first product intelligence system. It queries a free public-provider cascade directly from the browser:

- DummyJSON for free product search and prototype fields
- Fake Store API for free sample prices, ratings, and reviews
- Open Beauty Facts for free beauty identity and ingredient data
- Open Food Facts for free food identity data
- Open Library and Google Books for book and knowledge-product discovery
- Wikipedia for reference identity and explanation
- iTunes Search for public digital-product and media discovery

The Methodology view now includes a **102-source catalog** grouped into four honest states:

- **8 live/free** sources currently available to the public app or free-provider cascade
- **9 optional** sources that require credentials, accounts, or provider approval
- **26 watchlist** sources planned for compliant integrations
- **29 reference** sources for macro, trade, regulatory, logistics, and research context

The catalog is a source-universe roadmap, not a claim that every source is queried on every search. The UI explicitly shows which providers actually responded to the current search.

Product results use lightweight entrance animation, a subtle hero sweep, and interactive hover states. These are CSS effects only and do not change scoring or data interpretation.

## Advanced research scope

The search form includes progressive-disclosure controls for:

- Search mode: exact product, discovery, comparison, trend, reviews, profit, or deep research
- Primary market and comparison markets
- Research period
- Reporting currency
- Research objective

These fields define and display the user's research specification. The current free browser providers primarily return current product observations. They do not automatically create historical country comparisons, exchange-rate conversions, customs determinations, or market-specific demand conclusions. The interface states that limitation rather than presenting unsupported precision.

## Search behavior for unique queries

Provider search endpoints do not contain every product on the internet. MarketLens now queries multiple free domains in parallel, ranks available records by relevance, and returns a clearly labeled result set instead of displaying an empty screen. Results are grouped into exact verified identity, related/comparison, knowledge evidence, and unresolved search intent. These are not silently presented as exact marketplace matches, and the application does not fabricate thousands of products. Result volume is bounded by the records genuinely returned by the configured providers.

Exact identity matches are prioritized separately from related results. The first result can be a verified identity record only when its aliases or model identifier match the query. For example, `Samsung A25`, `Samsung Galaxy A25`, `Galaxy A25 5G`, and `SM-A256` resolve to the verified Samsung Galaxy A25 5G identity sourced from Samsung's official product documentation. Market-specific price and availability remain unavailable until a marketplace source returns them.

The application now has dedicated sections for **Search**, **Discovery**, **Compare**, **Trends**, **Reviews**, **Profit**, **Research**, and **Methodology**. Discovery filters current results by evidence coverage, price, and category/keyword. Trends and Reviews explicitly show when the free sources do not provide enough historical or review-text evidence.

Selecting a result opens `product.html`, a dedicated product intelligence page with the overview, evidence type, source link, score breakdown, confidence, interpretation, and limitations. Sparse identity/reference records receive **VERIFY** rather than being allowed to produce a false GO decision. A GO requires both the numeric threshold and sufficient observed commercial evidence.

The page normalizes returned records, removes duplicate product/brand matches, calculates evidence confidence, provides Demand/Profit/Risk/Competition Gap signals, shows GO/CAUTION/PASS decisions, compares results, and runs transparent profitability simulations.

The fixed decision formula is:

```text
MarketScore = Demand × 40% + Profit × 30% − Risk × 20% + Competition Gap × 10%
```

The domain version intentionally does not expose API keys. Credential-backed eBay, Keepa, Shopify, and DeepSeek connectors remain in the protected Streamlit backend repository at [abdullahbinfahad/marketlens.ai](https://github.com/abdullahbinfahad/marketlens.ai). No free public API can guarantee live exact coverage for every product, geography, price, or seller; those claims require authorized marketplace data.

## Local check

```bash
python - <<'PY'
from pathlib import Path
import re
html = Path('index.html').read_text()
assert 'dummyjson.com' in html
assert 'fakestoreapi.com' in html
assert 'world.openbeautyfacts.org' in html
Path('/tmp/marketlens-pages-inline.js').write_text('\n'.join(re.findall(r'<script>(.*?)</script>', html, re.S)))
PY
node --check /tmp/marketlens-pages-inline.js
```

The `CNAME` file preserves the custom hostname `www.marketlens-ai.com`; GitHub Pages redirects the apex `marketlens-ai.com` to that canonical host.
