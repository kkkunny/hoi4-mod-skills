# Land warfare command — Mechanics

> **Topic** — army command: theaters, armies, officer corps, command power, planning/battle plans, naval-invasion plans, paradrops, training.
> **Scope** — covered: how armies are organised and commanded. Not covered: division stats (`Division.md`), combat resolution (`Land battle.md`), terrain (`Terrain and weather.md`).
>
> **Sources & version anchors**
> - [w] game wiki *Land_warfare* (rev 84735), *Officer_corps* (rev 104772), *Battle_plan* (rev 104646), *Command_group* (rev 83528), via the Chinese mirror, snapshot 2026-09-10. Section anchors use mirror titles.
> - [m] modding wiki offline snapshot files in `[[hoi4-mod]]`'s `references/`.
> - [v] vanilla install **1.18.3 "Case Green"** (`launcher-settings.json`), root `/mnt/data/hoi4/Hearts of Iron IV`.
>
> Number resolution: target-version [v] > [m] > [w]; conflicts are listed under `## Unknowns`.

## Player view

- Theaters group armies for alerts, battle logs and reinforcement priority; armies are commanded by generals, and battle plans issue front lines, offensives, fallback lines and area defence. [w:Land_warfare §战区 §集团军]
- Officer corps: army/navy HQs hold at most 3 spirits each, the air HQ 2; spirits cost XP (academy 20, generic army/navy 35, command spirits 50) and some unlock via doctrine/ideology. [w:Officer_corps §精神]
- Military high command advisors unlock at general ranks 4 (expert) / 6 (master) / 8 (genius) and give tiered bonuses (e.g. army offence +5/+10/+15% division attack; army reformer +5/+10/+15% army XP; army planning +5/+10/+15% planning speed). [w:Officer_corps §最高统帅部]
- Preferred tactics cost 20 command power at the officer corps and raise the chance of choosing that tactic; doctrines cost 100 XP of the matching type by default. [w:Officer_corps §学说 §偏好战术]
- Command power: starts at 0, base cap 80, gained 0.4/day; career-officer spirit gives +30 cap, improvisation gives +0.2/day; spend it on traits (15), promotion (40), attachés (50, occupied), air-supply (0.05/plane) and ground crews (20/region). [w:Government §指挥点数] [v:common/defines/00_defines.lua → NCountry.BASE_COMMAND_POWER_GAIN, NCountry.STARTING_COMMAND_POWER, NCountry.BASE_MAX_COMMAND_POWER @1.18.3]
- Planning: divisions in a plan area accumulate +1% planning per day up to a cap of +30% base (each general planning skill point adds +2% cap and +5% gain: skill 4 → 38% cap, 2.4%/day); each 1% planning gives +1% attack and breakthrough; decay is −1%/day under AI control and −3%/day when manually ordered; a staff order gives +400% planning speed for 7 days. [w:Battle_plan §计划加成]
- Naval invasion plans: base capacity 4 divisions per order and 1 simultaneous plan; transport techs add +2 divisions/+2 plans; landing craft +2/+2 (or +4/+3 with MtG naval support tree); advanced landing craft +8/+4 (+5/+4); special-forces techs +2/+1 each; preparation is 7 days per division at tier 1 and planning gain suffers a −100% malus (reduced to −80% by an AaT doctrine). [w:Battle_plan §海军登陆 §准备时间]
- Naval invasion requires >50% naval supremacy along the path and at least 40% naval intelligence efficiency; the amphibious landing penalty is −50% attack/breakthrough, softened by invasion defence tech and the +50% breakthrough bonus that decays with landing progress. [w:Battle_plan §制海权 §入侵战斗]
- Paradrops: a plan needs a base of ≥50 transport planes and 45 planes per division; glider commander traits cut requirements −10% for 7 days and the AaT strategic airlift doctrine −50%; execution needs neutral or ≥70% air superiority along the route; landing status is 40% org with −80% org regain for 120 h and −30% combat penalty for 48 h; failed drops lose 54.5–90% strength, 47.4–99% manpower and 33.3–98% equipment plus 10% randomness. [w:Battle_plan §空降]
- Strategic redeployment: +5 km/h base scaled up to +10 with infrastructure and rail redeployment up to +25; costs 90% organisation; no attrition, fuel, terrain or weather penalties. [w:Land_warfare §战略再部署]
- Training: exercises give unit XP only up to Regular and country XP while trained units exist; commanders gain XP only from combat; exercises add +5% equipment attrition and set org to 20%. [w:Command_group §演习]
- Area defence multiplies the commander's division limit (×3) without penalties; fallback-line garrisons gain no planning but do accrue entrenchment. [w:Battle_plan §区域防御 §撤退线]

