---
name: hoi4-assets
description: Manage text, graphics directories, music, and sound assets in HOI4 mods. Covers localisation (text), gfx directory structure overview, and audio files.
---

# HOI4 Assets: Localisation, Graphics & Audio

This skill covers managing game assets — text (localisation), the gfx directory structure, music, and sound. For detailed sprite and GUI modding, see [[hoi4-gui]].

---

## Part 1: Localisation (Text)

This section covers adding and modifying text in Hearts of Iron IV through the localisation system.

## Quick Checklist

1. Create a `.yml` file in `<mod>/localisation/english/` for English
2. Filename must contain the language code: `filename_l_english.yml` (lowercase L, not uppercase i)
3. **File must be UTF-8 with BOM** encoding
4. First line must be `l_english:` (with colon)
5. Each entry: `localisation_key:0 "Text value"`

## Supported Languages

| Language | Code | Folder |
|---|---|---|
| English | `l_english` | `localisation/english/` |
| French | `l_french` | `localisation/french/` |
| German | `l_german` | `localisation/german/` |
| Spanish | `l_spanish` | `localisation/spanish/` |
| Brazilian Portuguese | `l_braz_por` | `localisation/braz_por/` |
| Polish | `l_polish` | `localisation/polish/` |
| Russian | `l_russian` | `localisation/russian/` |
| Japanese | `l_japanese` | `localisation/japanese/` |
| Simplified Chinese | `l_simp_chinese` | `localisation/simp_chinese/` |
| Korean | `l_korean` | `localisation/korean/` |

## Basic File Format

```
l_english:
 my_key: "Localised text here"
 my_key_desc: "Description text"
```

### Rules
- **UTF-8 with BOM** is mandatory
- Localisation keys: ASCII only (English letters, underscores, dots, hyphens, numbers)
- Version number (`:0`) is optional and ignored by the game
- Values must be enclosed in straight double quotes `"`
- Values must be on a single line
- Special characters or spaces in keys break the rest of the file
- Multiple language databases can be in one file (filename must contain all language codes)

## Key Types by Game Object

| Object | Key Format | Description Key Format |
|---|---|---|
| Country name | `TAG` | `TAG_DEF`, `TAG_ADJ` |
| Country ideology name | `TAG_ideology` | `TAG_ideology_DEF` |
| Party name | `TAG_ideology_party` | `TAG_ideology_party_long` |
| Focus name/desc | `focus_id` | `focus_id_desc` |
| Event title/desc | `event_id.t` / `event_id.d` | N/A |
| Event option | `event_id.a` | N/A |
| Idea name/desc | `idea_id` | `idea_id_desc` |
| Decision name/desc | `decision_id` | `decision_id_desc` |
| State name | `STATE_123` | N/A |
| Victory point | `VICTORY_POINTS_1234` | N/A |
| Character name/desc | `character_id` | `character_id_desc` |
| Equipment name | `equipment_type` | `equipment_type_short` |
| Country-specific equipment | `TAG_equipment_type` | `TAG_equipment_type_short` |

## Formatting Codes

### Colors
```
§RRed text§! §GGreen text§! §BBlue text§! §YYellow text§!
§WWhite text§! §DDark text§! §GGreen text§! §bBlack text§!
§gGrey text§! §MMagenta text§! §CCyan text§! §OLight orange§!
§HHighlight (yellow)§!
```

### Other Formatting
```
\n    Newline
\"    Quotation mark inside text
§%    Start of text region
§!    End of formatting
```

## Dynamic Values

```
"Political Power: £pol_power£"
"£political_power_gain£ Political Power per day"
```

Square brackets for commands:
```
"[Root.GetName]"           — Country name
"[From.GetNameDef]"        — Country name with "the"
"[Root.GetRulingIdeology]" — Ruling ideology name
```

## Replacing Vanilla Text (The replace/ Folder)

To override specific vanilla localisation without copying entire files, create:
```
<mod>/localisation/english/replace/mod_replace_l_english.yml
```

```
l_english:
 POLITICS_ADD_POLITICAL_POWER: "New PP: $VAL|=+0$."
```

Files in `replace/` folder take priority. This avoids needing to copy and maintain entire base game files.

## Duplicate Keys

Duplicates across files are logged in `.../Hearts of Iron IV/logs/text.log`. The chosen value isn't consistently predictable — avoid duplicates.

## Country Name Priority Chain

```
SCO_IRE_democratic_autonomy_integrated_puppet  # Most specific
  ↓
SCO_autonomy_dominion                           # Autonomy type
  ↓
SCO_IRE_subject                                 # Subject of specific country
  ↓
SCO_subject                                     # Any subject
  ↓
SCO_liberalism                                  # Sub-ideology type
  ↓
SCO_democratic                                  # Ideology group
  ↓
SCO                                             # Generic name
  ↓
COUNTRY_SCO_autonomy_dominion                   # Generic with specific
```

## Subject Name Variables

Available in country names:
- `$NONIDEOLOGY$`, `$NONIDEOLOGYADJ$`, `$NONIDEOLOGYDEF$`
- `$OVERLORD$`, `$OVERLORDADJ$`, `$OVERLORDNAMEDEF$`

## Scripted Localisation

Location: `<mod>/common/scripted_localisation/*.txt`

```
defined_text = {
    name = my_text
    text = {
        trigger = { has_government = democratic }
        localization_key = loc_demo
    }
    text = {
        trigger = { has_government = fascism }
        localization_key = loc_fasc
    }
    text = {
        localization_key = loc_default     # Always true, fallback
    }
}
```

Usage: `[ROOT.my_text]`

## Common Pitfalls

- **Wrong encoding** is the #1 cause of broken localisation files
- **Missing first line** (`l_english:`): all entries get assigned to no database
- **Localisation keys with special chars or spaces**: breaks the rest of the file
- **Multi-line values**: must use `\n` instead; actual newlines break the file
- **Filename doesn't contain `_l_english`**: file won't be loaded
- **Missing Unicode characters in fonts**: characters appear as `?` if the font lacks them

## References

This skill covers the essentials. For the complete, unabridged reference with exhaustive details, read the following file(s) under the master skill's references directory:

- `skills/hoi4-mod/references/Localisation.md` — Full reference for YAML format, encoding, color codes, and supported languages

## gfx/ Directory Overview

For graphical assets, the `gfx/` directory mirrors the game's directory structure and contains:

| Path | Content |
|---|---|
| `gfx/flags/` | Country flags in TGA format (82x52, 41x26, 10x7) |
| `gfx/interface/` | UI icons, focus icons, idea pictures, event pictures |
| `gfx/leaders/` | Character/leader portraits |
| `gfx/event_pictures/` | Event background images |
| `gfx/fonts/` | Bitmap font files |

Images alone are not enough — each needs a `spriteType` defined in `interface/*.gfx`. For detailed sprite and GUI modding, see [[hoi4-gui]].

For music and sound:
- `music/` — Music files referenced by `music/` folder or modded via `Music modding.md`
- `sound/` — Sound effects, modded via `Sound modding.md`
