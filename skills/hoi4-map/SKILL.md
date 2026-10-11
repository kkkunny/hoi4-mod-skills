---
name: hoi4-map
description: Modify the HOI4 map — provinces, terrain, states, resources, buildings, and victory points. Use this skill whenever the user wants to edit state ownership, add resources to states, change province terrain, modify the heightmap or rivers, add buildings to states, or restructure strategic regions.
---

# HOI4 Map & State Modding

Two domains: **map files** (bitmaps, definitions — the hardest part of HOI4 modding) and **states** (history, population, buildings). Strategic regions and supply areas: `Strategic region modding.md`, `Supply areas modding.md`.

## Map Files

| File | Format | Purpose |
|---|---|---|
| `map/provinces.bmp` | 24-bit RGB | province borders (one color = one province) |
| `map/definition.csv` | CSV | province terrain, continent, coastal flag |
| `map/heightmap.bmp` | 8-bit greyscale | terrain height |
| `map/world_normal.bmp` | 24-bit RGB | slopes/lighting |
| `map/terrain.bmp`, `rivers.bmp`, `trees.bmp` | 8-bit indexed | terrain/texture, rivers, trees |
| `map/buildings.txt`, `unitstacks.txt`, `cities.txt` | text | model/VP/city placement |
| `map/supply_nodes.txt`, `railways.txt` | text | starting logistics |
| `map/adjacencies.csv`, `adjacency_rules.txt` | CSV/text | straits, impassable rules |
| `map/default.map` | text | map configuration |

**BMP rules (critical)**: game needs BITMAPINFOHEADER (GIMP: check *do not write color space information*). 8-bit indexed files carry a color table the game reads by index — preserve it exactly; never use Paint.net/MS Paint for 8-bit; GIMP: "use custom palette"; Photoshop: save/load the color table. Details and fixes → `Map modding.md`.

## States

`history/states/*.txt` — **filenames irrelevant**; never rename vanilla files without `replace_path` (else both load). State categories in `common/state_categories/`.

```
state = {
    id = 123                    # MUST be sequential from 1 — gaps crash the game
    name = STATE_123
    manpower = 500000
    state_category = rural
    provinces = { 123 456 }
    
    history = {
        owner = POL
        victory_points = { 1234 10 }      # province ID, VP value; loc: VICTORY_POINTS_1234
        buildings = { infrastructure = 3 arms_factory = 1 }
        add_core_of = POL
        add_claim_by = GER
    }
}
```

## Key Rules

- Optional state fields: `impassable`, `resources = { steel = 10 }` (spelling: `aluminium`), `local_supplies`, `buildings_max_level_factor`.
- Date blocks (`1939.1.1 = { owner = GER }`) apply only **strictly after** the timestamp.
- **Adding provinces/states requires updating `buildings.txt`, `supply_nodes.txt`, and `railways.txt`** or the game crashes. New states also need matching `definition.csv` provinces.
- **Debug mode is required for map work**: without it MAP_ERROR blocks loading; Nudge tool (`-nudge`) edits positions visually → `Nudger.md`.
- Check `error.log` after loading into a country, not just to the main menu.
- Common state effects: `transfer_state`, `set_state_owner`, `set_state_controller`, `add_state_core`, `remove_state_core`, `add_claim_by`, `add_building_construction`, `add_resource`, `set_state_population`.

## Fatal Pitfalls

- **State ID gaps** → crash
- **New province without `buildings.txt`/`supply_nodes.txt`/`railways.txt` entries** → crash
- **8-bit BMP with regenerated color table** → corrupted map
- **State with no owner receiving a transfer effect** → crash
- **Paint.net/MS Paint for 8-bit map files** → corrupts the colormap

## References

Full details in the [[hoi4-mod]] skill's `references/` folder:

- `Map modding.md` — full map pipeline, BMP internals, province definitions
- `State modding.md` — all state arguments, buildings, population, categories
- `Strategic region modding.md`, `Supply areas modding.md`, `Nudger.md` — regions, logistics, map tool
- [[hoi4-country]] — country setup; [[hoi4-modifier]] — state modifiers
