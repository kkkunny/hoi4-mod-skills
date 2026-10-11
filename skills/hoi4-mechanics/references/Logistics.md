# Logistics — Mechanics

> **Topic** — supply: hub supply, state supply, air supply, trains and trucks, and the effects of shortage.
> **Scope** — covered: the supply system post-1.11. Not covered: fuel transport specifics (`Fuel.md`), building/rail construction UI (`Construction.md`), convoy warfare.
>
> **Sources & version anchors**
> - [w] game wiki *Logistics* (original: hoi4.paradoxwikis.com/Logistics), read via the Chinese mirror `cooderatom.github.io/hoi4-zh-wiki/Logistics.html` (mirror snapshot 2026-09-10, rev 96361, 100% translated). Section anchors use mirror titles.
> - [m] modding wiki offline snapshot files in `[[hoi4-mod]]`'s `references/`.
> - [v] vanilla install **1.18.3 "Case Green"** (`launcher-settings.json`), root `/mnt/data/hoi4/Hearts of Iron IV`.
>
> Number resolution: target-version [v] > [m] > [w]; conflicts are listed under `## Unknowns`.

## Player view

- Supply is not produced: a province's supply level is a carrying capacity, and each unit also has its own internal supply state; the three sources are hub supply (hubs/ports + rails + convoys), air supply (transport planes) and state supply (fixed per state). [w:Logistics §补给]
- Draw order is state supply first, hub supply second, air supply last; a unit pulls state supply even when hub supply already covers its needs. [w:Logistics §补给]
- Supply map mode (default F4): light blue = plenty, deep blue = little, purple = little/none but nobody affected, yellow = insufficient but partial, bright red = severe penalties; tooltips on units/hubs/ports/provinces show draw volumes and the rail connection to the capital. [w:Logistics §补给地图模式]
- Hub capacity: a rail connection to the capital gives 15 at rail level 1 and +5 per level up to 35 (level 5); a port link gives 8 at port level 1 and +3 per level up to 35 (level 10); the weakest link on the path caps the whole connection, and rivers count as a level-1 rail link. [w:Logistics §枢纽补给总能力]
- The capital hub is special: base 5 plus 0.3 per civilian factory, 0.6 per military factory and 0.4 per dockyard in the country. [w:Logistics §枢纽补给总能力]
- Hub range decays per province of distance; improve it by motorizing the hub (0/40/80 trucks, consuming neither fuel nor manpower), motorizing the army (auto-applies to hubs inside its supply range), upgrading rail links, upgrading infrastructure (max level reduces flow drop-off by 0.3), or building new hubs — the effects stack, but distance decay remains dominant. [w:Logistics §提升补给枢纽范围 §摩托化 §提高铁路连接等级 §提高基础设施等级 §新建补给枢纽]
- Trains run the rail network: hub tooltips show the trains needed, the logistics tooltip shows network stock vs need, and a shortfall cuts hub supply; train types are basic, squeeze (cheaper) and armored (resists logistic strikes); trains take no attrition but can be bombed. [w:Logistics §列车 §装甲列车]
- Captured rail must convert before use: 10 days in non-core territory, 5 in core territory, 0 in a civil war; rivers convert instantly; a hub needs an unbroken converted connection. [w:Logistics §夺取铁路与补给枢纽]
- State supply comes from victory points (0.2 base plus 0.05 per VP point; 50 VP ≈ 2.70), infrastructure (0.3 per level; damaged infra counts less), population (diminishing returns) and base supply; it is shared by every unit in the state — air wings and ships included, even hub-supplied ones — and contested states split it between controllers. [w:Logistics §州补给 §争夺中的州]
- Air supply: transport planes on the air-supply mission provide supply usable anywhere in their air region regardless of flight range, and it is drawn only after state and hub supply. [w:Logistics §空运补给]
- Supply shortage (red box icons) reduces speed, max org, org recovery, attacks (defender up to −50%, attacker up to −20%), breakthrough/defence; attrition starts below 35% supply; manpower reinforcement slows while equipment reinforcement is unaffected; being cut off from the capital blocks manpower, equipment and fuel reinforcement entirely; air units lose mission efficiency from their base's supply; naval units rarely suffer since they draw full hub supply, but recon/repair/fuel are affected. [w:Logistics §补给不足的影响 §陆军单位 §空军单位 §海军单位]
- Stored supply (supply grace) buffers about 2–3 days at full efficiency after leaving good supply; special-forces techs and an amphibious commander extend it. [w:Logistics §储存补给／补给宽限]
- Allies share hubs by default and can be toggled off per hub; you cannot use state supply on allied territory. [w:Logistics §盟友与补给]

## Engine view

