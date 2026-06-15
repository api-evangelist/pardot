# Salesforce Marketing Cloud Account Engagement (Pardot) (pardot)

Salesforce Marketing Cloud Account Engagement, formerly known as Pardot, is a B2B marketing automation platform tightly integrated with Salesforce CRM for lead generation, lead nurturing, email marketing, and marketing ROI reporting. The platform provides campaigns, forms, landing pages, dynamic content, lead scoring/grading, and Engagement Studio for multi-step nurture programs. Version 5 of the Account Engagement REST API uses Salesforce OAuth 2.0 authentication and requires a Business Unit ID header, with hosts at pi.pardot.com (production) and pi.demo.pardot.com (sandbox).

**APIs.json:** [https://raw.githubusercontent.com/api-evangelist/pardot/refs/heads/main/apis.yml](https://raw.githubusercontent.com/api-evangelist/pardot/refs/heads/main/apis.yml)

## Scope

- **Type:** Index

## Tags

- Marketing Automation
- B2B Marketing
- Lead Generation
- Email Marketing
- Salesforce
- Account Engagement

## Timestamps

- **Created:** 2026-05-11
- **Modified:** 2026-05-11

## APIs

### Account Engagement API v5

Version 5 REST API for managing prospects, accounts, campaigns, emails, forms, lists, and engagement programs in Marketing Cloud Account Engagement. Authentication uses Salesforce OAuth 2.0 with the pardot_api scope and the Pardot-Business-Unit-Id header.

- **Human URL:** [https://developer.salesforce.com/docs/marketing/pardot/guide/version5overview.html](https://developer.salesforce.com/docs/marketing/pardot/guide/version5overview.html)
- **Base URL:** `https://pi.pardot.com/api/v5`

#### Tags

- Marketing Automation
- Prospects
- Campaigns
- Email Marketing

#### Properties

- [Documentation](https://developer.salesforce.com/docs/marketing/pardot/guide/version5overview.html)
- [Authentication](https://developer.salesforce.com/docs/marketing/pardot/guide/authentication.html)
- [Getting Started](https://developer.salesforce.com/docs/marketing/pardot/guide/getting-started.html)
- [Postman Collection](collections/pardot.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/pardot.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

### Account Engagement API v3/v4 (Legacy)

Legacy v3/v4 REST API endpoints for Pardot resources. Still supported for many objects not yet migrated to v5; uses the same Salesforce OAuth 2.0 authentication scheme.

- **Human URL:** [https://developer.salesforce.com/docs/marketing/pardot/guide/overview.html](https://developer.salesforce.com/docs/marketing/pardot/guide/overview.html)
- **Base URL:** `https://pi.pardot.com/api`

#### Tags

- Marketing Automation
- Legacy API
- Prospects

#### Properties

- [Documentation](https://developer.salesforce.com/docs/marketing/pardot/guide/overview.html)
- [Postman Collection](collections/pardot.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/pardot.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

## Common Properties

- [GitHub Organization](https://github.com/pardot)
- [Website](https://www.salesforce.com/marketing/b2b-automation/)
- [Documentation](https://developer.salesforce.com/docs/marketing/pardot/guide/overview.html)
- [Pricing](https://www.salesforce.com/marketing/b2b-automation/pricing/)
- [Sign Up](https://www.salesforce.com/form/signup/freetrial-b2bma/)

## Maintainers

**FN:** Kin Lane
**Email:** kin@apievangelist.com
