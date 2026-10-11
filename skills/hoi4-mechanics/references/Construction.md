# Construction — Mechanics

> **Topic** — construction: buildings, shared slots, infrastructure, consumer-goods allocation, damage and repair, factory conversion.
> **Scope** — covered: the construction system. Not covered: resource extraction beyond its build-side hooks (see `Production.md`), what buildings do to supply/combat once built (future `Logistics.md` / `Land battle.md`), landmark/facility content (listed only as existence).
>
> **Sources & version anchors**
> - [w] game wiki *Construction* (original: hoi4.paradoxwikis.com/Construction), read via the Chinese mirror `cooderatom.github.io/hoi4-zh-wiki/Construction.html` (mirror snapshot 2026-09-10, rev 102988). Section anchors use mirror titles.
> - [m] modding wiki offline snapshot files in `[[hoi4-mod]]`'s `references/`.
> - [v] vanilla install **1.18.3 "Case Green"** (`launcher-settings.json`), root `/mnt/data/hoi4/Hearts of Iron IV`.
>
> Number resolution: target-version [v] > [m] > [w]; conflicts are listed under `## Unknowns`.

## Player view

- Construction projects are fixed structures built with civilian factories, placed in states or provinces. [w:Construction]
- Three placement classes: state buildings (per-state effects), shared buildings (compete for limited state slots; include every factory type), and province buildings (benefit the province controller regardless of who controls the state). [w:Construction §州级建筑] [w:Construction §共享建筑] [w:Construction §省级建筑]
- Shared slots come from the state category (metropolis 12 down to wasteland 0), are raised by techs and focuses, and cap at 25. [w:Construction §槽位数量]
- State buildings and their per-level effects include infrastructure (fuel capacity +1500, province supply +0.3, division speed +5%, org recovery +10%, strategic redeploy +1 km/h, resources +15%, construction speed +20%; at max level mud growth −80%), air bases (200 planes/level), AA, radar and dams. [w:Construction §州级建筑]
- Province buildings: naval base (supply capacity/throughput and local ship repair scale with level), land and coastal forts (−15% attacker penalty per level), railways, supply hubs and canal locks. [w:Construction §省级建筑]
- Experimental facilities (Götterdämmerung) are limited to one per state, consume 3 supply while active, unlock scientists, and enable facility-strike raids against them. [w:Construction §实验设施]
- Landmarks grant owner-only country modifiers and ignore regular strategic bombing; some (the Great Wall) actually apply province modifiers instead. [w:Construction §地标]
- Strategic locations raise local caps by +2 for naval base / land fort / coastal fort / air base. [w:Construction §战略要地]
- Demolition: hover a shared building and press the X; at war, one demolition per state per 30 days. [w:Construction §拆除]
- Consumer goods: a rounded-down share of total factories — military factories and traded-in factories included, dockyards excluded, damaged factories still counted — is reserved by the economy law, national spirits, advisors and stability (100% stability gives −5%; each 10% above 50% gives −1%). [w:Construction §消费品]
- The consumer-goods factory factor (CGFF) multiplies the current share: total = law share × (1 + CGFF), and multiple CGFF sources multiply. [w:Construction §消费品工厂系数]
- Factory conversion: civ↔mil via the state building view or the conversion button; civ→mil base cost 4000, mil→civ base cost 9000. [w:Construction §工厂转产]
- Repair: damaged buildings appear in the construction list, repair lines gain +200% output, and even unassigned lines advance 1% per day. [w:Construction §损坏与修复]
- Build speed in a state scales with its infrastructure (×1 at 0/5 up to ×2 at 5/5); a fully built-up state can gain one extra slot through a one-time 100 PP decision. [w:Construction §基础设施等级] [w:Construction §增建民用工厂的效率]

## Engine view

