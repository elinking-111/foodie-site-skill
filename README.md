# foodie-site-skill

A Claude Code skill that turns your personal food, restaurant, and travel notes into a beautiful, filterable, map-enabled static website — then deploys it to GitHub Pages.

## What it does

Give it your notes in any format — Apple Notes, pasted text, social media posts (小红书, etc.) — and it generates a complete website with:

- Filterable card grid by category, region, and area
- Interactive Leaflet map with pins for every place
- Visited/want-to-go status toggles
- Star/recommend markers
- Full-text search
- Add/edit/delete places (saved in localStorage)
- Data export
- Fully responsive design

## Supported input formats

- **Checklists**: `- [x] Place Name (description)`
- **Simple lists**: `Name - description`
- **Sectioned notes**: Headers like `## Beijing Food` group items by region
- **Freeform text**: Mixed formats with emoji markers (🌟=favorite, ✅=visited)
- **Social media**: Numbered lists, 📍 locations, 💰 prices, #hashtags

Works in both Chinese and English — all UI text auto-localizes.

## How to use

### Install as a Claude Code skill

```bash
claude install-skill https://github.com/elinking-111/foodie-site-skill
```

Then just tell Claude something like:
- "帮我把这些笔记做成一个美食地图网站"
- "Turn my restaurant list into a website"

### Architecture

The skill generates only two small files (`data.js` + `config.json`), then runs `setup.py` to customize a bundled production template into `index.html`. This avoids timeouts from generating 1100+ lines of HTML from scratch.

```
foodie-site-skill/
├── SKILL.md              # Skill instructions
├── assets/template.html  # Production HTML template (1100+ lines)
├── scripts/setup.py      # Config → HTML customizer
└── evals/evals.json      # Test cases
```

## Examples

| Input | Output |
|-------|--------|
| Chinese checklist (17 places) | Filterable site with Beijing/China/Overseas regions |
| English list (12 NYC spots) | Fully English UI, map centered on Manhattan |
| Xiaohongshu post (10 Chengdu places) | Social media format parsed, prices in descriptions |

## License

MIT
