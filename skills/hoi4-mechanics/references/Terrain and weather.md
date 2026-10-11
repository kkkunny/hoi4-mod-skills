# Terrain and weather — Mechanics

> **Topic** — terrain types and features, day/night, temperature, weather events, and their combat/movement/supply effects.
> **Scope** — covered: how terrain and weather modify combat, movement, attrition and supply. Not covered: attrition/accident formulas (`Attrition.md`), supply network itself (`Logistics.md`), combat resolution (`Land battle.md`).
> **Note** — Terrain and Weather are separate wiki pages, merged here because both are small cross-cutting modifier systems.
>
> **Sources & version anchors**
> - [w] game wiki *Terrain* (rev 98822, 98% translated) and *Weather* (rev 104647, 94%), via the Chinese mirror, snapshot 2026-09-10. Section anchors use mirror titles.
> - [m] modding wiki offline snapshot files in `[[hoi4-mod]]`'s `references/`.
> - [v] vanilla install **1.18.3 "Case Green"** (`launcher-settings.json`), root `/mnt/data/hoi4/Hearts of Iron IV`.
>
> Number resolution: target-version [v] > [m] > [w]; conflicts are listed under `## Unknowns`.

## Player view

- Every land province has a terrain type that sets base combat width (+ width per extra attack flank), movement cost, attrition, attacker attack/breakthrough penalties, max fort level, disease chance, supply-flow penalty, truck attrition and a temperature offset; full per-terrain table on the wiki (e.g. plains: width 70+35, no penalties; mountains: 50+25, +100% movement cost, −50% attack; urban: 80+40, −30% attack, −50% enemy air superiority). [w:Terrain §类型]
- Movement follows paths between province centres, composed proportionally of the underlying terrains (example: 40% mountain + 60% ocean → +40% movement cost instead of +100%); the path decides which direction-dependent features apply. [w:Terrain §路径]
- Terrain features: small river −30% attack/defence and −25% movement; large river −60%/−50%; amphibious landing −50%; forts −15% per level (each extra attack direction cancels one fort level, never the last); coastal forts reduce naval invasions and strait crossings. [w:Terrain §地形特征]
- Unit terrain adjusters are averaged across combat battalions and summed across support companies, then scaled by the unit's under-strength ratio; attack adjusters offset the feature penalties (capped by the feature's own penalty for amphibious/forts, not for rivers). [w:Terrain §单位专属调整值]
- Day/night cycle: each province gets ≈12 h day and 12 h night, drifting by date and latitude; at night: −50% land attack, −20% air detection, −25% naval hit chance, −100% carrier operations, −50% bombing aim, +10% naval retreat speed, 0.1× CAS damage — all reducible by explicitly tagged modifiers. [w:Weather §昼夜循环]
- Temperature: provinces have per-region yearly ranges (Manchuria −25…31 °C; southern England −5…25 °C; northern Norway −23…18 °C) plus terrain offsets (mountain −7, hills −3, forest −1, urban +3, jungle +5); extreme cold/hot provinces apply winter/heat attrition, and attack/breakthrough/org/recovery/entrenchment/supply-consumption penalties. [w:Weather §气温变化与范围 §来自气温的省份修正]
- Acclimatization: staying in extreme hot/cold provinces builds heat/cold adaptation, 3× faster in combat, up to halving the negative modifiers; adaptation decays when away, and 2× faster in the opposite climate. [w:Weather §来自气温的省份修正]
- Weather events (rain/storm/snow/blizzard/sandstorm, arctic waters, shark waters) modify air ops, naval detection/hit/retreat, bombing aim, supply and movement; state-level and province-level tables cover snow/deep snow/mud/flooded — e.g. mud: +70% attrition, −40% attack, −50% speed, +500% truck attrition. [w:Weather §州级修正 §省份级修正]
- Mud forms from stored water (rain, storms, melt) above 2 °C and evaporates faster up to 38 °C; snow only falls at ≤1 °C; snow melts above 0 °C (cap 12 °C); mud freezes at ≤−1 °C; higher infrastructure reduces mud growth up to −80%. [w:Weather §省份级修正]
- Terrain biases weather: snow/blizzard +80% in mountains, impossible in desert (−100%); mud +90% in marsh, +50% in plains, −30% in mountains; sandstorms +80% in desert. [w:Weather §地形对天气的影响]

## Engine view

