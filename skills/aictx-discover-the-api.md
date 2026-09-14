---
name: synsense-discover-the-api
description: >-
  Discover what the SynSense content API actually serves — every route, parameter and resource
  schema — using the API's own introspection surface, before writing any integration code.
api: SynSense Website Content API
base_url: https://www.synsense.ai/wp-json
auth: none
operations:
  - getApiIndex
  - listTypes
  - getType
  - listTaxonomies
  - getTaxonomy
  - listStatuses
  - listSearchResults
generated: '2026-09-14'
method: generated
source: openapi/aictx-website-content-api-openapi.json
---

# Discover the SynSense content API from the API itself

SynSense publishes no OpenAPI. It does not need to be guessed at: the API describes itself, and
that self-description is what the OpenAPI in this repository was derived from. Re-run these steps
to check whether this profile is still accurate.

## Steps

1. **Fetch the route index.** `getApiIndex` — `GET https://www.synsense.ai/wp-json/`. Roughly
   360 KB of JSON. Read:
   - `name` and `url` — confirms whose API this is (`SynSense`, `https://www.synsense.ai`).
   - `namespaces` — sixteen when written; `wp/v2` is the content surface, the rest are plugin
     namespaces.
   - `routes` — every path, the methods it accepts, and the full argument schema per method.
   - `authentication` — the only mechanism advertised is WordPress application passwords.
   Narrow it with `?namespace=wp/v2` if you only want the content routes.
2. **Get a resource's schema.** Send an HTTP `OPTIONS` to any collection, e.g.
   `OPTIONS /wp/v2/products_list`. The response carries a `schema` object with every property, its
   type, format and enum. This is the closest thing the API has to a contract, and it is
   authoritative in a way documentation is not.
3. **Enumerate content types.** `listTypes` — `GET /wp/v2/types` — and `getType` for one. This is
   how you find custom post types that a generic WordPress client would miss; on this site they
   are `products_list`, `careers_list`, `our_partners` and `awards`.
4. **Enumerate taxonomies.** `listTaxonomies` — `GET /wp/v2/taxonomies` — then `getTaxonomy` for
   the `rest_base` of each. `office`, `country`, `city`, `product_category`, `partners_category`
   and `awards_year` are the SynSense-specific ones.
5. **Enumerate statuses.** `listStatuses` — `GET /wp/v2/statuses`. Only `publish` is visible
   anonymously.
6. **Search across everything.** `listSearchResults` — `GET /wp/v2/search?search=<term>` — returns
   typed results with `id`, `title`, `url`, `type` and `subtype` across all public content.

## Guardrails

- Do not call administrative routes. `/wp/v2/settings`, `/wp/v2/plugins` and `/wp/v2/users/me` all
  return `401` anonymously, and they are not part of the public surface.
- Do not attempt writes. The public surface is read-only; a POST is refused `401`.
- No rate limits are published or signalled, which is not permission to hammer the origin. Page at
  `per_page=100`, cache, and poll with `modified_after`.
