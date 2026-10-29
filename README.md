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

## License

GPL-3.0, see [LICENSE](LICENSE). `src/categories.json` and `src/technologies/*.json` were copied unchanged from an open-source project at a fixed version on 29 October 2026; `src/truepx.json` and `evidence/` were written for TruePx.
