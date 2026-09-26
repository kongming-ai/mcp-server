# KongMing Growth MCP

Cross-border e-commerce growth data & analysis over MCP (Model Context Protocol).

**Endpoint**: `https://mcp.kmingai.com/mcp` · Transport: Streamable HTTP · Auth: `Authorization: Bearer <key>`

Live since 2026-09-26. Mainland-China direct: 5/5 reachable, 628ms avg.

## What you get

11 tools for DTC brands selling cross-border (Amazon / independent sites):

| Tool | Answers |
|---|---|
| query_mentions | Multi-channel customer mentions with sources & sample sizes |
| get_pain_points | Pain-point clustering, severity-ranked |
| get_sentiment_trend | Sentiment trend over time |
| query_competitor_mentions | Competitor mention queries |
| compare_competitors | Brand-vs-competitor comparison matrix |
| query_geo_visibility | AI-engine visibility per query (GEO) |
| get_geo_audit_snapshot | Self-contained AI-visibility audit snapshot |
| generate_strategic_synthesis | 7-layer strategic synthesis, evidence-backed |
| search_knowledge | Semantic search over your knowledge base |
| model_catalog_get | Model catalog (credits language) |
| media_estimate_price | Media pricing quote *(coming soon)* |

Every answer carries **sources, sample sizes and time windows**. Zero fabrication — insufficient data is stated as insufficient.

## Quickstart

1. Get an API key: [app.kmingai.com](https://app.kmingai.com) → Settings → Developer → Create API Key.
2. Point any MCP client at the endpoint with your key as a Bearer header — see [docs/quickstart.md](docs/quickstart.md) for per-client guides (Doubao Work, Tencent WorkBuddy, generic EN).
3. Ask: *"What are my customers complaining about in the last 30 days?"*

## Billing

Credits — **same price as the KongMing workspace, no markup for external shells.** Per-key usage caps. New accounts include trial credits.

## Docs

- [Quickstart & per-client guides](docs/quickstart.md)
- [server.json (MCP Registry metadata)](server.json)

## Status

- [x] Public endpoint live
- [x] 10/11 tools open (media pricing quote coming soon)
- [ ] Error codes -32008/-32009 activation (with E4 billing window)
- [ ] Marketplace listings (WorkBuddy / Doubao / Coze) in review pipeline

© KongMing AI — [www.kmingai.com](https://www.kmingai.com)