- Buildings are defined in `common/buildings/*.txt`; cost is `base_cost` plus `per_level_extra_cost` per level, and `infrastructure_construction_effect = yes` ties build speed to state infrastructure. [v:common/buildings/00_buildings.txt → buildings @1.18.3] [m:Building modding.md → Buildings, Cost]
- Infrastructure adds 1500 fuel capacity per level as the building modifier `max_fuel_building = 1.5` (thousands); the fuel silo adds `max_fuel_building = 100` per level. [v:common/buildings/00_buildings.txt → infrastructure.max_fuel_building, fuel_silo.max_fuel_building @1.18.3]
- The synthetic refinery's base fuel output is `fuel_gain_from_states = 2.0` per hour (48/day), scaled further by tech. [v:common/buildings/00_buildings.txt → synthetic_refinery.fuel_gain_from_states @1.18.3] [w:Construction §共享建筑]
- The shared-slot cap is `MAX_SHARED_SLOTS = 25`; per-category base slots live in `common/state_category/` files. [v:common/defines/00_defines.lua → NBuildings.MAX_SHARED_SLOTS=25 @1.18.3] [v:common/state_category/ @1.18.3] [w:Construction §槽位数量]
- Demolition cooldown is `DESTRUCTION_COOLDOWN_IN_WAR = 30` days. [v:common/defines/00_defines.lua → NBuildings.DESTRUCTION_COOLDOWN_IN_WAR @1.18.3] [w:Construction §拆除]
- Infrastructure per level: province supply `NSupply.INFRA_TO_SUPPLY = 0.3`; movement `NMilitary.INFRASTRUCTURE_MOVEMENT_SPEED_IMPACT = −0.05`; org recovery `NMilitary.INFRA_ORG_IMPACT = 0.5`; strategic redeploy `NMilitary.STRATEGIC_SPEED_INFRA_BASE = 5` up to `NMilitary.STRATEGIC_SPEED_INFRA_MAX = 10`; max-level mud `NBuildings.INFRASTRUCTURE_MUD_EFFECT = −0.8`. [v:common/defines/00_defines.lua → NSupply.INFRA_TO_SUPPLY, NMilitary.INFRASTRUCTURE_MOVEMENT_SPEED_IMPACT, NMilitary.INFRA_ORG_IMPACT, NMilitary.STRATEGIC_SPEED_INFRA_BASE, NMilitary.STRATEGIC_SPEED_INFRA_MAX, NBuildings.INFRASTRUCTURE_MUD_EFFECT @1.18.3] [w:Construction §州级建筑]
- Build speed multiplier = `1 + state_infra × INFRA_MAX_CONSTRUCTION_COST_EFFECT / max_level`; `INFRA_MAX_CONSTRUCTION_COST_EFFECT = 1` with infrastructure max level 5, so 5/5 infrastructure doubles build speed. [w:Construction §基础设施等级] [v:common/defines/00_defines.lua → NProduction.INFRA_MAX_CONSTRUCTION_COST_EFFECT=1 @1.18.3]
- Consumer-goods minimums: at least 1 factory or 10% is taken (`MINIMUM_NUMBER_OF_FACTORIES_TAKEN_BY_CONSUMER_GOODS_VALUE / _PERCENT`); the baseline share comes from the economy law (see `Production.md`) and `consumer_goods_factor` applies multiplicatively. [v:common/defines/00_defines.lua → NProduction.MINIMUM_NUMBER_OF_FACTORIES_TAKEN_BY_CONSUMER_GOODS_VALUE=1, ..._PERCENT=0.1 @1.18.3] [m:Modifiers.md → consumer_goods_factor] [w:Construction §消费品]
- Repair output: assigned factories work at `BASE_FACTORY_REPAIR_FACTOR = 2` (+200%); with none assigned the line still runs at `BASE_FACTORY_REPAIR = 1`; daily progress = median(1%, speed/cost×100%, 100%). [v:common/defines/00_defines.lua → NBuildings.BASE_FACTORY_REPAIR_FACTOR=2, NBuildings.BASE_FACTORY_REPAIR=1 @1.18.3] [w:Construction §损坏与修复]
- Damage: each building level has 100 HP; strategic bombing and land-combat damage scale with the building's damage factor (air base/radar 0.5, AA 0.1, others 1) times (unharmed levels / max)², and any damage strips a shared building's first-level effect entirely. [w:Construction §损坏与修复] [w:Construction §共享建筑]
- Per-level AA bombing reduction is `NAir.AA_INDUSTRY_AIR_DAMAGE_FACTOR = −0.12`; dispersed industry is an unstacked alternative and factories take the max of the two. [v:common/defines/00_defines.lua → NAir.AA_INDUSTRY_AIR_DAMAGE_FACTOR @1.18.3] [w:Construction §损坏与修复]
- Controlling a state's resources or buildings requires controlling provinces with at least 50% of its victory points. [w:Construction §州级建筑]

