# Tools Reference

Reference for the 11 tools available on the KongMing Growth MCP connector.

> Generated from the tool docstrings in `workbench/mcp/server.py` (2026-09-27). Parameter names are taken verbatim from the registered tool schemas; descriptions are translated and condensed from the same docstrings. Launch-set definition: `workbench/marketplace` first-launch tools manifest v1 (`docs/marketplace/first-launch-tools-v1.md`).

## Credit status

- **10 of 11 tools are priced and grantable today** — standard credits, same schedule as the KongMing workspace, no markup for external clients.
- **`media_estimate_price` is coming soon.** Until it is opened, calling it returns a clean `-32007` (tool not yet open) instead of an estimated price. Do not list it as available before it is enabled.
- Every tool below is read-only: none of them collects data, sends messages, or spends generation credits on their own.

## Conventions shared by all tools

| Convention | Detail |
|---|---|
| Time windows | `period` accepts `7d` / `30d` / `90d` / `365d` (default `30d`). Where supported, `date_from` / `date_to` (`YYYY-MM-DD`, inclusive) override `period` — use these when the user names an explicit month or date range. |
| Window basis | Analysis windows filter by the customer's own utterance time (mention date), not by collection date. |
| Competitor channels | Competitor-channel data is excluded by default. Only set `include_competitor: true` when the user explicitly asks for a competitor comparison. |
| Evidence | Numeric answers carry source row IDs, verbatim quotes and sample size, so every figure can be traced back to its source rows. Cells without data are shown as `N/A` — never as `0`. |

---

## query_mentions

Query your brand's customer mentions across channels (reviews, posts, uploaded records), filterable by time range, platform, rating, keyword and product. Returns mention counts, rating distribution, verbatim quotes and source row IDs.

Best for **single-point questions** — counts, content, quotes or a ranked list under one filter (e.g. "what are the return reasons, ranked": `keyword` filter, results come back sorted by volume). For trends or period-over-period comparisons use `get_sentiment_trend`; for pain-point-category priorities use `get_pain_points`.

| Parameter | Type | Required | Description |
|---|---|---|---|
| `period` | string | No | Time range: `7d` / `30d` / `90d` / `365d`. Default `30d`. |
| `platform` | string | No | Platform filter: `amazon_reviews` / `reddit` / `youtube` / `tiktok` / `trustpilot`, etc. Empty = all. |
| `rating` | string | No | Rating filter: `1` (single star) or `3-5` (a range). Empty = all. |
| `keyword` | string | No | Keyword searched in titles and body text. |
| `product_id` | string | No | Product UUID or ASIN. Empty = all. |
| `limit` | integer | No | Max rows returned. Default 20, cap 100. |
| `date_from` | string | No | Absolute start date `YYYY-MM-DD` (e.g. user names "all of May" → `2026-05-01`). Overrides `period` when given. |
| `date_to` | string | No | Absolute end date `YYYY-MM-DD`, inclusive. Pairs with `date_from`. |
| `include_competitor` | boolean | No | Include competitor-channel data (platforms ending in `_competitor`). Default `false` (excluded). |

---

## get_pain_points

Pain-point clustering: customer complaints aggregated by category, with severity, cross-platform distribution, emerging / resolved category detection and top-priority candidates. Every figure carries source IDs, verbatim quotes and sample size.

Best for **category-aggregated questions** ("what are customers complaining about", "which issues stand out most"). For raw quotes and counts on one specific topic use `query_mentions`; for "what should we prioritize overall" use `generate_strategic_synthesis`, which is the closing tool for synthesis-type questions.

| Parameter | Type | Required | Description |
|---|---|---|---|
| `period` | string | No | Analysis window: `7d` / `30d` / `90d`. Default `30d`. |
| `platform` | string | No | Platform filter. Empty = all. |
| `min_count` | integer | No | Minimum occurrences for a category to be reported. Default 3. |
| `include_other` | boolean | No | Include the uncategorized `other` noise bucket. Default `false`. |
| `include_competitor` | boolean | No | Include competitor-channel data. Default `false` (excluded). |

---

## get_sentiment_trend

Multi-channel sentiment trend: day- and week-level positive / neutral / negative time series, trend direction and period-over-period deltas, built from real customer mentions with source IDs, quotes and sample size.

