# Salesforce Marketing Cloud Account Engagement (Pardot)

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

Salesforce Marketing Cloud Account Engagement, formerly known as Pardot, is a B2B marketing automation platform tightly integrated with Salesforce CRM for lead generation, lead nurturing, email marketing, and marketing ROI reporting. The platform provides campaigns, forms, landing pages, dynamic content, lead scoring/grading, and Engagement Studio for multi-step nurture programs. Version 5 of the Account Engagement REST API uses Salesforce OAuth 2.0 authentication and requires a Business Unit ID header, with hosts at pi.pardot.com (production) and pi.demo.pardot.com (sandbox).

**Areas:** Marketing Automation · B2B Marketing · Lead Generation · Email Marketing · Salesforce · Account Engagement

## Salesforce Marketing Cloud Account Engagement (Pardot) on APIs.io

Everything profiled for Salesforce Marketing Cloud Account Engagement (Pardot) is published on APIs.io rather than in this repository. Start at the provider page — **[apis.io/providers/pardot/](https://apis.io/providers/pardot/)** — or go straight to one of the APIs or properties below.

### APIs (3)

- [Account Engagement API v5](https://apis.io/apis/pardot/account-engagement-api-v5/) — Version 5 REST API for managing prospects, accounts, campaigns, emails, forms, lists, and engagement programs in Marketing Cloud Account Engagement.…
- [Account Engagement API v3/v4 (Legacy)](https://apis.io/apis/pardot/account-engagement-api-v4/) — Legacy v3/v4 REST API endpoints for Pardot resources. Still supported for many objects not yet migrated to v5; uses the same Salesforce OAuth 2.0…
- [Salesforce Marketing Cloud Account Engagement (Pardot) Objects API](https://apis.io/apis/pardot/pardot-objects-api/) — The Objects API from Salesforce Marketing Cloud Account Engagement (Pardot) — 24 operation(s) for objects.

### Published properties

Each property below is a page on APIs.io carrying the machine-readable artifact and what it says.

#### MCP servers (1)

Model Context Protocol servers that expose the API to agents.

- [pardot-mcp.yml](https://apis.io/servers/pardot/pardot-mcp-yml/)

#### Agentic access (1)

Which operations an agent can call, which act, and which need a human in the loop.

- [Pardot Agentic Access](https://apis.io/agentic-access/pardot/pardot-agentic-access/)

#### Collections (2)

Postman and Open Collection request collections.

- [Salesforce Account Engagement (Pardot) API v5](https://apis.io/collections/pardot/open-pardot/)
- [Salesforce Account Engagement (Pardot) API v5 Objects API](https://apis.io/collections/pardot/open-pardot-objects-api/)

#### Overlays (1)

OpenAPI overlays, including conversational phrasing for each operation.

- [Pardot Objects Api Overlay](https://apis.io/overlays/pardot/pardot-objects-api-overlay/)

#### Security (4)

Authentication, trust centre, and vulnerability disclosure.

- [Pardot Authentication](https://apis.io/security/pardot/pardot-authentication/)
- [Pardot Domain Security](https://apis.io/security/pardot/pardot-domain-security/)
- [Pardot Trust Center](https://apis.io/security/pardot/pardot-trust-center/)
- [Pardot Vulnerability Disclosure](https://apis.io/security/pardot/pardot-vulnerability-disclosure/)

#### OAuth scopes (1)

The permissions the API asks for.

- [Pardot Scopes](https://apis.io/scopes/pardot/pardot-scopes/)

#### Plans and pricing (1)

Published plans, tiers and prices.

- [Pardot Plans Pricing](https://apis.io/plans/pardot/pardot-plans-pricing/)

#### Rate limits (1)

Published request limits.

- [Pardot Rate Limits](https://apis.io/rate-limits/pardot/pardot-rate-limits/)

## Where to find this API

Links to the company's own pages. This repository publishes no copy of their specifications — the canonical document is theirs and lives at the URLs below.

- [Website](https://www.salesforce.com/marketing/b2b-automation/)
- [Documentation](https://developer.salesforce.com/docs/marketing/pardot/guide/overview.html)
- [Getting started](https://developer.salesforce.com/docs/marketing/pardot/guide/getting-started.html)
- [API reference](https://developer.salesforce.com/docs/marketing/pardot/guide/version5overview.html)
- [Pricing](https://www.salesforce.com/marketing/b2b-automation/pricing/)
- [Sign up](https://www.salesforce.com/form/signup/freetrial-b2bma/)
- [Support](https://help.salesforce.com/s/articleView?id=sf.bundle_pardot_parent.htm&type=5)
- [Blog](https://developer.salesforce.com/blogs/)
- [Terms of service](https://www.salesforce.com/company/legal/sfdc-website-terms-of-service/)
- [Privacy policy](https://www.salesforce.com/company/privacy/)
- [GitHub](https://github.com/pardot)
- [Status](https://status.salesforce.com/)

## API Evangelist rating

**Kin Score 59.2** — strong · rubric 0.25.0 · scored 2026-10-23

| Facet | Score |
|---|---|
| Discoverability | 73.2 |
| Contract quality | 16.0 |
| Contract governance | 18.2 |
| Operational transparency | 60.5 |
| Developer ergonomics | 78.0 |
| Access clarity | 92.1 |

**Agent readiness 35.4** — agent-ready

| Agent-readiness dimension | Result |
|---|---|
| Spec Presence | yes |
| Agentic Access | derived (partial credit) |
| Reversibility Documented | — |
| MCP Server | — |
| Auth Clarity | served (partial credit) |
| Idempotency | — |
| Error Semantics | documented (partial credit) |
| OpenAPI Examples | — |
| Rate Limit Signal | documented (partial credit) |
| Event Surface Described | — |
| Agent Skills | derived (partial credit) |
| Well Known Catalog | — |
| Consent Identity | — |
| Agent Card | — |
| Dry Run Mode | — |
| Delegated Identity | documented (partial credit) |
| Protected Resource Metadata | — |
| Dynamic Client Registration | yes |
| Agentic Commerce | — |
| Pagination Ratio | na |
| Operation Distinctiveness | distinct |

**Regulatory alignment 45.1** — Horizontal (data, software, accessibility, platform)

The rubric is published at [github.com/api-evangelist/kin-score](https://github.com/api-evangelist/kin-score). The per-check breakdown behind these numbers — which checks were earned, partial or missed — along with score history and how this provider compares to its peers, is available at [apis.io/providers/pardot/](https://apis.io/providers/pardot/).

## How this profile is made

API Evangelist profiles a company's public API surface the way a developer would find it: the website, developer portal and documentation, the specifications it publishes, its public repositories, and its pricing, status and changelog pages. From that it derives the properties above — contracts split per API, workflows, schemas, collections, governance rules, plans and rate limits — and scores them against the published [Kin Score rubric](https://github.com/api-evangelist/kin-score).

The artifacts are published on [APIs.io](https://apis.io), not in this repository. This repository is the public record for Salesforce Marketing Cloud Account Engagement (Pardot): the place to say something is wrong, missing or out of date, and to ask for a re-score or a removal.

## Open an issue

- **Something about this listing** — a wrong description, a missing API, a stale artifact or a question about the rating: [open an issue on this repository](https://github.com/api-evangelist/pardot/issues/new).
- **A new API, or one that is not in the catalog yet** — [submit it on APIs.io](https://apis.io/add/).
- **Anything else** — [the APIs.io inbox](https://github.com/api-search/inbox/issues/new) or [info@apievangelist.com](mailto:info@apievangelist.com).