## Modding surfaces

- Building definitions, cost fields and slot behaviour: `common/buildings/*.txt` → Building modding.md. [m:Building modding.md → Buildings, Cost] [v:common/buildings/ @1.18.3]
- Railways and supply nodes are special buildings: starting levels live in `map/supply_nodes.txt` and `map/railways.txt`, mid-game construction must use `build_railway`, and the default `add_building_construction` on them crashes. [m:Building modding.md → supply_node, rail_way] [m:Map modding.md → Supply nodes and railways] [m:Effect.md → build_railway, add_building_construction]
- State categories (base slots): `common/state_category/*.txt`; runtime change effects are `set_state_category` and `increase_state_category`. [m:State modding.md → State categories] [m:Effect.md → set_state_category, increase_state_category] [v:common/state_category/ @1.18.3]
- Slot modifiers: `global_building_slots`, `global_building_slots_factor`, `local_building_slots(_factor)`. [m:List of modifiers.md → global_building_slots, local_building_slots]
- Construction speed: `production_speed_buildings_factor` plus per-building variants (`state_production_speed_*`, `production_speed_*`). [m:List of modifiers.md → production_speed_buildings_factor, state_production_speed_*]
- Repair speed: `repair_speed_*` / `state_repair_speed_*` factors. [m:List of modifiers.md → repair_speed_*]
- Factory conversion cost modifiers: `conversion_cost_civ_to_mil_factor`, `conversion_cost_mil_to_civ_factor`. [m:List of modifiers.md → conversion_cost_civ_to_mil_factor, conversion_cost_mil_to_civ_factor]
- Fuel capacity via the building modifier `max_fuel_building` (thousands) and the fuel silo; full fuel mechanics → `Fuel.md`. [m:List of modifiers.md → max_fuel_building]
- Buildings placed in state history → State modding.md, [[hoi4-map]]. [m:State modding.md]
- Industry techs behind construction speed/output live in `common/technologies/` → Technology modding.md, [[hoi4-tech]]. [m:Technology modding.md] [v:common/technologies/ @1.18.3]

## Unknowns

- `NMilitary.INFRA_ORG_IMPACT = 0.5` versus the wiki's org-recovery table (5 → +25%, 1 → −15%): the mapping between define and table is not stated by either source (unsourced).
- CGFF: the wiki describes the consumer-goods factory factor as multiplicative and `Modifiers.md` describes `consumer_goods_factor` as multiplicative, but no source states CGFF is implemented as that modifier (unsourced identification).
- Consumer-goods allocation beyond the floor rule (exact factory assignment when the share fluctuates) is not stated by the sources used here.
- Experimental facilities, landmarks and strategic locations are DLC content (Götterdämmerung / No Compromise No Surrender / Peace for Our Time); only existence and basic hooks are covered.
- The 4000/9000 conversion base costs come from the wiki table whose footnote points at `00_buildings.txt`, but the exact field name was not located in the 1.18.3 building file during verification (unsourced field).
