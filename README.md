# Blink Charging (blink-charging)

Blink Charging Co. (Nasdaq BLNK) is a United States electric vehicle charging company headquartered at 17301 Melford Boulevard, Bowie, Maryland, with additional offices in California, the United Kingdom, Belgium and India. It designs and sells Level 2 and DC fast chargers (Series 7, 8 and 9, Shasta, the EQ/PQ line and the HYC DC fast chargers), owns and operates chargers under both owner-operator and host-owned business models, and runs the Blink Network — the proprietary cloud platform behind its host portal, driver mobile app and fleet management product.

In the energy value chain Blink is a charge point operator and a load on the distribution grid, not a utility, retailer or system operator. It publishes no electricity usage, tariff, grid or market data of its own, and no consumer energy data right obligation applies to it in its home market of the United States. Its API posture is a closed door behind a live legal surface: the Blink Network Terms and Conditions (last modified 2 December 2025) explicitly govern "the Blink API", but no live developer portal, API reference, base URL or machine-readable contract is published anywhere on blinkcharging.com.

**APIs.json:** [https://raw.githubusercontent.com/api-evangelist/blink-charging/refs/heads/main/apis.yml](https://raw.githubusercontent.com/api-evangelist/blink-charging/refs/heads/main/apis.yml)

## Tags

- Energy
- United States
- EV Charging
- Electric Vehicles
- Charging Stations
- Grid
- Demand Response
- Fleet Management
- OCPP
- OpenADR
- Roaming

## Timestamps

- **Created:** 2026-07-27
- **Modified:** 2026-07-27

## APIs

No public, documented API is listed for Blink Charging.

Blink names an API in its own current terms of service, and it operated a named developer program (the BlinkMap API) in the past, but as of the 2026-07-27 bootstrap there is no live developer portal, no published API reference, no published base URL and no downloadable machine-readable contract. Nothing was invented to fill the gap, so `apis[]` is deliberately empty. See `review.yml` for every URL probed and the HTTP status returned.

What exists instead:

- **Blink API (named in the Blink Network Terms and Conditions, last modified 2025-12-02).** Section 8, "Using the Blink Website, API, or Mobile Applications", states you may access it "to obtain information regarding Blink Charging Station locations and other content and features" and that "You are responsible for all use of the Blink Website, API, and Mobile Applications under your username." No endpoint, base URL, reference or specification is published.
- **BlinkMap API (historical, retired surface).** The developer page at `https://prod.blinknetwork.com/developer.html` described a third-party integrator program returning "Blink-compatible EV charger locations, hours of operation, network status, and more", with access granted by request form and conditioned on the "Blink Network, LLC Data License Agreement". That host now refuses connections and the page was last archived in July 2021.
- **UK open data request (live).** `https://blinkcharging.com/en-gb/getintouch/blink-open-data-request` cites the UK Public Charge Point Regulations 2023 and offers charge point location, availability and usage data by Google Form request, reviewed by the Blink data team, shared with Eco-Movement, and delivered "in api format" on a "stipulated timeframe".

## Standards

Blink's standards adoption is real, but it sits below the public API layer — in the charger-to-backend and utility-to-network control planes, not in a developer contract.

- **OCPP 2.0.1** — certification announced 2025-09-16 for Blink Series 7, 8 and 9 chargers. Charger-to-CSMS device protocol, authored by the Open Charge Alliance, not a Blink developer API.
- **OpenADR 2.0** — the Blink Network was approved by the OpenADR Alliance on 2020-03-17, so "a utility or load management entity can readily integrate our entire charging network within their territory to enable load management control".
- **Roaming** — Blink participates in third-party roaming and data distribution networks (Hubject Intercharge, Eco-Movement, Paua). Blink publishes no OCPI endpoint of its own; every `/ocpi/versions` probe against a Blink host returned an SPA shell or 403.

## Enrichment round — 2026-07-27

A second pass re-ran the full contract discovery (OpenAPI on every API host root, GraphQL introspection, MCP `tools/list`, AsyncAPI, `/.well-known/*`) and still found no machine-readable contract. It did find four things the bootstrap round did not — all recorded under `enrichmentRounds` in `review.yml`:

- **An OCPI gate on a live Kong gateway.** `https://api.blinknetwork.com` runs `kong/2.8.1`. Unmatched paths return `{"message":"no Route matched with those values"}`, but every `/map`-prefixed path returns `503` with the body *"This service is currently unavailable. Please contact support if you need access to OCPI."* A route for OCPI exists, at the same `/map` prefix the retired BlinkMap API used, switched off for anonymous callers.
- **An unadvertised health surface.** `https://api.blinknetwork.com/health` returns a Spring Data REST/HAL root; `/health/actuator/health` returns `{"status":"UP"}`. No springdoc/OpenAPI surface is exposed behind it. This is not a status page and was not wired as one.
- **A Vanta trust centre.** `https://trust.blinkcharging.com` is live and titled "Blink Charging Trust Center", unlinked from site navigation and absent from all 985 sitemap URLs. Its certification list is not readable anonymously (request-signed GraphQL), so no certification is claimed. The one named compliance programme on the marketing site is FedRAMP **"In Process"** (announced 2024-06-13).
- **`blinknetwork.com` domain hygiene is weak.** No DMARC, no CAA, no DNSSEC, SPF ending in `+all`, TLS 1.2 only on the apex and no HSTS on any host — on the domain that carries the API gateway and both portals.

Artifacts added this round: `conformance/`, `lifecycle/`, `authentication/`, `packages/`, `well-known/`, `llms/`, `security/` (domain security + trust centre). Nothing was generated for OpenAPI, MCP, skills, sandbox, CLI, changelog or AsyncAPI — none exist, and `review.yml` records which `apis.yml` pointers were deliberately withheld rather than claimed.

## Duplicate warning

An existing api-evangelist repo at `all/blink` profiles this same company (Blink Charging Co., blinkcharging.com) under the aid `blink`, with three AE-modeled OpenAPI documents. These two repos should be merged rather than both published. See the `duplicateWarning` block in `review.yml`.
