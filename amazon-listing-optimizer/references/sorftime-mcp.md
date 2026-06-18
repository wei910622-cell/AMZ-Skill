# Sorftime MCP Data Layer

Use this reference when Sorftime MCP is configured or when the user mentions `amazon-sorftime-research-MCP-skill`, `amazon-analyse`, Sorftime, or full-dimensional Amazon Listing penetration analysis.

## Source Pattern

The referenced open-source workflow uses Sorftime MCP for Amazon competitive research and exposes a simple command pattern:

```text
/amazon-analyse ASIN MARKETPLACE
```

Examples:

```text
/amazon-analyse B07PWTJ4H1 US
/amazon-analyse B08N5WRWNW DE
```

Sorftime MCP configuration pattern:

```json
{
  "mcpServers": {
    "sorftime": {
      "type": "streamableHttp",
      "url": "https://mcp.sorftime.com/?key=YOUR_API_KEY",
      "name": "Sorftime MCP"
    }
  }
}
```

## Data To Prefer From Sorftime

When available, use Sorftime-derived data before asking the user for uploads:

- Product detail: ASIN, marketplace, title, bullets, description, brand, category, price, BSR, ratings, images, variations.
- Competitor listing data: competing ASINs, title structure, bullet themes, pricing, review count, ranking, and keyword layout.
- Keyword analysis: traffic sources, competitor keyword layout, long-tail keywords, rank/traffic opportunity, trend signals.
- Review sentiment: advantage clusters, pain points, objection themes, buyer language, improvement opportunities.
- Market insight: seasonality, competition landscape, sales/ranking changes, opportunity gaps.

## How To Feed Listing Optimization

Transform the research output into this normalized input before writing copy:

| Field | Meaning |
| --- | --- |
| `asin` | Target ASIN |
| `marketplace` | US, DE, UK, FR, etc. |
| `mode` | single_asin, batch_asins, or parent_child_asins |
| `current_title` | Existing title from data source |
| `current_bullets` | Existing bullet points |
| `product_facts` | Verified material, size, color, pack count, compatibility, package contents |
| `keywords` | Keyword library with priority metrics when available |
| `competitors` | Competitor ASIN research |
| `review_pains` | Pain points and objections from review sentiment |
| `trend_notes` | Seasonality or demand trend notes |
| `compliance_risks` | Claims, missing evidence, category restrictions |

## Fallback Rule

If Sorftime MCP is not configured or returns incomplete data:

1. Use other available Amazon data tools.
2. Search local files for Sorftime exports, keyword CSVs, HTML reports, or analysis JSON.
3. Ask the user for only the missing pieces.
4. Label any result based on partial data as provisional.

## Report Use

Use Sorftime-style full-dimensional reports for upstream research and positioning. Use the main skill to convert that research into publishable listing fields, keyword allocation, variation keyword distribution, and Excel/HTML deliverables.
