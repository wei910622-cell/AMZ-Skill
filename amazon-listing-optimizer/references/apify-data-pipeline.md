# Apify Amazon Data Pipeline

Use this reference when the workspace includes the Amazon Apify skills or when Sorftime MCP is unavailable. This layer fuses:

- `amazon-keyword-search`
- `amazon-product-detail`
- `amazon-research`
- `amazon-reviews`
- `competitor-analyzer`

## Priority

Use Apify after Sorftime MCP and before asking the user to upload data. If `APIFY_TOKEN` is missing, ask for the token or request uploaded/pasted data.

## Data Collection Flow

1. Keyword search: use `amazon-keyword-search` to search one or more keywords and return ASINs, titles, prices, ratings, review counts, sponsored status, images, and product URLs.
2. Product detail: use `amazon-product-detail` to fetch target and competitor listing data, including title, brand, price, rating, bullets, description, breadcrumbs, BSR, images, variants, seller, and model/manufacturer fields.
3. Research orchestration: use `amazon-research` when the user starts from keywords and needs search results, product detail, and reviews collected in one run.
4. Review mining: use `amazon-reviews` to fetch recent/helpful/critical/positive/all-star reviews. Prefer critical reviews for pain points and positive reviews for buyer language and validated benefits.
5. Competitor analysis: use `competitor-analyzer` to extract table-stakes features, differentiated selling points, marketing language patterns, visual/A+ strategy, price bands, and attackable gaps.

## Normalized Inputs For Listing Optimization

Convert outputs into:

| Field | Source |
| --- | --- |
| `asin` | product detail or keyword search |
| `marketplace` | domain or region |
| `current_title` | product detail |
| `current_bullets` | product detail `about_item` |
| `current_description` | product detail `product_description` |
| `brand` | product detail `brand_name` |
| `category` | product detail `breadcrumbs` and BSR |
| `price` | keyword search or product detail |
| `rating_summary` | product detail rating and distribution |
| `variants` | product detail `default_variant` and review `variantSpecs` |
| `keyword_candidates` | keyword search, competitor titles/bullets, review language |
| `review_pains` | critical reviews, aspects, AI summaries |
| `buyer_language` | positive reviews and repeated phrases |
| `competitor_gaps` | competitor-analyzer output |

## Default Scrape Strategy

- For keyword-led research, search 1-3 seed keywords, 1-3 pages each.
- For detail extraction, fetch top 5-10 competitor ASINs plus the target ASIN.
- For reviews, use recent + critical first. Use all-star mode only when the user wants deeper review mining and accepts higher cost/time.
- For batches larger than 20 ASINs or heavy review jobs, use async mode and track failed ASINs separately.

## Competitor Insight Rules

From competitor listings, extract:

- Common feature keywords appearing across at least 3 of top 5 competitors.
- Differentiated feature keywords appearing in only 1-2 competitors.
- First sentence or lead phrase of each bullet.
- Claims that appear often but are contradicted by negative reviews.
- Price and feature-count relationship.
- A+ and image message themes when available.

Use these insights to decide must-have copy, whitespace opportunities, and claims to avoid.

## Fallback

If Apify is unavailable, ask the user for:

- ASIN list or keyword list.
- Current title, bullets, description, and product facts.
- Keyword library.
- Competitor ASINs or competitor listing snippets.
- Review export or representative review samples.
