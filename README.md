# foodie-site-skill

A reusable coding-agent skill for building and iteratively evolving foodie or travel review websites from notes, pasted text, files, or social posts.

This skill exists for a very practical reason: many people already have valuable food and travel notes, but those notes usually stay trapped in memo apps, chat logs, or scattered lists. This skill turns that raw material into a navigable website, then keeps supporting the project as it grows.

It supports both:

- generating a new site from source notes
- upgrading an existing live site without wiping custom product work

## At a glance

- source material: notes, pasted text, exported files, social-media lists
- output: a browsable foodie or travel website
- delivery: generated from a reusable template and deployable to GitHub Pages
- follow-up support: iteration, shared interactions, showcase pages, owner-managed editing

## Before / After

### Before

```text
- [x] Baker & Spice - brunch, good coffee
- [ ] A hidden ramen place in Jing'an
- Cafe by the river (great for reading)
- 成都：几家想去的火锅和小酒馆
```

### After

```text
A live website with:
- searchable place cards
- category and region filters
- map browsing
- visited / want-to-go states
- a structure that can keep evolving instead of being rebuilt from zero
```

## Why this skill exists

- most people already have the content, but not the structure
- building the first usable version should be fast
- later iterations should not destroy custom work already added to the site
- a public-facing site often needs both content generation and product refinement over time

## What this skill is for

Use this skill when you want to turn a list of restaurants, cafes, bars, bakeries, destinations, or places of interest into a browsable website with:

- searchable cards
- category, region, and area filters
- map-based exploration
- visited / want-to-go states
- lightweight recommendation signals
- GitHub Pages deployment

It also covers common follow-up work on an existing site, such as:

- title and copy changes
- visual refinement
- map and filter improvements
- showcase / case-study pages
- shared interactions like comments or likes
- owner-managed editing of live entries

## Workflow modes

### 1. Greenfield build

For turning raw notes into a new site:

1. collect the notes
2. parse them into `data.js`
3. write `config.json`
4. generate `index.html` from the bundled template
5. deploy to GitHub Pages

### 2. Iterative upgrade

For improving an existing foodie or travel site:

1. inspect the current page and template
2. preserve existing visual and product logic
3. decide whether the change belongs in static content, browser state, or shared backend state
4. patch the live page and template together when needed
5. verify the latest deploy before closing out

## Quick workflow

```text
notes / pasted content
        ->
structured place data
        ->
site config
        ->
template-based site generation
        ->
GitHub Pages deploy
        ->
later upgrades, shared features, and showcase pages
```

## Input the skill can handle

- checklist notes like `- [x]`
- simple place lists
- sectioned notes grouped by city or theme
- messy freeform notes with emoji markers
- copied social-media food lists with `📍`, hashtags, and price hints

Chinese and English content are both supported.

## How it works

The skill is built around a template workflow instead of generating a full site from scratch each time.

- `data.js` stores the parsed places
- `config.json` defines site-level settings
- `scripts/setup.py` injects config into `assets/template.html`
- the result is a production-ready `index.html`

This keeps generation stable while still allowing iterative product changes on top.

## Install

```bash
claude install-skill https://github.com/elinking-111/foodie-site-skill
```

Then prompt your coding agent with requests like:

- `帮我把这份美食笔记做成一个网站`
- `Turn my cafe list into a GitHub Pages site`
- `Keep this existing foodie site, but add shared likes`
- `Make a showcase page explaining how this site was built`

## Typical use cases

### 1. Notes to site

You have a messy list of restaurants and cafes in Apple Notes, pasted text, or exported files and want a clean website without manually writing HTML.

### 2. Existing site upgrade

You already have a foodie or travel site and want to improve filters, cards, layout, copy, or interaction design without rebuilding from scratch.

### 3. Public showcase page

You want a separate page that explains how the site was made, what prompts were used, or how the product evolved over time.

## Prompt examples

### Build from notes

```text
Turn this restaurant note into a GitHub Pages site. Keep the content in Chinese, infer categories and regions from the note, generate the data and config files, build the site from the template, and deploy it.
```

### Upgrade an existing site

```text
Inspect this existing foodie site first. Keep the current design direction, but improve the filter hierarchy, make the interaction buttons clearer, and preserve any custom logic already in the live page.
```

### Add shared interaction

```text
Add a lightweight shared like feature to this site. Use a shared backend so all visitors see the same count, keep the implementation minimal, and do not require a full comment system.
```

### Create a showcase page

```text
Create a separate showcase page for this project that explains the workflow from raw notes to live site, summarizes the key prompts used, and presents the build process in a way that is easy for non-technical users to understand.
```

## Repo layout

```text
foodie-site-skill/
├── SKILL.md
├── README.md
├── assets/template.html
├── scripts/setup.py
└── evals/evals.json
```

## Design boundary

This repo is intentionally kept reusable. Project-specific modules such as personal community entry points, custom QR content, or one-off auth setup should stay in the consuming project rather than in the shared skill itself.

## License

MIT
