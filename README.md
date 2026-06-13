[Dice Scraper](https://apify.com/santamaria-automations/dice-scraper?fpr=data)

# Dice Scraper - US Tech & IT Jobs, Salaries & Descriptions

Extract tech and IT job listings from [Dice.com](https://www.dice.com) — the leading US job board for technology professionals with 50,000+ active listings. Get structured data including salaries, full job descriptions, company logos, employment types, skills, company profiles, and apply URLs. No code needed.

---

## What data can you extract?

Every job listing includes:

**Job details**

- Job title and unique ID
- Full description (plain text + raw HTML) with responsibilities, requirements, and qualifications
- Skills array extracted from job badges (e.g. JavaScript, Python, AWS, Docker)
- Salary: structured min/max + currency + period + display text (e.g. "$175,000 - $275,000/year")
- Employment type (FULL_TIME, CONTRACT, PART_TIME, THIRD_PARTY)
- Posting date, last modified date, and expiration date
- Direct apply URL

**Company details**

- Company name
- Company logo URL
- Company profile URL on Dice

**Company enrichment** (optional, via `includeCompanyInfo`)

- Headquarters location
- Employee count
- Company website
- Industry
- Company description
- Total active jobs on Dice
- Recruiters (name + title) from the company's team section

**Location**

- Full location string (e.g. "New York, NY, USA", "San Francisco, California, USA", "Remote")

**Meta**

- Source URL on Dice
- Which search query or URL found the job
- Scrape timestamp

---

## Pricing

| Event | Cost | Trigger |
| --- | --- | --- |
| Actor start | **$0.001** | Once per run |
| Per job result | **$0.003** | Each job added to dataset |

**Examples:**

| Jobs | Cost |
| --- | --- |
| 10 jobs | $0.03 |
| 100 jobs | $0.30 |
| 1,000 jobs | $3.00 |
| 10,000 jobs | $30.00 |

Every job includes full description, salary, company info, and apply URL. No hidden fees, no monthly subscription.

---

## Use with AI Agents (MCP)

Connect this actor to any MCP-compatible AI client — Claude Desktop, Claude.ai, Cursor, VS Code, LangChain, LlamaIndex, or custom agents.

**Apify MCP server URL:**

```
https://mcp.apify.com?tools=santamaria-automations/dice-scraper
```

**Example prompts:**

> "Use `dice-scraper` to find remote Senior Software Engineer jobs. Return the top 20 as a table with title, company, salary, and location."

> "Use `dice-scraper` to extract all Python developer jobs in New York with salary above $150k."

> "Search Dice for DevOps engineer jobs and compare salaries across different cities."

---

## How to use

### 1. Search URLs (recommended)

Go to [dice.com/jobs](https://www.dice.com/jobs), set up your filters (keyword, location, remote, date posted), and paste the URL. All filters are preserved automatically.

```
{
  "searchUrls": [
    "https://www.dice.com/jobs?q=software+engineer&location=Remote",
    "https://www.dice.com/jobs?q=data+engineer&location=New+York%2C+NY"
  ]
}
```

### 2. Search Keywords

Enter one or more keywords. The scraper searches Dice and returns matching jobs.

```
{
  "searchQueries": ["python developer", "data engineer", "devops"],
  "maxResultsPerQuery": 100
}
```

### 3. Combining modes

Use both at once — results are deduplicated across all sources.

```
{
  "searchUrls": ["https://www.dice.com/jobs?q=react&location=Remote"],
  "searchQueries": ["angular developer"],
  "maxResults": 200
}
```

### 4. With company enrichment

Enable `includeCompanyInfo` to fetch each company's profile page for HQ, employee count, website, industry, and recruiter contacts.

```
{
  "searchQueries": ["software engineer"],
  "includeJobDetails": true,
  "includeCompanyInfo": true,
  "maxResults": 50
}
```

---

## Input reference

| Field | Type | Default | Description |
| --- | --- | --- | --- |
| `searchUrls` | string[] | — | Pre-filtered search URLs from [dice.com/jobs](https://www.dice.com/jobs). |
| `searchQueries` | string[] | — | One or more search keywords (e.g. "python developer"). |
| `includeJobDetails` | boolean | `true` | Fetch each job's detail page for full description, company name/logo, employment type, skills, and expiry date. |
| `includeCompanyInfo` | boolean | `false` | Fetch each company's profile page for HQ, employees, website, industry, description, total jobs, and recruiters. |
| `maxConcurrency` | integer | `5` | Number of detail pages to fetch in parallel (1-20). |
| `maxResultsPerQuery` | integer | `100` | Max results per search URL or keyword. |
| `maxResults` | integer | `10` | Total cap across all queries (0 = unlimited). |
| `proxyConfiguration` | object | Apify auto | Proxy settings. Datacenter proxies work great. |

---

## Output example

```
{
  "id": "966e4caa72977062b0875b3030b41e0d",
  "title": "Senior Software Engineer",
  "company": "Zions Bancorporation",
  "company_logo_url": "https://d3qscgr6xsioh.cloudfront.net/abc123_transformed.png",
  "company_url": "https://www.dice.com/company-profile/212cac11-535d-4978",
  "location": "New York, NY, USA",
  "salary_min": 175000,
  "salary_max": 275000,
  "salary_currency": "USD",
  "salary_period": "year",
  "salary_text": "USD 175,000.00 - 275,000.00 per year",
  "skills": ["Java", "Python", "AWS", "Microservices", "Docker", "Kubernetes"],
  "employment_type": "FULL_TIME",
  "description_full": "About the Role\nWe are looking for a Senior Software Engineer to join our platform team.\n\nResponsibilities\n- Design and build scalable microservices\n- Mentor junior engineers\n- Drive technical architecture decisions\n\nRequirements\n- 5+ years experience with Java or Python\n- Experience with AWS/Azure cloud platforms\n- Strong understanding of distributed systems",
  "description_html": "<strong>About the Role</strong><br/>We are looking for a Senior Software Engineer...",
  "description_snippet": "We are looking for a Senior Software Engineer to join our platform team...",
  "posted_at": "2026-05-01T08:00:00Z",
  "modified_at": "2026-05-03T12:30:00Z",
  "expires_at": "2026-06-05T08:00:00Z",
  "source_url": "https://www.dice.com/job-detail/966e4caa-7297-7062-b087-5b3030b41e0d",
  "source_platform": "dice.com",
  "apply_url": "https://www.dice.com/job-detail/966e4caa-7297-7062-b087-5b3030b41e0d",
  "search_query": "software engineer",
  "scraped_at": "2026-05-05T14:30:00Z",
  "company_hq": "Salt Lake City, UT, USA",
  "company_employees": "10,000+",
  "company_website": "https://careers.zionsbancorp.com/",
  "company_industry": "Banking",
  "company_description": "Zions Bancorporation is a leading financial services company...",
  "company_total_jobs": 21,
  "company_recruiters": [
    {"name": "Gwen Trapp", "title": "Recruiter"},
    {"name": "John Smith", "title": "Senior Technical Recruiter"}
  ]
}
```

---

## Use cases

### Salary benchmarking

Dice publishes salary data for most tech roles. Aggregate salary ranges across titles, locations, and experience levels to build compensation intelligence for engineering teams.

### Tech hiring monitoring

Track which companies are hiring for specific technologies. Run searches for "kubernetes", "rust", "machine learning" to see demand trends in real time.

### Skills demand analysis

Use the extracted skills array to analyze which technologies are most in-demand across job listings. Track skill trends over time.

### Recruitment automation

Feed job listings into your CRM, ATS, or spreadsheet via Apify's API, webhooks, or integrations. Build alerts for new jobs matching your criteria.

### Competitor intelligence

Monitor competitor hiring activity by searching company names or tech stacks. Track when companies scale teams or open new locations. Use company enrichment to get recruiter contacts.

---

## Speed

| Jobs | ~Time | Memory |
| --- | --- | --- |
| 10 | **~3 seconds** | ~16 MB |
| 100 | **~10 seconds** | ~16 MB |
| 1,000 | **~60 seconds** | ~20 MB |
| 10,000 | **~8 minutes** | ~25 MB |

With `includeJobDetails: false`, the actor is ~5x faster (API-only, no detail page fetching).
With `includeCompanyInfo: true`, add ~1-2 seconds per unique company (cached across duplicate companies).

---

## Integrations

This actor works with all Apify integrations:

- **API** — trigger runs and download results programmatically
- **Webhooks** — get notified when a run completes
- **Zapier & Make** — connect to 5,000+ apps
- **Google Sheets** — export directly to a spreadsheet
- **Slack, Email** — get notifications with results

---

## Related actors

- [Indeed Scraper](https://apify.com/santamaria-automations/indeed-scraper) — 60+ countries, salary & company enrichment
- [Built In Scraper](https://apify.com/santamaria-automations/builtin-scraper) — US tech jobs with company tech stack & culture
- [LinkedIn Jobs Scraper](https://apify.com/santamaria-automations/linkedin-jobs-scraper) — LinkedIn job listings
- [RemoteOK Scraper](https://apify.com/santamaria-automations/remoteok-scraper) — Remote-only job listings

---

Something not working? [Create an issue](https://console.apify.com/actors/fTAoCKhpdGP55FVw4/issues) and we'll fix it fast.