# aiCTX (now SynSense)

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **Not from the company, and here with a question?** You are welcome here — we would rather be the
> front line and point you the right way than have a good report go nowhere. What this repository
> can answer is narrow, though, so it is worth knowing who you are actually looking for:
>
> - **A question about how the API works, an account, billing, or a bug in the service** — that is
>   the company's own support, not us. We profile this API; we do not operate it and cannot see
>   your account.
> - **A bug in an open-source project we only catalog** — file it on that project's own repository.
>   This has happened with a real and correct bug report that reached us instead of the people who
>   could fix it, which helped nobody.
> - **Anything about this listing itself** — the description, the tags, the rating, a missing or
>   wrong artifact — is ours. Open an issue here.
> - **Not sure, or something general about API Evangelist or APIs.io** — open an issue on the
>   [APIs.io Inbox](https://github.com/api-search/inbox) and we will route it.
>
> **This repository contains no software, and we will never ask you to download anything.** There is
> no build, release, installer, or binary here — only text and machine-readable API descriptions, so
> there is nothing here that can be "corrupt" or need "repairing". Any issue, comment, or email
> claiming otherwise and offering a download link is not from us and is hostile. Do not follow the
> link; it is a lure. Report it to GitHub and, if you like, tell us at
> [info@apievangelist.com](mailto:info@apievangelist.com) so we can take it down.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

**aiCTX AG was renamed SynSense AG in May 2020.** This repository is filed under the original
`aictx` slug; the company, its site and its software all trade as SynSense today.

SynSense is a neuromorphic computing company founded in Zurich in 2017 out of the Institute of
Neuroinformatics at the University of Zurich and ETH Zurich. It designs ultra-low-power
mixed-signal neuromorphic processors and smart sensors for always-on, sub-milliwatt edge
inference — Speck and DYNAP-CNN (event-driven vision), the Xylo family (audio and IMU), and the
DVS, Rigi and AEVEON sensor series.

## What this profile found

- **The silicon is not network-callable.** SynSense's developer surface is a stack of open-source
  Python libraries that drive its chips locally over USB/FPGA development kits: `samna` (device
  interface and runtime), `rockpool` (SNN training and deployment) and `sinabs` (PyTorch spiking
  CNNs). All are published on PyPI and actively released; see `packages/`.
- **One HTTP API is served publicly** — the WordPress REST content API at
  `https://www.synsense.ai/wp-json`. It is anonymous, read-only and CORS-open, and its custom post
  types carry real company data: products, partner organisations, offices, awards, open roles and
  news. `openapi/aictx-website-content-api-openapi.json` is a 40-operation OpenAPI 3.1 document
  **derived** by API Evangelist from surfaces the site itself serves — the route index at
  `/wp-json/` and the JSON Schema each collection returns to an HTTP `OPTIONS` request. SynSense
  publishes no OpenAPI of its own, and the derived document says so in its `info.description`,
  `x-generated-from` and `x-generated-by`.
- **Absences were probed, not assumed.** No MCP server, no A2A agent card, no `/.well-known/`
  documents on any host, no `llms.txt`, no status page, no pricing page, no security.txt, no
  OAuth/OIDC discovery, no trust centre or published certifications, and no rate-limit
  signalling. Each is recorded with the URL probed and the status it returned.

Website: https://www.synsense.ai/ · Developer community: https://www.synsense.ai/developercommunity/ ·
GitHub: https://github.com/synsense
