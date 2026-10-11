# Peace conference — Mechanics

> **Topic** — peace conferences: participants, war score, state value, bids and their costs, outcomes.
> **Scope** — covered: how the conference resolves a war. Not covered: world tension in general (`Diplomacy.md`), autonomy after puppeting (`Puppet.md`).
>
> **Sources & version anchors**
> - [w] game wiki *Peace_conference* (rev 89388, 88% translated), via the Chinese mirror, snapshot 2026-09-10. Section anchors use mirror titles.
> - [m] modding wiki offline snapshot files in `[[hoi4-mod]]`'s `references/`.
> - [v] vanilla install **1.18.3 "Case Green"** (`launcher-settings.json`), root `/mnt/data/hoi4/Hearts of Iron IV`.
>
> Number resolution: target-version [v] > [m] > [w]; conflicts are listed under `## Unknowns`.

## Player view

- A peace conference lets the winning side decide the loser's fate through demands; participants include all countries that fought the loser (not only allies), and war participation % drives the war score handed out over the first five conference turns. [w:Peace conference §Participants §战争贡献]
- Demand types: take state, liberate, puppet, change government (democracies can only create supervised states); With BBA/DLC additions there are take navy, demilitarised zone, resource rights, war reparations (half of the state's civilian factories for five years) and dismantle military industry — the last four only combined with puppet/change government. [w:Peace conference §界面]
- Costs = state value × modifiers; single demands cap at 9999; contested bids escalate (×1.2 per turn, cap 15), uncontested bids increase ×1.15→×1.6; refunds on giving up decay 100%/92%/84%/76% by contested turn; only one winner can hold a given state or ship. [w:Peace conference §要求花费 §回合与要求]
- State value = 10 base + 1 per 100k population + base resources + building values (civilian factory 5, military factory 8, dockyard 5, refinery 5, fuel silo 5, rocket site 1, nuclear 10, infrastructure 1, air base 1, AA 1, radar 4) + state-category value (wasteland 0 … megalopolis 48). [w:Peace conference §州价值]
- Taking capital ships costs 0.005 × IC + screening ships at 0.005 × IC; transferred ships keep 25% experience. [w:Peace conference §要求花费 §结论与效果]
- Outcome tension is computed per state (formula on the page) and attributed to the defeated side's conqueror; liberated states reduce tension; occupied states stay occupied unless they become cores. [w:Peace conference §结论与效果]
- AI behaviour: prefers own cores/claims, adjacent states, allies' cores, opposite-ideology takeovers, ideology-appropriate demand types; avoids contested/others' demanded/human-occupied states and enclaves; gives up gradually and ties break on remaining war score, influence distance and country score. [w:Peace conference §AI 行为]

## Engine view

- War score payout: `NDiplomacy.PEACE_SCORE_SCALE_FACTOR = 1.35`, `NDiplomacy.PEACE_SCORE_DISTRIBUTION = { 0.2, 0.2, 0.2, 0.2, 0.2 }` (five turns), `NDiplomacy.PEACE_SCORE_MINOR_BOOST_FRACTION = 0.05` (per-turn minimum share to small participants). [v:common/defines/00_defines.lua → NDiplomacy.PEACE_SCORE_SCALE_FACTOR, NDiplomacy.PEACE_SCORE_DISTRIBUTION, NDiplomacy.PEACE_SCORE_MINOR_BOOST_FRACTION @1.18.3] [w:Peace conference §战争贡献]
- State value ingredients: `NCountry.STATE_VALUE_BASE = 10.0`, `NCountry.STATE_VALUE_MANPOWER_FACTOR = 0.1` (per 10k population), `NCountry.STATE_VALUE_BUILDING_SLOTS_MULT = 4.0` (state-category slots × 4). [v:common/defines/00_defines.lua → NCountry.STATE_VALUE_BASE, NCountry.STATE_VALUE_MANPOWER_FACTOR, NCountry.STATE_VALUE_BUILDING_SLOTS_MULT @1.18.3] [w:Peace conference §州价值]
- Bid costs: `NDiplomacy.PEACE_ACTION_MAX_COST = 9999`, `NDiplomacy.PEACE_COST_FACTOR_CONTESTED_BID = 1.20`, `NDiplomacy.PEACE_COST_FACTOR_CONTESTED_MAX = 15`, `NDiplomacy.PEACE_COST_FACTOR_UNCONTESTED_BID_MIN = 1.15`, `NDiplomacy.PEACE_COST_FACTOR_UNCONTESTED_BID_MAX = 1.60`, `NDiplomacy.PEACE_COST_FACTOR_UNCONTESTED_BID_STEP = 0.15`, `NDiplomacy.PEACE_CONTEST_REFUND_FACTOR = { 1.0, 0.92, 0.84, 0.76 }`, `NDiplomacy.PEACE_COST_FACTOR_CAPITAL_SHIP_IC = 0.005`, `NDiplomacy.PEACE_COST_FACTOR_SCREENING_SHIP_IC = 0.005`, `NDiplomacy.PEACE_COST_FACTOR_STACK_DEMILITARIZED_ZONE = 0.25`, `NDiplomacy.PEACE_COST_FACTOR_STACK_WAR_REPARATION = 0.25`, `NDiplomacy.PEACE_COST_FACTOR_STACK_RESOURCE_RIGHTS = 0.25`, `NDiplomacy.PEACE_COST_FACTOR_STACK_DISMANTLE_INDUSTRY = 0.25`. [v:common/defines/00_defines.lua → NDiplomacy.PEACE_ACTION_MAX_COST, NDiplomacy.PEACE_COST_FACTOR_CONTESTED_BID, NDiplomacy.PEACE_COST_FACTOR_CONTESTED_MAX, NDiplomacy.PEACE_COST_FACTOR_UNCONTESTED_BID_MIN, NDiplomacy.PEACE_COST_FACTOR_UNCONTESTED_BID_MAX, NDiplomacy.PEACE_COST_FACTOR_UNCONTESTED_BID_STEP, NDiplomacy.PEACE_CONTEST_REFUND_FACTOR, NDiplomacy.PEACE_COST_FACTOR_CAPITAL_SHIP_IC, NDiplomacy.PEACE_COST_FACTOR_SCREENING_SHIP_IC, NDiplomacy.PEACE_COST_FACTOR_STACK_DEMILITARIZED_ZONE, NDiplomacy.PEACE_COST_FACTOR_STACK_WAR_REPARATION, NDiplomacy.PEACE_COST_FACTOR_STACK_RESOURCE_RIGHTS, NDiplomacy.PEACE_COST_FACTOR_STACK_DISMANTLE_INDUSTRY @1.18.3] [w:Peace conference §要求花费]
- Influence-based cost reduction is defined by `NDiplomacy.INFLUENCE_MIN_DIST_COST_MODIFIER = 0.70`, `NDiplomacy.INFLUENCE_MAX_DIST_COST_MODIFIER = 1.00`, `NDiplomacy.INFLUENCE_RATIO_CAPITAL = 0.05`, `NDiplomacy.INFLUENCE_RATIO_CORE = 0.45`, `NDiplomacy.INFLUENCE_RATIO_CONTROLLED = 0.5`, `NDiplomacy.INFLUENCE_PER_ADJACENCY = 0.05`, `NDiplomacy.INFLUENCE_MINOR_FACTOR = 1.0` (the wiki's 20/30/50% weights and −35% minor discount do not match these values — see Unknowns). [v:common/defines/00_defines.lua → NDiplomacy.INFLUENCE_MIN_DIST_COST_MODIFIER, NDiplomacy.INFLUENCE_MAX_DIST_COST_MODIFIER, NDiplomacy.INFLUENCE_RATIO_CAPITAL, NDiplomacy.INFLUENCE_RATIO_CORE, NDiplomacy.INFLUENCE_RATIO_CONTROLLED, NDiplomacy.INFLUENCE_PER_ADJACENCY, NDiplomacy.INFLUENCE_MINOR_FACTOR @1.18.3] [w:Peace conference §要求花费]
- Tension from outcomes: `NDiplomacy.TENSION_ANNEX_NO_CLAIM = 0.20`, `NDiplomacy.TENSION_ANNEX_CLAIM = 0.10`, `NDiplomacy.TENSION_ANNEX_CORE = -0.5`, `NDiplomacy.TENSION_PUPPET = 0`, `NDiplomacy.TENSION_FORCE_GOVERNMENT = -0.5`, `NDiplomacy.TENSION_LIBERATE = -1`, `NDiplomacy.TENSION_TAKE_ONE_CAPITAL_SHIP = 0`, `NDiplomacy.TENSION_STATE_VALUE = 0.10`, `NDiplomacy.TENSION_DEMILITARIZE_ZONE = -0.5`, `NDiplomacy.TENSION_WAR_REPARATION = 0`, `NDiplomacy.TENSION_RESOURCE_RIGHTS = 0`, `NDiplomacy.TENSION_DISMANTLE_INDUSTRY = 0` (the wiki's f-factor formula conflicts — see Unknowns). [v:common/defines/00_defines.lua → NDiplomacy.TENSION_ANNEX_NO_CLAIM, NDiplomacy.TENSION_ANNEX_CLAIM, NDiplomacy.TENSION_ANNEX_CORE, NDiplomacy.TENSION_PUPPET, NDiplomacy.TENSION_FORCE_GOVERNMENT, NDiplomacy.TENSION_LIBERATE, NDiplomacy.TENSION_TAKE_ONE_CAPITAL_SHIP, NDiplomacy.TENSION_STATE_VALUE, NDiplomacy.TENSION_DEMILITARIZE_ZONE, NDiplomacy.TENSION_WAR_REPARATION, NDiplomacy.TENSION_RESOURCE_RIGHTS, NDiplomacy.TENSION_DISMANTLE_INDUSTRY @1.18.3] [w:Peace conference §结论与效果]
- Ship XP retention: `NNavy.PEACE_ACTION_TRANSFER_NAVY_EXPERIENCE_RETAINED = 0.25`. [v:common/defines/00_defines.lua → NNavy.PEACE_ACTION_TRANSFER_NAVY_EXPERIENCE_RETAINED @1.18.3] [w:Peace conference §结论与效果]
- AI tie-breaks: `NAI.PEACE_BID_CONTEST_TIE_BREAKER_CONFERENCE_SCORE = 1.0`, `NAI.PEACE_BID_CONTEST_TIE_BREAKER_INFLUENCE_DISTANCE = 1.0`, `NAI.PEACE_BID_CONTEST_TIE_BREAKER_COUNTRY_SCORE = 1.0`. [v:common/defines/00_defines.lua → NAI.PEACE_BID_CONTEST_TIE_BREAKER_* @1.18.3] [w:Peace conference §AI 行为]

## Modding surfaces

- Cost modifiers: `common/peace_conference/cost_modifiers/00_generic_peace.txt` (generic and per-country) and `yalta_peace.txt` — tune bid prices per tag/state. [v:common/peace_conference/cost_modifiers/00_generic_peace.txt @1.18.3] [v:common/peace_conference/cost_modifiers/yalta_peace.txt @1.18.3]
- AI behaviour: `common/peace_conference/ai_peace/*.txt` (per-country rules). [v:common/peace_conference/ai_peace/ @1.18.3]
- Wargoal data: `common/wargoals/00_invasion.txt` etc.; state category values feed state value via `STATE_VALUE_BUILDING_SLOTS_MULT`. [v:common/wargoals/ @1.18.3] [v:common/state_category/ @1.18.3]
- Building values come from `common/buildings/00_buildings.txt`; resource values from state history. [v:common/buildings/00_buildings.txt @1.18.3]
- Demand effects for scripted conferences (e.g. Yalta) are script effects/decisions → [[hoi4-script]], Effect.md. [m:Effect.md]

## Unknowns

- Tension-from-outcomes: the page's `t = 2f × ln(3b + 5v + r + 2)/20` formula and f-values (2/0.5/0/1.25/0.75) conflict with the current `NDiplomacy.TENSION_ANNEX_NO_CLAIM`/`TENSION_ANNEX_CLAIM`/`TENSION_PUPPET` defines; both recorded, defines treated as current.
- Influence cost weights: page says 20%/30%/50% by distance class and −35% for minors; local defines are `RATIO_CAPITAL = 0.05`, `RATIO_CORE = 0.45`, `RATIO_CONTROLLED = 0.5`, `MINOR_FACTOR = 1.0` — version discrepancy, unresolved.
- `TENSION_TAKE_ONE_CAPITAL_SHIP = 0` locally vs "+0.25 tension per capital ship" on the page — unresolved.
- The f-factor scale (page) vs `TENSION_STATE_VALUE = 0.10` (file) both describe tension-per-state; not reconciled.
