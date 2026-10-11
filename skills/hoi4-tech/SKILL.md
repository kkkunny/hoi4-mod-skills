---
name: hoi4-tech
description: Create or modify technologies, doctrines, division templates, orders of battle, sub-units, and equipment in HOI4 mods. Use this skill whenever the user wants to add tech trees, set up starting OOBs and military units, define division templates, create equipment types, or set up module-based equipment for ships/tanks/planes.
---

# HOI4 Technology & Military Modding

Covers technologies, OOBs, division templates, sub-units, and equipment.

## File Locations

| Content | Path |
|---|---|
| Technologies, doctrines | `common/technologies/*.txt` |
| Tech categories/folders | `common/technology_tags/*.txt` |
| Orders of battle | `history/units/*.txt` (loaded from `history/countries/`) |
| Sub-units | `common/units/*.txt` |
| Equipment archetypes/variants/modules/upgrades | `common/units/equipment/` |
| Division name groups | `common/units/names_divisions/*.txt` |
| Tech tree GUI | `interface/countrytechtreeview.gui` / `.gfx` → [[hoi4-gui]] |

## Minimal Technology

```
technologies = {
    my_tech = {
        allow = { always = yes }               # continuously checked; always = no hides it
        categories = { infantry_tech }
        cost = 100
        start_year = 1936
        path = { leads_to = my_next_tech }     # children become invisible if allow_branch fails
        modifier = { army_defence_factor = 0.05 }
        enable_equipments = { infantry_equipment_1 }
    }
}
```

- **Doctrines** add `doctrine = yes` and use `categories = { land_doctrine }`.
- **Research bonuses**: `add_tech_bonus = { bonus = 1.0 uses = 2 category = infantry_tech }` (effects) or `research_bonus = { infantry_tech = { research_bonus = 0.10 } }` (ideas/focuses).
- **New branches must edit the tech tree GUI** (gridbox positions, tab buttons) — route through `Technology modding.md` + [[hoi4-gui]].
- Tech sharing groups: `common/technology_sharing/`, assigned via `add_to_tech_sharing_group`.

## OOB (Order of Battle)

Loaded from the country history file:
```
oob = "SCO_1936"                    # land; set_oob works inside if-statements
set_naval_oob = "SCO_1936_naval"    # naval; set_air_oob for air
```
- **A country with no OOB has broken buildings** — always provide at least a minimal one.
- DLC gear/units need `if = { limit = { has_dlc = "Man the Guns" } ... }` variants (MTG ships, NSB tanks, BBA planes).
- Division template minimal shape:
  ```
  division_template = {
      name = "Blueskirt Division"
      regiments = { infantry = { x = 0 y = 0 } artillery_brigade = { x = 1 y = 0 } }   # 5x5 grid
      support = { engineer = { x = 0 y = 0 } }                                        # 1x5 grid
  }
  ```
- Placement blocks (`division`, `ship`, `air_wings`) and production (`add_equipment_production`) → `Division modding.md`.

## Sub-Units & Equipment

```
# Sub-unit (common/units/)
infantry = {
    group = infantry                  # 'support' = support column only
    need = { infantry_equipment }
    manpower = 1000
    soft_attack = 3.0
}
# Equipment archetype (common/units/equipment/)
infantry_equipment = {
    type = infantry
    build_cost_ic = 0.5
    resources = { steel = 2 }
}
```

- Variants: `archetype = X`, `parent = X`, stats + cost. Variant `version_name` is **mandatory** for DLC ships/tanks/planes in production.
- **Modules** (MTG/NSB/BBA): slot on the archetype (`modules = { slot = { module_category = ... } }`), module definition (`category`, `add_stats`, `can_convert_from`). Slot's `module_category` must match the module's `category`.
- Pre-DLC equipment upgrades: `common/units/equipment/upgrades/`.
- All fields → `Unit modding.md`, `Equipment modding.md`.

## Fatal Pitfalls

- Country without OOB → broken buildings
- New tech branch without GUI edits → tech never visible
- Support company without `group = support` → cannot be placed in support column
- Module category mismatch → module not equipable
- Missing variant `version_name` → DLC equipment creation fails

## References

Full details in the [[hoi4-mod]] skill's `references/` folder:

- `Technology modding.md` — tech/doctrine arguments, categories, folders, GUI integration
- `Division modding.md` — full OOB format, templates, unit placement, production
- `Unit modding.md` — all sub-unit stats; `Equipment modding.md` — archetypes, modules, upgrades, DLC systems
