# truepx-technologies

Technology definitions that [TruePx](https://truepx.xyz) uses to recognize what a web page is built with: site builders and CMS, frameworks and libraries, hosting and CDNs, analytics, consent tools and more.

- `src/categories.json`: the categories
- `src/technologies/*.json`: the technologies, one file per first letter
- `src/truepx.json`: rules written for TruePx, described below

TruePx downloads these files at a fixed commit and checks each one against a checksum built into the extension.

## Rules written for TruePx

Authored rules, the ones TruePx's own authors wrote, and the evidence for them live in this repository.

- `src/truepx.json` holds the rules. TruePx downloads it with the other files in `src/` and adds its rules to the technologies; they never remove or replace anything.
- `evidence/` holds the evidence for each rule, in `evidence/truepx.json`. TruePx does not download it.

## Evidence format

`evidence/truepx.json` has an entry for each technology in `src/truepx.json`, under the same name:

```json
{
  "Example Technology": {
    "markers": [
      { "rule": "scriptSrc: /example\\.js", "source": "https://example.com/docs", "license": "MIT" },
      { "rule": "headers x-powered-by: Example", "source": "https://example.com/docs" }
    ],
    "positive": [
      { "url": "https://example.org/", "checked": "2026-10-29", "found": "/assets/example.js" },
      { "url": "https://example.net/", "checked": "2026-10-29", "found": "Example/2.1" }
    ]
  }
}
```

- `markers`: one for each pattern the technology has in `src/truepx.json` (for `dom`, one for each selector), each with where it comes from. A `rule` is written as in `src/truepx.json`: `<field>: <pattern>` for `url`, `scriptSrc`, `scripts`, `text`, `css` and `xhr`, and `<field> <key>: <pattern>` for `headers`, `cookies`, `meta`, `js` and `dom`, where the key is the header, cookie or meta name, the JavaScript property, or the CSS selector. A rule is split at the first `": "`, so a selector that contains `": "` can't be written. The pattern part of a `dom` marker is informational: a `dom` marker is matched to its rule by selector. `source` is a link to the technology's own code or documentation, or to the live page. `license` is the code's license, if the source is code.
- `positive`: sites where the technology was seen. Two at least, on different hosts, each with the date it was checked (`YYYY-MM-DD`, not in the future) and a short excerpt of what the page served (`found`). Each marker's pattern has to match the `found` of at least one site (a `dom` marker's pattern, and an empty one, need not), and every `found` has to make the markers detect the technology. Cookie rules are presence-only, because `found` for a cookie is its name, never its value.
- `singleSite`: for a technology whose positive sites are all on one host, why there is no second.

## License

GPL-3.0, see [LICENSE](LICENSE). `src/categories.json` and `src/technologies/*.json` were copied unchanged from an open-source project at a fixed version on 29 October 2026; `src/truepx.json` and `evidence/` were written for TruePx.
