# Raid — Mechanics

> **Topic** — raids (Götterdämmerung / 1.15): nuclear, air and paratrooper raids, their requirements, outcome tiers and effects.
> **Scope** — covered: the raid system. Not covered: nuclear production/fallout detail (`Nuclear bomb.md`), special projects (`Special project.md`), intel networks (`Intelligence agency.md`).
>
> **Sources & version anchors**
> - [w] game wiki *Raid* (rev 97109, 69% translated), via the Chinese mirror, snapshot 2026-09-10. Section anchors use mirror titles.
> - [m] modding wiki offline snapshot files in `[[hoi4-mod]]`'s `references/`.
> - [v] vanilla install **1.18.3 "Case Green"** (`launcher-settings.json`), root `/mnt/data/hoi4/Hearts of Iron IV`.
>
> Number resolution: target-version [v] > [m] > [w]; conflicts are listed under `## Unknowns`.

## Player view

- Raids come in three categories: nuclear raids, air raids and paratrooper raids; each raid has a preparation time, unit/equipment requirements, a base success chance and outcome tiers (Failure / Limited Success / Success / Critical Success). [w:Raid]
- Nuclear raids: air strike (30 days prep, 10 strategic/ICBM bombers + bomb, base 50%) and missile strike (14 days, 1 nuclear missile + bomb, base 80%); success tiers apply fallout for 180/365/730 days depending on weapon and tier; failures cost 50–75% of the bombers and 50 PP. [w:Raid §Nuclear Raids]
- Air raids (5 generic): facility strike (70 TAC/STR, base 65%), canal busting (70 TAC/STR + bouncing bomb project), dam busting (70 STR + bouncing bomb), oilfield raid (40 STR, target state with oil > 24), and giant-gun position strike (70 STR with Earthshaker Bomb); each tier scales losses, damage to the target building and repair-speed penalties. [w:Raid §Air Raids]
- Unique air raids (one launch only, e.g. Reichstag/Big Ben/Statue of Liberty/Kremlin bombings, Operation Jericho, leaflet drops) have country-specific unlock conditions and propaganda/war-support effects. [w:Raid §Air Raids]
- Paratrooper raids: steel mine raid (1 para division + 10 transports + 200 support equipment; disables 25–75% of a state's steel for 30–180 days), pathfinder raid (needs an ongoing naval invasion with ≥5% preparation; disables strategic deployment, −10% invasion penalty for 20–60 h) and the German-only Eben Emael raid (disables/damages forts). [w:Raid §Paratrooper Raids]
- Unique raids can be launched once only; the same target cannot be raided again until its cooldown lapses. [w:Raid]

## Engine view

- Raid framework: `NRaids.BASE_DAYS_TO_PREPARE = 7` (default preparation; each raid type overrides it in script), `NRaids.RAID_DEFAULT_TARGET_COOLDOWN_DAYS = 365`, `NRaids.RAID_OUTCOME_REPORT_DAYS_TO_LIVE = 30`, `NRaids.RAID_UNIT_SPEED_MULTIPLIER = 0.1`, `NRaids.MAX_DETECTED_TARGETS_PER_HOUR = 1`. [v:common/defines/00_defines.lua → NRaids.* @1.18.3] [w:Raid]
- Target detection/intel: `NRaids.TARGET_DETECTION_INTEL_TRESHOLD = 20.0`, `NRaids.TARGET_INTEL_PER_CIVILIAN_INTEL_OVER_COUNTRY = 0.5`, `NRaids.TARGET_INTEL_PER_ARMY_INTEL_OVER_COUNTRY = 0.5`, `NRaids.TARGET_INTEL_PER_NAVY_INTEL_OVER_COUNTRY = 0.5`, `NRaids.TARGET_INTEL_PER_AIRFORCE_INTEL_OVER_COUNTRY = 0.5`, `NRaids.TARGET_INTEL_PER_NETWORK_STRENGTH = 0.5`, `NRaids.TARGET_INTEL_FROM_CONTROLLED_NEIGHBOUR_STATES = 15.0`, `NRaids.TARGET_INTEL_PER_AIR_SUPERIORITY = 0.5`, `NRaids.TARGET_INTEL_FROM_DECRYPTION = 25.0`, `NRaids.TARGET_INTEL_PENALTY_PER_ENEMY_COUNTER_INTEL = 5.0`. [v:common/defines/00_defines.lua → NRaids.TARGET_DETECTION_INTEL_TRESHOLD, NRaids.TARGET_INTEL_PER_CIVILIAN_INTEL_OVER_COUNTRY, NRaids.TARGET_INTEL_PER_ARMY_INTEL_OVER_COUNTRY, NRaids.TARGET_INTEL_PER_NAVY_INTEL_OVER_COUNTRY, NRaids.TARGET_INTEL_PER_AIRFORCE_INTEL_OVER_COUNTRY, NRaids.TARGET_INTEL_PER_NETWORK_STRENGTH, NRaids.TARGET_INTEL_FROM_CONTROLLED_NEIGHBOUR_STATES, NRaids.TARGET_INTEL_PER_AIR_SUPERIORITY, NRaids.TARGET_INTEL_FROM_DECRYPTION, NRaids.TARGET_INTEL_PENALTY_PER_ENEMY_COUNTER_INTEL @1.18.3] [w:Raid]
- Naval commando raids: `NRaids.BASE_NAVAL_COMMANDO_RAID_DISTANCE = 1500` (km). [v:common/defines/00_defines.lua → NRaids.BASE_NAVAL_COMMANDO_RAID_DISTANCE @1.18.3]
- Nuclear raid category link and Army transfer flags: `NRaids.NUCLEAR_RAID_CATEGORY_NAME = "nuclear_raids"`, `NRaids.ARMY_TRANSFER_MOVE_SAFELY = true`, `NRaids.ARMY_TRANSFER_AVOID_ENEMY = true`. [v:common/defines/00_defines.lua → NRaids.* @1.18.3]

## Modding surfaces

- Raid definitions live in `common/raids/` (preparation, requirements, success tiers, counters) — no offline doc; vanilla files are the source. [v:common/raids/ @1.18.3]
- Raids consume special-project unlocks (`common/special_projects/`) and equipment → `Special project.md`, Equipment modding.md, [[hoi4-tech]]. [v:common/special_projects/ @1.18.3] [m:Equipment modding.md]
- Raid outcomes apply state modifiers (fallout, dam blown, refinery damage) — content modifiers, cross-referenced in `Nuclear bomb.md`. [w:Raid]
- Raid difficulty intel comes from the intel system → `Intelligence agency.md`. [m:List of modifiers.md → operation_boost_resistance_outcome]

## Unknowns

- The Raid page has no footnotes; the NRaids defines above were verified locally but are not page-cited.
- Generic raid cooldowns are not given on the page; only `RAID_DEFAULT_TARGET_COOLDOWN_DAYS = 365` is certain, and script may override per raid type.
- Unique-raid numbers exist for several countries on the page; the summary above keeps only representative values (full table not transcribed).
- The pathfinder raid's resistance modifier is printed inconsistently in the mirror ("+50~0"); treated as uncertain.
