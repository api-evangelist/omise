# Omise (omise)

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
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

Omise, now **Opn Payments** (part of Opn), is a Southeast Asian online payment gateway serving **Thailand, Japan, and Singapore**. Its REST API lets developers accept card payments and local methods - PromptPay, TrueMoney, internet and mobile banking, installments, and QR wallets - and manage the full money-movement lifecycle: Charges, Tokens, Sources, Customers, Refunds, Disputes, Transfers, Recipients, Schedules, Links, and Events/webhooks.

> **Naming:** The company rebranded from **Omise** to **Opn Payments** in 2022 (parent company **Opn**). Documentation now lives under both `docs.omise.co` and `docs.opn.ooo`, but the **API hosts are unchanged** - `api.omise.co` and `vault.omise.co` - and object/key prefixes stay Omise-native (`skey_`, `pkey_`, `tokn_`, `chrg_`, `src_`, `cust_`). This catalog entry uses the slug `omise`.

## Access model (read this first)

- **Two hosts.** The **core API** is `https://api.omise.co`. Raw card data is tokenized on a **separate PCI-scoped vault host**, `https://vault.omise.co`, so a card number never has to touch your server.
- **Two keys, HTTP Basic.** Every request uses **HTTP Basic auth with the key as the username and an empty password**.
  - **Secret key** (`skey_test_...` / `skey_...`) - used **server-side** against `api.omise.co` for charges, customers, refunds, transfers, and everything else.
  - **Public key** (`pkey_test_...` / `pkey_...`) - used **client-side** (typically via Omise.js) against `vault.omise.co` to create card tokens, and to create Sources.
- **Test vs. live.** `*_test_*` keys run in test mode - no funds move and no fees are charged. `*_test_*`-only helpers like `mark_as_paid` / `mark_as_failed` simulate outcomes. Live keys move real money.
- **API versioning.** Pin behavior per request with the optional `Omise-Version` header (for example `2019-05-29`); otherwise the account default applies.
- **Amounts.** All monetary amounts are **integers in the smallest currency unit** (satang for THB, yen for JPY).

