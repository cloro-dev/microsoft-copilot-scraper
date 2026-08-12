# Microsoft Copilot Scraper

[![Microsoft Copilot Scraper by cloro](https://github.com/cloro-dev/microsoft-copilot-scraper/blob/main/copilot-scraper-hero-image.png)](https://cloro.dev/copilot/?utm_source=github)

[![cloro](https://img.shields.io/badge/Powered%20by-cloro-blue?style=for-the-badge)](https://cloro.dev/)

The [Microsoft Copilot scraper](https://cloro.dev/copilot/?utm_source=github) by cloro returns Copilot's Bing-grounded answers as structured JSON: answer text and markdown, every cited source, shopping cards and business entries.

## How do you scrape Microsoft Copilot?

1. Get an API key at [cloro.dev](https://cloro.dev/?utm_source=github&utm_medium=readme).
2. POST a prompt to `https://api.cloro.dev/v1/monitor/copilot`.
3. Read the parsed fields from the JSON response.

Copilot is the surface most often skipped by visibility tools, and it is grounded in Bing rather than Google, so its source list frequently disagrees with every other engine on the same question. That disagreement is the reason to track it separately rather than pooling it into an average.

### Request sample (Python)

```python
import requests

payload = {
    'prompt': 'best CRM for B2B sales teams',
    'country': 'US',
    'include': {'markdown': True},
}

response = requests.post(
    'https://api.cloro.dev/v1/monitor/copilot',
    headers={'Authorization': 'Bearer YOUR_API_KEY'},
    json=payload,
)

print(response.json())
```

### Request sample (cURL)

```bash
curl -X POST https://api.cloro.dev/v1/monitor/copilot \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"prompt": "best CRM for B2B sales teams", "country": "US"}'
```

Node.js and async/webhook examples are in the [endpoint documentation](https://cloro.dev/docs/api-reference/endpoint/monitor-copilot).

### Request parameters

| Parameter | Description | Default |
| --- | --- | --- |
| `prompt`\* | The query or question (1-10,000 characters) | – |
| `country` | Country code for localized results (`US`, `GB`, `DE`) | `US` |
| `state` | US state code for finer localization | – |
| `include.markdown` | Return the answer as Markdown | `false` |
| `include.html` | Return a URL to the full HTML (expires after 24h) | `false` |
| `include.rawResponse` | Return the unparsed upstream payload | `false` |

\* Required

## What data does the Microsoft Copilot scraper return?

```json
{
  "success": true,
  "result": {
    "text": "For B2B sales teams, the most commonly recommended options are...",
    "sources": [
      { "position": 1, "url": "https://example.com/crm-guide", "label": "Example CRM Guide", "description": "Comparison of B2B CRM platforms..." }
    ],
    "shoppingCards": [{ "title": "CRM Starter Plan", "price": { "value": 29.0, "currency": "$" }, "store": "Example" }],
    "markdown": "For B2B sales teams, the most commonly recommended options are..."
  }
}
```

Alongside `text` and `markdown`:

1. **`sources`** — every cited URL with position, label and description, grounded in Bing's index.
2. **`shoppingCards`** — product cards with price, store, rating and reviews.
3. **`map`** — business and location entries with rating, reviews, address and operating status.
4. **`citationPills`** — inline citation chips where present.
5. **`rawResponse`** — the unparsed upstream payload.

Full field-level schemas are in the [endpoint reference](https://cloro.dev/docs/api-reference/endpoint/monitor-copilot).

## Use cases

- **Cross-engine coverage** — the engine most visibility dashboards under-cover, so the cheapest place to find a gap.
- **Enterprise and Microsoft-ecosystem queries** — Copilot skews toward these, and answers there differ from consumer surfaces.
- **Brand monitoring** — whether Copilot names you on the prompts your buyers ask.
- **Bing-index research** — Copilot's grounding makes it a readable proxy for what Bing surfaces.

## FAQ

### Why track Copilot separately from ChatGPT?

Different grounding. Copilot is Bing-backed, so on the same prompt it often cites a different set of domains. Pooling engines into one average hides being invisible on the one that matters to your category.

### Does cloro support Bing search directly?

No. cloro covers Google Search, Google News, and the AI surfaces including Copilot. Bing's own search APIs were retired in August 2025.

### Is scraping Copilot allowed?

cloro reads publicly visible responses. Check your own jurisdiction and terms.

### What is the recommended timeout?

60 seconds. Copilot streams its answer like the other AI surfaces.

## Learn more

- **Endpoint reference:** [cloro.dev/docs](https://cloro.dev/docs/api-reference/endpoint/monitor-copilot)
- **Product page:** [cloro.dev/copilot](https://cloro.dev/copilot/)

## Other cloro scrapers

[AI Mode](https://cloro.dev/ai-mode/) · [AI Overview](https://cloro.dev/ai-overview/) · [ChatGPT](https://cloro.dev/chatgpt/) · [Gemini](https://cloro.dev/gemini/) · [Google Search](https://cloro.dev/google-search/) · [Google News](https://cloro.dev/google-news/) · [Grok](https://cloro.dev/grok/) · [Perplexity](https://cloro.dev/perplexity/)

## Contact us

Questions or support: [r/cloroapi](https://www.reddit.com/r/cloroapi/).
