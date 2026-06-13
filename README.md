[Dice Scraper](https://apify.com/solidcode/dice-scraper?fpr=data)

# Dice Tech Jobs Scraper

Extract US tech job listings from Dice.com at scale. Get titles, companies, locations, salaries, full descriptions, remote and visa-sponsorship flags, easy-apply support, and direct apply links — for any keyword, location, or pasted Dice search URL.

## Why This Scraper?

- **Up to 500 results per search** — Dice's full server-side ceiling, with automatic pagination so you never hit the page-25 wall by accident
- **Full job descriptions out of the box** — every record includes the complete posting text (HTML and plain-text), not just the search snippet
- **Structured filters as first-class inputs** — keyword, location, radius, posted-within window, employment type, remote-only, easy-apply, and willing-to-sponsor are all proper input fields, no need to hand-craft Dice URLs
- **Batch keyword search** — run multiple job titles in a single execution and de-duplicate across queries automatically
- **Direct URL passthrough** — already have a Dice search dialed in on the website? Paste the URL and the scraper honors every filter you set there
- **Parsed salary fields** — alongside the raw `salary` string, you get `salaryMin`, `salaryMax`, `salaryCurrency`, and `salaryPeriod` parsed for you when the format is clean
- **Visa-sponsorship and easy-apply flags** — perfect for international candidates, recruiters, or job-aggregator sites that need to surface only sponsor-friendly or one-click apply roles
- **Pay only for what you get** — transparent per-result pricing, no monthly rental, no compute-time surprises

## Use Cases

**Recruiting & Sourcing**

- Build candidate pipelines by tracking which companies are hiring for specific tech stacks in your region
- Identify staffing firms vs direct employers via the `employerType` field
- Spot easy-apply roles in seconds for high-volume sourcing campaigns

**Lead Generation**

- Surface companies actively hiring engineers — strong intent signal for B2B SaaS, recruiting tools, dev-tool vendors, and benefits providers
- Filter by `willingToSponsor=true` to find employers open to international candidates and partner with immigration services
- Generate prospect lists of companies posting frequently in your category

**Market & Compensation Research**

- Track salary trends across cities, roles, and seniorities using parsed `salaryMin`/`salaryMax`
- Analyze remote-vs-hybrid-vs-onsite mix by location or employer
- Map demand for specific technologies (e.g. Rust, Kubernetes, LLMs) by volume and posted date

**Job Aggregation & Niche Boards**

- Power a tech-only or remote-only job board with fresh Dice listings
- Build niche aggregators (e.g. visa-sponsorship boards, contract-only boards, easy-apply boards) using the filter set
- Enrich existing job-board records with full descriptions and direct apply links

**Academic & Labor-Market Research**

- Build longitudinal datasets of tech-hiring activity by region and skill
- Study employer behavior around remote work, sponsorship, and contract employment
- Track posting volume against macro hiring trends

## Getting Started

### Simple Keyword Search

The fastest way to get started — one keyword, sensible defaults:

```
{
    "searchQueries": ["python developer"],
    "maxResultsPerQuery": 100
}
```

### Filtered Search — Recent Remote Roles

Narrow down to fresh, remote, easy-apply jobs in a specific market:

```
{
    "searchQueries": ["data engineer", "machine learning engineer"],
    "location": "Austin, TX",
    "radiusMiles": 50,
    "postedWithinDays": "SEVEN",
    "employmentType": "FULLTIME",
    "remoteOnly": true,
    "easyApply": true,
    "maxResultsPerQuery": 200
}
```

### Visa-Sponsorship Search

Surface only roles whose employers are willing to sponsor a work visa:

```
{
    "searchQueries": ["software engineer", "backend engineer"],
    "location": "New York, NY",
    "willingToSponsor": true,
    "postedWithinDays": "THREE",
    "maxResultsPerQuery": 150
}
```

### Direct Dice URL + Keyword Combo

Already have a search tuned on the Dice website? Paste the URL alongside your own keywords for a mixed batch:

```
{
    "searchQueries": ["DevOps engineer"],
    "startUrls": [
        "https://www.dice.com/jobs?q=site+reliability+engineer&location=Seattle%2C+WA&filters.employmentType=CONTRACTS&filters.easyApply=true"
    ],
    "maxResultsPerQuery": 250
}
```

## Input Reference

### Search

| Parameter | Type | Default | Description |
| --- | --- | --- | --- |
| `searchQueries` | string[] | `["python developer"]` | Job titles or keywords to search for, such as `"python developer"` or `"data scientist"`. Each keyword runs a separate search. |
| `location` | string | `""` | Where to search for jobs. Enter a city, state, ZIP code, or `"Remote"`. Example: `"Austin, TX"`. Dice is US-only — locations outside the US fall back to nationwide results. |
| `radiusMiles` | integer | `30` | How far from the location to search, in miles (1-100). Ignored when location is empty. |
| `startUrls` | string[] | `[]` | Paste full Dice search result URLs (e.g. `https://www.dice.com/jobs?q=...`) to scrape them directly. Useful when you already have a search dialed in on the Dice website. |

### Filters & Limits

| Parameter | Type | Default | Description |
| --- | --- | --- | --- |
| `maxResultsPerQuery` | integer | `100` | Maximum number of job listings to collect per search keyword (1-500). Dice caps results at 500 per search. Use a tighter location or filters to reach more unique jobs. |
| `postedWithinDays` | string | `""` (Any time) | Only show jobs posted within this time period. Options: `"Any time"`, `"Last 24 hours"` (`ONE`), `"Last 3 days"` (`THREE`), `"Last 7 days"` (`SEVEN`). |
| `employmentType` | string | `""` (Any) | Filter by job type. Pick one or leave on `"Any"` for all types. Options: `"Any"`, `"Full-time"` (`FULLTIME`), `"Part-time"` (`PARTTIME`), `"Contract"` (`CONTRACTS`), `"Third-party"` (`THIRD_PARTY`). |
| `remoteOnly` | boolean | `false` | Only show jobs marked remote / work-from-home. |
| `easyApply` | boolean | `false` | Only show jobs that support Dice's easy-apply flow. |
| `willingToSponsor` | boolean | `false` | Only show jobs whose employer is willing to sponsor work visas. |

## Output

Each record is one job posting with the full set of structured fields:

```
{
    "jobId": "5b2d4f7c-9a3e-4d12-b7e8-1f0c2a9b8d54",
    "title": "Senior Python Developer",
    "company": "Acme Cloud Systems",
    "companyUrl": "https://www.dice.com/company/10125678",
    "companyLogoUrl": "https://marketing-assets.dice.com/logos/acme-cloud.png",
    "location": "Austin, TX",
    "postedDate": "2026-04-22T14:08:00Z",
    "modifiedDate": "2026-04-24T09:31:00Z",
    "salary": "$140,000 - $170,000 per year",
    "salaryMin": 140000,
    "salaryMax": 170000,
    "salaryCurrency": "USD",
    "salaryPeriod": "year",
    "employmentType": "Full-time",
    "employmentTypes": ["Full-time"],
    "isRemote": true,
    "workFromHomeAvailability": "Yes",
    "workplaceTypes": ["Remote"],
    "easyApply": true,
    "willingToSponsor": false,
    "employerType": "Direct Hire",
    "summary": "Build and scale data pipelines for a fast-growing fintech...",
    "description": "We're looking for a Senior Python Developer to join our platform team...",
    "descriptionHtml": "<p>We're looking for a Senior Python Developer...</p>",
    "skills": ["Python", "AWS", "Kubernetes", "PostgreSQL"],
    "jobUrl": "https://www.dice.com/job-detail/5b2d4f7c-9a3e-4d12-b7e8-1f0c2a9b8d54",
    "searchQuery": "python developer",
    "searchLocation": "Austin, TX",
    "scrapedAt": "2026-04-25T13:45:12Z"
}
```

### All Available Fields

| Field | Type | Description |
| --- | --- | --- |
| `jobId` | string | Dice's internal job identifier (GUID). |
| `title` | string | Job title. |
| `company` | string | Employer name. |
| `companyUrl` | string | Dice company-profile URL. |
| `companyLogoUrl` | string | Logo URL. |
| `location` | string | Display location string. |
| `postedDate` | string | ISO 8601 — when the posting was first listed. |
| `modifiedDate` | string | ISO 8601 — last time the posting was updated. |
| `salary` | string | Raw salary string exactly as shown on Dice (e.g. `"$120k"`, `"$60-80/hour"`, `"Depends on Experience"`). |
| `salaryMin` | number | null | Parsed minimum compensation, when the format is clean enough to extract. |
| `salaryMax` | number | null | Parsed maximum compensation. |
| `salaryCurrency` | string | null | ISO 4217 currency code (e.g. `"USD"`). |
| `salaryPeriod` | string | null | `"hour"`, `"day"`, `"week"`, `"month"`, or `"year"`. |
| `employmentType` | string | Comma-joined employment types as Dice displays them (e.g. `"Full-time, Contract"`). |
| `employmentTypes` | string[] | Same data, pre-split into an array for easier downstream use. |
| `isRemote` | boolean | `true` if the job is remote-eligible. |
| `workFromHomeAvailability` | string | null | Dice's WFH flag (`"Yes"` / `"No"` / `"Hybrid"`). |
| `workplaceTypes` | string[] | Workplace tags such as `"Remote"`, `"On-site"`, `"Hybrid"`. |
| `easyApply` | boolean | Whether the job supports Dice's easy-apply flow. |
| `willingToSponsor` | boolean | Whether the employer is willing to sponsor work visas. |
| `employerType` | string | null | `"Direct Hire"`, `"Recruiter"`, `"Staffing Firm"`, etc. |
| `summary` | string | Short summary as shown on the search-results card. |
| `description` | string | Full job description with HTML stripped — perfect for plain-text indexing or LLM input. |
| `descriptionHtml` | string | Full description with original HTML preserved. |
| `skills` | string[] | Tagged skills when present in the posting (best-effort — often empty). |
| `jobUrl` | string | Public Dice detail URL for the job. |
| `searchQuery` | string | The keyword that surfaced this row (provenance). |
| `searchLocation` | string | The location used for the search (provenance). |
| `scrapedAt` | string | ISO 8601 timestamp of when the record was captured. |

## Tips for Best Results

- **Use specific keywords** — `"senior react developer"` returns far cleaner results than `"developer"`. Dice's search ranks broad terms loosely.
- **Narrow the location for better signal** — a tight `location` + small `radiusMiles` returns fewer but far more relevant jobs than a nationwide search. Dice caps every search at 500 results, so a focused search uses that budget on jobs you actually want.
- **Set `postedWithinDays: "ONE"` for fresh listings** — perfect for daily candidate-sourcing pipelines or fresh-job alerting.
- **Combine `remoteOnly` with a metro location** — Dice will return remote roles whose employers are headquartered in or hiring for that metro, useful for time-zone or compliance-aware searches.
- **Run multiple keywords in one execution** — populating `searchQueries` with several related titles (e.g. `["backend engineer", "platform engineer", "infrastructure engineer"]`) deduplicates across queries automatically and saves you spinning up multiple runs.
- **Paste a Dice URL when you already have a search dialed in** — `startUrls` honor every filter encoded in the URL, so any tuning you do on the Dice website carries through unchanged.

## Good to Know

- **500-results-per-search ceiling** — Dice's own search engine caps every query at 500 jobs (25 pages × 20 per page) regardless of how many matches actually exist. To reach more unique jobs, split a broad query into narrower ones (by city, role, or seniority) and run them in a single batch. *Actual counts may run roughly 1-3% under the requested cap because Dice occasionally repeats listings across pages — we deduplicate, which can shave a few off your requested total. This is expected.*
- **Full descriptions are populated by default** — every record includes the complete job description from the detail page. This is the most valuable field for LLM enrichment, candidate matching, and full-text indexing.
- **`skills` is best-effort** — Dice doesn't expose a structured skills tag on every posting. The field is always present (defaults to `[]`) so the dataset shape stays stable, but it'll be empty for many jobs.
- **Dice is US-only** — non-US locations silently fall back to nationwide US results rather than returning an error. If you need international coverage, this is not the right source.
- **Salary parsing is best-effort** — when Dice shows `"Depends on Experience"`, `"Competitive"`, or other free-text formats, the parsed `salaryMin`/`salaryMax` fields will be `null`. The raw `salary` string is always preserved.

## Pricing

**$0.80 per 1,000 results** — pay only for jobs you actually receive. **No compute charges — you only pay per result returned.**

| Results | Estimated Cost |
| --- | --- |
| 100 | $0.08 |
| 1,000 | $0.80 |
| 10,000 | $8.00 |

Half the price of comparable Dice scrapers on Apify, with richer output (parsed salaries, full descriptions, batch keyword input, structured filters). No monthly rental, no tiered plans.

## Integrations

Export data in JSON, CSV, Excel, or XML. Connect to 1,500+ apps via:

- **Zapier** / **Make** / **n8n** — Workflow automation
- **Google Sheets** — Direct spreadsheet export
- **Slack** / **Email** — Notifications on new results
- **Webhooks** — Get notified when a run completes
- **Apify API** — Full programmatic access

## Legal & Ethical Use

This actor is designed for legitimate recruiting, market research, lead generation, and labor-market analysis. Only public job listings are collected — no private employer dashboards, applicant data, or personal contact details. Users are responsible for complying with applicable laws and Dice's Terms of Service. Do not redistribute scraped listings as a substitute for Dice's own product, and do not use collected data for spam, harassment, candidate impersonation, or any unlawful purpose.