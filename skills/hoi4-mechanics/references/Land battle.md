# Land battle — Mechanics

> **Topic** — land combat: width, reserves, damage resolution, targeting, tactics, combat factors, damage aftermath.
> **Scope** — covered: how a land battle resolves hour by hour. Not covered: division composition (`Division.md`), terrain/weather values (`Terrain and weather.md`), supply sources (`Logistics.md`), air/naval support details.
>
> **Sources & version anchors**
> - [w] game wiki *Land_battle* (rev 102327, 98% translated) and *Combat_tactics* (rev 104932, 94%), via the Chinese mirror, snapshot 2026-09-10. Section anchors use mirror titles.
> - [m] modding wiki offline snapshot files in `[[hoi4-mod]]`'s `references/`.
> - [v] vanilla install **1.18.3 "Case Green"** (`launcher-settings.json`), root `/mnt/data/hoi4/Hearts of Iron IV`.
>
> Number resolution: target-version [v] > [m] > [w]; conflicts are listed under `## Unknowns`.

## Player view

- Land battle: attacker divisions try to enter a province held by defenders; combat resolves in hourly rounds; a side loses when no frontline division can continue, and on retreat the defender falls back (destroyed if no province is available). [w:Land_battle §导言]
- Combat width: base width and per-extra-flank width come from the battle province's terrain; flanking from additional provinces (and paradrops/naval invasions) adds width; over-width penalty = −100% × (total_width − battle_width)/battle_width, capped at −33%, and units only join while the penalty stays under −33%. [w:Land_battle §战斗宽度]
- Reserves: units that do not fit join the reserve; they take no damage, do not recover org and do not gain entrenchment, but still provide recon and AA; each reserve unit has a base 2% chance per hour to join (average ≈35 h), raised by radios, speed, doctrines and signal companies. [w:Land_battle §Reserves]
- Attacks and defences: attacks per hour = rounded(hardness-weighted attack/10); defences = rounded(defence/10); each attack consumes one defence point; hit chance is 10% while defences remain and 40% once they are exhausted. [w:Land_battle §造成伤害]
- Damage: HP die size 2 and org die size 4; per-hit damage = roll × base modifiers (0.06 strength / 0.053 org) × tactic/armour modifiers; unpierced armour upgrades the org die to 6 (≈+40% org damage). [w:Land_battle §造成伤害]
- Targeting: a division engages up to twice its own width; the priority target takes all coordinated damage (35% base + coordination, capped at 90%) while the rest is split across the engagement; the priority score favours hard attack (×1.2), reduced by org ratio and unpierced armour. [w:Land_battle §Targeting]
- Tactics: re-chosen every 12 hours from weighted lists per phase; initiative (higher commander skill; any division with higher recon gives +5 effective skill) decides who chooses second and can counter; preferred tactics cost 20 command power (national 25% / general 50% / marshal 25% weight bonuses, characters need skill 5); countering fully cancels a tactic's effects. [w:Combat_tactics §战术选取 §主动性 §偏好战术 §效果]
- Combat factors: terrain penalties (up to −50%; heavy units worse), river crossings (−30%/−60%), night (−50% attacks), forts (−15%/level, weakened by extra attack directions), encirclement (−30%), enemy air superiority (up to −35%), supply shortage (attacker −25%/−65%, defender −35%/−15%), over-width (−1%/1% up to −33%), stacking (−2% per division beyond 5, +3 per extra flank), multiple combats (−50%), paradrop (−30% for 48 h), amphibious (−50%), commander skill (+2.5%/point), planning (max +30% base, higher with doctrines), entrenchment (+2%/point), experience (−25% green … +75% elite), intel advantage (up to +15%). [w:Land_battle §战斗因素]
- Aftermath: 70% of HP losses convert to equipment and manpower losses; winners with equipment capture ratio (ECR) capture a share of enemy losses, and recover own losses at reliability² × 40%. [w:Land_battle §装备损伤 §装备缴获 §装备回收]
- Collateral: infrastructure in the state always takes damage while forts take it with 5% probability per hit; collateral = 0.1 × soft attack × attacks × damage scale; siege artillery doubles fort trigger chance and damage. [w:Land_battle §附带损伤]

