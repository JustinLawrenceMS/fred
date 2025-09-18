(The file `/Users/cotton_west/fred/README.md` exists, but is empty)
# Fred

Fred is a small PHP utility that builds and checks a set of Google Search URLs by replacing the domain `fbi.gov` in a prepared search query with a list of top-level domains (TLDs) from `tlds.json`, then requests each URL using Guzzle and reports whether the URL returned an HTTP 200 response.

This project is intentionally minimal and demonstrates:

- Reading configuration/data from a JSON file (`tlds.json`).
- Using `guzzlehttp/guzzle` as an HTTP client.
- Basic error handling around HTTP requests.

> Note: The default search query in `fred.php` contains a long, pre-built Google Search URL. The program replaces only the domain portion (`fbi.gov`) with each TLD from `tlds.json` to generate test URLs.

Contents
--------

- `fred.php` — Main class implementing the logic.
- `run_fred.php` — Tiny runner script that instantiates `Fred`.
- `tlds.json` — JSON file containing an array of TLD strings under the key `tlds`.
- `composer.json` / `vendor/` — Composer dependencies (Guzzle).

Requirements
------------

- PHP 8.0+ (the code uses typed properties and type hints).
- Composer for dependency management.

Installation
------------

1. Install dependencies with Composer (from the project root):

```bash
composer install
```

2. Ensure `tlds.json` exists and contains a JSON object with a `tlds` array. Example:

```json
{
	"tlds": ["fbi.gov", "example.com", "example.org"]
}
```

Usage
-----

Run the small runner script from the project root:

```bash
php run_fred.php
```

This will:

- Load the TLDs from `tlds.json`.
- For each TLD, substitute `fbi.gov` in the preconfigured Google search URL.
- Perform an HTTP GET using Guzzle.
- Print a short message indicating whether the URL returned a 200 response or an error message.

Example output
--------------

Accessible URL found: https://www.google.com/search?...site%3Aexample.com... 
URL not accessible (Status Code: 404): https://www.google.com/search?...site%3Anotfound.tld...
Error accessing URL: https://www.google.com/search?...site%3Abad.tld... - cURL error 6: Could not resolve host

Configuration
-------------

- `tlds.json`: The program expects this file in the project root by default. If you need to change the path, update the `$tlds_path` property in `Fred`.
- `fred.php`: The `$url` property contains the full Google search URL template. It currently uses `fbi.gov` as the domain to be replaced. If you want to change the query text or the domain, update this value.

Security & Ethical considerations
--------------------------------

This project performs automated HTTP requests to Google Search pages. Be mindful of the following:

- Sending many automated requests against Google may violate their terms of service and trigger rate limiting. Use responsibly and add delays/rate limiting if you run large batches.
- The default search query includes content referencing an organization (domain `fbi.gov`) — ensure you have a legitimate, non-harmful use for replacing and testing domains in the search URL.
- Do not use this script for abusive scanning or to attempt to circumvent access controls.

Potential improvements
----------------------

- Add rate limiting (sleep between requests) and retry/backoff logic.
- Allow passing the TLDs file path and/or search template via CLI arguments or environment variables.
- Output results to a file (CSV/JSON) for later analysis.
- Add tests and type checks (static analysis).

License
-------

This repository doesn't include an explicit license file. If you plan to publish or share this project, add a `LICENSE` file to indicate permitted uses.

Contact / Support
-----------------

If you have questions about the code or would like me to add features (CLI args, rate-limiting, output options, or tests), tell me what you'd like and I will update the code and README.
