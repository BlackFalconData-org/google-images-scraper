# Google Images Scraper

Extract structured data from [images.google.com](https://images.google.com) — images.google.com — images. Every record carries image dimensions and source page the source publishes. Structured rows, ready to import into a CRM or a spreadsheet. The actor supports keyword search and controllable result limits.

**[Google Images Scraper on Apify →](https://apify.com/blackfalcondata/google-images-scraper?fpr=1h3gvi)**

---

## 🚀 How to use this actor

> ### 💚 $5 free Apify credits — every month
> No credit card required. No commitment. Cancel anytime.

### 👉 [Sign up free on Apify →](https://console.apify.com/sign-up?fpr=1h3gvi)

1. **Click sign up** — pick GitHub, Google, or email; takes ~30 seconds
2. **Open this actor** — input is pre-filled with a working example
3. **Click Start** — export results as JSON, CSV, or Excel

Your **$5 monthly platform credit** is enough to run this actor right away — and again every month — scraping typically several hundred to several thousand results per run, depending on your input.

## Key features

**Search with filters** — Search by keyword and location. Filter by 📝 description format, and more.

**Incremental mode** — Only get new or changed images since your last run. Content hash per image — no duplicates, no re-processing.

**Change classification** — Track unchanged, expired, cross-run duplicate detection across runs. Build audit trails of how images evolve over time.

**Compact output** — Emit core fields only (AI-agent / MCP-friendly). Keeps response size small for LLM workflows.

**Result cap** — Stop after N images (up to 100.000). Set to 0 for the full catalog.

**Export anywhere** — Download as JSON, CSV, or Excel. Stream via Apify API, webhooks, or integrations with Make, Zapier, Airbyte, Keboola.

**Structured data** — Every image returns the same schema with consistent field naming. All fields always present — `null` when unavailable, never omitted.

---

## Use cases

**Data pipeline automation**
Integrate with your ETL pipeline to collect structured images from images.google.com on a schedule. Export to CSV, JSON, or directly to your database. Use compact mode to control output size.

**Market research**
Monitor images, track trends, and analyze market dynamics with structured, deduplicated data from images.google.com.

**Change monitoring**
Run daily or hourly in incremental mode to capture only new, updated, or expired ${NOUNS}. Perfect for price-tracking, churn analysis, and alerting pipelines.

**AI / LLM training data**
Structured JSON per image is ready for RAG pipelines, embeddings, and agent workflows. Compact mode trims tokens for LLM context windows.

---

## Quick start

```json
{
  "query": [
    "golden retriever puppy"
  ],
  "maxResults": 100,
  "compact": false,
  "excludeEmptyFields": false,
  "incrementalMode": false,
  "skipReposts": false,
  "emitUnchanged": false,
  "emitExpired": false,
  "notifyOnlyChanges": false,
  "notificationLimit": 5,
  "descriptionFormat": "all"
}
```

---

## Input parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `query` | array | — | One image search per line. Every term runs as its own search, in parallel. Required if no Start URLs are provided. |
| `maxResults` | integer | `100` | Maximum results for the whole run, across all searches (0 = unlimited). |
| `maxResultsPerQuery` | integer | `0` | Maximum results for each search term (0 = as many as Google returns, usually about 300). You pay only for the images delivered. |
| `country` | string | `"us"` | Two-letter country code the search runs from, e.g. us, gb, de, dk. |
| `language` | string | `"en"` | Interface language code, e.g. en, de, da, pt-BR. Affects titles and the mix of results. |
| `startUrls` | array | `[]` | Paste Google Images search URLs. Each URL becomes its own search task; results are merged and deduped across all URLs. When provided, startUrls REPLACE the query field (unsupported URLs are skipped, and if none is supported the run stops with an input error). |
| `compact` | boolean | `false` | Core fields only (for AI-agent/MCP workflows). |
| `excludeEmptyFields` | boolean | `false` | Drop null, empty-string, and empty-array fields from each record before push. Smaller payloads for AI agents and dashboards. |
| `incrementalMode` | boolean | `false` | Compare against previous run state. stateKey is optional — defaults to a value derived from search inputs (queries, locations, startUrls, country) so different filter sets never share state. |
| `stateKey` | string | — | Optional. Stable identifier for the tracked search universe. Leave empty to auto-generate from search inputs. |
| `skipReposts` | boolean | `false` | When incremental, skip listings whose content matches an expired listing from a prior run (cross-run duplicate detection). |
| `emitUnchanged` | boolean | `false` | When incremental mode is on, also emit listings whose content has not changed since the last run. |
| `emitExpired` | boolean | `false` | When incremental mode is on, also emit listings that were seen before but are no longer found. |
| `notifyOnlyChanges` | boolean | `false` | When Incremental Mode is on, only send notifications for NEW and UPDATED listings. Has no effect outside incremental mode. |
| `notificationLimit` | integer | `5` | Maximum number of listings included in each notification message (1–20). |
| `telegramToken` | string | — | Telegram bot token (from @BotFather). Required for Telegram notifications. |
| `telegramChatId` | string | — | Telegram chat or channel ID (e.g. "-100123456789"). Required when telegramToken is set. |
| `discordWebhookUrl` | string | — | Discord incoming webhook URL. Server Settings → Integrations → Webhooks → New Webhook. |
| `slackWebhookUrl` | string | — | Slack incoming webhook URL. api.slack.com/messaging/webhooks. |
| `whatsappPhoneNumberId` | string | — | Your WhatsApp Business phone-number ID (numeric, from Meta dashboard). Required when whatsappAccessToken is set. |
| `whatsappAccessToken` | string | — | WhatsApp Cloud API permanent access token (System User token from Meta Business). Recipient must have messaged the business number within the last 24h (service-conversation window — free since Nov 2024). |
| `whatsappTo` | string | — | Recipient phone in E.164 format without + (e.g. "436641234567"). Recipient must have messaged your business number within last 24h. |
| `webhookUrl` | string | — | Receives a JSON POST with {metadata, items} after each run. Universal escape hatch for n8n / Make / Zapier / custom backends. |
| `webhookHeaders` | object | — | Optional JSON object of custom headers (e.g. {"Authorization":"Bearer ..."}). |
| `appConnector` | string | — | Optional. Pick a connected app under Settings → API & Integrations to receive your results (including any contact details). Best-effort across MCP connectors as Apify expands its catalog. |
| `mcpIssueTeam` | string | — | Only when the connected app is an issue tracker: the team (name or ID) the summary issue is created under, if that app requires one. |
| `descriptionFormat` | enum | `"all"` | Choose which representation of the listing description to include. `all` keeps every variant; the others keep only the selected one. |

---

## Output fields

Every image returns the same 26-field schema. Missing values are `null` — never omitted.

- `position`
- `query`
- `title`
- `sourcePageUrl`
- `portalUrl`
- `imageUrl`
- `googleUrl`
- `imageWidth`
- `imageHeight`
- `thumbnailUrl`
- `thumbnailWidth`
- `thumbnailHeight`
- `displayedDomain`
- `searchUrl`
- `scrapedAt`
- `imageId`
- `sourceName`
- `fileSize`
- `dominantColor`
- `listingId`
- `searchQuery`
- `contentQuality`
- `detailFetched`
- `source`
- `contentHash`
- `changeType`

---

## Sample output

One object per image. Here is a real example from a production run:

```json
{
  "position": 1,
  "query": "golden retriever puppy",
  "title": "Golden Retriever: Pros, Cons & Diet Guide | Ollie",
  "sourcePageUrl": "https://blog.ollie.com/breed-deets-golden-retriever/",
  "portalUrl": "https://blog.ollie.com/breed-deets-golden-retriever/",
  "imageUrl": "https://blog.ollie.com/wp-content/uploads/250506_Livingroom_SHOT-10_0173_FR-scaled.jpg",
  "googleUrl": "https://www.google.com/imgres?imgurl=https%3A%2F%2Fblog.ollie.com%2Fwp-content%2Fuploads%2F250506_Livingroom_SHOT-10_0173_FR-scaled.jpg&imgrefurl=https%3A%2F%2Fblog.ollie.com%2Fbre…",
  "imageWidth": 2560,
  "imageHeight": 1707,
  "thumbnailUrl": "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSe1mLcW_M8xRI0J5UcnG6soPw9kd8XQOgz7Y4sOLFZoA&s=10",
  "thumbnailWidth": 678,
  "thumbnailHeight": 452
}
```

*Truncated — full records contain 26 fields. See Output fields for the complete schema.*

**[Try Google Images Scraper now — $5 free credit, no credit card →](https://apify.com/blackfalcondata/google-images-scraper?fpr=1h3gvi)**

---

## Pricing

Pay only for what you extract. No subscription required — Apify's free $5 credit covers thousands of results.

| Event | Price (USD) |
| --- | --- |
| Actor Start | $0.00005 |
| Image | $0.000099 |

See the [actor on Apify](https://apify.com/blackfalcondata/google-images-scraper?fpr=1h3gvi) for current pricing.

---

## FAQ

**How do I scrape images.google.com?**
Use this actor on Apify to extract structured data from images.google.com. Configure your search query and filters in the input, then click Start — no coding required.

**How do I get images.google.com data as JSON, CSV, or Excel?**
The actor writes each image to Apify's dataset. Download as JSON, CSV, or Excel from the Console, stream via the API, or push to Make, Zapier, Airbyte, or Keboola.

**Is it legal to scrape images.google.com?**
Web scraping of publicly available data is generally legal. This actor only accesses publicly visible information. Always check images.google.com's terms of service for your specific use case.

**How much does it cost?**
Pay-per-event pricing — you only pay for ${NOUNS} extracted. Apify's free $5 credit is enough to run thousands of results before you pay anything.

**How does incremental mode work?**
Each image gets a content hash. On subsequent runs, only new or changed images are emitted — saving time, compute, and storage. Expired images can be tracked separately.

**Do I need an API key or credentials?**
No. Just sign up for Apify, paste your input, and click Start. No credit card required.

---

## Related products by Black Falcon Data

[Browse all Black Falcon Data actors →](https://apify.com/blackfalcondata?fpr=1h3gvi)

---

## Getting started with Apify

New to Apify? [Create a free account with $5 credit](https://console.apify.com/sign-up?fpr=1h3gvi) — no credit card required.

1. Sign up — $5 platform credit included
2. Open [Google Images Scraper](https://apify.com/blackfalcondata/google-images-scraper?fpr=1h3gvi) and configure your input
3. Click **Start** — export results as JSON, CSV, or Excel

Need more later? [See Apify pricing](https://apify.com/pricing?fpr=1h3gvi).

---

## About Black Falcon Data

Black Falcon Data builds production-grade web scrapers for job boards and marketplace data. Browse our full actor catalog at [www.blackfalcondata.com](https://www.blackfalcondata.com).

---

*Last updated: 2026 11*