## Engine view

- Over-width: `NMilitary.COMBAT_OVER_WIDTH_PENALTY = -1` per 1% and `NMilitary.COMBAT_OVER_WIDTH_PENALTY_MAX = -0.33`. [v:common/defines/00_defines.lua → NMilitary.COMBAT_OVER_WIDTH_PENALTY, COMBAT_OVER_WIDTH_PENALTY_MAX @1.18.3] [w:Land_battle §战斗宽度]
- Reserve join: `NMilitary.REINFORCE_CHANCE = 0.02`. [v:common/defines/00_defines.lua → NMilitary.REINFORCE_CHANCE @1.18.3] [w:Land_battle §Reserves]
- Hit resolution: `NMilitary.BASE_CHANCE_TO_AVOID_HIT = 90` and `NMilitary.CHANCE_TO_AVOID_HIT_AT_NO_DEF = 60`. [v:common/defines/00_defines.lua → NMilitary.BASE_CHANCE_TO_AVOID_HIT, CHANCE_TO_AVOID_HIT_AT_NO_DEF @1.18.3] [w:Land_battle §造成伤害]
- Damage dice and scaling: `NMilitary.LAND_COMBAT_STR_DICE_SIZE = 2`, `NMilitary.LAND_COMBAT_ORG_DICE_SIZE = 4`, `NMilitary.LAND_COMBAT_STR_DAMAGE_MODIFIER = 0.06`, `NMilitary.LAND_COMBAT_ORG_DAMAGE_MODIFIER = 0.053`, `NMilitary.LAND_COMBAT_ORG_ARMOR_ON_SOFT_DICE_SIZE = 6`. [v:common/defines/00_defines.lua → NMilitary.LAND_COMBAT_STR_DICE_SIZE, NMilitary.LAND_COMBAT_ORG_DICE_SIZE, NMilitary.LAND_COMBAT_STR_DAMAGE_MODIFIER, NMilitary.LAND_COMBAT_ORG_DAMAGE_MODIFIER, NMilitary.LAND_COMBAT_ORG_ARMOR_ON_SOFT_DICE_SIZE @1.18.3] [w:Land_battle §造成伤害]
- Targeting: `NMilitary.ENGAGEMENT_WIDTH_PER_WIDTH = 2.0`, `NMilitary.DAMAGE_SPLIT_ON_FIRST_TARGET = 0.35` (coordination cap 0.9 is not exposed as a define), `NMilitary.SOFT_ATTACK_TARGETING_FACTOR = 1.0`, `NMilitary.HARD_ATTACK_TARGETING_FACTOR = 1.2`. [v:common/defines/00_defines.lua → NMilitary.* @1.18.3] [w:Land_battle §Targeting]
- Combat factors: `NMilitary.BASE_NIGHT_ATTACK_PENALTY = -0.5`, `BASE_FORT_PENALTY = -0.15`, `ENCIRCLED_PENALTY = -0.3`, `ENEMY_AIR_SUPERIORITY_IMPACT = -0.35`, `COMBAT_SUPPLY_LACK_ATTACKER_ATTACK = -0.25`, `..._ATTACKER_DEFEND = -0.65`, `..._DEFENDER_ATTACK = -0.35`, `..._DEFENDER_DEFEND = -0.15`, `COMBAT_STACKING_START = 5`, `COMBAT_STACKING_EXTRA = 3`, `COMBAT_STACKING_PENALTY = -0.02`, `MULTIPLE_COMBATS_PENALTY = -0.5`, `PARADROP_PENALTY = -0.3`, `PARADROP_HOURS = 48`, `PLANNING_MAX = 0.3`, `EXPERIENCE_COMBAT_FACTOR = 0.25`; amphibious `NNavy.AMPHIBIOUS_LANDING_PENALTY = -0.5`; intel `NIntel.ARMY_INTEL_COMBAT_BONUS_MAX_BONUS = 0.15` (starts at 5 intel, caps at 50). [v:common/defines/00_defines.lua → NMilitary.*, NNavy.AMPHIBIOUS_LANDING_PENALTY, NIntel.ARMY_INTEL_COMBAT_BONUS_MAX_BONUS, NIntel.ARMY_INTEL_COMBAT_BONUS_MIN_INTEL_FOR_BONUS, NIntel.ARMY_INTEL_COMBAT_BONUS_MAX_INTEL_FOR_BONUS @1.18.3] [w:Land_battle §战斗因素]
- Aftermath: `NMilitary.EQUIPMENT_COMBAT_LOSS_FACTOR = 0.7`, `NMilitary.BASE_CAPTURE_EQUIPMENT_RATIO = 0`, `NMilitary.RELIABILTY_RECOVERY = 0.4` (typo is the game's). [v:common/defines/00_defines.lua → NMilitary.* @1.18.3] [w:Land_battle §装备损伤 §装备回收]
- Collateral fort chance: `NMilitary.LAND_COMBAT_FORT_DAMAGE_CHANCE = 5` (percent). [v:common/defines/00_defines.lua → NMilitary.LAND_COMBAT_FORT_DAMAGE_CHANCE @1.18.3] [w:Land_battle §附带损伤]
- Tactics: `NMilitary.TACTIC_SWAP_FREQUENCEY = 12`, `RECON_SKILL_IMPACT = 5`, `INITIATIVE_PICK_COUNTER_ADVANTAGE_FACTOR = 0.35`, `PREFERRED_TACTIC_COMMAND_POWER_COST = 20`, `COUNTRY_PREFERRED_TACTIC_WEIGHT_FACTOR = 0.25`, `PREFERRED_TACTIC_CHARACTER_SKILL_LEVEL_REQUIRED = 5`, `ARMY_GENERAL_PREFERRED_TACTIC_WEIGHT_FACTOR = 0.5`, `FIELD_MARSHAL_PREFERRED_TACTIC_WEIGHT_FACTOR = 0.25`, `COMBAT_MOVEMENT_SPEED = 0.33`. [v:common/defines/00_defines.lua → NMilitary.* @1.18.3] [w:Combat_tactics §战术选取 §效果]

## Modding surfaces

- Combat tactics are defined in `common/combat_tactics.txt` (phases, triggers, weights, counters, modifiers) → [[hoi4-define]]-adjacent content, see the wiki page; file verified present. [v:common/combat_tactics.txt @1.18.3] [w:Combat_tactics §默认值]
- Combat/battle modifiers: `army_attack_factor`, `army_defence_factor`, `land_night_attack`, `max_planning(_factor)`, `planning_speed`, `combat_width_factor`, `river_crossing_factor`, `terrain_penalty_reduction`, `fortification_damage`, `fortification_collateral_chance`. [m:List of modifiers.md → army_attack_factor, army_defence_factor, land_night_attack, max_planning, combat_width_factor, river_crossing_factor, fortification_damage, fortification_collateral_chance]
- Division stats that feed battle (attack/defence/breakthrough/width) → `Division.md`, Unit modding.md, [[hoi4-tech]]; terrain-side factors → `Terrain and weather.md`. [m:Unit modding.md] [m:List of modifiers.md → army_attack_factor]
- Combat defines → Defines.md, [[hoi4-define]]. [m:Defines.md §NMilitary]
- Commanders, traits and planning skills → Character modding.md, [[hoi4-character]]. [m:Character modding.md]

## Unknowns

- The coordination cap (90%) is stated only in a page footnote and is not exposed as a define (unsourced in files).
- Modifier keys for equipment capture ratio and recovery boosts were not located in the snapshot during this pass (unsourced).
- The wiki's worked damage examples involve rounding choices the pages do not formalize; only the define-level formulas are treated as exact here.
- Tactics content (full list) is deliberately not transcribed — see the page tables; `common/combat_tactics.txt` is the modding source of truth.
