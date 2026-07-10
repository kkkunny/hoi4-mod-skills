# CLAUDE.md

This project provides a skill system for Hearts of Iron IV mod making. A master skill (`hoi4-mod`) gives the big picture and points to specialized sub-skills for each domain.

## Game Paths

- **Vanilla game**: `../hoi4/Hearts of Iron IV/` (relative to this project root)
- **Mod storage** (Linux): `~/.local/share/Paradox Interactive/Hearts of Iron IV/mod/`
- **Mod storage** (Windows): `C:\Users\<Username>\Documents\Paradox Interactive\Hearts of Iron IV\mod\`
- **Workshop mods**: `.../Paradox Interactive/Hearts of Iron IV/workshop/content/394360/`

## Project Structure

```
hoi4-mod-skills/
├── CLAUDE.md              # This file
├── docs/                  # Original reference docs (49 .md files from HOI4 wiki)
├── skills/                # Skill definitions (one directory per skill)
│   ├── hoi4-mod/          # Master skill + references/
│   │   ├── hoi4-mod.md
│   │   └── references/    # 49 unabridged reference files
│   ├── hoi4-script/       # effects + triggers + scopes (merged)
│   ├── hoi4-tech/         # tech + division + unit + equipment (merged)
│   ├── hoi4-gui/          # gfx + interface + scripted-gui (merged)
│   ├── hoi4-map/          # map + state (merged)
│   ├── hoi4-assets/       # localisation + gfx overview
│   ├── hoi4-country/      # Country creation (standalone)
│   ├── hoi4-focus/        # National focuses (standalone)
│   ├── hoi4-event/        # Events (standalone)
│   ├── hoi4-idea/         # Ideas (standalone)
│   ├── hoi4-decision/     # Decisions (standalone)
│   ├── hoi4-character/    # Characters (standalone)
│   ├── hoi4-ideology/     # Ideologies (standalone)
│   ├── hoi4-autonomy/     # Autonomy states (standalone)
│   ├── hoi4-bookmark/     # Bookmarks (standalone)
│   ├── hoi4-ai/           # AI configuration (standalone)
│   ├── hoi4-onaction/     # On actions (standalone)
│   ├── hoi4-modifier/     # Modifiers (standalone)
│   ├── hoi4-define/       # Defines (standalone)
│   └── hoi4-trouble/      # Troubleshooting (standalone)
└── README.md
```

## Skill Architecture

**20 skills total** — 1 master + 19 sub-skills (6 merged, 13 standalone).

### Master Skill

**`hoi4-mod`** — Entry point. Covers mod creation from scratch, full game directory structure (with pointers to sub-skills for each directory), modding workflow, and a complete reference document index. Contains `references/` with 49 unabridged HOI4 wiki documents.

### Merged Skills (6)

| Skill | Merged from | Covers |
|---|---|---|
| `hoi4-script` | effect + trigger + scope | Scripting language core — effects, conditions, scope navigation |
| `hoi4-tech` | tech + division + unit + equipment | Technologies, doctrines, OOB, division templates, sub-units, equipment, modules |
| `hoi4-gui` | gfx + interface + scripted-gui | Sprites, .gfx/.gui files, animations, scripted GUI, focus inlay windows |
| `hoi4-map` | map + state | Map editing (BMP formats, provinces), state history (population, buildings, VP) |
| `hoi4-assets` | localisation + brief gfx | Localisation (all languages, formatting, replace/), gfx directory overview, music/sound |
| `hoi4-mod` | mod-structure merged in | Mod structure, .mod files, dependencies, workflow — all now in master |

### Standalone Skills (13)

| Skill | Covers |
|---|---|
| `hoi4-country` | Country tags, flags, history files, colors, starting setup |
| `hoi4-focus` | National focus trees, shared/joint focuses, continuous focuses |
| `hoi4-event` | Country/news/state events, MTTH, event targets |
| `hoi4-idea` | National spirits, laws, designers, hidden ideas |
| `hoi4-decision` | Decisions, missions, categories, targeted decisions |
| `hoi4-character` | Country leaders, advisors, unit leaders, portraits, traits |
| `hoi4-ideology` | Ideology groups, sub-ideologies, party names |
| `hoi4-autonomy` | Autonomy states, puppet levels, subject naming |
| `hoi4-bookmark` | Start date bookmarks, country selection screen |
| `hoi4-ai` | AI strategy plans, ai_will_do, AI focuses, templates |
| `hoi4-onaction` | On-action hooks (startup, monthly, war, diplomatic, etc.) |
| `hoi4-modifier` | Static/dynamic modifiers, applying via ideas/traits |
| `hoi4-define` | Game constants via define overrides |
| `hoi4-trouble` | Error log analysis, common crashes, debugging techniques |

## Key Conventions

1. **Encoding**: All `.txt` files UTF-8 without BOM; localisation `.yml` files UTF-8 with BOM
2. **File naming**: The game loads files based on content, not filename; use new files instead of overwriting base game files for compatibility
3. **Replace vs Add**: To change vanilla content, copy the entire file and modify; to add new content, create a new file with a unique name
4. **Sprite system**: Images need a `spriteType` definition in `interface/*.gfx` files to be used; just placing images in `gfx/` is insufficient
5. **Error log**: `.../Documents/Paradox Interactive/Hearts of Iron IV/logs/error.log` is the primary debugging tool

## Reference Documentation

The master skill's `references/` directory contains 49 detailed reference documents. When the current skill's content is insufficient — for example, when an exhaustive parameter list or an obscure edge case is needed — read the relevant reference file directly. Each sub-skill's "References" section lists which file(s) to consult.
