# Production — Mechanics

> **Topic** — military equipment production: factories, production lines, efficiency, resources, conversion, consumer-goods baseline.
> **Scope** — covered: the production system. Not covered: construction (planned `Construction.md`), trade, MIO progression, licensing (see game wiki *Production §生产授权*).
>
> **Sources & version anchors**
> - `[w]` — game wiki *Production* (original: `hoi4.paradoxwikis.com/Production`), read via the Chinese mirror `cooderatom.github.io/hoi4-zh-wiki/Production.html` (mirror snapshot 2026-09-10, rev 104639; the English site is unreachable from this environment). Section anchors use mirror titles.
> - `[m]` — modding wiki offline snapshot files in `[[hoi4-mod]]`'s `references/`.
> - `[v]` — vanilla install **1.18.3 "Case Green"** (`launcher-settings.json`), root `/mnt/data/hoi4/Hearts of Iron IV`.
>
> Number resolution: target-version `[v]` > `[m]` > `[w]`; conflicts are listed under `## Unknowns`, never silently resolved.

## Player view

- Three factory types: civilian (construction, trade, consumer goods, licenses, intelligence), military (land + air equipment), naval dockyards (ships, submarines, convoys). [w:Production §工厂]
- Displayed daily output per factory: civilian 4 → 5, military 3.5 → 4.5, dockyard 2 → 2.5 when powered. [w:Production §工厂]
- Military and naval equipment runs on production lines: one item per line; the line box shows item and queue position, assigned MIO, efficiency, factory count, delete button, priority up/down, conversion-mode toggle, variant role, designer, needed/stockpiled counts, and resource icons that turn red when short. [w:Production §生产线] [w:Production §生产线界面]
- A line holds at most 150 factories and several lines can build the same item; ship lines cap dockyards (5 capital ship, 10 screens/submarines, 15 convoys, 5 floating harbor), and special army units (railway guns, super-heavy howitzers, land cruisers) cap at 5 factories. [w:Production §生产线]
- Efficiency starts at 10% (unmodified) and grows daily toward a 50% cap (unmodified); industry tech, research and focuses raise both. [w:Production §生产效率]
- Switching a line keeps part of the efficiency depending on similarity: 90% same-model variant, 70% same-family model, 30% direct upgrade/downgrade, 20% indirect upgrade/downgrade, 10% anything else; Dispersed Industry I–V and Flexible Line reduce the loss. [w:Production §生产效率保持]
- Seven strategic resources: oil (fuel), aluminium, rubber, tungsten, steel, chromium, coal; shortage hits the lowest-priority lines first and reaches −100%; more resources come from trade, infrastructure, excavation tech, prospecting, synthetic refineries, conquest, or peace-conference rights. [w:Production §资源] [w:Production §获取资源]
- A share of civilian factories is reserved for consumer goods, so the player sees fewer civilian factories available for construction. [w:Production §工厂]
- Lines can be toggled to conversion mode: obsolete or foreign same-chassis equipment is turned into the target item with no efficiency loss, and the line falls back to building from scratch when nothing convertible is in stock. [w:Production §装备转换]
- Ships are refitted (Task Force panel), not converted; a refit can be cancelled safely below 20% progress. [w:Production §装备转换]

## Engine view

