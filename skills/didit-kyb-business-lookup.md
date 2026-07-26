---
name: Business verification (KYB) lookup
description: Search a company registry then pull the billable full business profile.
api: openapi/didit-openapi-original.json
operations: [post_v3_kyb_search, post_v3_kyb_select]
auth: x-api-key header
---

# Business verification (KYB) lookup

Two-step KYB: a free search, then a billable profile pull.

## Steps
1. **Search** — `POST /v3/kyb/search/` (`post_v3_kyb_search`) with company name + country.
   Free; returns candidate registry matches.
2. **Select the company** — `POST /v3/kyb/select/` (`post_v3_kyb_select`) with the chosen match
   to retrieve the billable full profile: registry data, UBO/ownership, officers, and entity AML.

## Rules
- Auth: `x-api-key` header. KYB profile pull is billed ($2.00); search is free.
- Combine with a KYC session for the beneficial owners you must verify as individuals.
