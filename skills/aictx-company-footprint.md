---
name: synsense-company-footprint
description: >-
  Build a current picture of SynSense as an organisation — offices, partners, awards, open roles
  and news — from the company's own public content API, without scraping HTML.
api: SynSense Website Content API
base_url: https://www.synsense.ai/wp-json
auth: none
operations:
  - listOffices
  - listCountries
  - listCities
  - listPartners
  - listPartnerCategories
  - listAwards
  - listAwardYears
  - listJobs
  - getJob
  - listPosts
generated: '2026-09-14'
method: generated
source: openapi/aictx-website-content-api-openapi.json
---

# Profile SynSense as an organisation

Everything the corporate site shows about the company is available as JSON. Use this instead of
parsing pages.

## Steps

1. **Offices.** `listOffices` — `GET /wp/v2/office?per_page=100`. Six terms when written: `zurich`,
   `nanjing`, `shanghai`, `chengdu`, `beijing`, `ningbo`. `listCountries` and `listCities` are the
   two sibling taxonomies.
2. **Open roles.** `listJobs` — `GET /wp/v2/careers_list?per_page=100&_fields=id,slug,title,link,office,country,city`.
   Each role carries `office`, `country` and `city` as arrays of term ids; join them against step 1.
   `getJob` returns one role. Hiring mix is a genuine signal — read the titles, not just the count.
3. **Partners.** `listPartners` — `GET /wp/v2/our_partners?per_page=100`, with
   `listPartnerCategories` for the classifier. Fifty-nine entries when written, so page it:
   `per_page` is capped at 100 and `X-WP-TotalPages` tells you how many pages there are.
4. **Awards.** `listAwards` — `GET /wp/v2/awards?per_page=100`, classified by `awards_year`
   (`listAwardYears`). Forty-eight entries when written.
5. **News.** `listPosts` — `GET /wp/v2/posts?per_page=100&orderby=date&order=desc`. To poll for
   change, use `modified_after` with an ISO 8601 timestamp rather than refetching the collection.

## Conventions that apply to every step

- Totals are in the `X-WP-Total` and `X-WP-TotalPages` response headers, and the next page is in a
  `Link` header with `rel="next"`. The JSON body carries neither.
- `_fields` trims the response; `_embed` inflates related media and terms in one round trip.
- Titles are rendered HTML and arrive with entities escaped (`R&#038;D`). Unescape before display.
- Every read is safe and idempotent. There is no write surface to protect: the collections answer
  `Allow: GET`, and an anonymous write is refused `401`.

## Errors

`400 rest_invalid_param` on a bad parameter — `data.details.<param>.code` gives the specific
reason (`rest_out_of_bounds`, `rest_not_in_enum`). `401 rest_forbidden_context` if you ask for
`context=edit`; you cannot, and do not need to. See `errors/aictx-problem-types.yml`.
