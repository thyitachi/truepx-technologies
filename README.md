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
      { "rule": "headers x-powered-by: Example", "source": "https://example.com/docs" },
      { "rule": "implies: PHP", "source": "https://example.com/docs/requirements" }
    ],
    "positive": [
      { "url": "https://example.org/", "checked": "2026-10-29", "found": "/assets/example.js" },
      { "url": "https://example.net/", "checked": "2026-10-29", "found": "Example/2.1" }
    ],
    "negative": [
      { "found": "/assets/sample.js", "why": "a script of another project with a similar name" }
    ]
  }
}
```

- `markers`: one for each pattern the technology has in `src/truepx.json` (for `dom`, one for each selector; for `implies`, one for each technology it implies), each with where it comes from. A `rule` is written as in `src/truepx.json`: `<field>: <pattern>` for `url`, `scriptSrc`, `scripts`, `text`, `css` and `xhr`; `<field> <key>: <pattern>` for `headers`, `cookies`, `meta`, `js` and `dom`, where the key is the header, cookie or meta name, the JavaScript property, or the CSS selector; and `implies: <value>`, with the value written as it is in `src/truepx.json`. A rule is split at the first `": "`, so a selector that contains `": "` can't be written. A `dom` marker is matched to its rule by selector: the pattern after the selector is only a note, and the patterns that count are the rule's own `text`, `attributes` and `properties` patterns. `source` is a link to the technology's own code or documentation, or to the live page; for `implies` it is the documentation or package manifest that shows the technology needs the other one. `license` is the code's license, if the source is code.
- `positive`: sites where the technology was seen. Two at least, on different sites (`www.x.com`, `shop.x.com` and `x.com` are one site, while `a.github.io` and `b.github.io` are two), each with the date it was checked (`YYYY-MM-DD`, not in the future) and a short excerpt of what the page served (`found`, at most 200 characters, with no query string, token, session or cookie value in it). Every pattern of the markers has to match the `found` of at least one site, the `text`, `attributes` and `properties` patterns of a `dom` rule included. A rule that only says something is there (a `dom` selector with no patterns, an empty pattern, an `implies`) has nothing to match. And every `found` has to make the markers detect the technology.
- `negative`: at least one near miss, a real excerpt (`found`, at most 200 characters, with no query string, token, session or cookie value in it) from a page that looks like the technology but isn't it, and `why` it isn't. None of the technology's patterns may match it.
- Cookie rules are presence-only: a cookie rule has no pattern, and `found` for a cookie is its name, never its value. When a technology's markers are all cookies (an `implies` apart), the `found` of a `positive` is one of those names, and the `found` of a `negative` is the name of another cookie: made only of letters, digits and `` !#$%&'*+-.^_`|~ `` (all that RFC 6265 allows in a cookie name, so no `=`, `;`, `:`, space, quote, slash or brace), and none of the technology's own.
- `singleSite`: for a technology whose positive sites are all on one site, why there is no second.

A rule for a technology that is already in `src/technologies/` can add patterns and `implies`, but not `cats`, `requires`, `requiresCategory` or `excludes`, which would change what is detected today (a category it gained would reclassify it, and set off the technologies that wait for that category). A new technology needs `cats`, with at least one category, and its `excludes` may name only technologies that are new in `src/truepx.json` too.

## License

GPL-3.0, see [LICENSE](LICENSE). `src/categories.json` and `src/technologies/*.json` were copied unchanged from an open-source project at a fixed version on 29 October 2026; `src/truepx.json` and `evidence/` were written for TruePx.
