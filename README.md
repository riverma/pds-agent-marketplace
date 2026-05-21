# PDS Agent Marketplace

An AI-native marketplace of **skills and workflow automation** for the
[NASA Planetary Data System (PDS)](https://pds.nasa.gov/) — discoverable on a
website and installable directly into AI coding tools such as Claude Code.

The catalog packages PDS engineering workflows (release notes, issue and
pull-request creation, dependency and security triage) as AI **skills** that
install and run in seconds.

> Built on the open-source [slim-framework](https://github.com/NASA-AMMOS/slim-framework).
> Skills are sourced from [NASA-PDS/pds-agent-skills](https://github.com/NASA-PDS/pds-agent-skills).

## Architecture

This repository follows a **single source of truth** philosophy:

- **`static/data/registry.json`** — the hand-authored source of truth. It holds
  the marketplace identity and every skill entry.
- **`.claude-plugin/marketplace.json`** — **generated** from `registry.json` by
  `src/conf/generate-marketplace.js`. Claude Code reads this when a user runs
  `/plugin marketplace add`. It is committed so consumers don't need to build.
- **`static/marketplace/skills/`** — the content of each skill (`SKILL.md`,
  scripts, assets). One folder per skill.
- The Docusaurus website renders the registry into a searchable catalog.

```
registry.json  ──(npm run prebuild)──▶  marketplace.json
   (you edit)                              (generated, committed)
```

## Getting started

```bash
npm ci          # install dependencies
npm start       # local dev server
npm run build   # production build (runs the generators first)
npm run serve   # preview the production build
```

## Adding a skill

The marketplace is **registry-driven** — you hand-author `registry.json` and the
build generates everything else. To add a skill:

1. **Create the skill folder** under `static/marketplace/skills/<skill-name>/`
   with a `SKILL.md` (and any `scripts/` or `assets/`). `SKILL.md` needs YAML
   frontmatter:
   ```markdown
   ---
   name: <skill-name>
   description: What the skill does and when to use it.
   ---
   ```
2. **Add an entry** to the `skills` array in `static/data/registry.json`:
   ```json
   {
     "name": "<skill-name>",
     "displayName": "Human Readable Name",
     "description": "What it does and when to use it",
     "category": "development-workflow",
     "tags": ["github", "automation", "pds"],
     "example": "An example request a user might make",
     "lastUpdated": "2026-05-21"
   }
   ```
   You author only these fields. The build derives `type`, `skill_file_url`,
   and `zip_file_path` — do not write them by hand.
3. **Regenerate** the manifest: `npm run prebuild`.
4. **Commit** both `registry.json` and `.claude-plugin/marketplace.json`.

## Modifying a skill

- **Change skill behavior** — edit the files under
  `static/marketplace/skills/<skill-name>/`.
- **Change how it appears in the catalog** (name, description, tags, category,
  example) — edit that skill's entry in `static/data/registry.json`, then run
  `npm run prebuild`.
- **Remove a skill** — delete its folder and its `registry.json` entry, then run
  `npm run prebuild`.

Categories are defined in `registry.json` under `metadata.categoryIcons`; add a
new `"<category>": "<emoji>"` pair to introduce one.

## Customizing the site

Branding lives in `docusaurus.config.js` (under the `CUSTOMIZE ME` banner): the
title, tagline, URLs, organization, navbar/footer links, and the hero
`brandingConfig`. The logo is `static/img/logo.svg`; colors are in
`src/css/custom.css`.

## License

Apache 2.0 — see [LICENSE](LICENSE).

## Credits

- Skills adapted from [NASA-PDS/pds-agent-skills](https://github.com/NASA-PDS/pds-agent-skills) (Apache 2.0).
- Built on the [slim-framework](https://github.com/NASA-AMMOS/slim-framework) (Apache 2.0).
