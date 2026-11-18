# Yellow Pages Scraper [Just 💰$0.07]

Extract structured data from [yellowpages.com](https://yellowpages.com) — yellowpages.com US business listings by keyword and city or ZIP code. Every record has the business name and phone number plus the street address, website and email where the listing shows them. Star ratings and opening hours are added where published.

**[Yellow Pages Scraper [Just 💰$0.07] on Apify →](https://apify.com/blackfalcondata/yellowpages-com-scraper?fpr=1h3gvi)**

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

**Detail enrichment** — Fetch full descriptions, contact information for each record.

**Incremental mode** — Only get new or changed records since your last run. Content hash per record — no duplicates, no re-processing.

**Change classification** — Track unchanged, expired, cross-run duplicate detection across runs. Build audit trails of how records evolve over time.

**Compact output** — Emit core fields only (AI-agent / MCP-friendly). Keeps response size small for LLM workflows.

**Description truncation** — Cap description length per ${NOUN} to control output size and cost.

**Result cap** — Stop after N records (up to 1.000). Set to 0 for the full catalog.

**Export anywhere** — Download as JSON, CSV, or Excel. Stream via Apify API, webhooks, or integrations with Make, Zapier, Airbyte, Keboola.

**Structured data** — Every record returns the same schema with consistent field naming. All fields always present — `null` when unavailable, never omitted.

---

## Use cases

**Data pipeline automation**
Integrate with your ETL pipeline to collect structured records from yellowpages.com on a schedule. Export to CSV, JSON, or directly to your database. Use compact mode to control output size.

**Market research**
Monitor records, track trends, and analyze market dynamics with structured, deduplicated data from yellowpages.com.

**Change monitoring**
Run daily or hourly in incremental mode to capture only new, updated, or expired ${NOUNS}. Perfect for price-tracking, churn analysis, and alerting pipelines.

**Lead generation**
Extract contact details alongside ${NOUNS} to build targeted outreach lists for sales teams, partnerships, or B2B prospecting.

**AI / LLM training data**
Structured JSON per record is ready for RAG pipelines, embeddings, and agent workflows. Compact mode trims tokens for LLM context windows.

---

## Quick start

```json
{
  "query": "plumber",
  "location": "Los Angeles, CA",
  "maxResults": 100,
  "includeDetails": true,
  "descriptionMaxLength": 0,
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
| `query` | string | — | What to search for: a business category or name, e.g. "plumber", "dentist", "pizza". Use a JSON array for several searches in one run, e.g. ["plumber","electrician"]. Required if no startUrls provided. |
| `location` | string | — | Where to search: city + state, ZIP code, or state, e.g. "Los Angeles, CA" or "90001". Use a JSON array for several locations in one run. Leave empty for a nationwide search. |
| `maxResults` | integer | `100` | Maximum total businesses (0 = unlimited). |
| `startUrls` | array | `[]` | Paste yellowpages.com search URLs. Each URL becomes its own search task; results are merged and deduped by business ID across all URLs. When provided, startUrls REPLACE the search term / location fields (unsupported URLs are skipped, and if none is supported the run stops with an input error). |
| `includeDetails` | boolean | `true` | Fetch each business's full profile: description, services, payment methods, languages, social links, photos and extra emails. Slower, and much richer. |
| `descriptionMaxLength` | integer | `0` | Truncate description to N chars. 0 = no truncation. |
| `compact` | boolean | `false` | Core fields only — name, category, phone, address, website, rating, URL (for AI-agent/MCP workflows). |
| `excludeEmptyFields` | boolean | `false` | Drop null, empty-string, and empty-array fields from each record before push. Smaller payloads for AI agents and dashboards. |
| `incrementalMode` | boolean | `false` | Compare against previous run state and return only new or changed businesses. stateKey is optional — defaults to a value derived from your search inputs (search terms, locations, startUrls) so different searches never share state. |
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
| `lat` | number | — | Latitude of the search centre. Pair with lon. Used as the search location when Location is empty, and as the centre of the radius filter. |
| `lon` | number | — | GPS longitude. Pair with lat. |
| `radiusKm` | integer | — | Keep only businesses within this many kilometres of lat/lon. Businesses without coordinates are kept. Leave empty for no radius filter. |

---

## Output fields

Every record returns the same 64-field schema. Missing values are `null` — never omitted.

- `id`
- `ypid`
- `listingId`
- `ypListingId`
- `name`
- `category`
- `categories`
- `specialties`
- `phone`
- `phones`
- `phoneE164`
- `email`
- `emails`
- `website`
- `additionalWebsites`
- `socialLinks`
- `address`
- `street`
- `city`
- `province`
- `state`
- `postalCode`
- `country`
- `latitude`
- `longitude`
- `timezone`
- `distanceMiles`
- `rating`
- `reviewCount`
- `openingHours`
- `openNowText`
- `openNow`
- `hoursText`
- `description`
- `slogan`
- `services`
- `paymentMethods`
- `languages`
- `brands`
- `yearEstablished`
- `yearsInBusiness`
- `isClaimed`
- `sponsored`
- `isAd`
- `listingTier`
- `logo`
- `imageUrl`
- `photos`
- `chainId`
- `url`
- `portalUrl`
- `searchKeyword`
- `searchTerm`
- `searchLocation`
- `position`
- `totalInSearch`
- `detailsScraped`
- `searchQuery`
- `contentQuality`
- `detailFetched`
- `scrapedAt`
- `source`
- `contentHash`
- `changeType`

---

## Sample output

One object per record. Here is a real example from a production run:

```json
{
  "id": "9270807",
  "ypid": "9270807",
  "listingId": "54d2f627b0524adabf315846064eb47d2325afa989f2844f776f646c7a28372b",
  "ypListingId": "1001664366983",
  "name": "Ritz Plumbing Heating, Air & Electrical",
  "category": "Plumbers",
  "categories": [
    "Plumbers",
    "Air Conditioning Contractors & Systems",
    "Air Conditioning Service & Repair",
    "Building Contractors",
    "Building Contractors-Commercial & Industrial",
    "Electricians",
    "Furnace Repair & Cleaning",
    "Furnaces-Heating",
    "Heating Contractors & Specialties",
    "Heating Equipment & Systems",
    "Plumbing Contractors-Commercial & Industrial",
    "Plumbing-Drain & Sewer Cleaning",
    "Sewer Contractors",
    "Water Heaters"
  ],
  "specialties": [
    "$29.95 Drain Work, $49.95 Any Visual Leak Detection"
  ],
  "phone": "(323) 452-4915",
  "phones": [
    "(323) 452-4915"
  ],
  "phoneE164": "+13234524915",
  "email": "ritzplumbing@gmail.com"
}
```

*Truncated — full records contain 64 fields. See Output fields for the complete schema.*

**[Try Yellow Pages Scraper [Just 💰$0.07] now — $5 free credit, no credit card →](https://apify.com/blackfalcondata/yellowpages-com-scraper?fpr=1h3gvi)**

---

## Pricing

Pay only for what you extract. No subscription required — Apify's free $5 credit covers thousands of results.

| Event | Price (USD) |
| --- | --- |
| Actor Start | $0.000045 |
| Result | $0.000072 |

See the [actor on Apify](https://apify.com/blackfalcondata/yellowpages-com-scraper?fpr=1h3gvi) for current pricing.

---

## FAQ

**How do I scrape yellowpages.com?**
Use this actor on Apify to extract structured data from yellowpages.com. Configure your search query and filters in the input, then click Start — no coding required.

**How do I get yellowpages.com data as JSON, CSV, or Excel?**
The actor writes each record to Apify's dataset. Download as JSON, CSV, or Excel from the Console, stream via the API, or push to Make, Zapier, Airbyte, or Keboola.

**Is it legal to scrape yellowpages.com?**
Web scraping of publicly available data is generally legal. This actor only accesses publicly visible information. Always check yellowpages.com's terms of service for your specific use case.

**How much does it cost?**
Pay-per-event pricing — you only pay for ${NOUNS} extracted. Apify's free $5 credit is enough to run thousands of results before you pay anything.

**How does incremental mode work?**
Each record gets a content hash. On subsequent runs, only new or changed records are emitted — saving time, compute, and storage. Expired records can be tracked separately.

**Do I need an API key or credentials?**
No. Just sign up for Apify, paste your input, and click Start. No credit card required.

---

## Related products by Black Falcon Data

[Browse all Black Falcon Data actors →](https://apify.com/blackfalcondata?fpr=1h3gvi)

---

## Getting started with Apify

New to Apify? [Create a free account with $5 credit](https://console.apify.com/sign-up?fpr=1h3gvi) — no credit card required.

1. Sign up — $5 platform credit included
2. Open [Yellow Pages Scraper [Just 💰$0.07]](https://apify.com/blackfalcondata/yellowpages-com-scraper?fpr=1h3gvi) and configure your input
3. Click **Start** — export results as JSON, CSV, or Excel

Need more later? [See Apify pricing](https://apify.com/pricing?fpr=1h3gvi).

---

## About Black Falcon Data

Black Falcon Data builds production-grade web scrapers for job boards and marketplace data. Browse our full actor catalog at [www.blackfalcondata.com](https://www.blackfalcondata.com).

---

*Last updated: 2026 11*
