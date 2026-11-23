# Google Ads Transparency API — n8n Workflow & MCP Server

Competitor ad-creative monitoring for AI agents, n8n automations and ad-intelligence pipelines. Resolve a brand, domain or advertiser ID against Google's public Ads Transparency Center and return every creative with format, first/last seen dates, landing domain and preview image.

- **Apify Actor:** [Google Ads Transparency API](https://apify.com/structlabs/google-ads-transparency-api)
- **MCP server:** `https://mcp.apify.com/?actors=structlabs/google-ads-transparency-api`
- **n8n workflow:** [`n8n/google-ads-transparency-api-workflow.json`](n8n/google-ads-transparency-api-workflow.json)
- **Category:** Ad creative intelligence
- **Pricing (Gold):** $1.19 per 1,000 creatives + $0.05 per 1,000 starts. [Full pricing](https://apify.com/structlabs/google-ads-transparency-api)

## What it returns

One row per creative with `query`, `advertiser_id`, `advertiser_name`, `advertiser_total_ads`, `creative_id`, `format`, `first_shown_at`, `last_shown_at`, `landing_domain`, `preview_image_url`, `ad_url`, `region` and `scraped_at`.

## Quick start with n8n

1. Download [`n8n/google-ads-transparency-api-workflow.json`](n8n/google-ads-transparency-api-workflow.json).
2. In n8n: **Workflows → Import from File**.
3. Add `APIFY_TOKEN` to the n8n environment (Settings → Environment Variables).
4. Open the workflow, change the query or schedule, and run once.
5. The HTTP Request node calls `run-sync-get-dataset-items` and returns the creative rows directly.

## Quick start with Python

```bash
export APIFY_TOKEN=<your-token>
python examples/quickstart.py
```

## Quick start with Node.js

```bash
export APIFY_TOKEN=<your-token>
node examples/quickstart.mjs
```

## Input

```json
{
  "queries": ["nike.com"],
  "region": "US",
  "maxAdsPerQuery": 40,
  "pageSize": 40
}
```

`queries` accepts domains (`nike.com`), brand names (`nike`) or `AR…` advertiser IDs. `region` accepts ISO-2 codes; an empty string searches all regions. `formats` accepts `image`, `text`, `video`; `startDate` and `endDate` accept `YYYY-MM-DD`.

## Sample output

Three real sanitized rows from a succeeded cloud run are in [`examples/sample_output.json`](examples/sample_output.json). One row:

```json
{
  "query": "nike.com",
  "advertiser_id": "AR16735076323512287233",
  "advertiser_name": "Nike, Inc.",
  "advertiser_total_ads": 2,
  "creative_id": "CR18266859753747513345",
  "format": "image",
  "first_shown_at": "2026-11-22T08:00:59+00:00",
  "last_shown_at": "2026-11-23T10:08:09+00:00",
  "landing_domain": "nike.com",
  "preview_image_url": "https://tpc.googlesyndication.com/archive/simgad/10022120879025303363",
  "ad_url": "https://adstransparency.google.com/advertiser/AR16735076323512287233/creative/CR18266859753747513345",
  "region": "US",
  "scraped_at": "2026-11-23T10:54:39.762355+00:00"
}
```

The endpoint exposes creative metadata, not ad copy. Video creatives may have no static preview image; the row still carries the creative ID and ad URL. No personal data is collected.

## Use cases

- Monitor a competitor's live creative rotation by market.
- Track how long each ad has been running and when it was last seen.
- Build a creative library for an LLM or a design team.
- Feed new creatives into Slack, Sheets, Airtable or a review queue.

## MCP configuration

Copy [`mcp/mcp_config.json`](mcp/mcp_config.json) into your MCP client (Claude Desktop, Cursor, Windsurf):

```json
{
  "mcpServers": {
    "apify-google-ads-transparency-api": {
      "url": "https://mcp.apify.com/?actors=structlabs/google-ads-transparency-api"
    }
  }
}
```

## Links

- Apify Store: https://apify.com/structlabs/google-ads-transparency-api
- n8n: https://n8n.io
- MCP: https://modelcontextprotocol.io

## License

MIT