- Rail flow: `NSupply.RAILWAY_BASE_FLOW = 10` plus `NSupply.RAILWAY_FLOW_PER_LEVEL = 5` per level (level 1 = 15 … level 5 = 35); damaged rail subtracts `NSupply.RAILWAY_FLOW_PENALTY_PER_DAMAGED = 5` down to `NSupply.RAILWAY_MIN_FLOW = 5`; `NSupply.MAX_RAILWAY_LEVEL = 5`. [v:common/defines/00_defines.lua → NSupply.RAILWAY_BASE_FLOW, RAILWAY_FLOW_PER_LEVEL, RAILWAY_FLOW_PENALTY_PER_DAMAGED, RAILWAY_MIN_FLOW, MAX_RAILWAY_LEVEL @1.18.3] [w:Logistics §枢纽补给总能力]
- Port flow: `NSupply.NAVAL_BASE_FLOW = 5` plus `NSupply.NAVAL_FLOW_PER_LEVEL = 3` (level 1 = 8 … level 10 = 35). [v:common/defines/00_defines.lua → NSupply.NAVAL_BASE_FLOW, NAVAL_FLOW_PER_LEVEL @1.18.3] [w:Logistics §枢纽补给总能力]
- Capital hub capacity: `NSupply.CAPITAL_SUPPLY_BASE = 5`, `NSupply.CAPITAL_SUPPLY_CIVILIAN_FACTORIES = 0.3`, `NSupply.CAPITAL_SUPPLY_MILITARY_FACTORIES = 0.6`, `NSupply.CAPITAL_SUPPLY_DOCKYARDS = 0.4`. [v:common/defines/00_defines.lua → NSupply.CAPITAL_SUPPLY_BASE, NSupply.CAPITAL_SUPPLY_CIVILIAN_FACTORIES, NSupply.CAPITAL_SUPPLY_MILITARY_FACTORIES, NSupply.CAPITAL_SUPPLY_DOCKYARDS @1.18.3]
- Distribution falloff: hubs `NSupply.NODE_INITIAL_SUPPLY_FLOW = 2.8` with `NODE_STARTING_PENALTY_PER_PROVINCE = 0.5` and `NODE_ADDED_PENALTY_PER_PROVINCE = 0.7`; naval hubs `NAVAL_BASE_INITIAL_SUPPLY_FLOW = 3.3` (0.84 / 1.1); capital 5.0 (0.5 / 1.2); the distance curve is sigmoid (`SUPPLY_FLOW_DIST_LOGISTICS_FALLOFF_K = 1.3`, `..._MIDPOINT = 2.3`, `..._SCALAR = 0.9`, `..._MIN_PENALTY_SCALE = 0.25`); rivers `SUPPLY_FLOW_PENALTY_CROSSING_RIVERS = 0.2`; max infrastructure reduces drop-off by `SUPPLY_FLOW_DROP_REDUCTION_AT_MAX_INFRA = 0.3`. [v:common/defines/00_defines.lua → NSupply.* @1.18.3] [w:Logistics §枢纽补给范围 §天气与地形]
- Motorization: full bonus `NSupply.SUPPLY_HUB_FULL_MOTORIZATION_BONUS = 2.2`, truck cost `SUPPLY_HUB_FULL_MOTORIZATION_TRUCK_COST = 60`, diminishing returns `SUPPLY_HUB_MOTORIZATION_MARGINAL_EFFECT_DECAY = 1.6`, rail-level bonus `NODE_FLOW_BONUS_PER_RAIL_LEVEL = 0.34`. [v:common/defines/00_defines.lua → NSupply.* @1.18.3]
- State supply: `NSupply.INFRA_TO_SUPPLY = 0.3` per level, `NSupply.VP_TO_SUPPLY_BASE = 0.2`, `NSupply.VP_TO_SUPPLY_BONUS_CONVERSION = 0.05` per VP point, damaged infra counts as `SUPPLY_FROM_DAMAGED_INFRA = 0.15`, and `NSupply.SUPPLY_BASE_MULT = 0.2` (one unit of the state field `local_supplies` equals 0.2 supply). [v:common/defines/00_defines.lua → NSupply.* @1.18.3] [m:State modding.md → local_supplies] [w:Logistics §州补给]
- Capture cooldowns: `NSupply.RAILWAY_CONVERSION_COOLDOWN = 10`, `..._CORE = 5`, `..._CIVILWAR = 0`; `NSupply.RIVER_RAILWAY_LEVEL = 1`. [v:common/defines/00_defines.lua → NSupply.* @1.18.3]
- Trains and trucks: `NSupply.SUPPLY_POINTS_PER_TRAIN = 1.0`, `NUM_RAILWAYS_TRAIN_FACTOR = 0.03`, `MIN_TRAIN_SUPPLY_FACTOR = 0.5`, `MIN_TRAIN_REQUIREMENT = 2`, `TRUCK_ATTRITION = 0.003`, `TRUCK_ATTRITION_FACTOR = 0.65`, `BASE_TRUCK_HP = 100`, `BASE_TRAIN_HP = 100`, `DEFAULT_STARTING_TRUCK_RATIO = 1.5`, `DEFAULT_STARTING_TRAIN_RATIO = 1`; train survivability uses `TRAIN_ARMOR_TARGETING_WEIGHT = 0.01` and `TRAIN_ANTI_AIR_*`. [v:common/defines/00_defines.lua → NSupply.* @1.18.3] [w:Logistics §列车]
- Shortage penalties: `NMilitary.SUPPLY_GRACE = 72` hours (3 days), attrition threshold `NSupply.SUPPLY_THRESHOLD_FOR_ARMY_ATTRITION = 0.35`, out-of-supply caps `NMilitary.MAX_OUT_OF_SUPPLY_DAYS = 30`, `OUT_OF_SUPPLY_ATTRITION = 0.2`, `OUT_OF_SUPPLY_SPEED = -0.8`, `OUT_OF_SUPPLY_MORALE = -0.2`. [v:common/defines/00_defines.lua → NMilitary.*, NSupply.* @1.18.3] [w:Logistics §补给不足的影响]
- Air supply conversion: `NCountry.AIR_SUPPLY_CONVERSION_SCALE = 0.01`. [v:common/defines/00_defines.lua → NCountry.AIR_SUPPLY_CONVERSION_SCALE @1.18.3] [w:Logistics §空运补给]
- Capital relocation: requires `NSupply.MIN_SURRENDER_LIMIT_TO_MOVE_SUPPLY_CAPITAL = 0.15`, cooldown `COOLDOWN_DAYS_AFTER_MOVING_SUPPLY_CAPITAL = 30`, supply resumes after `DAYS_TO_START_GIVING_SUPPLY_AFTER_MOVING_SUPPLY_CAPITAL = 7` and is full after `DAYS_TO_START_GIVING_FULL_SUPPLY_AFTER_MOVING_SUPPLY_CAPITAL = 21`; `SUPPLY_PATH_MAX_DISTANCE = 15`; disruption recovers `SUPPLY_DISRUPTION_DAILY_RECOVERY = 1.5` per day; army supply ratio climbs `ARMY_SUPPLY_RATIO_SPEED_GAIN_PER_HOUR = 0.01` per hour up to `ARMY_MAX_SUPPLY_RATIO_GAIN_PER_HOUR = 0.15`. [v:common/defines/00_defines.lua → NSupply.* @1.18.3]
- Node-type base supply: `NSupply.BUILT_NODE_BASE_SUPPLY_ADD = 0.6`, `LOCAL_NODE_BASE_SUPPLY_ADD = 0.5`, `NAVAL_NODE_BASE_SUPPLY_ADD = 0.3`, `CAPITAL_NODE_BASE_SUPPLY_ADD = 0`. [v:common/defines/00_defines.lua → NSupply.* @1.18.3]
- Building costs: rail `base_cost = 170` plus `per_level_extra_cost = 130`; supply hub `base_cost = 20000`; naval base `5000 + 1000` per level. [v:common/buildings/00_buildings.txt → rail_way, supply_node, naval_base @1.18.3] [w:Construction §省级建筑]

