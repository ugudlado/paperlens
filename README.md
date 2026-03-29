# PaperLens

Research papers, made understandable. Community-driven plain-language breakdowns with inline jargon definitions, before/after impact, tiered Q&A, and curated references.

**Live site**: https://ugudlado.github.io/paperlens/

## Adding a Paper

1. Create `papers/{slug}.json` following the schema in `papers/attention-is-all-you-need.json`
2. Add the slug to `papers/index.json`
3. Open a Pull Request

Paper slugs must be lowercase and hyphen-separated: `my-paper-title`.

## Development

```bash
pnpm install
pnpm dev   # https://paperlens.localhost
```

No build step. Edit files, refresh browser.

## Stack

- Static HTML/CSS/JS — no framework, no build tool
- Hosted on GitHub Pages
- Papers stored as JSON files in `papers/`
