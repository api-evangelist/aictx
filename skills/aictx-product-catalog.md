---
name: synsense-product-catalog
description: >-
  Read SynSense's product catalogue — the neuromorphic silicon and the software stack — from the
  company's own public content API, with categories and datasheet attachments resolved.
api: SynSense Website Content API
base_url: https://www.synsense.ai/wp-json
auth: none
operations:
  - listProducts
  - getProduct
  - listProductCategories
  - getProductCategory
  - listMedia
  - getMediaItem
generated: '2026-09-14'
method: generated
source: openapi/aictx-website-content-api-openapi.json
---

# Read the SynSense product catalogue

SynSense models both its silicon and its software as products. Eight entries were present when
this skill was written: Rigi Series, AEVEON, DVS Series, Speck, Xylo, Rockpool, Sinabs and SAMNA.

## Before you start

No credentials. The base URL is `https://www.synsense.ai/wp-json` and every call below is a plain
anonymous `GET`. If a call returns `401`, you have asked for `context=edit` — drop it.

## Steps

1. **List the catalogue.** `listProducts` — `GET /wp/v2/products_list?per_page=100`.
   Read `X-WP-Total` from the response headers to confirm you have everything; the body is a bare
   JSON array with no envelope and no count in it. `per_page` is capped at 100, and asking for
   more returns `400 rest_invalid_param` with `data.details.per_page.code = rest_out_of_bounds`.
2. **Trim the payload.** Add `&_fields=id,slug,title,link,product_category,featured_media` to keep
   the response small. The full record includes rendered HTML content.
3. **Resolve categories.** Each product carries `product_category` as an array of term ids. Call
   `listProductCategories` (`GET /wp/v2/product_category?per_page=100`) once and join locally
   rather than calling `getProductCategory` per id.
4. **Resolve datasheets and images.** `featured_media` is a single media id; call `getMediaItem`
   (`GET /wp/v2/media/{id}`) and read `source_url` and `mime_type`. Prefer `&_embed` on step 1,
   which inflates both the media and the terms in one round trip.
5. **Fetch one product in detail.** `getProduct` — `GET /wp/v2/products_list/{id}`. Ids are bare
   integers and are not stable across a site rebuild; `slug` and `link` are the durable keys.

## What you will not find here

The substantive product copy lives in a per-post-type `acf` object whose shape the API does not
declare — treat it as free-form and do not assume field names. Datasheets, dev-kit manuals and
firmware are files under `/wp/v2/media` and on
[the developer download page](https://www.synsense.ai/developer/), not structured fields.

This API describes products; it does not operate them. SynSense silicon is driven locally by the
`samna`, `rockpool` and `sinabs` Python packages over a USB/FPGA dev kit — there is no network
API for the chips.

## Errors

`400 rest_invalid_param` (a query parameter failed validation — read `data.details`),
`404 rest_post_invalid_id` (no product with that id), `404 rest_no_route` (wrong path — re-read
the route index via `getApiIndex`). Full catalogue: `errors/aictx-problem-types.yml`.