- Base factory speed multipliers: `BASE_FACTORY_SPEED` = 4 (civ), `_MIL` = 3.5, `_NAV` = 2.0; powered (`POWERED_*`): 5 / 4.5 / 2.5. [v:common/defines/00_defines.lua → NProduction.BASE_FACTORY_SPEED, BASE_FACTORY_SPEED_MIL, BASE_FACTORY_SPEED_NAV, POWERED_FACTORY_SPEED, POWERED_FACTORY_SPEED_MIL, POWERED_FACTORY_SPEED_NAV @1.18.3] [w:Production §工厂]
- Military factory output = base speed × factory-output modifiers × production efficiency; the two modifiers apply independently. [w:Production §工厂]
- Efficiency growth: daily gain = `0.001 · cap² / current`; time to a level t = `500 · (eff/cap)²` days; reaching the cap takes up to 500 days and costs up to ≈166.6 factory-days of output versus running at cap from day one. [w:Production §生产效率]
- Efficiency bounds are defines: start `BASE_FACTORY_START_EFFICIENCY_FACTOR` = 10, cap `BASE_FACTORY_MAX_EFFICIENCY_FACTOR` = 50. [v:common/defines/00_defines.lua → NProduction.BASE_FACTORY_START_EFFICIENCY_FACTOR, BASE_FACTORY_MAX_EFFICIENCY_FACTOR @1.18.3] [w:Production §生产效率]
- Efficiency is tracked per factory slot; adding factories leaves existing slots untouched and starts the new ones low, and unused slots decay daily (`EFFICIENCY_LOSS_PER_UNUSED_DAY` = 1); when factories are lost, the losses come off the bottom of the list. [w:Production §生产效率] [v:common/defines/00_defines.lua → NProduction.EFFICIENCY_LOSS_PER_UNUSED_DAY @1.18.3]
- Retention factors are defines: variant 90, family 70, parent↔child 30, archetype 20, unrelated 10 (`BASE_FACTORY_EFFICIENCY_BALANCE_FACTOR` = 0.1). [v:common/defines/00_defines.lua → NProduction.BASE_FACTORY_EFFICIENCY_VARIANT_CHANGE_FACTOR, ..._FAMILY_CHANGE_FACTOR, ..._PARENT_CHANGE_FACTOR, ..._ARCHETYPE_CHANGE_FACTOR, ..._BALANCE_FACTOR @1.18.3] [w:Production §生产效率保持]
- On a switch with retention bonuses: `new = CPE · (RET + (1 − RET) · BON)`, floored at the start efficiency — the bonus reduces the loss, it is not added to the retention. [w:Production §计算含保持加成的生产效率]
- Line caps: `MAX_MIL_FACTORIES_PER_LINE` = 150; naval `DEFAULT_MAX_NAV_FACTORIES_PER_LINE` = 10, `CAPITAL_SHIP_MAX_NAV_FACTORIES_PER_LINE` = 5, `CONVOY_MAX_NAV_FACTORIES_PER_LINE` = 15, `FLOATING_HARBOR_MAX_NAV_FACTORIES_PER_LINE` = 5; `RAILWAY_GUN_MAX_MIL_FACTORIES_PER_LINE` = 5. [v:common/defines/00_defines.lua → NProduction.* @1.18.3] [w:Production §生产线]
- Resource shortage: each missing unit adds −5% per factory (`PRODUCTION_RESOURCE_LACK_PENALTY` = −0.05); per factory the worst resource penalty applies, capped at −100%; the line displays the average and turns red above a 0.8 penalty ratio (`RESOURCE_PENALTY_WARNING_CRITICAL_RATIO`). [v:common/defines/00_defines.lua → NProduction.PRODUCTION_RESOURCE_LACK_PENALTY, RESOURCE_PENALTY_WARNING_CRITICAL_RATIO @1.18.3] [w:Production §资源]
- Equipment can depend on at most 3 different strategic resources (`MAX_EQUIPMENT_RESOURCES_NEED` = 3). [v:common/defines/00_defines.lua → NProduction.MAX_EQUIPMENT_RESOURCES_NEED @1.18.3]
- Extraction bonuses: excavation tech +10% per level (5 levels) and infrastructure +15% per level (`NBuildings.INFRASTRUCTURE_RESOURCE_BONUS` = 0.15) apply to excavated resources only; a state connected to the capital by rail or sea gets +20% (`NBuildings.SUPPLY_ROUTE_RESOURCE_BONUS` = 0.2). [w:Production §资源] [v:common/defines/00_defines.lua → NBuildings.INFRASTRUCTURE_RESOURCE_BONUS, SUPPLY_ROUTE_RESOURCE_BONUS @1.18.3]
- Resources cannot be stockpiled, except fuel refined from oil. [w:Production §资源]
- Land conversion cost = `(base chassis cost × (0.9 − tech) + new module cost) × production modifiers + scrap cost` (`BASE_LAND_EQUIPMENT_CONVERSION_IC_COST_FACTOR` = 0.9); a conversion consumes strategic resources only when the new design needs resources the old one did not. [w:Production §装备转换] [v:common/defines/00_defines.lua → NProduction.BASE_LAND_EQUIPMENT_CONVERSION_IC_COST_FACTOR @1.18.3]
- Naval refit cost = `old design cost × (0.2 + new module production modifier) + (module upgrade or new module cost) × production modifier + scrap cost` (`BASE_NAVAL_EQUIPMENT_CONVERSION_IC_COST_FACTOR` = 0.2). [w:Production §装备转换] [v:common/defines/00_defines.lua → NProduction.BASE_NAVAL_EQUIPMENT_CONVERSION_IC_COST_FACTOR @1.18.3]
- Conversion-speed bonuses cut the price difference between converted and new-built equipment, not the whole cost (wiki example: `30 − 10 × (1 + 0.4) = 16`); naval refit-speed bonuses instead act as extra dockyard output while refitting. [w:Production §装备转换速度] [w:Production §舰船改装速度]
- Ship refit cancellation is free below 20% progress (`SHIP_REFIT_MAX_PROGRESS_TO_CANCEL` = 0.2). [v:common/defines/00_defines.lua → NProduction.SHIP_REFIT_MAX_PROGRESS_TO_CANCEL @1.18.3] [w:Production §装备转换]
- Dockyards have no efficiency; each dockyard delivers 2 IC/day by default (2.5 powered), modified only by dockyard output. [w:Production §舰船] [v:common/defines/00_defines.lua → NProduction.BASE_FACTORY_SPEED_NAV, POWERED_FACTORY_SPEED_NAV @1.18.3]
- Railway guns and similar special equipment gain no passive efficiency growth; only base efficiency applies. [w:Production §生产效率]
- Consumer goods: the economy law sets the baseline (`consumer_goods_expected_value`, 0.50 down to 0.10 across vanilla laws); the `consumer_goods_factor` modifier (spirits, ideas, advisors) adjusts it multiplicatively; at minimum 1 factory or 10% is taken. [v:common/ideas/_economic.txt → consumer_goods_expected_value @1.18.3] [m:Modifiers.md → consumer_goods_expected_value, consumer_goods_factor] [v:common/defines/00_defines.lua → NProduction.MINIMUM_NUMBER_OF_FACTORIES_TAKEN_BY_CONSUMER_GOODS_VALUE=1, _PERCENT=0.1 @1.18.3]

