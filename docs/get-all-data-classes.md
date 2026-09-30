# Get All Data Classes

**Endpoint:** `GET https://haveibeenpwned.com/api/v3/dataClasses`

**Description:** Returns the complete master list of every data category that has appeared across any breach in HIBP's system. Individual breach records reference these same names in their `DataClasses` field (see [Get a Single Breach](get-a-single-breach.md)).

**Parameters:** None.

## Example request

`GET https://haveibeenpwned.com/api/v3/dataClasses`

## Example response (excerpt)

```json
[
  "Academic records",
  "Account balances",
  "Address book contacts",
  ...
  "Years of professional experience"
]
```

## Response structure

A flat JSON array of strings, with no nesting and no objects. Live testing confirmed it is sorted alphabetically, matching HIBP's documented behavior.

## Verification notes

1. **Count:** the live response contained 165 data classes in September 2026, confirmed by counting the returned list. HIBP's documentation says the list will expand over time, so treat any count as a snapshot, not a fixed number.
2. **Sensitive categories:** the list includes highly sensitive personal data categories, for example "HIV statuses," "Sexual orientations," "Political views," "Religions," "Government issued IDs," and "Races." Applications consuming breach data that includes these classes should take extra care with display, storage, and compliance with data protection regulations such as GDPR and CCPA.
