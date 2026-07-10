---
name: hoi4-mod
description: Master skill for Hearts of Iron IV mod creation. Covers mod structure, game directory layout, and modding workflow. Also serves as an index to specialized sub-skills.
---

# Hearts of Iron IV Modding - Master Skill

This skill provides a comprehensive overview of creating mods for Hearts of Iron IV. For detailed implementation on specific topics, refer to the corresponding sub-skill.

## The Golden Rule

**Mods use the same file and folder structure as the game itself.** Whatever exists in the base game can be overridden by placing a file with the same name at the same relative path in your mod. New content uses new filenames.

## Mod File Structure

### Local Mod

Local mods live in:
- **Linux**: `~/.local/share/Paradox Interactive/Hearts of Iron IV/mod/`
- **Windows**: `C:\Users\<Username>\Documents\Paradox Interactive\Hearts of Iron IV\mod\`

Each mod requires:
1. A **`.mod` file** (any name) — mod metadata for the launcher
2. A **mod folder** (or zip) with game data in the same structure as the game
3. A **`descriptor.mod`** inside the mod folder (UTF-8 without BOM, same content as .mod)
4. A **`thumbnail.png`** (optional, 1:1 ratio, <1MB for ParadoxMods)

Use the launcher's *Mods* -> *Mod Tools* -> *Create Mod* to auto-generate the structure. **Case sensitivity matters** on Mac/Linux.

### The `.mod` File

```
name="Test1"
path="mod/test1"
tags={ "Alternative History" }
picture="thumbnail.png"
supported_version="1.17.*"
version="1.0b"
```

### Submods and Dependencies

```
name="testmodB"
path="mod/testmodb/"
dependencies={ "testmodA" }
supported_version="1.17.*"
```
Mods load alphabetically by default. Dependencies enforce ordering. The dependency name must match the other mod's `name` field exactly.

### Steam Workshop Mods

Stored at `.../workshop/content/394360/`. Use `remote_file_id` in the `.mod` file alongside `archive` path.

### Key Rules

- All `.txt` files: **UTF-8 without BOM**
- Localisation `.yml` files: **UTF-8 with BOM**
- `descriptor.mod`: **UTF-8 without BOM**
- To overwrite vanilla: use the **same filename** at the same path
- To add content: use a **different filename**
- Minimize vanilla file overwrites for compatibility
- Check `.../Documents/Paradox Interactive/Hearts of Iron IV/logs/error.log` when things break

## Game Directory Structure

### `common/` — Game Data Definitions

| Path | Purpose | Sub-skill |
|---|---|---|
| `common/country_tags/` | 3-letter country tag registrations | [[hoi4-country]] |
| `common/countries/` | Country colors, graphical culture | [[hoi4-country]] |
| `common/characters/` | Character definitions (leaders, advisors) | [[hoi4-character]] |
| `common/ideas/` | National spirits, laws, designers | [[hoi4-idea]] |
| `common/idea_tags/` | Idea categories | [[hoi4-idea]] |
| `common/national_focus/` | Focus trees and shared focuses | [[hoi4-focus]] |
| `common/continuous_focus/` | Continuous focus palettes | [[hoi4-focus]] |
| `common/decisions/` | Decisions and categories | [[hoi4-decision]] |
| `common/technologies/` | Technologies and doctrines | [[hoi4-tech]] |
| `common/technology_tags/` | Technology categories/folders | [[hoi4-tech]] |
| `common/units/` | Sub-units, equipment types | [[hoi4-tech]] |
| `common/units/equipment/` | Equipment definitions | [[hoi4-tech]] |
| `common/bookmarks/` | Start date definitions | [[hoi4-bookmark]] |
| `common/ideologies/` | Ideology groups and types | [[hoi4-ideology]] |
| `common/autonomous_states/` | Autonomy levels (puppet types) | [[hoi4-autonomy]] |
| `common/names/` | Character name lists per country | [[hoi4-character]] |
| `common/modifiers/` | Static/dynamic modifier templates | [[hoi4-modifier]] |
| `common/modifier_definitions/` | Modifier UI definitions | [[hoi4-modifier]] |
| `common/scripted_effects/` | Reusable effect macros | [[hoi4-script]] |
| `common/scripted_triggers/` | Reusable trigger macros | [[hoi4-script]] |
| `common/scripted_guis/` | Scripted GUI definitions | [[hoi4-gui]] |
| `common/scripted_localisation/` | Dynamic localization | [[hoi4-assets]] |
| `common/on_actions/` | Game event hooks | [[hoi4-onaction]] |
| `common/defines/` | Game constants (use override file!) | [[hoi4-define]] |
| `common/ai_strategy/` | AI behavior definitions | [[hoi4-ai]] |
| `common/ai_strategy_plans/` | AI strategy plans (focus order) | [[hoi4-ai]] |
| `common/ai_focuses/` | AI focus selection preferences | [[hoi4-ai]] |
| `common/dynamic_modifiers/` | Dynamic modifier definitions | [[hoi4-modifier]] |
| `common/opinion_modifiers/` | Diplomatic opinion modifiers | [[hoi4-modifier]] |
| `common/bop/` | Balance of power definitions | (see references) |
| `common/military_industrial_organization/` | MIO definitions | (see references) |
| `common/resources/` | Resource definitions | (see references) |
| `common/abilities/` | Commander abilities | (see references) |
| `common/operations/` | Spy operations | (see references) |

### `history/` — Starting Game State

| Path | Purpose | Sub-skill |
|---|---|---|
| `history/countries/` | Starting data per country tag | [[hoi4-country]] |
| `history/states/` | State ownership, population, buildings | [[hoi4-map]] |
| `history/units/` | Order of battle (starting military) | [[hoi4-tech]] |
| `history/general/` | Generic character generation effects | [[hoi4-character]] |

### Other Top-Level Directories

| Path | Purpose | Sub-skill |
|---|---|---|
| `events/` | Event definitions | [[hoi4-event]] |
| `map/` | Map data (provinces, terrain, rivers) | [[hoi4-map]] |
| `gfx/` | Graphical assets (flags, icons, portraits) | [[hoi4-gui]] (sprites) / [[hoi4-assets]] (overview) |
| `gfx/flags/` | Country flags (TGA, 82x52 / 41x26 / 10x7) | [[hoi4-country]] |
| `interface/` | UI definitions (.gui and .gfx files) | [[hoi4-gui]] |
| `localisation/` | Text in all supported languages | [[hoi4-assets]] |
| `music/` | Music files | [[hoi4-assets]] |
| `sound/` | Sound effects | [[hoi4-assets]] |
| `portraits/` | Portrait pools for randomly generated leaders | [[hoi4-character]] |

## Modding Workflow

### 1. Create the Mod Structure
Use the launcher or manually create the `.mod` file and folder.

### 2. Decide What to Change
Identify which game files control the feature. Look at base game files: `../hoi4/Hearts of Iron IV/`

### 3. Create Files
- To **extend**: new files with unique names
- To **override**: copy the vanilla file and modify

### 4. Test
Enable the mod, check `error.log`, use debug mode (`-debug`).

### 5. Iterate
Fix errors — common pitfalls: wrong encoding, missing sprite definitions, duplicate IDs, missing localisation.

## Choosing the Right Sub-Skill

- **Create a new country** -> [[hoi4-country]]
- **Create/modify a focus tree** -> [[hoi4-focus]]
- **Add events** -> [[hoi4-event]]
- **Add decisions/missions** -> [[hoi4-decision]]
- **Add national spirits/ideas/laws** -> [[hoi4-idea]]
- **Write effects / triggers / scopes** -> [[hoi4-script]]
- **Apply modifiers** -> [[hoi4-modifier]]
- **Modify game constants** -> [[hoi4-define]]
- **Add localisation/text** -> [[hoi4-assets]]
- **Create characters (leaders/advisors)** -> [[hoi4-character]]
- **Add technologies / OOB / divisions / equipment** -> [[hoi4-tech]]
- **Add ideologies** -> [[hoi4-ideology]]
- **Mod autonomy/puppet levels** -> [[hoi4-autonomy]]
- **Mod the UI / sprites / scripted GUI** -> [[hoi4-gui]]
- **Edit the map / states** -> [[hoi4-map]]
- **Add bookmarks** -> [[hoi4-bookmark]]
- **Add on_actions** -> [[hoi4-onaction]]
- **Configure AI behavior** -> [[hoi4-ai]]
- **Debug errors** -> [[hoi4-trouble]]

## Key Conventions

1. **UTF-8 without BOM** for all `.txt` files
2. **UTF-8 with BOM** for all localisation `.yml` files
3. **Use forward slashes** `/` in file paths, not backslashes
4. **Prefix focus/idea IDs** with country tag to avoid duplicates
5. **Create new files** instead of overwriting vanilla wherever possible
6. **Check error.log** when something doesn't work
7. **Use `replace_path`** sparingly to unload specific vanilla folders

## Reference Documentation

This skill includes a **`references/`** directory with 49 detailed reference documents from the HOI4 modding wiki. These contain the full, unabridged specifications — each sub-skill covers the essentials, but reference files have exhaustive parameter lists, edge cases, and advanced usage.

**When you need more detail than a sub-skill provides, read the corresponding reference file directly.**

| Topic | Reference file(s) |
|---|---|
| Mod structure / setup | `references/Mod structure.md` |
| Country creation | `references/Country creation.md` |
| National focuses | `references/National focus modding.md` |
| Events | `references/Event modding.md` |
| Ideas | `references/Idea modding.md` |
| Decisions | `references/Decision modding.md` |
| Effects, triggers, scopes | `references/Effect.md`, `references/Triggers.md`, `references/Scopes.md` |
| Data structures | `references/Data structures.md` |
| Modifiers (full list) | `references/Modifiers.md`, `references/List of modifiers.md` |
| Localisation | `references/Localisation.md` |
| Characters | `references/Character modding.md` |
| States | `references/State modding.md` |
| Map modding | `references/Map modding.md` |
| Map nudger tool | `references/Nudger.md` |
| Strategic regions | `references/Strategic region modding.md` |
| Supply areas | `references/Supply areas modding.md` |
| Division / OOB | `references/Division modding.md` |
| Technology | `references/Technology modding.md` |
| Units / equipment | `references/Unit modding.md`, `references/Equipment modding.md` |
| Ideologies | `references/Ideology modding.md` |
| Graphics / sprites | `references/Graphical asset modding.md` |
| Portraits | `references/Portrait modding.md` |
| Interface / GUI | `references/Interface modding.md` |
| Scripted GUI | `references/Scripted GUI modding.md` |
| Bookmarks | `references/Bookmark modding.md` |
| On actions | `references/On actions.md` |
| Autonomy states | `references/Autonomy state modding.md` |
| Defines | `references/Defines.md` |
| AI modding | `references/AI focuses.md` |
| Troubleshooting | `references/Troubleshooting.md` |
| Balance of power | `references/Balance of power modding.md` |
| Factions | `references/Faction modding.md` |
| Buildings | `references/Building modding.md` |
| Namelists | `references/Namelist modding.md` |
| Music / Sound | `references/Music modding.md`, `references/Sound modding.md` |
| Console commands | `references/Console commands.md` |
| MIO | `references/Military industrial organization modding.md` |
| Cosmetic tags | `references/Cosmetic tag modding.md` |
| Resources | `references/Resources modding.md` |
| Fonts | `references/Font modding.md` |
| Particles / Posteffects | `references/Particle modding.md`, `references/Posteffect modding.md` |
| Entities (3D models) | `references/Entity modding.md` |
| Achievements | `references/Achievement modding.md` |
