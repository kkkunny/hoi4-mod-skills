---
name: hoi4-mod
description: Master skill for Hearts of Iron IV mod creation. Use this skill whenever the user wants to create a new HOI4 mod, understand mod structure and file layout, figure out which game files to modify, or learn the modding workflow. Also serves as an index of all specialized sub-skills — use this first, then follow [[double-bracket links]] to the relevant sub-skill for detailed implementation.
---

# HOI4 Modding — Master Skill

Entry point for creating HOI4 mods: mod setup, file layout, and routing to the specialized sub-skills and `references/`.

## Golden Rule

**Mods mirror the game's file structure: same path + same filename = override vanilla; a new filename = add content.** Minimize vanilla overrides for compatibility.

## Mod Setup

- Local mods: Linux `~/.local/share/Paradox Interactive/Hearts of Iron IV/mod/`; Windows `.../Documents/Paradox Interactive/Hearts of Iron IV/mod/`
- Each mod needs: a `.mod` file, the mod folder (or zip) with game data, and `descriptor.mod` inside the folder (same content as the `.mod` file)
- Optional `thumbnail.png` (1:1, <1MB for ParadoxMods). Launcher: *Mods → Mod Tools → Create Mod* auto-generates the structure. Case sensitivity matters on Mac/Linux.
- Submods: `dependencies = { "testmodA" }` in the `.mod` file — name must match the other mod's `name` exactly. Otherwise mods load alphabetically.
- Steam Workshop mods live under `.../workshop/content/394360/`.
- Full details (descriptors, launcher quirks, upload) → `references/Mod structure.md`.

Minimal `.mod` file:
```
name="Test1"
path="mod/test1"
tags={ "Alternative History" }
supported_version="1.17.*"
```

## Hard Rules

| File | Encoding |
|---|---|
| All `.txt` files | UTF-8 **without** BOM |
| `descriptor.mod` | UTF-8 **without** BOM |
| Localisation `.yml` | UTF-8 **with** BOM |

- Forward slashes `/` in paths, never backslashes
- Prefix IDs (focuses, ideas, events) with a tag or namespace to avoid duplicate-ID bugs
- Check `.../Hearts of Iron IV/logs/error.log` when anything breaks → [[hoi4-trouble]]
- Use `replace_path` in the `.mod` file only to unload whole vanilla folders (total overhauls)

## Where Content Lives

| Path | Holds | Skill |
|---|---|---|
| `common/country_tags/`, `common/countries/` | Country tags, colors, graphical culture | [[hoi4-country]] |
| `history/countries/` | Starting state per tag: politics, tech, OOB link | [[hoi4-country]] |
| `common/characters/`, `common/names/`, `portraits/`, `history/general/` | Characters, namelists, portrait pools, generic characters | [[hoi4-character]] |
| `common/ideas/`, `common/idea_tags/` | National spirits, laws, designers, advisors | [[hoi4-idea]] |
| `common/national_focus/`, `common/continuous_focus/` | Focus trees, shared/joint/continuous focuses | [[hoi4-focus]] |
| `events/` | Events (country, news, state, unit leader) | [[hoi4-event]] |
| `common/decisions/` | Decisions, missions, targeted decisions | [[hoi4-decision]] |
| `common/technologies/`, `common/technology_tags/` | Techs, doctrines, tech categories | [[hoi4-tech]] |
| `common/units/`, `common/units/equipment/` | Sub-units, equipment, modules, upgrades | [[hoi4-tech]] |
| `history/units/` | Orders of battle | [[hoi4-tech]] |
| `common/ideologies/` | Ideology groups and types | [[hoi4-ideology]] |
| `common/autonomous_states/` | Autonomy levels (puppet types) | [[hoi4-autonomy]] |
| `common/bookmarks/` | Start-date bookmarks | [[hoi4-bookmark]] |
| `common/modifiers/`, `common/dynamic_modifiers/`, `common/modifier_definitions/`, `common/opinion_modifiers/` | Modifier templates, definitions, opinion modifiers | [[hoi4-modifier]] |
| `common/scripted_effects/`, `common/scripted_triggers/` | Reusable effect/trigger macros | [[hoi4-script]] |
| `common/on_actions/` | Event hooks | [[hoi4-onaction]] |
| `common/defines/` | Game constants (override file only) | [[hoi4-define]] |
| `common/ai_strategy/`, `common/ai_strategy_plans/`, `common/ai_focuses/` | AI behavior, focus orders, plans | [[hoi4-ai]] |
| `common/scripted_guis/` | Scripted GUI definitions | [[hoi4-gui]] |
| `map/`, `history/states/` | Map bitmaps/definitions, state history | [[hoi4-map]] |
| `interface/` | GUI layouts (`.gui`) and sprite definitions (`.gfx`) | [[hoi4-gui]] |
| `gfx/` | Image assets — every image also needs a spriteType | [[hoi4-gui]], [[hoi4-assets]] |
| `localisation/`, `common/scripted_localisation/` | All text, per-language `.yml` files, dynamic localisation | [[hoi4-assets]] |
| `music/`, `sound/` | Music tracks and sound effects | [[hoi4-assets]] |

Topics with **no dedicated sub-skill** — read the reference directly:

| Topic | Reference |
|---|---|
| Balance of power | `references/Balance of power modding.md` |
| Buildings (definitions, beyond state history) | `references/Building modding.md` |
| Console commands | `references/Console commands.md` |
| Cosmetic tags | `references/Cosmetic tag modding.md` |
| Entities (3D models) | `references/Entity modding.md` |
| Factions | `references/Faction modding.md` |
| Military industrial organizations | `references/Military industrial organization modding.md` |
| Resources (definitions) | `references/Resources modding.md` |
| Strategic regions, supply areas | `references/Strategic region modding.md`, `references/Supply areas modding.md` |
| Particles, posteffects | `references/Particle modding.md`, `references/Posteffect modding.md` |
| Achievements | `references/Achievement modding.md` |
| Commander abilities, spy operations (`common/abilities/`, `common/operations/`) | No offline reference — use the online wiki |

## Workflow

1. Create the mod structure (launcher or manually)
2. Identify the vanilla files controlling the feature — read them in the game install (`../hoi4/Hearts of Iron IV/`)
3. Extend with new files, or override with a same-name copy
4. Test: enable the mod, launch with `-debug`, read `error.log`
5. Iterate — most failures are encoding, missing sprites, duplicate IDs, or missing localisation → [[hoi4-trouble]]

## References

`references/` (in this skill folder) is a full offline snapshot of the HOI4 modding wiki (49 files). Each sub-skill routes to its own files; for anything a sub-skill lacks, read the matching reference.

Prefer the offline snapshot. Fetch the online wiki only for content newer than the snapshot or topics not covered locally:

- Game wiki: https://hoi4.paradoxwikis.com/Hearts_of_Iron_4_Wiki — mirror: https://cooderatom.github.io/hoi4-zh-wiki/Hearts_of_Iron_4_Wiki
- Modding wiki: https://hoi4.paradoxwikis.com/Mod_structure — mirror: https://cooderatom.github.io/hoi4-zh-wiki/Mod_structure.html
- Mirror rule: replace `hoi4.paradoxwikis.com` with `cooderatom.github.io/hoi4-zh-wiki`; mirrored article URLs end with `.html`.
