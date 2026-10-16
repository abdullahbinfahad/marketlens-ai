# MarketLens AI — Master Prompt Audit

**Audit date:** 2026-10-16
**Public application:** https://www.marketlens-ai.com/
**Public source repository:** https://github.com/abdullahbinfahad/marketlens-ai
**Protected analytical backend:** https://github.com/abdullahbinfahad/marketlens.ai

## Current architecture

The custom domain is a browser-native static application hosted by GitHub Pages. Its public search flow uses a provider-independent normalization and scoring layer implemented in `index.html` and calls approved free public endpoints directly from the browser. The source registry is maintained in `source_registry.js`.

The protected Streamlit repository contains the Python data layer, provider adapters, deterministic analytics, optional credential-backed connectors, and backend-oriented deployment documentation. GitHub Pages cannot safely run Python or hold provider secrets, so credentialed providers are not exposed in the public client.

## Working now

- Search-first MarketLens interface.
- Free live queries to DummyJSON, Fake Store API, Open Beauty Facts, and Open Food Facts when the query is relevant.
- Canonical product normalization with source/provider attribution.
- Duplicate reduction for returned product records.
- Deterministic Demand, Profit, Risk, and Competition Gap scoring.
- Fixed 40/30/20/10 decision model with GO, CAUTION, and PASS thresholds.
- Evidence confidence shown separately from the market score.
- Product detail/provenance panel.
- Comparison view.
- Deterministic profitability simulator.
- Methodology and limitations disclosure.
- 68-entry source universe showing live, optional, watchlist, and reference status.
- Product-result entrance animation and restrained UI motion.
- GitHub Pages validation workflow and JavaScript syntax checks.
- Broad-query fallback that ranks up to 60 closest available catalog records when an exact provider search returns no match, with explicit fallback labeling.

### Verified catalog coverage boundary

The available managed workbook was checked during the final audit and contains **15 real product records**. The free browser providers add a small public prototype catalog and sample listings. Therefore, the public site does not claim thousands of exact results for every query. It returns ranked closest-available records when possible and labels them clearly.

A truthful requirement for thousands of results and broad comparisons needs a connected 20K+ managed catalog or authorized marketplace backend. Generating duplicates or invented products would corrupt the scoring, comparison, and evidence model, so it is intentionally not used.

## Verified integrations

| Provider | Actual status | Verification |
|---|---|---|
| DummyJSON Products | Implemented and live-verified | Browser search smoke tests returned normalized products |
| Fake Store API | Implemented and live-verified | Browser search/provider response checks |
| Open Beauty Facts | Implemented and live-verified | Browser search/provider response checks; relevant beauty queries |
| Open Food Facts | Implemented in source catalog/live-capable path | Provider endpoint is configured; response depends on query relevance |
| eBay, Keepa, Shopify, SerpApi, DataForSEO, Apify, RapidAPI | Catalogued as optional; not live-verified in the public client | Credentials/account authorization not supplied |
| Google Trends, Amazon, Walmart, Etsy, shipping providers, trade and research sources | Catalogued as watchlist/reference; not claimed as live | Integration and licensing/access requirements vary |

## Important acceptance gaps

The following master-prompt requirements are not claimed complete on the static public domain:

1. Seven fully separate search workflows with mode-specific provider routing.
2. Authorized marketplace credentials for a genuine marketplace API such as eBay Browse.
3. Historical trend series from an authorized trend provider.
4. Authorized review retrieval and review-sample analytics.
5. Backend source health, rate limiting, caching, and structured operational logs.
6. Saved reports, watchlists, user accounts, and persistent research history on the public domain.
7. PDF/CSV deep-research report generation from the public domain.
8. AI-generated research synthesis with structured output and citation evaluation.
9. Full mobile/desktop automated browser test coverage.

These are documented gaps, not hidden placeholders. The current free public product remains usable for live prototype product intelligence and deterministic calculation workflows.

## Next highest-value improvement

Deploy the protected Streamlit backend as the server-side API for the public domain, then add one authorized marketplace integration—preferably eBay Browse API—behind that backend. The browser should call the backend for credentialed data while retaining the free-provider fallback. The integration should be marked live only after a real authorized smoke test passes.

## Security and data handling

- No provider secrets are present in the public HTML or JavaScript.
- Optional credentials remain a backend configuration concern.
- Public provider responses are treated as untrusted data and escaped before rendering.
- Missing values remain unavailable rather than being converted to zero.
- Scores are deterministic calculations, not LLM-generated arithmetic.
- GO, CAUTION, and PASS are research-prioritization labels, not sales guarantees.
