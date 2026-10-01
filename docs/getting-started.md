# Getting Started

## Who this is for

This guide is for a developer who has never used the Have I Been Pwned API before and wants a working request as fast as possible. It intentionally skips edge cases, error handling, and the full field reference; those live on the other pages in this documentation. If you already know the API, start with the endpoint pages instead.

## Before you begin

You don't need an account, an API key, or any setup. Both endpoints used in this guide require nothing but a browser.

- A web browser. That's the only requirement for this guide.
- If you later want to call the API from code instead of a browser, read the [Required header](overview.md#required-header) section on the Overview page first; it explains a rule that only applies outside the browser.

## Step 1: Check a password

This is the fastest way to see the API return real data, and it uses the endpoint with no authentication and no rate limit.

1. Open a new browser tab.
2. Paste in this address and press Enter:

   `https://api.pwnedpasswords.com/range/5BAA6`

3. You'll see a long list of lines; each one looks like `{hash-suffix}:{count}`. That's every password hash HIBP knows about that starts with the same 5 characters you sent.
4. Use your browser's find function (Ctrl+F or Cmd+F) and search for `1E4C9B93F3F0682250B6CF8331B7EE68FD8`. You should find a line ending in a large number: that's how many times "password" has appeared in breach data HIBP has collected.

**What just happened:** `5BAA6` is the first 5 characters of the SHA-1 hash of the word "password". You never sent the actual password, only a small piece of its hash, and the API sent back every match it has for that piece. This is called k-anonymity, and it's explained in more depth on the [Check a Password](check-a-password.md) page.

## Step 2: Look up a known breach

Now try the other kind of lookup this API offers: checking whether a specific company has been in a known breach.

1. Open a new browser tab.
2. Paste in this address and press Enter:

   `https://haveibeenpwned.com/api/v3/breach/Adobe`

3. You'll get back a block of data describing the Adobe breach: when it happened, how many accounts were affected, and what kind of data was exposed.

Unlike Step 1, this endpoint lives on a different domain (`haveibeenpwned.com`, not `api.pwnedpasswords.com`). Testing it directly in a browser works because browsers automatically identify themselves; if you ever call this endpoint from code, you'll need to set that identification yourself. See [Required header](overview.md#required-header) on the Overview page.

## You're done

You've now made two real requests to two different HIBP endpoints, without an account, a key, or any setup. From here:

- For every field in that Adobe response explained in detail, see [Get a Single Breach](get-a-single-breach.md).
- For the full list of categories a breach can expose, see [Get All Data Classes](get-all-data-classes.md).
- For how the password check actually works under the hood, see [Check a Password (Pwned Passwords Range API)](check-a-password.md).
