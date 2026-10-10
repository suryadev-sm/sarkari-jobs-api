# Sarkari Jobs API (India) - SarkariMap

Free API, RSS feed and embeddable widget for Indian government jobs (Sarkari Naukri), results, admit cards and answer keys.

Data comes from [SarkariMap](https://sarkarimap.org), an ad-free government job portal covering all Indian states.

| | |
|---|---|
| Website | https://sarkarimap.org |
| API documentation | https://developers.sarkarimap.org |
| Free widget for your site | https://sarkarimap.org/widget |
| RSS feed | https://sarkarimap.org/rss.xml |

## What you can build

- A job alert bot for Telegram, WhatsApp or Discord
- A "latest government jobs" section on a coaching, college or cyber cafe website
- A state-wise job app or browser extension
- Research and dashboards on public sector hiring in India

## 1. Jobs API (JSON)

Base URL: `https://sarkarimap.org/api/v1`

### Get an API key

Every request needs an API key. To get one, write to support@sarkarimap.org or see the [API documentation](https://developers.sarkarimap.org).

Send the key in either header:

```
Authorization: Bearer sm_live_your_key_here
x-api-key: sm_live_your_key_here
```

### List jobs

`GET /api/v1/jobs`

| Parameter | Example | Notes |
|---|---|---|
| `state` | `Uttar Pradesh`, `Bihar`, `All India` | Full state name |
| `category` | `LatestJob`, `OfflineJob`, `Result`, `AdmitCard`, `AnswerKey`, `Syllabus`, `Admission` | |
| `dateFrom`, `dateTo` | `2026-10-01` | Filter by publish date |
| `page` | `1` | Default 1 |
| `limit` | `20` | Default 20, maximum 100 |

```bash
curl -H "Authorization: Bearer sm_live_your_key_here" \
  "https://sarkarimap.org/api/v1/jobs?state=Delhi&category=LatestJob&limit=5"
```

Response:

```json
{
  "page": 1,
  "limit": 5,
  "total": 27,
  "totalPages": 6,
  "plan": "free",
  "creditsRemaining": null,
  "results": [
    {
      "title": "IIT Delhi Junior Project Assistant (Admin) Recruitment 2026",
      "slug": "iit-delhi-junior-project-assistant-admin-recruitment-2026",
      "state": "Delhi",
      "category": "LatestJob",
      "postDateLabel": "",
      "applicationDeadline": "2026-10-23T00:00:00.000Z"
    }
  ]
}
```

The public page for any job is `https://sarkarimap.org/job/{slug}`.

### Get one job

`GET /api/v1/jobs/{slug}`

```bash
curl -H "Authorization: Bearer sm_live_your_key_here" \
  "https://sarkarimap.org/api/v1/jobs/iit-delhi-junior-project-assistant-admin-recruitment-2026"
```

### Plans

| Plan | Fields |
|---|---|
| Free | Basic fields: title, slug, state, category, post date, application deadline |
| Paid | Full job data: description, official links, important dates, salary |

### JavaScript

```js
const res = await fetch('https://sarkarimap.org/api/v1/jobs?category=LatestJob&limit=10', {
  headers: { Authorization: 'Bearer sm_live_your_key_here' },
});
const { results } = await res.json();
results.forEach((job) => console.log(job.title, 'https://sarkarimap.org/job/' + job.slug));
```

### Python

```python
import requests

r = requests.get(
    "https://sarkarimap.org/api/v1/jobs",
    params={"state": "Bihar", "limit": 10},
    headers={"Authorization": "Bearer sm_live_your_key_here"},
    timeout=20,
)
for job in r.json()["results"]:
    print(job["title"], "https://sarkarimap.org/job/" + job["slug"])
```

## 2. Widget (no key, no coding)

Paste this where the list should appear. It shows currently open vacancies and updates on its own.

```html
<div class="sarkarimap-widget"><a href="https://sarkarimap.org/">SarkariMap</a> par latest sarkari naukri</div>
<script async src="https://sarkarimap.org/widget.js?count=8"></script>
```

For one state, add its code to the script address, for example `state=up`, `state=bihar`, `state=rajasthan` or `state=mp`. You can pick a state and copy ready-made code at https://sarkarimap.org/widget.

## 3. RSS feed

`https://sarkarimap.org/rss.xml` carries the latest posts and works with any feed reader or automation tool.

## Please note

- Job details are compiled from official notifications. Always confirm dates and eligibility on the official website before applying.
- If you use this data on a public site or app, a link back to [SarkariMap](https://sarkarimap.org) is appreciated.
- Found a wrong date or a broken link? Open an issue in this repository.
