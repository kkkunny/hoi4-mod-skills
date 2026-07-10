---
name: hoi4-tech
description: Create or modify the military-technical side of HOI4 — technologies, doctrines, division templates, orders of battle, sub-units, and equipment types with modules and variants.
---

# HOI4 Technology & Military Modding

This skill covers the complete military-technical domain: technologies, division templates, orders of battle, sub-units, and equipment types.

---

## Part 1: Technologies & Doctrines

### File Locations

- Technologies: `<mod>/common/technologies/*.txt`
- Technology categories/folders: `<mod>/common/technology_tags/*.txt`
- Tech tree GUI: `<mod>/interface/countrytechtreeview.gui`
- Doctrine tree GUI: `<mod>/interface/countrydoctrinetreeview.gui`

### Basic Technology

```
technologies = {
    my_technology = {
        allow = { always = yes }
        categories = { infantry_tech }
        cost = 100
        start_year = 1936
        base_days = 200
        
        path = { leads_to = my_next_tech }
        
        modifier = { army_defence_factor = 0.05 }
        
        on_research_complete = {
            country_event = my_event.0
        }
        
        enable_equipments = { infantry_equipment_1 }
        enable_subunits = { infantry artillery_brigade }
        enable_buildings = { arms_factory }
        max_building_level = { infrastructure = 5 }
    }
}
```

### Triggers

- `allow`: Checked continuously. If false, can't research manually but still visible. `always = no` for hidden techs.
- `allow_branch`: If false, technology and all `path`-connected children become invisible.

### Category/Folder Setup

In `<mod>/common/technology_tags/*.txt`:
```
my_category = {
    name = my_cat_name
    folder = my_folder
    position = { x = 0 y = 0 }
    categories = { sub_cat }
}
```

### Research Bonuses

Via ideas/focuses:
```
research_bonus = {
    infantry_tech = { research_bonus = 0.10 }
}
```

Via effects:
```
add_tech_bonus = { bonus = 1.0 uses = 2 category = infantry_tech }
```

### Doctrines

```
my_doctrine = {
    doctrine = yes
    categories = { land_doctrine }
    cost = 100
    start_year = 1936
    path = { leads_to = next_doctrine }
    modifier = { army_attack_factor = 0.10 }
}
```

### Technology Sharing Groups

In `<mod>/common/technology_sharing/*.txt`:
```
my_tech_group = {
    research_bonus_from_members = 0.05
    max_bonus = 0.25
}
```
Assign: `add_to_tech_sharing_group = my_tech_group` in country history.

### GUI Integration (Mandatory for New Branches)

New tech branches require GUI edits: gridbox positions, offsets, tab buttons, icon placements. Files: `countrytechtreeview.gui`, `countrytechtreeview.gfx`, `countrydoctrinetreeview.gui`.

---

## Part 2: Order of Battle & Division Templates

### OOB Location

Files in `<mod>/history/units/*.txt`. Filename irrelevant — just match what's loaded.

### Loading an OOB

In `<mod>/history/countries/TAG*.txt`:
```
oob = "SCO_1936"                    # Traditional land OOB
set_oob = "SCO_1936"                # Same, but as effect (works in if statements)
set_naval_oob = "SCO_1936_naval"    # Naval OOB
set_air_oob = "SCO_1936_air"        # Air OOB
```

**Countries without an OOB get broken buildings** — buildings added via effect won't apply.

### DLC-Dependent OOB

```
if = {
    limit = { has_dlc = "Man the Guns" }
    set_naval_oob = "SCO_1936_naval_mtg"
}
if = {
    limit = { NOT = { has_dlc = "Man the Guns" } }
    set_naval_oob = "SCO_1936_naval_legacy"
}
```
Similarly for No Step Back (tanks) and By Blood Alone (air).

### Division Template

```
division_template = {
    name = "Blueskirt Division"
    
    regiments = {                              # Combat battalions (5x5 grid)
        infantry = { x = 0 y = 0 }
        infantry = { x = 0 y = 1 }
        artillery_brigade = { x = 1 y = 0 }
    }
    support = {                                # Support companies (1x5 grid)
        engineer = { x = 0 y = 0 }
        artillery = { x = 0 y = 1 }
    }
    
    division_names_group = USA_INF_01
    is_locked = yes
    force_allow_recruiting = yes
    division_cap = 12
    priority = 0                         # 0=Reserves, 1=Default, 2=Elite
    template_counter = 10
    override_model = GER_infantry_entity
}
```

### Unit Placement

**Land division:**
```
units = {
    division = {
        name = "1st Blueskirt Division"
        # OR: division_name = { is_name_ordered = yes name_order = 35 }
        location = 9392
        division_template = "Blueskirt Division"
        start_experience_factor = 0.2
        start_equipment_factor = 0.5
        start_manpower_factor = 0.3
        force_equipment_variants = {
            infantry_equipment_0 = { owner = SCO amount = 14 }
        }
    }
}
```

**Navy:**
```
ship = {
    name = "SCO Belfast"
    type = ship_hull_light_1
    equipment = ship_hull_light_1
    version_name = "Celtic Series"
    location = 9392
    start_experience_factor = 0.3
}
```