Best for any question about **change over time** ("is it improving", "vs last week", "compare two periods"). It performs the time-series aggregation itself — do not chain multiple `query_mentions` calls and stitch two snapshots together, which yields non-comparable windows.

| Parameter | Type | Required | Description |
|---|---|---|---|
| `period` | string | No | Analysis window: `7d` / `30d` / `90d` / `365d`. Default `30d`. |
| `platform` | string | No | Platform filter. Empty = all. |
| `product_id` | string | No | Product filter (UUID or ASIN). Empty = all. |
| `date_from` | string | No | Absolute start date `YYYY-MM-DD` (use when the user names a month or date range). Overrides `period` when given. |
| `date_to` | string | No | Absolute end date `YYYY-MM-DD`, inclusive. Pairs with `date_from`. |
| `include_competitor` | boolean | No | Include competitor-channel data. Default `false` (excluded). |

---

## query_competitor_mentions

Query competitor-related signals: the mentions where customers name a competitor by name — verbatim quotes, sentiment and category — filterable by competitor, sentiment and keyword. Answers "who do customers compare us against, and on what".

| Parameter | Type | Required | Description |
|---|---|---|---|
| `period` | string | No | Time range: `7d` / `30d` / `90d`. Default `30d`. |
| `competitor` | string | No | Competitor name filter (exact match against analyzed competitor names). Empty = all. |
| `sentiment` | string | No | Sentiment filter: `positive` / `negative` / `neutral` / `mixed`. Empty = all. |
| `keyword` | string | No | Keyword searched in the original text. |
| `limit` | integer | No | Max rows returned. Default 20, cap 50. |

---

## compare_competitors

Build a brand-vs-competitor comparison matrix: each competitor's latest rating, review count and price side by side with your comparable products, plus the main pain-point categories customers raise about each competitor. Every figure carries source IDs and sample size; cells without data show `N/A` (never a fabricated `0`).

| Parameter | Type | Required | Description |
|---|---|---|---|
| `window_days` | integer | No | Pain-point category aggregation window in days. Default 30. |

---

## query_geo_visibility

AI search visibility detail: the raw audit records of where your brand was mentioned or cited in answers from AI engines (Gemini grounding, Perplexity, ChatGPT search, etc.), filterable by engine, brand-mentioned status and sentiment. Every conclusion carries source IDs, verbatim quotes and sample size.

| Parameter | Type | Required | Description |
|---|---|---|---|
| `period` | string | No | Time range: `7d` / `30d` / `90d`. Default `30d`. |
| `engine` | string | No | Engine filter, e.g. `gemini_grounding` / `perplexity_sonar` / `chatgpt_search`. Empty = all. |
| `brand_mentioned` | boolean | No | Restrict to records where the brand was mentioned (`true`) or not mentioned (`false`). Empty = all. |
| `sentiment` | string | No | Sentiment filter: `positive` / `neutral` / `negative`. Empty = all. |
| `limit` | integer | No | Max detail rows returned. Default 20, cap 50. |

---

## get_geo_audit_snapshot

Fetch the most recent AI-visibility audit snapshot: AI Visibility Score (AVS), brand mention rate, citation rate, negative-mention rate, per-engine breakdown, and the real audit call volume within the window. The snapshot is read from the aggregated score table (not recomputed) and carries an evidence envelope with source IDs.

| Parameter | Type | Required | Description |
|---|---|---|---|
| `window_days` | integer | No | Audit-volume statistics window in days. Default 30. |

---

## generate_strategic_synthesis

Generate a strategic synthesis from a senior strategist's point of view: customer-voice data distilled into a 7-layer strategic framework (moat / conversion poison / hidden goldmine / perception bias / competitor baseline / warning signals) plus the single most important thing for the CEO and prioritized actions. Every conclusion carries structured evidence (metric, value, source IDs, sample size); inferences are explicitly labeled as inferred with the reasoning bridge stated; when data is insufficient it says so rather than filling the gap.

| Parameter | Type | Required | Description |
|---|---|---|---|
| `period` | string | No | Synthesis window: `7d` / `30d` / `90d`. Default `30d`. |

---

## search_knowledge

