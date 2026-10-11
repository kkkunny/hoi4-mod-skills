# Division — Mechanics

> **Topic** — divisions: templates, battalions, support companies, and how stats aggregate.
> **Scope** — covered: division composition and aggregate statistics. Not covered: combat resolution (`Land battle.md`), equipment/designer details (Equipment modding.md), training and deployment flow.
>
> **Sources & version anchors**
> - [w] game wiki *Division* (rev 104065, 99% translated), *Land_units* (rev 104726, 100%), *Division_designer* (rev 102633, 67% — untranslated text used directly), via the Chinese mirror, snapshot 2026-09-10.
> - [m] modding wiki offline snapshot files in `[[hoi4-mod]]`'s `references/`.
> - [v] vanilla install **1.18.3 "Case Green"** (`launcher-settings.json`), root `/mnt/data/hoi4/Hearts of Iron IV`.
>
> Number resolution: target-version [v] > [m] > [w]; conflicts are listed under `## Unknowns`.

## Player view

- A division is the smallest unit the player controls directly; it is created from a template, and templates are edited with army experience (the Save button shows the total cost; experimenting is free until saving). [w:Division §师编制]
- Structure: up to 5 combat regiments (columns) × up to 5 battalions each (4 before army-doctrine upgrades), i.e. at most 25 battalions; a battalion type can repeat freely, but each support-company type only once, and at most 5 support companies. [w:Division §师编制] [w:Division_designer §Battalions] [w:Division_designer §支援连]
- XP costs: add a battalion 5; add a regiment of a new type 25; replace/move a battalion 5; support add/replace/remove 10; copying, renaming, icons and equipment changes are free. [w:Division §陆军经验花费] [w:Division_designer §Battalions §支援连]
- Equipment priorities (Reserves / default / Elite) decide reinforcement order; equipment in the field is not returned unless replaced or removed by a template change. [w:Division_designer §Equipment priorities §师装备]
- Battalion categories: infantry, mobile, armored (combat), plus combat-support, mobile-support and armored-support battalions; support companies form their own category; a regiment column may only mix battalions of one category. [w:Land_units §命名规则] [w:Division_designer §Battalions]
- Stat aggregation: sums (HP, soft/hard attack, air attack, defence, breakthrough, weight, supply use, fuel capacity, casualties), averages (org, org recovery, hardness of front-line battalions only), worst value (speed = slowest battalion, including some support companies), and per-equipment values (reliability). [w:Land_units §基础属性 §战斗属性]
- Armour and piercing = 40% of the highest value + 60% of the average (example: 2 infantry + 2 light tanks → hardness 40%; an armour-70 tank with infantry gives 49, then 41.9 with two infantry). [w:Division §战斗营 §Strategy/装甲 §Strategy/硬度值]
- Combat width: most battalions 2, artillery 3, AT/AA 1; support companies add 0; techs can cut infantry width by 0.4 (Mass Assault branch) or mountaineers by 0.2. [w:Land_units §战斗宽度] [w:Land_battle §战斗宽度]
- Speed: towed AT/AA support companies do not slow a division, but a towed AT combat battalion drags a fast division to its 4 km/h; recon companies do influence speed and can themselves slow the division down. [w:Division §支援连]
- Under-strength scaling: current combat value uses the lower of manpower ratio and cost-weighted equipment ratio; per-battalion equipment stats scale by the lower of manpower ratio and unweighted equipment ratio; required equipment additionally scales by the cost-weighted ratio; support companies and mechanized equipment count as required. [w:Division §Strategy/不满编时的属性]
- The division "type" (icon and sample unit) is the highest weighted priority across unit definitions; ties are resolved by the first unit in the template. [w:Division §师的单位类型]
- Armored divisions: some countries start with historical templates; everyone else receives a free "Armored-Division" template on first tank research; swapping light for medium/heavy tanks costs 5 XP per battalion. [w:Division §装甲编制]
- Support companies (effects): engineers entrenchment/fort/river; recon terrain speed + tactics countering; military police suppression; maintenance reliability; field hospital manpower return and casualty XP loss reduction; logistics supply/fuel consumption; signal initiative. [w:Division §支援连] [w:Land_units §单位类型/支援连]
- Rules of thumb from the wiki: 1 artillery battalion ≈ 3 infantry battalions of soft attack at half the width; 20-width divisions defend better, 40-width attack better; org is hit about twice as often as HP. [w:Division §Strategy/软攻 §规模]

