# Attrition — Mechanics

> **Topic** — attrition (land) and accidents (air/navy): causes, org and equipment loss formulas, and mitigation.
> **Scope** — covered: losses outside combat. Not covered: combat losses (`Land battle.md`), terrain/weather values (`Terrain and weather.md`), resistance/occupation systems (future `Occupation.md`).
>
> **Sources & version anchors**
> - [w] game wiki *Attrition_and_accidents* (rev 103007, 99% translated), via the Chinese mirror `cooderatom.github.io/hoi4-zh-wiki/Attrition_and_accidents.html`, snapshot 2026-09-10. Section anchors use mirror titles.
> - [m] modding wiki offline snapshot files in `[[hoi4-mod]]`'s `references/`.
> - [v] vanilla install **1.18.3 "Case Green"** (`launcher-settings.json`), root `/mnt/data/hoi4/Hearts of Iron IV`.
>
> Number resolution: target-version [v] > [m] > [w]; conflicts are listed under `## Unknowns`.

## Player view

- Attrition costs equipment and organization outside combat (never manpower); only land units suffer attrition, while air and naval units suffer accidents with similar effects, especially in training or bad weather. [w:Attrition §导言]
- Base attrition is 0%; causes stack: training +5%; hostile terrain (desert/jungle/marsh/mountain) up to +50%; weather up to +70%; resistance up to +30%; border wars up to +10%; supply below 35% up to +20%; a moving unit multiplies the total by 1.2. [w:Attrition §Causes]
- Map-off supply trucks: base 0.3%; all sources are scaled ×0.65 but terrain effects reach +400% and weather +500% for them specifically. [w:Attrition §Causes]
- Organization: a division loses 0.08 × attrition org per hour regardless of combat; an out-of-supply division also loses up to −0.2 recovery, maxing out after 30 days. [w:Attrition §组织度损失]
- Equipment: hourly loss = 0.005 × attrition × equipment count × (1 − reliability) → a daily rate of about 12% × attrition × (1 − reliability); reliability above 100% gives no extra protection; every equipment variant is computed separately. [w:Attrition §装备损失]
- Air accidents: base 10% adjusted by a 0.1× balance factor, reliability (up to 2× at 0% reliability), carriers 1.5× and pilot exercises +20%; hourly chance = 10% × modifiers × (100% − reliability) × 2; average effect 0.35%; average loss 0.07% × (100% − reliability) per hour per airbase — about 0.34%/day at 80% reliability and 0.17%/day at 90%; losses are low enough that combat stats are usually worth more than reliability. [w:Attrition §空军单位]
- Naval accidents: ships in exercises have 2% chance per hour to suffer org −30%, HP −5% and 4 extra strength damage, with a 30% chance to crit for ×4; mines in mined waters trigger at up to 5% saturation for org −50%, 50 strength damage and a 14% crit chance; Pride of the Fleet scales accident and crit chances by 0.01; a 4% balance factor applies to naval accidents overall. [w:Attrition §海军单位]
- Reducing losses: maintenance companies and higher-reliability designs; logistics companies and better supply; leaving rough terrain; acclimatization (special forces techs, winter logistics, Adaptable/winter-specialist commanders); occupation laws and counter-intelligence against resistance attrition; limiting equipment available to training units; national spirits/advisors that cut attrition; pre-start difficulty sliders (disable achievements). [w:Attrition §减少装备损失的方法]

## Engine view

