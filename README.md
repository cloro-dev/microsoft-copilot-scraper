# Microsoft Copilot Scraper

[![Microsoft Copilot scraper by cloro](https://github.com/cloro-dev/microsoft-copilot-scraper/blob/main/copilot-scraper-hero-image.png)](https://cloro.dev/copilot/?utm_source=github)

[![cloro](https://img.shields.io/badge/Powered%20by-cloro-blue?style=for-the-badge)](https://cloro.dev/)

The [Microsoft Copilot Scraper](https://cloro.dev/copilot/) by cloro lets developers programmatically interact with Microsoft Copilot and collect AI-powered responses along with structured metadata. Instead of manual data collection, you can retrieve results as parsed JSON, raw HTML, or other formats for integration into your workflows.

You can use cloro's Copilot Scraper for development tools integration, Microsoft ecosystem research, and enterprise-focused queries. It handles dynamic AI-generated content, supports real-time extraction, and removes the need to manage authentication, sessions, or anti-bot systems.

## How it works

The Copilot scraper handles the rendering, parsing, and delivery of results in your requested format. You provide your query, API credentials, and optional parameters as shown below.

### Request sample (Python)

```python
import json
import requests

# API parameters
payload = {
    'prompt': 'How can I improve team productivity using Microsoft 365 tools?',
    'country': 'US',
    'include': {
        'markdown': True,
        'rawResponse': True
    }
}

# Get a response
response = requests.post(
    'https://api.cloro.dev/v1/monitor/copilot',
    headers={'Authorization': 'Bearer YOUR_API_KEY'},
    json=payload
)

# Print response to stdout
print(response.json())

# Save response to a JSON file
with open('response.json', 'w') as file:
    json.dump(response.json(), file, indent=2)
```

### Request sample (cURL)

```bash
curl -X POST https://api.cloro.dev/v1/monitor/copilot \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "prompt": "How can I improve team productivity using Microsoft 365 tools?",
    "country": "US",
    "include": {
      "markdown": true,
      "rawResponse": true
    }
  }'
```

### Request sample (Node.js)

```javascript
const axios = require("axios");

const payload = {
  prompt: "How can I improve team productivity using Microsoft 365 tools?",
  country: "US",
  include: {
    markdown: true,
    rawResponse: true,
  },
};

axios
  .post("https://api.cloro.dev/v1/monitor/copilot", payload, {
    headers: {
      Authorization: "Bearer YOUR_API_KEY",
      "Content-Type": "application/json",
    },
  })
  .then((response) => {
    console.log(response.data);
  })
  .catch((error) => {
    console.error("Error:", error);
  });
```

### Request parameters

| Parameter             | Description                                                                 | Default value |
| --------------------- | --------------------------------------------------------------------------- | ------------- |
| `prompt`\*            | The question or prompt to ask Copilot (1-10,000 characters)                 | –             |
| `country`             | Optional country/region code for localized results (e.g., `US`, `GB`, `DE`) | `US`          |
| `state`               | Optional US state code for state-level geo-targeting (e.g., `CA`, `TX`, `NY`). Only valid when `country` is `US`. See [supported codes](https://api.cloro.dev/v1/states?country=US). | –             |
| `include.markdown`    | Include response in Markdown format when set to true                        | `false`       |
| `include.html`        | Include URL to full HTML response when set to true (URL expires after 24h)  | `false`       |
| `include.rawResponse` | Include raw streaming response events for debugging                         | `false`       |

\* Mandatory parameters

---

### Output samples

The Copilot Scraper API returns a structured JSON object containing Copilot's AI-generated response and metadata.

**Structured JSON output snippet:**

```json
{
  "success": true,
  "result": {
    "text": "To improve team productivity using Microsoft 365 tools, I recommend implementing the following strategies: utilize SharePoint for document collaboration, leverage Teams for communication, use Power Automate for workflow automation...",
    "sources": [
      {
        "position": 1,
        "url": "https://docs.microsoft.com/en-us/microsoft-365/",
        "label": "Microsoft 365 Documentation",
        "description": "Official documentation for Microsoft 365 productivity tools and features..."
      },
      {
        "position": 2,
        "url": "https://example.com/productivity-tips",
        "label": "Productivity Best Practices",
        "description": "Industry best practices for maximizing team productivity with Microsoft tools..."
      }
    ],
    "shoppingCards": [
      {
        "type": "shoppingProducts",
        "layout": "Carousel",
        "products": [
          {
            "product": {
              "id": "prod_456",
              "groupId": "group_456"
            },
            "offerId": "offer_456",
            "url": "https://example.com/surface-pro-9",
            "name": "Microsoft Surface Pro 9",
            "description": "Powerful 2-in-1 tablet for professional productivity",
            "images": [
              {
                "title": "Surface Pro 9 front view",
                "url": "https://example.com/surface.jpg"
              }
            ],
            "specifications": [
              {
                "displayName": "Color",
                "values": ["Platinum", "Graphite", "Sapphire"]
              },
              {
                "displayName": "Storage",
                "values": ["256GB", "512GB", "1TB"]
              }
            ],
            "tags": ["tablet", "2-in-1", "professional"],
            "price": {
              "amount": 999.0,
              "currency": "USD",
              "currencySymbol": "$"
            },
            "seller": "Microsoft Store",
            "sellerLogoUrl": "https://example.com/microsoft-logo.png",
            "brandName": "Microsoft",
            "rating": {
              "value": 4.7,
              "count": 542
            },
            "canTrackPrice": true
          }
        ]
      }
    ],
    "html": "https://storage.cloro.dev/results/c45a5081-808d-4ed3-9c86-e4baf16c8ab8/page-1.html", // URL expires after 24 hours
    "markdown": "**To improve team productivity using Microsoft 365 tools**, I recommend implementing the following strategies...",
    "rawResponse": [
      {
        "type": "text",
        "text": "To improve team productivity using Microsoft 365 tools..."
      },
      {
        "type": "source",
        "data": {
          "url": "https://docs.microsoft.com/en-us/microsoft-365/",
          "title": "Microsoft 365 Documentation"
        }
      }
      // ... additional WebSocket events
    ]
  }
}
```

## Microsoft ecosystem integration

Copilot integrates with the Microsoft ecosystem, with knowledge about Microsoft products, services, and best practices.

### Microsoft expertise features

- **Microsoft 365 knowledge**: Coverage of Office apps, Teams, SharePoint, and more
- **Development tools**: Visual Studio, Azure, and Microsoft development platforms
- **Enterprise solutions**: Enterprise-grade Microsoft services and deployment strategies
- **Best practices**: Common approaches for Microsoft tool implementation and optimization

### Sources array structure

Each source in the `result.sources` array contains:

| Field         | Type    | Description                                   |
| ------------- | ------- | --------------------------------------------- |
| `position`    | integer | Position order of the source in the response  |
| `url`         | string  | Direct URL to the source content              |
| `label`       | string  | Source name or publication                    |
| `description` | string  | Brief description of what the source contains |

### Citation pills array structure

When the Copilot answer carries pill chips, the `result.citationPills` array exposes each cited source as a self-contained entry. When a pill cites N sources, the array contains N entries sharing the same `citationPillId` but with per-source `label`, `url`, and `domain`. Group by `citationPillId` to recover pill-level structure. The field is omitted when no pills are present.

| Field            | Type    | Description                                                                                                                                                                                                  |
| ---------------- | ------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `label`          | string  | Per-source title from the citation event (e.g. `"Microsoft 365 Documentation"`). Always present; may be an empty string when the event ships no title — read `domain` / `url` for source identity in that case. |
| `citationPillId` | integer | 1-based ordinal shared by all entries from the same chip.                                                         |
| `url`            | string  | Direct URL of the cited source.                                                                                   |
| `domain`         | string  | Host extracted from `url`, for grouping and display.                                                              |
| `description`    | string  | Source snippet when present. Omitted when absent.                                                                 |
| `position`       | integer | 1-based position of this source in the sibling `result.sources` array.                                            |

## Shopping cards

cloro's Copilot scraper extracts shopping cards automatically when Copilot returns product or commercial information. No additional parameters are required. Shopping cards are included by default when available in the response.

### Shopping card features

- **Product data**: Each shopping card includes product information with identifiers, URLs, names, descriptions, and specifications
- **Product images**: Multiple images with titles for each product
- **Specifications**: Product specifications like Color, Size, and other attributes with available values
- **Pricing information**: Structured price data with amount, currency code, and currency symbol
- **Ratings**: Product ratings with review counts
- **Seller information**: Seller name and logo for brand recognition
- **Product variants**: Track different configurations through specifications
- **Price tracking**: Know which products support price tracking

### Use cases for shopping cards

- **E-commerce monitoring**: Track how Copilot surfaces and recommends Microsoft products and services
- **Product catalog building**: Extract structured product data for building product databases
- **Competitive analysis**: Monitor Microsoft hardware pricing and specifications across different regions
- **Inventory tracking**: Monitor product availability and specifications for Microsoft ecosystem products
- **Marketing intelligence**: Understand which products Copilot highlights in responses

## Business and location data (Map entries)

When Copilot returns business or place information (restaurants, stores, attractions, etc.), cloro automatically extracts business data from Copilot's local entity data (primarily Google). No additional parameters are required. Map entries are included by default when available in the response.

**Example response with map entries:**

```json
{
  "success": true,
  "result": {
    "text": "Yes — the Smoky Mountains have several family-friendly theme parks...",
    "map": [
      {
        "name": "WonderWorks Pigeon Forge",
        "position": 1,
        "placeId": "ChIJEYpTsk__W4gR2sQLGzOjE3o",
        "location": {
          "address": "100 Music Rd, Pigeon Forge, TN 37863",
          "latitude": 35.823257,
          "longitude": -83.5787498
        },
        "phoneNumber": "(865) 868-1800",
        "url": "http://www.wonderworksonline.com/pigeon-forge/",
        "reviews": [
          {
            "count": 10625,
            "rating": 4.3,
            "providerName": "Google",
            "providerIconUrl": null,
            "url": "https://maps.google.com/?cid=8796553937077126362"
          }
        ],
        "photos": [
          {
            "url": "https://lh3.googleusercontent.com/gps-cs-s/example",
            "altText": null,
            "providerName": null,
            "providerUrl": "https://maps.google.com"
          }
        ],
        "openState": "Open · Closes 9 PM",
        "category": "Amusement park",
        "price": null,
        "layerLabel": "Theme Parks"
      },
      {
        "name": "Anakeesta",
        "position": 2,
        "placeId": "ChIJ0_BT8DxWWYgR4JxBzZpLjH4",
        "location": {
          "address": "576 Parkway, Gatlinburg, TN 37738",
          "latitude": 35.713083,
          "longitude": -83.5117751
        },
        "phoneNumber": "(865) 325-2400",
        "url": "https://www.anakeesta.com/",
        "reviews": [
          {
            "count": 15738,
            "rating": 4.2,
            "providerName": "Google",
            "providerIconUrl": null,
            "url": "https://maps.google.com/?cid=9118746473759087840"
          }
        ],
        "photos": [
          {
            "url": "https://lh3.googleusercontent.com/gps-cs-s/example",
            "altText": null,
            "providerName": null,
            "providerUrl": "https://maps.google.com"
          }
        ],
        "openState": "Open · Closes 8 PM",
        "category": "Theme park",
        "price": null,
        "layerLabel": "Theme Parks"
      }
    ]
  }
}
```

### Map entry features

- **Nested location data:** Each map entry includes a `location` object with `address`, `latitude`, and `longitude`, plus a Google Place ID (`placeId`)
- **Multi-provider reviews:** Review aggregations in a `reviews` array with `count`, `rating`, `providerName`, and review page `url`
- **Business photos:** A `photos` array with `url`, `altText`, `providerName`, and `providerUrl` for each photo
- **Live status:** `openState` field shows current open/closed status (e.g. "Open · Closes 9 PM")
- **Categorization:** `category` and `layerLabel` fields for business type and map layer grouping

### Use cases for map entries

- **Local business monitoring:** Track how Copilot surfaces and recommends businesses in different regions
- **Location intelligence:** Extract structured location data with coordinates for mapping and geospatial analysis
- **Competitive analysis:** Monitor which businesses Copilot highlights for location-based queries
- **Directory building:** Build business directories from Copilot's Google-sourced location data

## Practical Copilot scraper use cases

1. **Microsoft 365 optimization:** Get expert advice on configuring and optimizing Microsoft 365 for team productivity.
2. **Development guidance:** Receive technical guidance for Microsoft development tools and platforms.
3. **Enterprise integration:** Learn best practices for integrating Microsoft solutions in enterprise environments.
4. **Workflow automation:** Discover ways to automate business processes using Microsoft Power Platform.
5. **Technical troubleshooting:** Get help with Microsoft product issues and optimization strategies.
6. **Training and documentation:** Generate training materials and documentation for Microsoft tools adoption.

## Why choose cloro?

- **Simple integration:** API design with documentation and examples.
- **Reliable performance:** >99% uptime and low latencies (P50 < 30s, P90 < 60s)
- **No infrastructure hassle:** We handle rate limiting and browser management.
- **Microsoft expertise:** Access to Copilot's knowledge of the Microsoft ecosystem.
- **Developer support:** Responsive support team to help with integration and troubleshooting.

## FAQ

### Is scraping Copilot allowed?

Any website is legal to be scraped as long as the information is publicly accessible.

### What makes cloro's Copilot scraper unique?

cloro's Copilot endpoint provides reliable access to Microsoft Copilot's AI assistance with:

- **Microsoft ecosystem expertise** for development tools and enterprise solutions
- **Structured data extraction** for integration into your workflows
- **Real-time assistance** for Microsoft product questions and optimization

### What's the recommended timeout for requests?

We don't recommend putting any timeout, given that our system retries automatically. We recommend setting up a retry mechanism in case of failure.

### Does the API support different countries?

Yes, you can specify country codes like `US`, `GB`, `DE`, `JP`, `CN`, `IN`, `BR` and more to get localized results relevant to specific regions.

### What kind of questions work best with Copilot?

Copilot works well for questions related to Microsoft products, development tools, enterprise solutions, technical guidance, and workflow optimization within the Microsoft ecosystem.

## Learn more

For detailed documentation, advanced features, and integration guides, visit:

- **API documentation:** [cloro.dev/docs](https://cloro.dev/docs/)
- **Copilot scraper page:** [cloro.dev/copilot](https://cloro.dev/copilot/)

## Other available scrapers

- **[AI Mode](https://cloro.dev/ai-mode/)** - Extracts structured data from Google AI Mode for general knowledge queries, workflow optimization, and technical guidance.
- **[AI Overview](https://cloro.dev/ai-overview/)** - Extracts structured data from Google AI Overview for search result analysis and AI-curated insights.
- **[ChatGPT](https://cloro.dev/chatgpt/)** - Extracts structured data from ChatGPT with features including shopping cards, raw response data, and query fan-out.
- **[Copilot](https://cloro.dev/copilot/)** - Extracts structured data from Microsoft Copilot with sources, shopping cards, and map entries.
- **[Gemini](https://cloro.dev/gemini/)** - Extracts structured data from Google Gemini for complex reasoning, content generation, and source confidence scoring.
- **[Google Search](https://cloro.dev/google-search/)** - Extracts structured data from Google Search results, including organic results, People Also Ask questions, related searches, and optional AI Overview data.
- **[Google News](https://cloro.dev/google-news/)** - Extracts structured news articles from Google News with titles, snippets, sources, dates, and thumbnail images for news monitoring and media tracking.
- **[Grok](https://cloro.dev/grok/)** - Extracts structured data from Grok for current events, news tracking, and real-time information gathering.
- **[Perplexity](https://cloro.dev/perplexity/)** - Extracts structured data from Perplexity AI with real-time web sources, automatically detecting and extracting data objects.

## Contact us

If you have questions or need support, reach out to us at [support@cloro.dev](mailto:support@cloro.dev).

---

Built with ❤️ by the cloro team
