# Overview

> Independent portfolio sample written by Georgia Lainiotis. Not affiliated with or endorsed by Have I Been Pwned or Troy Hunt. Breach and data class information comes from [haveibeenpwned.com](https://haveibeenpwned.com) and is used under the [Creative Commons Attribution 4.0 International License](https://creativecommons.org/licenses/by/4.0/). Last verified: September 2026.

## What this covers

This documentation covers the free, key-less endpoints of the Have I Been Pwned (HIBP) API v3: checking a single known breach by name, retrieving the master list of breach data categories, and checking whether a password has appeared in known breach data. All examples and findings were independently tested against live API responses, then cross-checked against HIBP's official documentation ([haveibeenpwned.com/API/v3](https://haveibeenpwned.com/API/v3)).

## Scope

This documentation covers only endpoints that require no API key:

- Getting a single breach by name
- Getting all data classes
- Checking a password via the Pwned Passwords range API

Endpoints that search by email address or domain require a paid HIBP subscription key and are outside the scope of this documentation. HIBP's public offering also includes the free `GET /breaches` and `GET /latestBreach` endpoints, which are deliberately not covered here and are a candidate for a future addition.

## Authentication

No authentication is required for any endpoint documented here. All requests can be made directly from a browser or any HTTP client.

## Required header

Read this before making any request outside a browser. Per HIBP's documentation, every request to the haveibeenpwned.com endpoints (breach and data class lookups) must include a `User-Agent` request header identifying the calling application. A missing `User-Agent` results in an HTTP `403` response. Browsers send this header automatically, which is why testing in a browser address bar works without extra setup. Any code you write (a script, an app, a curl command) must set the header explicitly, or every request will fail.

## Response codes

| Code | Meaning |
|------|---------|
| `200` | Success. The requested data is returned. |
| `400` | Bad request. The input did not meet the expected format. |
| `403` | Forbidden. Usually a missing or invalid `User-Agent` header. |
| `404` | Not found. For example, the breach name does not exist. |
| `429` | Too many requests. Rate limit exceeded. This does not apply to the Pwned Passwords endpoint, which has no rate limit. |
| `503` | Service unavailable. |

Every example response in this documentation was pulled from the live API, not copied from HIBP's own sample documentation. Where live behavior differed from HIBP's official documentation, the discrepancy is called out in a **Verification notes** section on the relevant page.

## Attribution

Breach and data class information in this documentation is provided by Have I Been Pwned ([haveibeenpwned.com](https://haveibeenpwned.com)) under the [Creative Commons Attribution 4.0 International License](https://creativecommons.org/licenses/by/4.0/). HIBP asks that anywhere its data is used, the source is identified with a link to haveibeenpwned.com. The Pwned Passwords API has no attribution requirement, and it is credited here anyway.

## Pages in this documentation

- [Getting Started](getting-started.md)
- [Get a Single Breach](get-a-single-breach.md)
- [Get All Data Classes](get-all-data-classes.md)
- [Check a Password (Pwned Passwords Range API)](check-a-password.md)
- [Changelog](changelog.md)
