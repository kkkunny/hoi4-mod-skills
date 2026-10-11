# Fuel — Mechanics

> **Topic** — fuel: production from oil, storage, and consumption by army, air force and navy.
> **Scope** — covered: the fuel economy. Not covered: oil/resource trade (`Trade.md`), supply network details (future `Logistics.md`), raids that target fuel or refineries (future `Raid.md`).
>
> **Sources & version anchors**
> - [w] game wiki *Fuel* (rev 94544, 100% translated), via the Chinese mirror `cooderatom.github.io/hoi4-zh-wiki/Fuel.html`, snapshot 2026-09-10. Section anchors use mirror titles.
> - [m] modding wiki offline snapshot files in `[[hoi4-mod]]`'s `references/`.
> - [v] vanilla install **1.18.3 "Case Green"** (`launcher-settings.json`), root `/mnt/data/hoi4/Hearts of Iron IV`.
>
> Number resolution: target-version [v] > [m] > [w]; conflicts are listed under `## Unknowns`.

## Player view

- Fuel runs trucks, tanks, aircraft and ships; it is refined from oil at 48 fuel per day per unit of oil, plus a flat 48/day base for every country; synthetic refineries add 48–168/day depending on tech and ignore economy-law penalties; economy laws below War Economy reduce the oil→fuel conversion. [w:Fuel §生产]
- Fuel is storable, unlike other resources: base 50000, +1500 per infrastructure level, +100000 per fuel silo level, with some economy laws lowering capacity; countries start 25% full, capitulation hands 50% to the enemy and annexation transfers 25%. [w:Fuel §储存]
- The logistics window sets the fuel-priority order between army, navy and air force. [w:Fuel §消耗]
- Army: divisions with oil-burning equipment (trucks, halftracks, tanks, self-propelled guns; some support companies count) burn 12× their nominal daily fuel; 0 while idle, strategic redeploying or being naval-transported; ×2 while fighting and moving; internal reserve lasts 4 days (extendable by modules such as fuel drums); penalties begin below 25% reserve; at zero fuel speed drops to ×0.4, oil-powered equipment stats to ×0.1, and training grants no XP; units refill at up to 2× consumption while in supply, and fuel flow stops entirely when a supply area is ≥50% overloaded. [w:Fuel §陆军]
- Air: every mission consumes fuel (rocket interceptors are the exception) at 8.4× the nominal daily rate; day-only or night-only operations reduce it; intercepting is 0.2×, kamikaze 0.75×, port strike 1.2×, logistics strike 1.2×, training 0.6×; out of fuel, mission efficiency is ×0.25. [w:Fuel §飞机]
- Navy: 2.4× the nominal daily rate; ×2 in combat, 0 while idle or in port; training 0.6× with escort ships at 0.15×; ships refuel at naval bases at 2× rate scaled by the base's supply state; out of fuel: speed −75%, range 0%, attack −50%, submarine torpedo attack −80%. [w:Fuel §舰船]

## Engine view

