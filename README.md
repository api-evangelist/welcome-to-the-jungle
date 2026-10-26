# Welcome to the Jungle

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

Welcome to the Jungle is a Paris-headquartered employer-branding and hiring platform. Its Welcome Hiring
Suite bundles Welcome Employer Brand company showcases, Welcome ATS (historically shipped as Welcome Kit),
Welcome Job Matching and Welcome Sourcing, alongside the welcometothejungle.com job board and media
property.

## The API

The company publishes the **Welcome to the Jungle Solutions API** — a token-gated REST API over recruiting
and employer-branding data.

- Developer portal / documentation: https://developers.welcomekit.co/
- Base URL: `https://www.welcomekit.co/api/v1/external`
- Authentication: OAuth bearer access token (`Authorization: Bearer …`, or `?access_token=…`), issued on
  request through https://help.welcometothejungle.com/ — there is no self-service key.
- Surface: Jobs (jobs, dependencies, departments, offices), Candidates (candidates, comments, emails,
  documents, current user), Employer Branding (organizations, images, videos, embed, tools, sectors),
  Analytics (moves) and Media (WTTJ articles) — 36 documented operations.
- Several endpoints (`GET /jobs/all`, `GET /organizations`, `GET /cms/articles/all`) require a dedicated
  partnership before the `su_*` / `cms_*` scopes are granted.

## A second, undocumented API

Probing `api.welcomekit.co` turned up a **live first-party GraphQL API** the developer portal never
mentions:

- Endpoint: `https://api.welcomekit.co/api/v1/graphql`
- **Anonymous introspection is enabled** — the complete schema (95 types, 36 queries, 11 mutations) was
  read with no credential on 2026-09-04 and is saved verbatim in `graphql/`.
- **Data is authorization-gated** — every root field returns `unauthorized` without a token.
- It enforces a query-complexity budget of 150 (the standard single-shot IntrospectionQuery is rejected
  at complexity 181), so the schema was captured by walking `__type(name:)` one type at a time.

This is the only complete machine-readable contract Welcome to the Jungle publishes, and it is **not the
same API** as the documented REST surface. 17 GraphQL fields have no REST equivalent — including job
counts, existence checks, salary benchmarking, APEC and LinkedIn job-distribution integrations, and a
per-object `policies` permission model — while 15 REST operations have no GraphQL equivalent, notably the
whole candidate email/document/comment write surface and the job status transition. The divergence is
enumerated in `mcp/welcome-to-the-jungle-tool-crosswalk.yml`.

Because it is undocumented, it carries no published version, deprecation policy, change notice or support
commitment. Treat it as reachable, not as supported.

## What this profile found

**Published, and captured here:** a live introspectable GraphQL schema, a complete 29-scope OAuth scope reference, an error-code registry, a
documented `page` / `per_page` + `X-Total` pagination model, incremental-sync filters
(`created_after` / `updated_after` / `published_after`), two public status pages, an `llms.txt` on the
docs host, a GDPR / UK GDPR privacy program with a named DPO, and an actively maintained open-source
React design system (`welcome-ui`).

**Not published, recorded as honest absences:** no OpenAPI, Swagger or AsyncAPI document on any host,
no first-party API client SDK in any package registry, no MCP server, no A2A agent card, no
`/.well-known/*` document on any host, no webhooks or event surface, no rate limits, no idempotency
mechanism, no API changelog, no deprecation policy or `Sunset`/`Deprecation` headers, no SLA, no sandbox,
no CLI, no security.txt, no vulnerability-disclosure program and no trust center.

Nothing in this repository was generated to stand in for a contract the provider does not publish. The
`mcp/` manifest is explicitly a **candidate** (`deployment.mode: none`) derived from the documented REST
operations, and is pointed at with `X-MCPServerCandidate` rather than `MCPServer` so it cannot read as a
live agent surface.

- Company: https://www.welcometothejungle.com/en
- Product: https://solutions.welcometothejungle.com/en/
- Developer docs: https://developers.welcomekit.co/
- GitHub: https://github.com/WTTJ