## Engine view

- Planning: `NMilitary.PLANNING_MAX = 0.3`, `NMilitary.PLANNING_GAIN = 0.02`, `NMilitary.PLANNING_DECAY = 0.01`, `NMilitary.PLAYER_ORDER_PLANNING_DECAY = 0.03`. [v:common/defines/00_defines.lua → NMilitary.PLANNING_MAX, NMilitary.PLANNING_GAIN, NMilitary.PLANNING_DECAY, NMilitary.PLAYER_ORDER_PLANNING_DECAY @1.18.3] [w:Battle_plan §计划加成]
- Naval invasion planning: `NMilitary.NAVAL_INVASION_PLANNING_BONUS_GAIN = 0.02`, `NMilitary.NAVAL_INVASION_PLANNING_BONUS_MALUS = -1` (with AaT doctrine −0.8; see Unknowns); landing combat uses `NNavy.AMPHIBIOUS_LANDING_PENALTY = -0.5`, `NNavy.AMPHIBIOUS_INVADE_ATTACK_LOW = 0.2`, `NNavy.AMPHIBIOUS_INVADE_DEFEND_LOW = 1.5`, `NNavy.AMPHIBIOUS_INVADE_ATTACK_HIGH = 1`, `NNavy.AMPHIBIOUS_INVADE_DEFEND_HIGH = 1`, `NNavy.AMPHIBIOUS_INVADE_LANDING_PENALTY_DECREASE = 3.5`. [v:common/defines/00_defines.lua → NMilitary.NAVAL_INVASION_PLANNING_BONUS_GAIN, NMilitary.NAVAL_INVASION_PLANNING_BONUS_MALUS, NNavy.AMPHIBIOUS_LANDING_PENALTY, NNavy.AMPHIBIOUS_INVADE_ATTACK_LOW, NNavy.AMPHIBIOUS_INVADE_DEFEND_LOW, NNavy.AMPHIBIOUS_INVADE_ATTACK_HIGH, NNavy.AMPHIBIOUS_INVADE_DEFEND_HIGH, NNavy.AMPHIBIOUS_INVADE_LANDING_PENALTY_DECREASE @1.18.3] [w:Battle_plan §海军登陆]
- Paradrops: `NAir.MIN_PLANE_COUNT_PARADROP = 50`, `NAir.BASE_UNIT_WEIGHT_IN_TRANSPORT_PLANES = 45.0`, `NCountry.PARADROP_AIR_SUPERIORITY_RATIO = 0.7`, `NAir.PARACHUTE_FAILED_STR_DIV = 10.0`, `NAir.PARACHUTE_FAILED_MANPOWER_DIV = 100.0`, `NAir.PARACHUTE_FAILED_EQUIPMENT_DIV = 50.0`, `NAir.PARACHUTE_DISRUPTED_STR_DIV = 2.2`, `NAir.PARACHUTE_DISRUPTED_MANPOWER_DIV = 1.9`, `NAir.PARACHUTE_DISRUPTED_EQUIPMENT_DIV = 1.5`, `NAir.PARACHUTE_PENALTY_RANDOMNESS = 0.1`, `NAir.PARACHUTE_COMPLETE_ORG = 0.4`, `NAir.PARACHUTE_ORG_REGAIN_PENALTY_MULT = -0.8`, `NAir.PARACHUTE_ORG_REGAIN_PENALTY_DURATION = 120`, `NMilitary.PARADROP_PENALTY = -0.3`, `NMilitary.PARADROP_HOURS = 48`. [v:common/defines/00_defines.lua → NAir.MIN_PLANE_COUNT_PARADROP, NAir.BASE_UNIT_WEIGHT_IN_TRANSPORT_PLANES, NCountry.PARADROP_AIR_SUPERIORITY_RATIO, NAir.PARACHUTE_FAILED_STR_DIV, NAir.PARACHUTE_FAILED_MANPOWER_DIV, NAir.PARACHUTE_FAILED_EQUIPMENT_DIV, NAir.PARACHUTE_DISRUPTED_STR_DIV, NAir.PARACHUTE_DISRUPTED_MANPOWER_DIV, NAir.PARACHUTE_DISRUPTED_EQUIPMENT_DIV, NAir.PARACHUTE_PENALTY_RANDOMNESS, NAir.PARACHUTE_COMPLETE_ORG, NAir.PARACHUTE_ORG_REGAIN_PENALTY_MULT, NAir.PARACHUTE_ORG_REGAIN_PENALTY_DURATION, NMilitary.PARADROP_PENALTY, NMilitary.PARADROP_HOURS @1.18.3] [w:Battle_plan §空降]
- Redeployment: `NMilitary.STRATEGIC_SPEED_INFRA_BASE = 5.0`, `NMilitary.STRATEGIC_SPEED_INFRA_MAX = 10.0`, `NMilitary.STRATEGIC_REDEPLOY_ORG_RATIO = 0.1`, `NMilitary.STRATEGIC_SPEED_RAIL_BASE = 10.0` (the wiki states 15; see Unknowns), `NMilitary.STRATEGIC_SPEED_RAIL_MAX = 25.0`. [v:common/defines/00_defines.lua → NMilitary.STRATEGIC_SPEED_INFRA_BASE, NMilitary.STRATEGIC_SPEED_INFRA_MAX, NMilitary.STRATEGIC_REDEPLOY_ORG_RATIO, NMilitary.STRATEGIC_SPEED_RAIL_BASE, NMilitary.STRATEGIC_SPEED_RAIL_MAX @1.18.3] [w:Land_warfare §战略再部署]
- Training: `NMilitary.TRAINING_ATTRITION = 0.05`, `NMilitary.TRAINING_ORG = 0.2`, `NMilitary.UNIT_EXPERIENCE_PER_TRAINING_DAY = 0.0015`, `NMilitary.TRAINING_EXPERIENCE_SCALE = 62.0`, `NCountry.ARMY_COUNT_DAILY_LERP_FOR_TRAINING_XP = 0.002`, `NCountry.ARMY_COUNT_DAILY_DECREASE_FOR_TRAINING_XP = 0.1`; commander XP only in combat via `NMilitary.ARMY_LEADER_XP_GAIN_PER_UNIT_IN_COMBAT = 0.1`. [v:common/defines/00_defines.lua → NMilitary.*, NCountry.* @1.18.3] [w:Command_group §演习]
- Officer corps and command power: spirit costs/limits and HQ ranks are content/defines (ranks: `NCharacter.SPECIALIST_ADVISOR_MIN_RANK = 4`, `NCharacter.EXPERT_ADVISOR_MIN_RANK = 6`, `NCharacter.GENIUS_ADVISOR_MIN_RANK = 8`); command power `NCountry.STARTING_COMMAND_POWER = 0`, `NCountry.BASE_MAX_COMMAND_POWER = 80`, `NCountry.BASE_COMMAND_POWER_GAIN = 0.4`; preferred tactics `NMilitary.PREFERRED_TACTIC_COMMAND_POWER_COST = 20`. [v:common/defines/00_defines.lua → NCharacter.*, NCountry.*, NMilitary.PREFERRED_TACTIC_COMMAND_POWER_COST @1.18.3] [w:Officer_corps §最高统帅部 §偏好战术]
- Border conflicts (WtT): winner takes the contested state plus PP, stalemates hand the defender PP, losers suffer a retry cooldown; only one conflict per country pair at a time. [w:Land_warfare §边境冲突]

