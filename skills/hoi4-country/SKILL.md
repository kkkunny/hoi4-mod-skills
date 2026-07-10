---
name: hoi4-country
description: Create or modify countries in HOI4 mods. Use this skill whenever the user wants to add a new nation, set up a country's starting territory, politics, military, or technology, assign a country to the map, or create country flags and colors.
---

# HOI4 Country Creation

This skill covers everything needed to create or modify a country in Hearts of Iron IV.

## Quick Checklist

1. Define a **3-character country tag** in `<mod>/common/country_tags/*.txt`
2. Create a **country file** in `<mod>/common/countries/`
3. Define characters in `<mod>/common/characters/`
4. Create a **history file** in `<mod>/history/countries/` (filename starts with tag)
5. Create an **OOB file** in `<mod>/history/units/` and load it in history
6. Add **localisation** in `<mod>/localisation/english/*_l_english.yml` (UTF-8-BOM)
7. Add **flags** for each size/ideology to `<mod>/gfx/flags/`
8. Optionally assign states in `<mod>/history/states/*.txt`

## Country Tag

Defined in `<mod>/common/country_tags/*.txt`:
```
SCO = "countries/Scotland.txt"
```

### Forbidden Tags
These cause internal conflicts: `NOT`, `AND`, `TAG`, `OOB`, `LOG`, `NUM`, `RED`, and purely numeric tags like `123`.

### Dynamic Tags
```
dynamic_tags = yes
```
Marks subsequent tags as dynamic (for civil war breakaway countries). **If insufficient dynamic countries exist, the game crashes** when too many civil wars occur.

## Country File

Location: `<mod>/common/countries/`

```
graphical_culture = commonwealth_gfx
graphical_culture_2d = commonwealth_2d
color = rgb { 2  10  222 }
```

### Colors

Optional `<mod>/common/countries/colors.txt` (this file **must** be named `colors.txt`, so it overwrites vanilla):
```
SCO = {
    color = rgb { 2  10  222 }
    color_ui = rgb { 255 255 155 }
}
```
- `color`: political map mode color
- `color_ui`: UI elements, division counters
- Color saturation is multiplied by 0.6, value by 0.8 in-game
- Supports `rgb` (0-255 or 0-1 scale) and `hsv` (0-1 scale)

## History File

Location: `<mod>/history/countries/TAG - name.txt` (first 3 chars must be the tag)

```
capital = 121                    # State ID, not province ID

oob = "SCO_1936"

set_research_slots = 3
set_stability = 0.7
set_war_support = 0.5

recruit_character = SCO_ronald_mcdonald    # BEFORE set_politics

add_ideas = {
    war_economy
    extensive_conscription
}
set_technology = {
    infantry_weapons = 1
    infantry_weapons1 = 1
    early_fighter = 1
}

set_popularities = {
    democratic = 80
    communism = 10
    fascism = 10
}
set_politics = {
    ruling_party = democratic
    last_election = "1932.11.8"
    election_frequency = 48
    elections_allowed = yes
}
```

### DLC-Dependent Content

Use if statements for DLC-specific tech/OOB:
```
if = {
    limit = { has_dlc = "Man the Guns" }
    set_technology = { ... }
    set_naval_oob = "SCO_1936_naval_mtg"
}
if = {
    limit = { NOT = { has_dlc = "Man the Guns" } }
    set_technology = { ... }
    set_naval_oob = "SCO_1936_naval_legacy"
}
```

### Puppeting

Put in the **subject's** history file, **before** set_politics and set_popularities:
```
ENG = {
    if = {
        limit = { has_dlc = "Together for Victory" }
        set_autonomy = {
            target = SCO
            autonomous_state = autonomy_integrated_puppet
        }
        else = { puppet = SCO }
    }
}
```

### Date-Dependent History

```
1939.1.1 = {
    oob = "SCO_1939"
    set_technology = { atomic_research = 1 }
}
```
Applies on start dates **strictly** later than the timestamp.

### Variants

```
create_equipment_variant = {
    name = "Celtic Series"
    type = ship_hull_submarine_1
    name_group = SCO_SS_HISTORICAL
    modules = {
        fixed_ship_torpedo_slot = ship_torpedo_sub_1
        fixed_ship_engine_slot = sub_ship_engine_1
    }
    obsolete = yes
}
```

## Flags

Location: `<mod>/gfx/flags/`

Format: **uncompressed 32-bit ARGB TGA** (no RLE compression)

| Type | Size | Filesize |
|---|---|---|
| Standard | 82x52 px | ~17 KiB |
| Medium | 41x26 px | ~4.2 KiB |
| Small | 10x7 px | ~324 bytes |

Naming:
- Ideology-specific: `SCO_neutrality.tga` (highest priority)
- Generic fallback: `SCO.tga`

## Country Name (Localisation)

English localisation in `<mod>/localisation/english/*_l_english.yml` (UTF-8-BOM):
```
l_english:
 SCO: "Scotland"
 SCO_DEF: "Scotland"
 SCO_ADJ: "Scottish"
 SCO_democratic: "Republic of Scotland"
 SCO_democratic_DEF: "the Republic of Scotland"
 SCO_democratic_ADJ: "Scottish"
```

Name priority (highest to lowest):
1. Autonomous state with specific overlord
2. Autonomous state
3. Subject of specific country
4. Subject
5. Ideology type
6. Ideology group
7. TAG
8. COUNTRY

Subject names support `$NONIDEOLOGY$` and `$OVERLORD$` variables.

### Party Names
```
 SCO_communism_party: "SCP"
 SCO_communism_party_long: "Scottish Communist Party"
```
Format: `TAG_ideology_party` and `TAG_ideology_party_long`

### Equipment Renaming
```
 SCO_infantry_equipment: "Infantry Equipment Mk.I"
 SCO_infantry_equipment_short: "Inf. Eq. Mk.I"
```

## Character Names

Location: `<mod>/common/names/*.txt`

```
TAG = {
    male = { names = { Name "Multi-Word Name" } }
    female = { names = { Name } }
    surnames = { Surname "Multi-Word Surname" }
    callsigns = { Callsign }
}
```

## Placing the Country on the Map

The country exists as soon as its tag is registered. To have it own territory at game start, edit `<mod>/history/states/*.txt` to set the owner.

For mid-game appearance, use effects like `add_state_core` and `transfer_state`.

## Crucial Pitfalls

- **Missing OOB = broken buildings**: countries without an order of battle will have non-functional buildings
- **Recruit before set_politics**: country leader must be recruited *before* `set_politics`
- **History file loading order**: history files execute in country_tags order; puppeting can reset politics
- **Popularities must add to 100** exactly
- **Extra closing brackets** in history files stop execution prematurely

## References

This skill covers the essentials. For the complete, unabridged reference with exhaustive details, read the following file(s) under the master skill's references directory:

- `skills/hoi4-mod/references/Country creation.md` — Full reference for country tags, history files, flags, localisation, and order of battle.
