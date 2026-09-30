# Get a Single Breach

**Endpoint:** `GET https://haveibeenpwned.com/api/v3/breach/{name}`

**Description:** Retrieves full details for one specific data breach, identified by its stable `Name` value (not its display `Title`, which can change over time).

## Parameters

| Parameter | Location | Required | Description |
|-----------|----------|----------|-------------|
| `name` | URL path | Yes | The stable breach identifier, for example `Adobe`. Case-insensitive. |

## Example request
`GET https://haveibeenpwned.com/api/v3/breach/Adobe`

## Example response

```json
{
  "Name": "Adobe",
  "Title": "Adobe",
  "Domain": "adobe.com",
  "BreachDate": "2013-10-04",
  "AddedDate": "2013-12-04T00:00:00Z",
  "ModifiedDate": "2022-05-15T23:52:49Z",
  "PwnCount": 152445165,
  "Description": "In October 2013, 153 million Adobe accounts were breached with each containing an internal ID, username, email, <em>encrypted</em> password and a password hint in plain text. The password cryptography was poorly done and many were quickly resolved back to plain text. The unencrypted hints also <a href=\"http://www.troyhunt.com/2013/11/adobe-credentials-and-serious.html\" target=\"_blank\" rel=\"noopener\">disclosed much about the passwords</a> adding further to the risk that hundreds of millions of Adobe customers already faced.",
  "LogoPath": "https://logos.haveibeenpwned.com/Adobe.png",
  "Attribution": null,
  "DisclosureUrl": null,
  "DataClasses": ["Email addresses", "Password hints", "Passwords", "Usernames"],
  "IsVerified": true,
  "IsFabricated": false,
  "IsSensitive": false,
  "IsRetired": false,
  "IsSpamList": false,
  "IsMalware": false,
  "IsSubscriptionFree": false,
  "IsStealerLog": false
}
```

Field order above reflects the exact order observed in the live response, not an alphabetized or reorganized list.

## Field reference

| Field | Type | Description |
|-------|------|-------------|
| `Name` | string | A stable, permanent identifier for the breach. It never changes once assigned. Use it, not `Title`, for any code that needs to reliably reference a specific breach. |
| `Title` | string | A display-friendly label for the breach. It may change over time (for example, if HIBP updates it for clarity), so do not use it as a lookup key. |
| `Domain` | string | The primary website domain associated with the breach, in standard lowercase. |
| `BreachDate` | string (date, YYYY-MM-DD) | The date the breach is believed to have occurred. Per HIBP's documentation, this is "not always accurate": breaches are often discovered and reported well after the fact. Treat it as a guide, not a certainty. |
| `AddedDate` | string (datetime, ISO 8601) | When this record was added to HIBP's database. It may show a precise time or exactly `00:00:00Z`; both are valid. HIBP's documentation says this field has "precision to the minute," though live testing showed second-level precision (see Verification notes). |
| `ModifiedDate` | string (datetime, ISO 8601) | When this record was last modified. Per HIBP's documentation, it is always equal to or later than `AddedDate`. Verified finding: `ModifiedDate` varies independently per breach. Adobe was added in 2013 and modified in 2022, while LinkedIn's `ModifiedDate` is identical to its `AddedDate` (`2016-05-21T21:35:40Z`), meaning it has not been modified since it was added. Updates appear to happen case by case as new information emerges, not through bulk updates to the whole database. |
| `PwnCount` | number | Total number of accounts loaded into HIBP for this breach. HIBP says this is usually less than the total reported by the media, because of duplicate or invalid data in the source. |
| `Description` | string (contains HTML) | A human-readable summary of the breach. Live testing confirmed this field routinely contains embedded HTML, including `<em>` tags for emphasis and `<a href="...">` links. Quote marks inside the HTML are JSON-escaped (`\"`) but are ordinary double quotes once parsed. Applications displaying this field should either render it as HTML or strip all tags before displaying it as plain text. |
| `LogoPath` | string (URL) | A direct link to the breached company's logo image, always in PNG format per HIBP's documentation. HIBP's own sample shows a relative filename (for example, `Adobe.png`), but live testing returned a full absolute URL (`https://logos.haveibeenpwned.com/Adobe.png`), a minor drift between documented sample and live behavior. |
| `Attribution` | string or null | Credits a source or researcher, when requested by the data provider. Frequently `null`. |
| `DisclosureUrl` | string or null | Not documented in HIBP's breach model as of this writing, and `null` in every record tested (Adobe and LinkedIn). By its name it likely links to the original public disclosure of the breach, but that is an inference, not a confirmed fact. Do not assume it will contain a usable value. |
| `DataClasses` | array of strings | The categories of data exposed in this breach (for example, "Passwords" and "Email addresses"). These match the names in the master list returned by the [Data Classes](get-all-data-classes.md) endpoint. |
| `IsVerified` | boolean | `true` if HIBP has confirmed the breach is legitimate with high confidence. |
| `IsFabricated` | boolean | `true` if the breach is considered unlikely to be genuine, though it may still contain real email addresses. |
| `IsSensitive` | boolean | `true` if the breach involves sensitive personal circumstances. When `true`, the public API does not return email addresses for this breach. |
| `IsRetired` | boolean | `true` if the breach's data has been permanently removed from HIBP's system. |
| `IsSpamList` | boolean | `true` if the data came from a spam mailing list rather than an actual security compromise. |
| `IsMalware` | boolean | `true` if the data came from malware or credential-stealing campaigns rather than a compromise of the company's own systems. |
| `IsSubscriptionFree` | boolean | Marks a breach as subscription-free. Per HIBP, this flag has no effect on other attributes; it is used only in domain searches where a sufficiently sized subscription is not present. |
| `IsStealerLog` | boolean | `true` if the breach data came specifically from stealer-log malware, a distinct category from general malware. |

## Verification notes

- **Discrepancy:** `DisclosureUrl` is undocumented by HIBP and was always `null` in testing.
- **Discrepancy:** `AddedDate` and `ModifiedDate` precision differs from the documentation ("to the minute" stated, second-level precision observed).
- **Discrepancy:** `LogoPath` in the live response is a full URL, while HIBP's documented sample shows a relative filename.
- **Confirmed:** `Description` HTML markup is a recurring pattern, not a one-off exception.