## Modding surfaces

- Commander traits, ranks and advisor slots → Character modding.md, [[hoi4-character]]. [m:Character modding.md]
- Planning/redeploy/paradrop/invasion defines → Defines.md §NMilitary/§NAir/§NNavy, [[hoi4-define]]. [m:Defines.md §NMilitary]
- Officer corps spirits have no offline reference: commander abilities and operations are listed by the master skill as online-only; use vanilla files (`common/abilities/`, `common/ideas/` where applicable). [v:common/abilities/ @1.18.3]
- Battle plans, theaters and area defence are UI/engine systems tuned by the defines above; no script files. [w:Battle_plan]
- Border-conflict events/decisions → Event modding.md, Decision modding.md, [[hoi4-event]] / [[hoi4-decision]]. [m:Event modding.md] [m:Decision modding.md]

## Unknowns

- `NMilitary.STRATEGIC_SPEED_RAIL_BASE` is 10.0 in 1.18.3 while the wiki says 15.0; the local value is used.
- The naval-invasion capacity figures (division/plan counts) mix base-game and MtG-tree branches; the AaT "−80%" reduction of the planning malus is page prose not tied to a define (uncertain).
- Officer corps spirit costs (20/35/50 XP) and HQ bonuses (+5/10/15%) are page content; file locations were not traced (no offline reference).
- The area-defence division limit (24 base, 72 under area defence) is inferred by the page from 72÷3; the base 24 is not stated (uncertain).
- The Land_warfare page's naval-invasion division counts (10/50) conflict with the Battle plan page (4 base, techs add more); both recorded, the Battle plan ladder is treated as current.