**APIs.json:** [https://raw.githubusercontent.com/api-evangelist/omise/refs/heads/main/apis.yml](https://raw.githubusercontent.com/api-evangelist/omise/refs/heads/main/apis.yml)

## Tags

- Payments
- Payment Gateway
- Thailand
- Southeast Asia
- Charges
- Tokens
- Sources
- PromptPay
- Cards
- Fintech

## Timestamps

- **Created:** 2026-07-12
- **Modified:** 2026-07-12

## APIs

### Omise Charges API

Create, retrieve, list, and update charges - the core payment object. Supports auto or manual capture (authorize then capture), reverse, expire, and test-mode mark_as_paid / mark_as_failed. Charges are funded by a token, a saved customer card, or a Source.

- **Human URL:** [https://docs.omise.co/charges-api](https://docs.omise.co/charges-api)
- **Base URL:** `https://api.omise.co`

### Omise Tokens API

Single-use tokenization of raw card data on the PCI-scoped vault host, authenticated with the public key. Create a token client-side (via Omise.js) or server-side, then reference it when creating a charge or saving a card.

- **Human URL:** [https://docs.omise.co/tokens-api](https://docs.omise.co/tokens-api)
- **Base URL:** `https://vault.omise.co`

### Omise Sources API

Create and retrieve payment Sources for non-card, local methods - PromptPay, TrueMoney, internet and mobile banking, installments, and QR wallets (Alipay+, WeChat Pay, GCash, ShopeePay, and more) - each carrying a redirect, offline, or app_redirect flow.

- **Human URL:** [https://docs.omise.co/sources-api](https://docs.omise.co/sources-api)
- **Base URL:** `https://api.omise.co`

### Omise Customers API

Create, list, retrieve, update, and delete customers, and manage saved cards. Attaching a card token to a customer saves it for repeat and recurring charges.

- **Human URL:** [https://docs.omise.co/customers-api](https://docs.omise.co/customers-api)
- **Base URL:** `https://api.omise.co`

### Omise Refunds API

Issue full or partial refunds against a charge and list them, nested under a charge or across the account.

- **Human URL:** [https://docs.omise.co/refunds-api](https://docs.omise.co/refunds-api)
- **Base URL:** `https://api.omise.co`

### Omise Disputes API

List disputes (all, open, pending, closed), retrieve a dispute, respond with evidence, accept a dispute, and manage supporting documents.

- **Human URL:** [https://docs.omise.co/disputes-api](https://docs.omise.co/disputes-api)
- **Base URL:** `https://api.omise.co`

### Omise Transfers API

Move funds from your balance out to a bank-account recipient, with fee, VAT, and net breakdowns; test-mode mark_as_sent / mark_as_paid simulate the payout lifecycle.

- **Human URL:** [https://docs.omise.co/transfers-api](https://docs.omise.co/transfers-api)
- **Base URL:** `https://api.omise.co`

### Omise Recipients API

Manage the individual or corporation recipients that transfers pay out to - create, list, retrieve, update, delete, and verify - each holding a destination bank account and verification status.

- **Human URL:** [https://docs.omise.co/recipients-api](https://docs.omise.co/recipients-api)
- **Base URL:** `https://api.omise.co`

### Omise Schedules API

Automate recurring charges and transfers on a day, week, or month period with day-of-month or weekday rules and start / end dates; inspect generated occurrences.

- **Human URL:** [https://docs.omise.co/schedules-api](https://docs.omise.co/schedules-api)
- **Base URL:** `https://api.omise.co`

### Omise Links API

Generate shareable payment Links for a fixed amount and currency without custom checkout code; single- or multiple-use, with associated charges listed.

- **Human URL:** [https://docs.omise.co/links-api](https://docs.omise.co/links-api)
- **Base URL:** `https://api.omise.co`

### Omise Events API

Retrieve and list account events - each a keyed record wrapping the object that changed. Events back Omise **webhooks**, which POST the same payloads to your configured `webhook_uri`.

- **Human URL:** [https://docs.omise.co/events-api](https://docs.omise.co/events-api)
- **Base URL:** `https://api.omise.co`

### Omise Account and Balance API

Retrieve your account profile (country, currencies, webhook URI, transfer config, default API version) and your current balance - total, transferable, and reserve.

- **Human URL:** [https://docs.omise.co/balance-api](https://docs.omise.co/balance-api)
- **Base URL:** `https://api.omise.co`

## WebSocket

**No.** Omise exposes a REST API over HTTPS plus outbound **webhooks** (HTTP POST to your `webhook_uri`). There is **no documented public WebSocket (`ws://` / `wss://`) endpoint**, so no AsyncAPI artifact is included. See `review.yml`.

## Artifacts

- [OpenAPI](openapi/omise-openapi.yml) - grounded representative subset of the REST API
- [Postman Collection](collections/omise.postman_collection.json)
- [Open Collection](collections/omise.opencollection.json)
- [Authentication](authentication/omise-authentication.yml)
- [Plans / Pricing](plans/omise-plans-pricing.yml)
- [Rate Limits](rate-limits/omise-rate-limits.yml)
- [FinOps](finops/omise-finops.yml)
- [Domain Security](security/omise-domain-security.yml)
- [Review](review.yml)

## Fidelity note

Paths, hosts, methods, auth, and object identifiers are **grounded** in the published Omise documentation (`docs.omise.co`) and live host probes (2026-07-12). Request/response **schemas in the OpenAPI are a representative subset** of documented fields rather than a byte-for-byte mirror, and a few nested card operations are modeled rather than each independently confirmed - these are flagged honestly in `review.yml`. **Pricing is per-transaction and country-specific; the rates referenced are indicative and `reconciled: false`.**

## Common Properties

- [GitHub Organization](https://github.com/omise)
- [LinkedIn](https://www.linkedin.com/company/opn-payments)
- [Website](https://www.omise.co)
- [Documentation](https://docs.omise.co)
- [Plans](plans/omise-plans-pricing.yml)
- [Rate Limits](rate-limits/omise-rate-limits.yml)
- [Fin Ops](finops/omise-finops.yml)

## Maintainers

**FN:** Kin Lane
**Email:** kin@apievangelist.com
