# Willhaben.at Scraper

Extract structured data from [willhaben.at](https://willhaben.at) — willhaben.at — Austria's largest classifieds platform. All four sections including jobs, paste any search URL with filters, and incremental change tracking.

**[Willhaben.at Scraper on Apify →](https://apify.com/blackfalcondata/willhaben-all-scraper)**

---

## Key features

**Search with filters** — Search by keyword and location. Filter by section, immobilien sub-type, and more.

**Detail enrichment** — Fetch full job descriptions, structured metadata for each listing.

**Incremental mode** — Only get new or changed listings since your last run. Content hash per listing — no duplicates, no re-processing.

---

## Use cases

**Data pipeline automation**
Integrate with your ETL pipeline to collect structured listings from willhaben.at on a schedule. Export to CSV, JSON, or directly to your database. Use compact mode to control output size.

**Market research**
Monitor listings, track trends, and analyze market dynamics with structured, deduplicated data from willhaben.at.

---

## Quick start

```json
{
  "query": "fahrrad",
  "maxResults": 50,
  "includeDetails": true
}
```

---

## Input parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `searchUrl` | string | — | Paste any willhaben.at search URL — section, sub-type, keyword and all filters are extracted automatically. Other fields below override individual values if needed. |
| `section` | enum | `"marktplatz"` | Which part of willhaben.at to scrape. |
| `immoSubType` | enum | `"mietwohnungen"` | Category within the Immobilien section. Only used when section = immobilien. |
| `query` | string | — | Search keyword (optional). Leave empty to get all listings in the section. |
| `priceMin` | integer | — | Minimum listing price in EUR. Not applicable for Jobs. |
| `priceMax` | integer | — | Maximum listing price in EUR. Not applicable for Jobs. |
| `jobLocation` | string | — | City or location filter for jobs (e.g. 'Wien'). Only used when section = jobs. |
| `jobRegion` | string | — | Federal state or region filter (e.g. 'wien'). Only used when section = jobs. |
| `jobOperationArea` | string | — | Job operation/industry area filter. Only used when section = jobs. |
| `jobEmploymentMode` | string | — | Employment type filter (e.g. 'vollzeit', 'teilzeit'). Only used when section = jobs. |
| `jobPosition` | string | — | Position/seniority filter. Only used when section = jobs. |
| `jobCompanyType` | string | — | Type of employer (e.g. 'private', 'public'). Only used when section = jobs. |
| `jobTimeLimit` | string | — | Contract duration filter (e.g. 'unbefristet'). Only used when section = jobs. |
| `maxResults` | integer | `50` | Maximum number of listings to return. 0 = unlimited. |
| `includeDetails` | boolean | `true` | Fetch the full listing detail page for each result. Adds description, contact info, and richer field data. Slower but more complete. |
| `descriptionMaxLength` | integer | `0` | Truncate description text to N characters. 0 = no truncation. |
| `compact` | boolean | `false` | Return core fields only (listingId, title, price, location, section-key fields). Smaller payload for AI-agent and MCP workflows. |
| `incrementalMode` | boolean | `false` | Only emit new or changed listings. Requires stateKey. Unchanged listings are skipped. Change tracking is free — no extra charge per run. |
| `stateKey` | string | — | Stable identifier for this tracked search (e.g. 'vw-golf-wien'). Required when incrementalMode is true. Different searches must use different keys. |

---

## FAQ

**Is it legal to scrape willhaben.at?**
Web scraping of publicly available data is generally legal. This actor only accesses publicly visible information. Always check the target site's terms of service for your specific use case.

**Which sections are supported?**
All four main sections: Marktplatz (classifieds), Immobilien (real estate), Autos (cars), and Jobs. Each section has its own set of output fields.

**Can I use a search URL directly?**
Yes — paste any willhaben.at search URL into the `searchUrl` field and all filters, keywords, and section type are extracted automatically. Manual overrides still apply.

**How does incremental mode work?**
Each listing gets a content hash based on title, price, description, and other key fields. On subsequent runs, only new or changed listings are emitted — unchanged ones are silently skipped. No extra charge for change tracking.

**How do I scrape jobs in a specific city?**
Set `section` to `jobs` and use `jobRegion` (e.g. `wien`) or `jobLocation` for city-level filtering. Combine with a keyword for more targeted results.

**What does detail enrichment add for jobs?**
With `includeDetails: true`, jobs gain full description text, contact name and email, remote work flag, language skill requirements, and apply URL.

---

## Known limitations

- Willhaben's attribute-bag API (Marktplatz, Immobilien, Autos) rate-limits at sustained high concurrency. Detail enrichment runs at a conservative 5 concurrent requests to avoid 429 errors.
- Jobs detail pages do not always include contact email — this depends on the employer's settings.
- Immobilien contact email is rarely exposed as a structured field; it is only extracted when present in the CONTACT/URL attribute.
- Real-time price changes may take a few minutes to reflect in search results due to willhaben's own caching.

---

## Related products by Black Falcon Data

- [StepStone Scraper](https://github.com/BlackFalconData-org/stepstone-scraper) — Job listings from 18 European portals
- [Indeed Job Scraper](https://github.com/BlackFalconData-org/indeed-job-scraper) — Indeed job listings with salary data
- [Glassdoor Job Scraper](https://github.com/BlackFalconData-org/glassdoor-job-scraper) — Glassdoor listings with company ratings

---

*Last updated: 2026 03*
