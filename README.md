# truepx-technologies

Technology definitions that [TruePx](https://truepx.xyz) uses to recognize what a web page is built with: site builders and CMS, frameworks and libraries, hosting and CDNs, analytics, consent tools and more.

- `src/categories.json`: the categories
- `src/technologies/*.json`: the technologies, one file per first letter

TruePx downloads these files at a fixed commit and checks each one against a checksum built into the extension.

## License

GPL-3.0, see [LICENSE](LICENSE). The files in `src/` were copied unchanged from an open-source project at a fixed version on 29 October 2026; this repository holds only those files.
