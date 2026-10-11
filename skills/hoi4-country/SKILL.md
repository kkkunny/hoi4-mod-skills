---
name: hoi4-country
description: Create or modify countries in HOI4 mods. Use this skill whenever the user wants to add a new nation, set up a country's starting territory, politics, military, or technology, assign a country to the map, or create country flags and colors.
---

# HOI4 Country Creation

Covers creating a country: tag, country file, history, flags, names, and placing it on the map.

## Checklist

| Step | File |
|---|---|
| 1. Tag registration | `common/country_tags/*.txt` |
| 2. Country file (color, culture) | `common/countries/` |
| 3. Characters | `common/characters/` → [[hoi4-character]] |
| 4. History (capital, politics, tech, OOB link) | `history/countries/TAG - name.txt` (filename starts with the tag) |
| 5. OOB | `history/units/` → [[hoi4-tech]] |
| 6. Localisation | `localisation/english/*_l_english.yml` → [[hoi4-assets]] |
| 7. Flags | `gfx/flags/` |
| 8. Starting territory (optional) | `history/states/*.txt` → [[hoi4-map]] |

## Tag Registration

```
SCO = "countries/Scotland.txt"      # path is informational
```

- **Forbidden tags** (internal conflicts): `NOT`, `AND`, `TAG`, `OOB`, `LOG`, `NUM`, `RED`, and purely numeric tags like `123`
- `dynamic_tags = yes` marks subsequent tags as dynamic (civil war breakaways). **Too few dynamic tags = crash** when civil wars occur.

## Minimal Country

`common/countries/Scotland.txt`:
```
graphical_culture = commonwealth_gfx
graphical_culture_2d = commonwealth_2d
color = rgb { 2 10 222 }
```

`history/countries/SCO - Scotland.txt`:
```
capital = 121                    # State ID, not province ID
oob = "SCO_1936"
recruit_character = SCO_ronald_mcdonald     # BEFORE set_politics
set_popularities = { democratic = 80 communism = 10 fascism = 10 }   # must sum to 100
set_politics = { ruling_party = democratic }
set_technology = { infantry_weapons = 1 }
```

## Key Rules

- **Every country needs an OOB** — without one, buildings no longer function. DLC-specific gear needs `if = { limit = { has_dlc = "..." } }` branches.
- Colors: `common/countries/colors.txt` (**must** be named `colors.txt`, so it overrides vanilla). `color` = map mode, `color_ui` = UI. Options → `Country creation.md`.
- Flags: uncompressed **32-bit ARGB TGA**. Sizes 82x52 / 41x26 / 10x7. Name `TAG.tga`; ideology-specific `SCO_democratic.tga` takes priority.
- Date blocks in history (`1939.1.1 = { ... }`) apply only to start dates **strictly after** the timestamp.
- Puppeting goes in the **subject's** history file, before `set_politics` (else politics reset). Non-TFV fallback: `puppet = SCO`. → [[hoi4-autonomy]]
- Country name localisation priority: autonomous state > subject status > ideology > `TAG` → full chain and party/equipment keys in `Country creation.md`.
- A country exists once its tag is registered; mid-game appearance uses `transfer_state` / `add_state_core`.

## Fatal Pitfalls

- **No OOB** → all buildings broken
- **Leader recruited after `set_politics`** → leader never takes over
- **Popularities not summing to 100** → `set_popularities` fails
- **Extra closing bracket** in a history file → execution stops prematurely
- **Too few dynamic tags** → crash on civil wars

## References

Full details in the [[hoi4-mod]] skill's `references/` folder:

- `Country creation.md` — tags, history options, color formats, name priority chain, party/equipment keys
- [[hoi4-character]] — characters; [[hoi4-tech]] — OOB; [[hoi4-map]] — states; [[hoi4-assets]] — localisation