## Modding surfaces

- Rail and supply-node placement → `map/supply_nodes.txt` and `map/railways.txt` (invalid definitions crash the game) → Map modding.md, [[hoi4-map]]; mid-game rail construction uses the `build_railway` effect, while default building effects on them crash → Building modding.md, Effect.md. [m:Map modding.md → Supply nodes and railways] [m:Building modding.md → supply_node, rail_way] [m:Effect.md → build_railway]
- State base supply: the `local_supplies` field in state history (1 unit = 0.2 supply) → State modding.md, [[hoi4-map]]. [m:State modding.md → local_supplies]
- Supply modifiers: `supply_consumption_factor`, `out_of_supply_factor`, `no_supply_grace`, `extra_marine_supply_grace`, `extra_paratrooper_supply_grace`, `special_forces_no_supply_grace`, `special_forces_out_of_supply_factor`. [m:List of modifiers.md → supply_consumption_factor, out_of_supply_factor, no_supply_grace, extra_marine_supply_grace]
- Supply hubs/ports/rails are normal building definitions plus a naval supply hub type (`naval_supply_hub`) → Building modding.md, [[hoi4-map]]. [m:Building modding.md → Supply hub, Naval supply hub, Railway]
- NSupply defines → Defines.md, [[hoi4-define]]. [m:Defines.md §NSupply]

## Unknowns

- Air supply conversion: only `NCountry.AIR_SUPPLY_CONVERSION_SCALE = 0.01` is anchored; how plane count and mission efficiency map to actual air supply is not stated by the sources used (unsourced).
- Naval supply behaviour is wiki prose; no navy-specific supply defines beyond fuel were checked (unsourced).
- `NODE_FLOW_BONUS_PER_RAIL_LEVEL`, `BUILT_NODE_BASE_SUPPLY_ADD`, `LOCAL_NODE_BASE_SUPPLY_ADD`, `NAVAL_NODE_BASE_SUPPLY_ADD` are cited from their define comments only; exact formulas were not traced (unsourced).