## Engine view

- Armour/piercing blend: `NMilitary.ARMOR_VS_AVERAGE = 0.4` and `NMilitary.PEN_VS_AVERAGE = 0.4` (40% max + 60% average). [v:common/defines/00_defines.lua → NMilitary.ARMOR_VS_AVERAGE, PEN_VS_AVERAGE @1.18.3] [w:Division §战斗营]
- Piercing thresholds: `NMilitary.PIERCING_THRESHOLDS = { 1, 0.75, 0.5, 0 }` with damage values `{ 1, 0.8, 0.65, 0.5 }` — no penalty at full piercing, −20% at ≥75%, −35% at ≥50%, −50% below. [v:common/defines/00_defines.lua → NMilitary.PIERCING_THRESHOLDS, PIERCING_THRESHOLD_DAMAGE_VALUES @1.18.3] [w:Land_units §战斗属性/穿透]
- Unpierced armour raises the org damage dice from 1d4 to 1d6: `NMilitary.LAND_COMBAT_ORG_ARMOR_ON_SOFT_DICE_SIZE = 6`. [v:common/defines/00_defines.lua → NMilitary.LAND_COMBAT_ORG_ARMOR_ON_SOFT_DICE_SIZE @1.18.3] [w:Land_battle §造成伤害]
- Movement org drain `NMilitary.HOURLY_ORG_MOVEMENT_IMPACT = -0.2` per hour, with movement speed reduced up to `NMilitary.ZERO_ORG_MOVEMENT_MODIFIER = -0.8` at zero org. [v:common/defines/00_defines.lua → NMilitary.HOURLY_ORG_MOVEMENT_IMPACT, ZERO_ORG_MOVEMENT_MODIFIER @1.18.3] [w:Land_units §组织度]
- Initiative feeds reinforcement: `NMilitary.ARMY_INITIATIVE_REINFORCE_FACTOR = 0.25` (each 1% initiative = +0.25% join chance). [v:common/defines/00_defines.lua → NMilitary.ARMY_INITIATIVE_REINFORCE_FACTOR @1.18.3] [w:Land_units §主动性]
- Default fuel capacity per division: 96 hours (`NMilitary.FUEL_CAPACITY_DEFAULT_HOURS = 96`). [v:common/defines/00_defines.lua → NMilitary.FUEL_CAPACITY_DEFAULT_HOURS @1.18.3] [w:Land_units §其他统计数值/燃料容量]
- Terrain adjusters average across combat battalions and sum across support companies; under-strength divisions scale their bonuses/penalties proportionally. [w:Terrain §单位专属调整值]

## Modding surfaces

- Templates (composition, placement, production integration) → Division modding.md, [[hoi4-tech]]; sub-unit stats → Unit modding.md; equipment-derived stats (hardness, armour, fuel) → Equipment modding.md. [m:Division modding.md] [m:Unit modding.md] [m:Equipment modding.md]
- Unit type priority weights and `type = { ... }` blocks live in `common/units/*.txt` (e.g. infantry `priority = 600`). [v:common/units/infantry.txt → priority @1.18.3] [m:Unit modding.md]
- Division stat modifiers: `army_attack_factor`, `army_defence_factor`, `army_core_attack_factor`, `army_core_defence_factor`, `army_speed_factor`, `army_org(_factor)`, `army_morale(_factor)`, `training_time_army(_factor)`, `experience_gain_army_unit(_factor)`, `land_reinforce_rate`, `combat_width_factor`, `dig_in_speed(_factor)`, `max_dig_in(_factor)`, `dont_lose_dig_in_on_attack`, `recon_factor_while_entrenched`. [m:List of modifiers.md → army_attack_factor, army_defence_factor, army_org, army_morale, land_reinforce_rate, combat_width_factor]
- Division-related defines → Defines.md §NMilitary/NDeployment, [[hoi4-define]]. [m:Defines.md §NMilitary]

## Unknowns

- The *Division designer* page is only 67% translated; untranslated English text was used directly for those parts.
- The `NDeployment` namespace was not inspected for usable deployment keys in this pass (unsourced scope note).
- Per-battalion stat tables (HP/org/equipment per unit type) are content data in `common/units/` and are deliberately not transcribed here.
- The under-strength rule's exact rounding is described only in prose (unsourced formula details).
