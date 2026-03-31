# Willhaben Scraper

Extract structured job listings from [willhaben.at](https://willhaben.at) — Austria's largest job portal. Salary data, employer contact info, and 7 search filters.

**[Willhaben Scraper on Apify →](https://apify.com/blackfalcondata/willhaben-scraper)**

---

## Key features

**7 search filters** — Filter by region (Bundesland), professional field, employment type, position level, company type, and posting recency. Combine with keyword search.

**Detail enrichment** — Fetch full job descriptions, contact name and email, language skill requirements, remote work flag, and company profile per listing.

**Incremental mode** — Only get new or changed jobs since your last run. Content hash per listing — no duplicates, no re-processing.

---

## Use cases

**Job market monitoring**
Track job postings in Austria by region and sector. Export structured data to CSV, JSON, or your own database for analysis.

**Recruitment pipeline**
Collect fresh postings daily with contact info enriched. Use incremental mode to only process new arrivals.

---

## Quick start

```json
{
  "query": "Software Engineer",
  "location": "Wien",
  "maxResults": 25,
  "includeDetails": true
}
```

---

## Input parameters

| Parameter | Type | Default | Description |
|---|---|---|---|
| `query` | string | — | Job search keywords. Leave empty to browse all jobs. |
| `location` | string | — | City or district name (e.g. "Wien"). |
| `region` | enum | — | Austrian federal state (Wien, Steiermark, Tirol…). |
| `operationArea` | enum | — | Professional field (IT/EDV, Marketing, Handwerk…). |
| `employmentMode` | enum | — | Employment type (Vollzeit, Teilzeit, Freiberuflich, Geringfügig). |
| `position` | enum | — | Position level (Mitarbeiter:in, Leitung, Praktikum…). |
| `companyType` | enum | — | Staffing agency or direct employer. |
| `timeLimit` | enum | — | Posted within last 24h, 3 days, or week. |
| `maxResults` | integer | `25` | Maximum results (0 = unlimited). |
| `includeDetails` | boolean | `true` | Fetch full job details: contact info, description, language skills, remote. |
| `descriptionMaxLength` | integer | `0` | Truncate description to N chars. 0 = no truncation. |
| `compact` | boolean | `false` | Core fields only — for AI-agent and MCP workflows. |
| `incrementalMode` | boolean | `false` | Only emit new or changed jobs since last run. Requires stateKey. |
| `stateKey` | string | — | Stable key for tracked search (e.g. "software-wien"). Required for incremental mode. |

---

## FAQ

**Is it legal to scrape willhaben.at?**
Web scraping of publicly available data is generally legal. This actor only accesses publicly visible job listings. Always check the target site's terms of service for your specific use case.

**How does incremental mode work?**
Each job gets a content hash. On subsequent runs, only new or changed jobs are emitted — unchanged listings are silently skipped. No extra charge for change tracking.

**Does it return contact details?**
Yes, with `includeDetails: true`. Contact name and email are returned when the employer has made them available on the listing page.

**What is compact mode?**
Returns core fields only (title, company, salary, location, URL). Smaller payload for AI-agent and MCP workflows where full descriptions are not needed.

---

## Known limitations

- Contact email is only available when the employer has explicitly published it on the listing page — not all postings include it.
- Salary data is shown when the employer discloses it; many Austrian job postings omit salary.
- Pagination is capped at willhaben's maximum of 90 results per page; very large result sets are fetched in parallel pages.

---

## Related products by Black Falcon Data

- [Willhaben.at Scraper](https://github.com/BlackFalconData-org/willhaben-all-scraper) — All four sections: classifieds, real estate, cars and jobs in one actor
- [Arbeitsagentur Jobs Feed](https://github.com/BlackFalconData-org/arbeitsagentur-jobs-feed) — Germany's official employment portal
- [StepStone Scraper](https://github.com/BlackFalconData-org/stepstone-scraper) — Job listings from 18 European portals

---

*Last updated: 2026 03*
