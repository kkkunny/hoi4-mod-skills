---
name: hoi4-map
description: Modify the HOI4 map and states — provinces, terrain, BMP files, state history, population, resources, buildings, and victory points.
---

# HOI4 Map & State Modding

This skill covers both the map (provinces, terrain, BMP files) and states (history, population, buildings, victory points). Map modding is one of the most technically demanding aspects of HOI4 modding.

---

## Part 1: Map Files

| File | Format | Purpose |
|---|---|---|
| `map/provinces.bmp` | 24-bit RGB BMP | Province borders (each color = one province) |
| `map/definition.csv` | CSV | Province info (terrain, continent, coastal status) |
| `map/terrain.bmp` | 8-bit indexed BMP | Terrain texture assignment |
| `map/heightmap.bmp` | 8-bit greyscale BMP | Height data |
| `map/world_normal.bmp` | 24-bit RGB BMP | Normal mapping (slopes/lighting) |
| `map/rivers.bmp` | 8-bit indexed BMP | River placement |
| `map/trees.bmp` | 8-bit indexed BMP | Tree model placement |
| `map/buildings.txt` | Text | Building model positions |
| `map/unitstacks.txt` | Text | Unit model positions and VP icons |
| `map/supply_nodes.txt` | Text | Starting supply nodes |
| `map/railways.txt` | Text | Starting railways |
| `map/adjacencies.csv` | CSV | Province border rules (straits, impassable) |
| `map/adjacency_rules.txt` | Text | Complex adjacency logic |
| `map/ambient_object.txt` | Text | Permanent world models |
| `map/cities.txt` | Text | City model assignment |
| `map/default.map` | Text | Map configuration (filenames for bitmaps) |
| `map/terrain/colormap_rgb_cityemissivemask_a.dds` | DDS | World coloring + city lights |
| `map/terrain/colormap_water_0.dds` | DDS | Ocean coloring |

### BMP Format Requirements (Critical)

The game expects **BITMAPINFOHEADER**. GIMP must check **"do not write color space information"** when exporting.

**24-bit RGB** (`provinces.bmp`, `world_normal.bmp`): Most editors work. Paint.net OK.

**8-bit Greyscale** (`heightmap.bmp`): GIMP Greyscale mode; Photoshop Indexed with greyscale color table. Paint.net/MS Paint: DO NOT USE.

**8-bit Indexed** (`cities.bmp`, `rivers.bmp`, `terrain.bmp`, `trees.bmp`):
- Uses a colormap/color table (256 max). Game reads pixel **index values**, not colors.
- Colormap must be preserved exactly.
- Paint.net/MS Paint: DO NOT USE (regenerate colormap on save).
- GIMP: use "use custom palette" when converting to Indexed.
- Photoshop: save/load color table from Image → Mode → Color Table.

### Fixing Broken 8-bit Maps

1. Keep base game copy open
2. Edit content as 24-bit RGB copy
3. Copy pixel data from 24-bit into the 8-bit file with original colormap
4. Photoshop: save color table to .ACT, reload when converting to Indexed

### Essential Practices

- **Debug mode necessary**: without it, MAP_ERROR prevents game load
- **Nudge tool**: in `<game>/tools/` or with `-nudge` launch option
- **Check error.log after loading into a country** (not just main menu)
- **`replace_path`**: unload vanilla folders (states, strategic regions, supply) for total overhauls
- Adding new provinces/states requires updating `buildings.txt`, `supply_nodes.txt`, and `railways.txt` — or crashes occur

### Province Definition (definition.csv)

```
id;name;r;g;b;type;continent;is_coastal;terrain
123;STATE_123;255;0;128;land;europe;false;plains
```

---

## Part 2: States

States defined in `<mod>/history/states/*.txt`. **Filenames irrelevant** — only content matters. Don't change filenames of existing files.

### Basic State

```
state = {
    id = 123
    name = STATE_123
    manpower = 500000
    state_category = rural
    
    provinces = { 123 456 7890 }
    
    history = {
        owner = POL
        controller = LIT
        victory_points = { 1234 10 }
        
        buildings = {
            infrastructure = 3
            industrial_complex = 2
            arms_factory = 1
            naval_base = 2
        }
        
        add_core_of = POL
    }
}
```

### Mandatory Arguments

- `id = 123`: Unique integer. **Must be sequential from 1** — gaps crash the game.
- `name = STATE_123`: Localisation key
- `manpower = 500000`: Starting population
- `state_category = my_category`: From `common/state_categories/`
- `provinces = { 123 456 }`: Province IDs in the state

### Optional

```
impassable = yes
resources = { steel = 10 aluminium = 20 }
local_supplies = 8.3
buildings_max_level_factor = 0.5
```

Base resources: `oil`, `aluminium` (British spelling), `rubber`, `tungsten`, `steel`, `chromium`.

### State History

#### Ownership
```
history = {
    owner = POL
    controller = LIT        # Only if different from owner
}
```

#### Victory Points
```
victory_points = { 1234 10 }     # Province ID, VP value
victory_points = { 5678 5 }      # Separate entry per VP province
```
Localisation: `VICTORY_POINTS_1234: "City Name"`. Icon positions via `map/unitstacks.txt` / Nudge.

#### Buildings
```
buildings = {
    infrastructure = 3
    industrial_complex = 2
    arms_factory = 1
    dockyard = 1
    naval_base = 2
    air_base = 1
    bunker = 2
    anti_air_building = 1
    synthetic_refinery = 1
    fuel_silo = 1
    radar_station = 1
    rocket_site = 1
    nuclear_reactor = 1
}
```

#### Cores and Claims
```
add_core_of = POL               # Full core
add_claim_by = GER              # Claim only
```

#### Date-Dependent
```
history = {
    owner = POL
    1939.1.1 = {
        owner = GER
        controller = GER
        buildings = { infrastructure = 5 }
    }
}
```
Applies on dates **strictly after** the timestamp.

### State Categories

In `<mod>/common/state_categories/*.txt`:
```
my_state_category = {
    identifier = "my_category"
    color = { 100 200 50 }
    local_modifier = { max_factories_factor = 0.5 }
    max_slots = 10
    locked_slots = 2
}
```

### Common State Effects

```
add_state_core = TAG
remove_state_core = TAG
transfer_state = TAG               # Owner + controller
set_state_owner = TAG
set_state_controller = TAG
add_claim_by = TAG
add_building_construction = { type = arms_factory level = 1 }
add_resource = { type = steel amount = 20 }
set_state_population = 500000
set_state_category = urban
```

---

## Common Pitfalls

- **Wrong BMP format**: check bitdepth and header — most common map issue
- **Paint.net/MS Paint for 8-bit files**: corrupts colormap; never use
- **State IDs not sequential**: gaps cause crashes
- **Missing `buildings.txt`/`supply_nodes.txt` update**: crash with new provinces
- **GIMP color space info**: must uncheck
- **State without owner + transfer effect**: causes crash
- **Editing vanilla state filenames without `replace_path`**: both loaded, duplicate data

## References

For complete, unabridged references, read:

- `skills/hoi4-mod/references/Map modding.md` — Full map reference: provinces, terrain, heightmap, BMP format details
- `skills/hoi4-mod/references/State modding.md` — Complete state reference: history, population, buildings, categories
- `skills/hoi4-mod/references/Nudger.md` — Nudge tool reference for map adjustments
- `skills/hoi4-mod/references/Strategic region modding.md` — Strategic region and weather zone definitions
- `skills/hoi4-mod/references/Supply areas modding.md` — Supply area and logistics definitions
