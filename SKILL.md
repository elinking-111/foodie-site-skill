---
name: foodie-site
description: "Build or iteratively evolve a foodie/travel review website from notes and deploy it to GitHub Pages. Use this skill both for greenfield generation from Apple Notes, pasted text, files, or social posts and for ongoing upgrades to an existing site: visual refinement, map/filter changes, showcase pages, shared interactions, and owner-managed content editing."
---

# Foodie Site Generator

Turn a user's collection of restaurants, cafes, bars, bakeries, travel destinations, and places of interest into a filterable, map-enabled website — then keep evolving it as the site grows.

## Overview

This skill supports 2 modes:

1. **Greenfield mode**: build a new site from notes or pasted lists
2. **Iteration mode**: improve an existing foodie/travel site without wiping out its current product work

The core principle is:

- In greenfield mode, do not hand-write a brand-new app if a working template exists.
- In iteration mode, do not blindly regenerate the whole page if the live site already contains custom logic.

If the repo includes both `assets/template.html` and `index.html`, treat the template as the mirror of the live page and keep them aligned when making product-layer changes.

## Live Reference

For a real deployed example of the kind of site this skill can produce, see:

- https://foodie.govibepark.com/

## Workflow A: Greenfield Build

Use this when the user wants to turn notes, lists, or social-media food content into a new site.

1. **Collect** — get the source notes
2. **Parse** — convert notes into `data.js`
3. **Configure** — write `config.json`
4. **Build** — run the setup script to produce `index.html`
5. **Deploy** — push to GitHub Pages

## Workflow B: Iterative Upgrade

Use this when the user already has a site and wants to keep evolving it.

Common iteration requests:

- lighter or calmer visual design
- homepage title and wording changes
- map, filter, and hierarchy refinement
- showcase or case-study pages
- shared comments or likes
- owner-managed editing of live restaurant data

Recommended iteration order:

1. Inspect the current `index.html`, `assets/template.html`, and related JS/CSS before deciding on an approach.
2. Preserve the site's existing visual language unless the user explicitly wants a redesign.
3. Decide whether the feature belongs in static content, local browser state, or a shared backend.
4. If the repo has a template mirror, patch both the live page and template together.
5. After changes, verify the latest GitHub Pages build before telling the user the update is live.

## Step 1: Collect the Notes

Support these input modes:

**Paste text**: user pastes notes directly.

**File path**: user gives a local path. Read it.

**Apple Notes (macOS)**:
```bash
osascript -e 'tell application "Notes" to set n to body of note "NOTE_NAME"' | python3 -c "import sys,re; print(re.sub('<[^>]+>','',sys.stdin.read()))"
```
If it fails, ask the user to paste the note manually.

**Social-media content**: copied text from 小红书 / Xiaohongshu, Yelp, TripAdvisor, WeChat articles, or similar sources. Treat these as pasted text; do not scrape.

## Step 2: Parse Notes into `data.js`

User notes can be messy. Be flexible.

### Input formats to handle

- checklist: `- [x]` and `- [ ]`
- simple list: `Name - description`
- sectioned lists: `## 北京美食`, `COFFEE:`
- freeform notes with emoji markers
- social-media lists with numbered items, `📍` locations, hashtags, and price hints

### Parsing rules

For each line that looks like a place:

1. **Visited**: `- [x]`, `✅`, `(visited)`, `been there` → `visited: true`
2. **Star**: `🌟`, `⭐`, `★`, `必吃`, `必去`, `推荐` → `star: true`
3. **Name**: primary text before parenthetical or dash. Split Chinese + English when useful.
4. **Desc**: text after parentheses, `-`, or `—`
5. **Category** by keywords:
   - `coffee`: 咖啡, coffee, cafe, latte
   - `restaurant`: default for most food entries
   - `bar`: 酒吧, cocktail, wine, speakeasy, 精酿
   - `bakery`: 面包, 蛋糕, pastry, bakery, bagel
   - `culture`: 美术馆, 书店, 展览, museum, temple
   - `explore`: 山, 湖, park, hiking, camping
   - `leisure`: live music, comedy, 温泉, leisure
6. **Region**: infer from section headers or location context
7. **Area**: infer from district, neighborhood, `📍`, or hashtags
8. **Tags**: collect useful hashtags or recurring descriptors

### Output format

Write `data.js` like this:

```javascript
const DATA = [
  {"name":"Store Name","nameEn":"","cat":"coffee","region":"北京","area":"鼓楼/南锣","desc":"描述","star":false,"visited":true,"tags":[]}
];
```

## Step 3: Write `config.json`