**Air:**
```
air_wings = {
    SCO_fighter = {
        type = fighter_equipment_1
        name = "1st SCO Fighter Wing"
        location = 9392
        mission = air_superiority
        size = 100
    }
}
```

### Equipment Production

```
instant_effect = {
    add_equipment_production = {
        equipment = {
            type = infantry_equipment_0
            creator = SCO
            version_name = "Variant name"       # Mandatory for ships/tanks/planes with DLC
        }
        requested_factories = 1
        progress = 0.19
        efficiency = 100
    }
}
```

---

## Part 3: Sub-Units & Equipment

### Sub-Unit Definitions

Location: `<mod>/common/units/*.txt`

```
infantry = {
    group = infantry                        # 'support' for support companies
    need = { infantry_equipment infantry_equipment_1 }
    manpower = 1000
    training_time = 90
    maximum_speed = 4.0
    soft_attack = 3.0
    hard_attack = 0.5
    defense = 20.0
    breakthrough = 2.0
    armor = 0
    suppression = 1.0
    weight = 0.5
}
```

- `group = support`: can only go in support column (1x5)
- Other groups (`infantry`, `mobile`, `armor`, `artillery`): combat battalions (5x5)

Common sub-unit IDs: `infantry`, `cavalry`, `motorized`, `mechanized`, `marine`, `mountaineer`, `paratrooper`, `artillery_brigade`, `anti_air_brigade`, `anti_tank_brigade`, `light_armor`, `medium_armor`, `heavy_armor`, `engineer`, `recon`, `military_police`, `maintenance_company`, `field_hospital`, `logistics_company`, `signal_company`.

### Equipment Archetype

Location: `<mod>/common/units/equipment/*.txt`

```
equipments = {
    infantry_equipment = {
        type = infantry
        category = infantry_equipment
        group = infantry
        build_cost_ic = 0.5
        resources = { steel = 2 }
        reliability = 0.9
    }
}
```

### Equipment Variants

```
infantry_equipment_0 = {
    archetype = infantry_equipment
    parent = infantry_equipment
    reliability = 0.9
    defense = 20
    breakthrough = 2
    resources = { steel = 1 }
    build_cost_ic = 1
}
```

### Equipment Modules (DLC systems)

For MTG ships, NSB tanks, BBA planes. Location: `<mod>/common/units/equipment/modules/`

```
ship_torpedo_sub_1 = {
    category = fixed_ship_torpedo_slot
    add_stats = { sub_attack = 15 build_cost_ic = 1 }
    can_convert_from = ship_torpedo_sub_1
    convert_cost_ic = 0.5
    resources = { steel = 1 }
}
```

Module slots on the equipment archetype:
```
ship_hull_submarine_1 = {
    archetype = ship_hull_submarine
    modules = {
        fixed_ship_torpedo_slot = {
            module_category = fixed_ship_torpedo_slot
            allowed_module_categories = { fixed_ship_torpedo_slot }
        }
    }
}
```

### Equipment Upgrades (Pre-DLC)

Location: `<mod>/common/units/equipment/upgrades/`
```
ship_reliability_upgrade = {
    max_level = 5
    reliability = 0.1
    cost = 0.05
}
```

### Creating Variants in History

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

### Equipment Bonuses (via Ideas)

```
equipment_bonus = {
    tank = {
        armor_value = 0.05
        reliability = 0.05
        build_cost_ic = -0.05
    }
}
```

### Common Equipment Types

| Category | Archetype |
|---|---|
| Infantry | `infantry_equipment` |
| Artillery | `artillery_equipment`, `anti_air_equipment`, `anti_tank_equipment` |
| Light Armor | `gw_tank_equipment` |
| Medium Armor | `medium_tank_equipment` |
| Heavy Armor | `heavy_tank_equipment` |
| Fighters | `fighter_equipment` |
| CAS | `close_air_support_equipment` |
| Naval Bombers | `naval_bomber_equipment` |
| Submarines | `submarine_equipment` / `ship_hull_submarine` |
| Destroyers | `destroyer_equipment` / `ship_hull_light` |

### Division Name Groups

Location: `<mod>/common/units/names_divisions/*.txt`
```
USA_INF_01 = {
    for_countries = { USA }
    is_name_ordered = yes
    ordered = { 1 = "1st 'Big Red One'" 2 = "2nd 'Indianhead'" }
    fallback_name = "Infantry Division"
}
```

---

## Common Pitfalls

- **New tech not showing**: forgot to edit GUI files
- **No OOB = broken buildings**: always include at least a minimal OOB
- **DLC equipment in wrong OOB**: MTG ships in non-MTG OOB causes issues
- **`group = support`** required for support column placement
- **Module category must match slot's `module_category`**
- **Variant `version_name` mandatory** in production for DLC ships/tanks/planes

## References

For complete, unabridged references, read:

- `skills/hoi4-mod/references/Technology modding.md` — Full technology and doctrine definitions, categories, GUI integration
- `skills/hoi4-mod/references/Division modding.md` — Complete OOB format, template options, all placement details
- `skills/hoi4-mod/references/Unit modding.md` — Full sub-unit definitions and all stat fields
- `skills/hoi4-mod/references/Equipment modding.md` — Complete equipment archetypes, modules, upgrades, and DLC-specific systems