- Terrain/river/fort basics: `NMilitary.RIVER_CROSSING_PENALTY = -0.3`, `NMilitary.RIVER_CROSSING_PENALTY_LARGE = -0.6`, `NMilitary.RIVER_CROSSING_SPEED_PENALTY = -0.25`, `NMilitary.RIVER_CROSSING_SPEED_PENALTY_LARGE = -0.5`, `NMilitary.BASE_FORT_PENALTY = -0.15`. [v:common/defines/00_defines.lua → NMilitary.* @1.18.3] [w:Terrain §地形特征]
- Amphibious invasion: `NNavy.AMPHIBIOUS_LANDING_PENALTY = -0.5`, movement cost `NNavy.AMPHIBIOUS_INVADE_MOVEMENT_COST = 24`, speed base `NNavy.AMPHIBIOUS_INVADE_SPEED_BASE = 0.5`, attack/defence ramp `NNavy.AMPHIBIOUS_INVADE_ATTACK_LOW = 0.2`, `..._DEFEND_LOW = 1.5`, `..._ATTACK_HIGH = 1`, `..._DEFEND_HIGH = 1`, decay `..._LANDING_PENALTY_DECREASE = 3.5`; map scale `NGame.MAP_SCALE_PIXEL_TO_KM = 7.114`. [v:common/defines/00_defines.lua → NNavy.*, NGame.MAP_SCALE_PIXEL_TO_KM @1.18.3] [w:Terrain §路径 §地形特征]
- Night and acclimatization: `NMilitary.BASE_NIGHT_ATTACK_PENALTY = -0.5`, `NAir.DETECT_CHANCE_FROM_NIGHT = -0.2`, `NAir.CAS_NIGHT_ATTACK_FACTOR = 0.1`, `NMilitary.ACCLIMATIZATION_IN_COMBAT_SPEED_FACTOR = 3`, `NMilitary.ACCLIMATIZATION_LOSS_SPEED_FACTOR = 2.0`. [v:common/defines/00_defines.lua → NMilitary.*, NAir.* @1.18.3] [w:Weather §昼夜循环 §来自气温的省份修正]
- Mud: `NBuildings.INFRASTRUCTURE_MUD_EFFECT = -0.8` at max infrastructure. [v:common/defines/00_defines.lua → NBuildings.INFRASTRUCTURE_MUD_EFFECT @1.18.3] [w:Weather §省份级修正]
- Weather data: `common/weather.txt` (temperature update rules, terrain temperature modifiers) plus strategic-region files that define monthly weather and temperature ranges. [v:common/weather.txt @1.18.3] [v:map/strategicregions/ @1.18.3] [w:Weather §参考资料]
- Terrain-related combat modifiers also flow through the land-battle factor list (`Land battle.md`); naval night static modifiers live in `common/modifiers/`. [v:common/modifiers/ @1.18.3] [m:Modifiers.md] [w:Weather §参考资料]

## Modding surfaces

- Terrain per province: `map/definition.csv` / terrain bitmaps → Map modding.md, [[hoi4-map]]. [m:Map modding.md] [v:map/definition.csv @1.18.3]
- Weather/temperature per region: strategic region files → Strategic region modding.md, [[hoi4-map]]; weather simulation constants → `common/weather.txt`. [m:Strategic region modding.md] [v:common/weather.txt @1.18.3]
- Weather/terrain modifiers: `army_attack_factor`, `army_defence_factor`, `river_crossing_factor`, `terrain_penalty_reduction`, `winter_attrition(_factor)`, `heat_attrition(_factor)`, `acclimatization_cold_climate_gain_factor`, `attrition`, `attrition_for_controller`. [m:List of modifiers.md → army_attack_factor, river_crossing_factor, terrain_penalty_reduction, winter_attrition, heat_attrition, acclimatization_cold_climate_gain_factor]
- Terrain adjusters per unit type are defined on units/equipment → Unit modding.md, [[hoi4-tech]]. [m:Unit modding.md]
- Weather/terrain defines → Defines.md, [[hoi4-define]]. [m:Defines.md §NMilitary §NNavy §NBuildings]

## Unknowns

- Two weather-table cells look internally inconsistent in the mirror and are reproduced as printed: Snow state recovery +30% (vs negative in harsher weather) and deep-snow cold acclimatization +5% (vs +0.1%/+0.2% elsewhere) — unverified.
- The amphibious sentence about "−80% scaling and +50% breakthrough" versus its define pair (`ATTACK_LOW = 0.2`, `DEFEND_LOW = 1.5`) is obscure; only the raw define values are treated as certain.
- The per-terrain adjuster table by unit type lives on a separate wiki list page and was not transcribed here (unsourced scope note).
- The province terrain definition field (`map/definition.csv`) was not re-verified field-by-field in this pass beyond the file existing.