Analyze the parsed data and write a config file that adapts the template to the dataset.

```json
{
  "siteTitle": "小王の探店日记",
  "siteSubtitle": "吃喝玩乐指南",
  "siteBadge": "小王の探店日记",
  "heroSub": "Eat · Drink · Explore · Travel",
  "lang": "zh-CN",
  "searchPlaceholder": "搜索店名、区域、关键词...",
  "categories": [
    {"key":"all","label":"全部","emoji":""},
    {"key":"coffee","label":"咖啡","emoji":"☕"},
    {"key":"restaurant","label":"餐厅","emoji":"🍜"}
  ],
  "regions": [
    {"key":"all","label":"全部地区","emoji":"🌏"},
    {"key":"北京","label":"北京","emoji":"🏙️"}
  ],
  "stats": [
    {"id":"statTotal","label":"TOTAL"},
    {"id":"statBeijing","label":"BEIJING","filter":"d.region==='北京'"}
  ],
  "areaCoords": {
    "鼓楼/南锣": [39.9397, 116.3972]
  },
  "regionCenters": {
    "北京": [39.9042, 116.4074]
  },
  "districtMap": {},
  "mapJumps": [
    {"key":"beijing","label":"北京","center":[39.9042,116.4074],"zoom":12}
  ],
  "statusLabels": {"all":"全部","visited":"已去过","unvisited":"想去"}
}
```

Key rules:

- only include categories actually used in the data, plus `all`
- only include regions actually present, plus `all`
- map every unique `area` to approximate coordinates
- define region centers for fallback map behavior
- create useful stat counters based on the geography of the data
- keep `districtMap` empty unless the user explicitly needs hierarchical sub-area filters
- set `lang` based on content language; let the setup script localize UI text

## Step 4: Build `index.html`

Run the setup script:

```bash
python3 <skill-path>/scripts/setup.py config.json <skill-path>/assets/template.html index.html
```

This injects the config into the production-quality template.

In greenfield mode:

- do not hand-write `index.html`
- use the template workflow

In iteration mode:

- if the current live page already has substantial custom logic, patch that page directly
- do not destroy product-layer changes by regenerating from scratch unless the user explicitly wants a reset

## Shared Interaction Layer

When the user wants shared interactions rather than browser-local state, use a backend such as Firebase or another hosted data layer.

Good fits for a shared backend:

- shared comments
- shared likes or lightweight reactions
- owner-managed edits that should be visible to all visitors

Guidelines:

- lightweight reactions can be anonymous if abuse risk is low
- comments may require authenticated users when identity matters
- owner editing must be backed by shared storage, not just local modal state
- if backend rules or permissions change, they must be deployed separately from page code

If a CLI is unavailable, use the package via `npx` when possible.

## Owner Editing

If the user wants to fix a restaurant classification, rename a venue, or delete a bad entry for all visitors, treat that as a shared content-editing request, not a local browser edit.

A valid owner-editing flow should mean:

- only an allowed owner identity can see edit controls
- edits write to shared storage
- all visitors see the updated result

Do not confuse local form editing with a true shared CMS layer.

## Showcase Pages

Use a separate public page when the user wants to explain:

- how the site was built
- prompts or workflows used during the project
- product evolution over time
- before/after capability demos

Keep showcase content separate from the main discovery page unless the user explicitly wants cross-linking from the hero.

## Step 5: Deploy to GitHub Pages

1. Check GitHub auth: `gh auth status`
2. Initialize git if needed
3. Create a public repo if needed:
   - `gh repo create <name> --public --source=. --push`
4. Enable Pages:
   - `gh api repos/<owner>/<repo>/pages -X POST -f "build_type=legacy" -f "source[branch]=main" -f "source[path]=/"`
5. Add custom domain via `CNAME` if needed
6. Push changes
7. Verify the latest Pages build:
   - `gh api repos/<owner>/<repo>/pages/builds --jq '.[0] | {status, error: .error.message, commit}'`
8. Only tell the user the update is live after the latest build reports `built`

## Content Updates

For content-only updates:

- re-parse notes
- regenerate `data.js`
- commit and push

For live-site product updates:

- inspect whether the request affects content, UI, or shared state
- preserve existing custom code
- keep template and live page aligned if both exist
- verify the deploy before closing out

## Important

- Always ask for the site title first.
- `data.js` and `index.html` must live together.
- Local browser state such as visited toggles can remain in localStorage.
- Shared collaborative features should use a backend source of truth.
- If `gh` is unavailable, give manual GitHub instructions.
- Default to the simplest production path that preserves the user's current site rather than replacing it.