Semantic search over your private knowledge base (product materials, policies, uploaded company documents). Returns the most relevant passages, their source file names and similarity. This is the first tool to call for questions about your own company's information.

| Parameter | Type | Required | Description |
|---|---|---|---|
| `query` | string | Yes | The question or key information to retrieve, in natural language (Chinese or English). |
| `limit` | integer | No | Number of passages returned. Default 8, cap 20. |
| `min_similarity` | number | No | Minimum similarity threshold, 0–1. Default 0.3. |

---

## model_catalog_get

Read the current model catalog: the model list, filter axes, anchor credits, scale multipliers and generation times. This is the data source behind the model selector; the catalog is maintained by the system on a schedule and needs no refresh call.

| Parameter | Type | Required | Description |
|---|---|---|---|
| *(none)* | — | — | Takes no parameters. |

---

## media_estimate_price

Pre-quote lookup (free): returns the credit preview price for a generation tier and whether the current balance is sufficient. Spec parameters (images: resolution / aspect ratio / number of images; video: duration / definition / orientation) must match the final submission exactly — the preview price a customer sees is the price charged at submission. Light tools (subtitle removal / watermark removal / video upscale) are also quoted through this tool; video upscale is priced by duration, so the source video's length must be passed.

**Status: coming soon.** Calling it returns `-32007` (tool not yet open) until it is enabled.

| Parameter | Type | Required | Description |
|---|---|---|---|
| `task_type` | string | Yes | Task tier: `image_standard` / `image_hd` / `video_draft` / `video_standard` / `video_premium` / `video_brand`, or a light tool: `remove_subtitle` / `remove_watermark` / `upscale_video`. |
| `resolution` | string | No | Image resolution: `1K` / `2K` / `4K` (image tiers only; tier default if empty). For video upscale: `1080p` / `2k` / `4k`. |
| `aspect_ratio` | string | No | Image aspect ratio: `1:1` / `3:4` / `4:3` / `9:16` / `16:9` / `21:9`. Tier default if empty. |
| `n` | integer | No | Number of images, 1–4 (image tiers only). Tier default if empty. |
| `duration` | number | No | Source video duration in seconds (video upscale only; if empty, quoted at the most expensive 60-second cap). |
| `fps` | integer | No | Target frame rate `30` / `60` (video upscale only, default 30). |
| `seconds` | integer | No | Output duration in seconds (video tiers only; tier default if empty — allowed values vary by tier, e.g. `6`/`8`/`10`/`12`/`15` for fast draft, `4`/`8`/`12` for standard output). |
| `definition` | string | No | Definition (video tiers only, tier default if empty): `480p` / `720p` / `768p`, etc. — allowed values depend on the model. |
| `orientation` | string | No | Video orientation (video tiers only, tier default if empty): `9:16` / `16:9`, etc. |
| `model` | string | No | Model choice (leave empty for the tier's default model) — a model ID from the catalog, e.g. `doubao-seedance-2` / `nano-banana-pro`. Always confirm the selectable list by reading `model_catalog_get`. |
| `quality` | string | No | Quality tier `low` / `medium` / `high` (only effective for `gpt-image-2-official`; that model's default if empty). |

---

## Tool-selection cheat sheet

| Question shape | Tool |
|---|---|
| Counts / quotes / ranked list under one filter | `query_mentions` |
| What customers complain about, by category, with priorities | `get_pain_points` |
| Change over time / period comparison | `get_sentiment_trend` |
| Who customers compare us against, and on what | `query_competitor_mentions` |
| Side-by-side brand vs competitor matrix | `compare_competitors` |
| Where we appear in AI engine answers | `query_geo_visibility` |
| Latest AI-visibility scorecard in one read | `get_geo_audit_snapshot` |
| What should we do overall (closing synthesis) | `generate_strategic_synthesis` |
| Anything about the customer's own company documents | `search_knowledge` |
| Which models exist and what they cost in credits | `model_catalog_get` |
| Preview price before a media generation | `media_estimate_price` *(coming soon)* |

## Sources

- Tool definitions and parameter semantics: `workbench/mcp/server.py` (registered tool docstrings).
- Launch-set membership and pricing status: `workbench/marketplace` first-launch tools manifest v1 (`docs/marketplace/first-launch-tools-v1.md`).