## Modding surfaces

- Base speeds, line caps, penalties: override the `NProduction` block in `common/defines/` → [[hoi4-define]], `Defines.md`. [m:Defines.md §NProduction] [v:common/defines/00_defines.lua → NProduction @1.18.3]
- Factory output modifiers: `industrial_capacity_factory` (Factory Output), `industrial_capacity_dockyard` (Dockyard Output). [m:List of modifiers.md → industrial_capacity_factory, industrial_capacity_dockyard]
- Efficiency modifiers: `production_factory_start_efficiency_factor`, `production_factory_max_efficiency_factor`, `production_factory_efficiency_gain_factor`, `line_change_production_efficiency_factor` (Production Efficiency Retention). [m:List of modifiers.md → production_factory_*, line_change_production_efficiency_factor]
- Consumer goods: `consumer_goods_expected_value` and `consumer_goods_factor` are modifier keys usable in ideas/focuses; `consumer_goods_factor` combines multiplicatively across sources. [m:Modifiers.md → consumer_goods_factor] [m:List of modifiers.md → consumer_goods_factor]
- Economy laws live in `common/ideas/_economic.txt` (category `economy` with `law = yes`); law files load in Unicode filename order — `_economic.txt` loads after `TAG.txt`, a pitfall when overriding laws. [m:Idea modding.md → law = yes; file order] [v:common/ideas/_economic.txt @1.18.3]
- Equipment resource costs are set per equipment with `resources = { ... }` in `common/units/equipment/*.txt` → Equipment modding.md, [[hoi4-tech]]. [m:Equipment modding.md → resources] [v:common/units/equipment @1.18.3]
- Industry techs that change output/efficiency → `common/technologies/*.txt`, Technology modding.md, [[hoi4-tech]]. [m:Technology modding.md] [v:common/technologies @1.18.3]
- MIO traits modify the equipment produced under them; MIOs and policies load only when a new game is created. [m:Military industrial organization modding.md → Warning]
- Buildings (civilian/military/dockyard definitions, costs) → Building modding.md; costs are priced in industrial capacity. [m:Building modding.md → Buildings, Cost] [v:common/buildings @1.18.3]
- Strategic resource definitions and icon strip → Resources modding.md; `common/resources/00_resources.txt`. [m:Resources modding.md → Resources] [v:common/resources/00_resources.txt @1.18.3]

## Unknowns

- **Defines version conflict**: [m:Defines.md §NProduction] lists `BASE_FACTORY_SPEED` = 5 / `_MIL` = 4.50 / `_NAV` = 2.5 and has no `POWERED_*` entries; `[v]` 1.18.3 has 4 / 3.5 / 2.0 plus `POWERED_*` 5 / 4.5 / 2.5, and `[w]` matches `[v]`. Treat base-speed numbers as version-specific; check the target release's defines.
- **Efficiency formula coefficient unsourced**: neither source states that the wiki's `0.001` coefficient equals `BASE_FACTORY_EFFICIENCY_GAIN × BASE_FACTORY_EFFICIENCY_BALANCE_FACTOR / 100` (inference, not sourced).
- **Energy/coal not distilled**: `[v]` has energy defines (`RESOURCE_TO_ENERGY_COEFFICIENT`, `BASE_ENERGY_COST`, ...) governing the powered bonus; generation/cost mechanics are out of scope here.
- **Consumer-goods allocation flow not distilled**: exact factory rounding/priority for consumer goods is not covered by the sources listed here; planned with `Construction.md`.
- **Licensing not covered** (penalties, license cost ladder): see [w:Production §生产授权] and `[v]` `LICENSE_*` defines in `NProduction`.