- Land attrition: `NMilitary.UNIT_UPKEEP_ATTRITION = 0`, `NMilitary.TRAINING_ATTRITION = 0.05`, `NMilitary.BORDER_WAR_ATTRITION_FACTOR = 0.1`, `NMilitary.ATTRITION_WHILE_MOVING_FACTOR = 1.2`, `NMilitary.ATTRITION_DAMAGE_ORG = 0.08`, `NMilitary.ATTRITION_EQUIPMENT_LOSS_CHANCE = 0.005`; supply treshold `NSupply.SUPPLY_THRESHOLD_FOR_ARMY_ATTRITION = 0.35`; out-of-supply caps `NMilitary.OUT_OF_SUPPLY_ATTRITION = 0.2`, `NMilitary.OUT_OF_SUPPLY_MORALE = -0.2`, `NMilitary.MAX_OUT_OF_SUPPLY_DAYS = 30`; truck attrition `NSupply.TRUCK_ATTRITION = 0.003`, `NSupply.TRUCK_ATTRITION_FACTOR = 0.65`. [v:common/defines/00_defines.lua → NMilitary.*, NSupply.* @1.18.3] [w:Attrition §Causes §组织度损失 §装备损失]
- Air accidents: `NAir.ACCIDENT_CHANCE_BASE = 0.1`, `NAir.ACCIDENT_CHANCE_BALANCE_MULT = 0.1`, `NAir.ACCIDENT_CHANCE_RELIABILITY_MULT = 2`, `NAir.ACCIDENT_CHANCE_CARRIER_MULT = 1.5`, `NAir.AIR_WING_XP_TRAINING_MISSION_ACCIDENT_FACTOR = 0.2`, `NAir.ACCIDENT_EFFECT_MULT = 0.007`. [v:common/defines/00_defines.lua → NAir.* @1.18.3] [w:Attrition §空军单位]
- Naval accidents: `NNavy.TRAINING_ACCIDENT_CHANCES = 0.02`, `NNavy.TRAINING_ACCIDENT_ORG_LOSS_FACTOR = 0.3`, `NNavy.TRAINING_ACCIDENT_STRENGTH_LOSS_FACTOR = 0.05`, `NNavy.TRAINING_ACCIDENT_STRENGTH_LOSS = 4`, `NNavy.TRAINING_ACCIDENT_CRITICAL_HIT_CHANCES = 0.3`, `NNavy.TRAINING_ACCIDENT_CRITICAL_HIT_DAMAGE_SCALE = 4`, `NNavy.NAVAL_MINES_ACCIDENT_ORG_LOSS_FACTOR = 0.5`, `NNavy.NAVAL_MINES_ACCIDENT_STRENGTH_LOSS = 50`, `NNavy.NAVAL_MINES_ACCIDENT_CRITICAL_HIT_CHANCES = 0.14`, `NNavy.NAVAL_ACCIDENT_CHANCE_REDUCTION_ON_POTF = 0.01`, `NNavy.NAVAL_ACCIDENT_CRITICAL_HIT_CHANCE_REDUCTION_POTF = 0.01`, `NNavy.ACCIDENTS_CHANCE_BALANCE_FACTOR = 0.04`, `NNavy.ATTRITION_WHILE_MOVING_FACTOR = 1.2`, `NNavy.ATTRITION_DAMAGE_ORG = 0.01`, `NNavy.ATTRITION_DAMAGE_STR = 0.03`, `NNavy.ATTRITION_STR_DAMAGE_CHANCE = 0.2`. [v:common/defines/00_defines.lua → NNavy.* @1.18.3] [w:Attrition §海军单位]
- Mine effect cap: `naval_mines_effect` static modifier (up to 5% trigger chance). [v:common/modifiers/ @1.18.3] [m:Modifiers.md → naval_mines_effect] [w:Attrition §海军单位]

## Modding surfaces

- Attrition/weather modifiers: `attrition`, `attrition_for_controller`, `unit_upkeep_attrition_factor`, `winter_attrition(_factor)`, `heat_attrition(_factor)`, `acclimatization_cold_climate_gain_factor`. [m:List of modifiers.md → attrition, attrition_for_controller, unit_upkeep_attrition_factor, winter_attrition, heat_attrition]
- Equipment reliability is an equipment/design stat and drives both equipment losses and accidents → Equipment modding.md, [[hoi4-tech]]. [m:Equipment modding.md]
- Terrain/weather attrition sources (terrain factors, weather events) are data in map/strategic-region files → Map modding.md, Strategic region modding.md, [[hoi4-map]]; values → `Terrain and weather.md`. [m:Map modding.md] [m:Strategic region modding.md]
- Resistance attrition and its mitigation laws belong to the occupation system (future `Occupation.md`); for now see occupation-law ideas → [[hoi4-idea]]. [m:Idea modding.md]
- Attrition defines → Defines.md, [[hoi4-define]]. [m:Defines.md §NMilitary §NAir §NNavy §NSupply]

## Unknowns

- The wiki states mines' critical hits multiply damage ×5 in prose while its own footnote points at `TRAINING_ACCIDENT_CRITICAL_HIT_DAMAGE_SCALE = 4`; the discrepancy is recorded, not resolved (only the define value is certain).
- "Causes" percentages (terrain/weather/resistance/border-war contributions) come from the wiki table; no individual define anchors them in this pass (unsourced values).
- Navy attrition defines are retained in the files (`NNavy.ATTRITION_WHILE_MOVING_FACTOR`, `NNavy.ATTRITION_DAMAGE_ORG`, `NNavy.ATTRITION_DAMAGE_STR`, `NNavy.ATTRITION_STR_DAMAGE_CHANCE`) but the page notes naval attrition is mostly legacy; active use was not verified.
- The air accident "per airbase per hour" aggregation model was taken from the page's worked formulas; intermediate rounding rules were not verified against the engine.
