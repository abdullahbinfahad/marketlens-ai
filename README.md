# MarketLens AI — Custom Domain Application

This repository serves [marketlens-ai.com](https://marketlens-ai.com/) through GitHub Pages.

The published homepage is a browser-native, search-first product intelligence system. It queries three free public providers directly from the browser:

- DummyJSON for free product search and prototype fields
- Fake Store API for free sample prices, ratings, and reviews
- Open Beauty Facts for free beauty identity and ingredient data

The page normalizes returned records, removes duplicate product/brand matches, calculates evidence confidence, provides Demand/Profit/Risk/Competition Gap signals, shows GO/CAUTION/PASS decisions, compares results, and runs transparent profitability simulations.

The fixed decision formula is:

```text
MarketScore = Demand × 40% + Profit × 30% − Risk × 20% + Competition Gap × 10%
```

The domain version intentionally does not expose API keys. Credential-backed eBay, Keepa, Shopify, and DeepSeek connectors remain in the protected Streamlit backend repository at [abdullahbinfahad/marketlens.ai](https://github.com/abdullahbinfahad/marketlens.ai).

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
