# Check a Password (Pwned Passwords Range API)

**Endpoint:** `GET https://api.pwnedpasswords.com/range/{first5HashChars}`

> **Important:** This endpoint is hosted on a different domain (`api.pwnedpasswords.com`) from the breach and data class endpoints (`haveibeenpwned.com`), even though the same organization runs both.

**Description:** Checks whether a password has appeared in known breach data, using a privacy-preserving method called k-anonymity. Instead of sending a full password, or even its full hash, the client sends only the first 5 characters of the password's SHA-1 hash. The API returns every known hash suffix that starts with those 5 characters, and the client checks locally, on its own device, whether its full hash appears in the list. The password itself is never transmitted to HIBP.

## Parameters

| Parameter | Location | Required | Description |
|-----------|----------|----------|-------------|
| `first5HashChars` | URL path | Yes | The first 5 characters of the SHA-1 hash of the password being checked. Not case-sensitive. |

## Example request

`GET https://api.pwnedpasswords.com/range/5BAA6`

## Example response (excerpt, plain text, not JSON)

```
003CD215739D7C1B2218670D26F81408237:2
003D68EB55068C33ACE09247EE4C639306B:29
00658BFD1E05761042698D19D32CD9F1A8F:15
```

## Response format

Unlike the other two endpoints, this response is plain text, not JSON. There are no brackets, braces, or quotes. Each line follows the format `{hash-suffix}:{count}`.

- **Hash suffix:** the remaining 35 characters of the SHA-1 hash (the part after the 5 characters you sent in the request).
- **Count:** how many times a password producing that exact full hash has appeared in HIBP's breach data.

## Worked example

The SHA-1 hash of the password "password" is `5BAA61E4C9B93F3F0682250B6CF8331B7EE68FD8`. The first five characters, `5BAA6`, go in the request. The response contains the line `1E4C9B93F3F0682250B6CF8331B7EE68FD8:52372427`, which matches the remaining 35 characters of the hash. So this password had appeared 52,372,427 times in HIBP's data when checked in September 2026. The count changes as HIBP adds data.

## Verification notes

**Confirmed:** the response is plain text, not JSON. This is worth flagging, since a developer expecting JSON by default (as with the other two endpoints) could waste time trying to parse it and failing.

**Discrepancy:** the result count is significantly higher than documented. HIBP's documentation says a range search "typically returns approximately 800 hash suffixes." Live testing across five prefixes, spanning the full range of possible values and not only common passwords, consistently returned far more:

| Prefix tested | Result count |
|---------------|--------------|
| `00000` (start of range) | 2,509 |
| `5BAA6` ("password") | 1,978 |
| `80000` (middle of range) | 1,990 |
| `A3F2C` (random) | 1,955 |
| `FFFFF` (end of range) | 2,047 |

All five results fall in a consistent band (roughly 1,950 to 2,510), regardless of position in the range or whether the prefix belongs to a common password. This strongly suggests the documented figure of about 800 is outdated and reflects an earlier, smaller dataset. Developers should not rely on it for capacity planning (for example, estimating response size or memory allocation) and should design for a variable response size, reasonably expected to be in the low thousands as of current testing.

**Observed:** responses are sorted by hash suffix value in ascending order, from `00...` toward `FF...` within a single response. This may allow more efficient client-side search (for example, binary search) than a linear scan.

**Confirmed:** no rate limit. Per HIBP's documentation, this endpoint has no rate limit, unlike the breach and paste endpoints.