- Oil is the designated fuel resource: `NGame.FUEL_RESOURCE = "oil"`; only one resource can carry the fuel dynamic at a time. [v:common/defines/00_defines.lua → NGame.FUEL_RESOURCE @1.18.3] [m:Resources modding.md → Additional notes]
- Country fuel: `NCountry.BASE_FUEL_GAIN_PER_OIL = 2` per hour (×24 = 48/day per oil), `NCountry.BASE_FUEL_GAIN = 2.0` per hour flat, `NCountry.BASE_FUEL_CAPACITY = 50000`, `NCountry.STARTING_FUEL_RATIO = 0.25`. [v:common/defines/00_defines.lua → NCountry.BASE_FUEL_GAIN_PER_OIL, NCountry.BASE_FUEL_GAIN, NCountry.BASE_FUEL_CAPACITY, NCountry.STARTING_FUEL_RATIO @1.18.3] [w:Fuel §生产] [w:Fuel §储存]
- Fuel seized on capitulation and annexation: `NProduction.CAPITULATE_FUEL_RATIO = 0.5` and `NProduction.ANNEX_FUEL_RATIO = 0.25`. [v:common/defines/00_defines.lua → NProduction.CAPITULATE_FUEL_RATIO, NProduction.ANNEX_FUEL_RATIO @1.18.3] [w:Fuel §储存]
- Army constants: `NMilitary.ARMY_FUEL_COST_MULT = 0.5` per hour (12/day), `NMilitary.ARMY_IDLE_FUEL_MULT = 0`, `NMilitary.ARMY_STRATEGIC_DEPLOYMENT_FUEL_MULT = 0`, `NMilitary.ARMY_NAVAL_TRANSFER_FUEL_MULT = 0`, `NMilitary.ARMY_COMBAT_FUEL_MULT = 1` and `NMilitary.ARMY_MOVEMENT_FUEL_MULT = 1` (they stack), `NMilitary.FUEL_CAPACITY_DEFAULT_HOURS = 96`, `NMilitary.FUEL_PENALTY_START_RATIO = 0.25`, `NMilitary.OUT_OF_FUEL_SPEED_MULT = 0.4`, `NMilitary.OUT_OF_FUEL_EQUIPMENT_MULT = 0.1`, `NMilitary.OUT_OF_FUEL_TRAINING_XP_GAIN_MULT = 0`, `NMilitary.ARMY_MAX_FUEL_FLOW_MULT = 2`, `NMilitary.SURPLUS_SUPPLY_RATIO_FOR_ZERO_FUEL_FLOW = 0.5`. [v:common/defines/00_defines.lua → NMilitary.* @1.18.3] [w:Fuel §陆军]
- Air: `NAir.FUEL_COST_MULT = 0.35` per hour (8.4/day), `NAir.MISSION_FUEL_COSTS` array (intercept 0.2, kamikaze 0.75, port strike 1.2, logistics strike 1.2, training 0.6), `NAir.MISSION_EFFICIENCY_MULT_AT_LACK_OF_FUEL = 0.25`. [v:common/defines/00_defines.lua → NAir.FUEL_COST_MULT, NAir.MISSION_FUEL_COSTS, NAir.MISSION_EFFICIENCY_MULT_AT_LACK_OF_FUEL @1.18.3] [w:Fuel §飞机]
- Navy: `NNavy.FUEL_COST_MULT = 0.1` per hour (2.4/day), `NNavy.IN_COMBAT_FUEL_COST = 2`, `NNavy.ON_BASE_FUEL_COST = 0`, `NNavy.TRAINING_FUEL_COST_FOR_ESCORT_SHIPS = 0.15`, `NNavy.MAX_FUEL_FLOW_MULT`, and the out-of-fuel factors `NNavy.OUT_OF_FUEL_SPEED_FACTOR = −0.75`, `NNavy.OUT_OF_FUEL_RANGE_FACTOR = 0`, `NNavy.OUT_OF_FUEL_ATTACK_FACTOR = −0.5`, `NNavy.OUT_OF_FUEL_TORPEDO_FACTOR = −0.8`. [v:common/defines/00_defines.lua → NNavy.* @1.18.3] [w:Fuel §舰船]
- Building-side fuel: infrastructure `max_fuel_building = 1.5` (1500 capacity/level), fuel silo `max_fuel_building = 100` (100000/level), synthetic refinery `fuel_gain_from_states = 2.0` per hour base. [v:common/buildings/00_buildings.txt → infrastructure, fuel_silo, synthetic_refinery @1.18.3]

## Modding surfaces

- Fuel modifiers: `base_fuel_gain`, `base_fuel_gain_factor`, `fuel_gain`, `fuel_gain_factor` (per oil), `fuel_gain_from_states`, `fuel_gain_factor_from_states` (refineries), `fuel_cost`, `max_fuel_building`, `army_fuel_capacity_factor`. [m:List of modifiers.md → base_fuel_gain, fuel_gain, fuel_cost, max_fuel_building, army_fuel_capacity_factor] [m:Modifiers.md → fuel_cost, base_fuel_gain, fuel_gain, fuel_gain_from_states]
- Fuel capacity buildings: fuel silo and infrastructure are normal building definitions → Building modding.md. [m:Building modding.md → Buildings] [v:common/buildings/00_buildings.txt @1.18.3]
- Equipment fuel use is defined per equipment (`fuel_consumption`) and per sub-unit (`own_equipment_fuel_consumption_mult`). [v:common/units/equipment/ @1.18.3] [v:common/units/ @1.18.3]
- Fuel-affecting economy law effects and fuel techs live in `common/ideas/_economic.txt` and `common/technologies/` → [[hoi4-idea]], [[hoi4-tech]]. [v:common/ideas/_economic.txt @1.18.3] [v:common/technologies/ @1.18.3]
- Designating which resource is fuel → Resources modding.md note on `FUEL_RESOURCE`. [m:Resources modding.md → Additional notes]

## Unknowns

- The rocket-interceptor fuel exemption is stated only by the wiki (no file evidence checked).
- The full per-mission aircraft fuel array exists in `[v]` but only the five ratios quoted by the wiki are listed here.
- `NNavy.MAX_FUEL_FLOW_MULT` value was not verified in this pass (key exists; the wiki states 2).
- How the logistics window resolves fuel priority between army/navy/air is not documented in the sources used (unsourced).
