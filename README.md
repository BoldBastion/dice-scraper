[Dice Scraper](https://apify.com/deltaspider/dice-scraper?fpr=data)

Automatically and efficiently scrape Dice.com job postings

## Input

All input fields are optional.

- Search term
- Search location
- Custom URL with your own filters
- Maximum number of results (default: 500)

Dice supports a lot of filters. To scrape using your own filters, go to dice.com/jobs and choose your filters. Then copy the URL and use it as the input.

Use the Apify UI for easy input or the following JSON schema for API integration:

```
{
  "keyword": "your search term",
  "location": "New York, NY, USA",
  "filter_by_url": "https://www.dice.com/jobs?....",
  "max_results": 500
}
```

## Output

A list of Dice.com postings that match your input criteria.

Example:

```
[
  {
  "id": "13b55a0cb0405436e937130cd35ab119",
  "title": "Director, Data Analytics",
  "jobLocation": {
    "displayName": "New York, New York, USA"
  },
  "postedDate": "2025-01-30T14:24:59Z",
  "detailsPageUrl": "https://www.dice.com/job-detail/36495ca6-a270-44ab-a441-aef6d29cae88",
  "companyPageUrl": "https://www.dice.com/company/91074191",
  "companyLogoUrl": "https://d3qscgr6xsioh.cloudfront.net/3BLKz0IQfyUE8xIcye6w_transformed.png?format=webp",
  "companyLogoUrlOptimized": "https://d3qscgr6xsioh.cloudfront.net/3BLKz0IQfyUE8xIcye6w_transformed.png?format=webp",
  "salary": "$120,000 - $130,000",
  "clientBrandId": "91074191",
  "companyName": "CUNY Building Performance Lab",
  "employmentType": "Full-time",
  "summary": "Through its partnership with the City of New York, CUNY s Building Performance Laboratory is hiring qualified energy management professionals to serve as on-site consultants and fill critical staffing capacity needs at the Department of Citywide Administrative Services  ( DCAS ) Division of Energy Management ( DEM ). For background, DEM serves as the hub for energy management for City government operations. DEM develops the City s annual Heat, Light, and Power Budget; manages the City s electric",
  "isFeatured": true,
  "jobId": "13b55a0cb0405436e937130cd35ab119",
  "score": 438.85495,
  "easyApply": false,
  "willingToSponsor": false,
  "employerType": "Direct Hire",
  "workFromHomeAvailability": "FALSE",
  "isRemote": false,
  "modifiedDate": "2025-01-30T14:24:59Z",
  "guid": "36495ca6-a270-44ab-a441-aef6d29cae88",
  "workplaceTypes": [
    "Hybrid"
  ]
},
  ...
]
```

# apify-dice

# dice-scraper