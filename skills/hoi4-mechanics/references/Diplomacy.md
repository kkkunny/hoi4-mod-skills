# Diplomacy — Mechanics

> **Topic** — diplomacy, world tension and factions.
> **Scope** — covered: diplomatic actions and their prices, tension as a global resource, faction rules/goals. Not covered: autonomy (`Puppet.md`), occupation (`Occupation.md`), peace deals (`Peace conference.md`).
>
> **Sources & version anchors**
> - [w] game wiki *Diplomacy* (rev 89558), *World tension* (rev 103592), *Faction* (rev 104439, 91% translated), via the Chinese mirror, snapshot 2026-09-10. Section anchors use mirror titles.
> - [m] modding wiki offline snapshot files in `[[hoi4-mod]]`'s `references/`.
> - [v] vanilla install **1.18.3 "Case Green"** (`launcher-settings.json`), root `/mnt/data/hoi4/Hearts of Iron IV`.
>
> Number resolution: target-version [v] > [m] > [w]; conflicts are listed under `## Unknowns`.

## Player view

- Diplomacy is bilateral (each side's opinion/success variables are independent); faction creation is the only collective action — its members join defensive wars, and invited members may join offensive wars. [w:Diplomacy]
- Guarantees cost 15 PP for the first and +15 more per existing guarantee (target must share ideology; tension gate); when a guaranteed country is attacked, the guarantor joins — except when it also guarantees the attacker. [w:Diplomacy]
- Improve relations costs 10 PP to start and 0.2 PP/day with the same ideology (0.4 otherwise), capped at +100. [w:Diplomacy]
- Attachés need a host at war: 100 PP and 50 command power (continuously occupied). [w:Diplomacy]
- Non-aggression pacts cannot be cancelled for 12 months or without a shared border; afterwards cancellation follows force-ratio rules by months since signing (2:1, then 1:1, then 1:2); pacts expire after 48 months. [w:Diplomacy]
- Faction invite rules: inviter must be a leader; the target needs >30% ruling-ideology support; kicked nations cannot be re-invited for 90 days; a called-to-arms war cannot be left. [w:Diplomacy]
- Boost party popularity costs 0.25 PP/day; staging a coup needs a 200 PP escalation (max 0.5 PP/day) plus equipment, chooses a starting state, and succeeds instantly in one-state countries above 40% support. [w:Diplomacy]
- World tension (0–100 display, buffer above 100) rises from war declarations, annexation in peace deals, volunteers, faction joins and wargoal creation; each event decays 0.005%/day; it lowers wargoal cost/time (up to −50% at 100%) and adds up to +40% war support; joining an in-progress civil war needs 50% tension. [w:World tension]
- Ideology gates (base, before modifiers): justify wargoal — democracies 100%, non-aligned 50%, communist/fascist none; faction join — democratic 80% (50% while defensive), non-aligned 40%; lend-lease 50%/60%; volunteers 50%/40%; guarantee 25%/40% (democratic/non-aligned). [w:World tension]
- Factions: members share military access and exact intel; leaving marks the member as a faction traitor (−75 opinion); leadership can change (200 PP; 180-day lockout afterwards); rules cost 1 faction initiative to add or remove. [w:Faction]
- Faction features: goals (short/mid/long) grant initiative and bonuses; joint research unlocks tech sharing (base +5% per supporting scientist × member count, cap 50%); doctrine sharing costs 1 initiative per branch and grants 10 mastery/month (+2 per theater commander); garrison manpower contributions of 0%/5%/10%/15%. [w:Faction]

## Engine view

- Diplomatic prices: `NDiplomacy.MIN_WARGOAL_JUSTIFY_COST = 2`, `NDiplomacy.GUARANTEE_COST = 15`, `NDiplomacy.BASE_IMPROVE_RELATION_COST = 10`, `NDiplomacy.BASE_IMPROVE_RELATION_SAME_IDEOLOGY_GROUP_MAINTAIN_COST = 0.2`, `NDiplomacy.BASE_IMPROVE_RELATION_DIFFERENT_IDEOLOGY_GROUP_MAINTAIN_COST = 0.4`, `NDiplomacy.BASE_SEND_ATTACHE_COST = 100`, `NDiplomacy.BASE_SEND_ATTACHE_CP_COST = 50`, `NDiplomacy.BASE_BOOST_PARTY_POPULARITY_DAILY_PP = 0.25`, `NDiplomacy.BASE_STAGE_COUP_TOTAL_COST = 200`, `NDiplomacy.BASE_STAGE_COUP_DAILY_PP = 0.5`. [v:common/defines/00_defines.lua → NDiplomacy.* @1.18.3] [w:Diplomacy]
- Pacts and truces: `NDiplomacy.NAP_EXPIRY_MONTHS = 48`, `NDiplomacy.NAP_UNBREAKABLE_MONTHS = 12`, `NDiplomacy.NAP_FORCE_BALANCE_RULE_MONTHS = 6`, `NDiplomacy.NAP_BREAK_FORCE_BALANCE_1/2/3 = 2/1/0.5`, `NDiplomacy.TRUCE_BREAK_COST_PP = 200`, `NDiplomacy.BASE_TRUCE_PERIOD = 180`, `NDiplomacy.DIPLOMACY_REQUEST_EXPIRY_DAYS = 30`, `NDiplomacy.NUM_DAYS_TO_ENABLE_REINVITE_KICKED_NATIONS = 90`. [v:common/defines/00_defines.lua → NDiplomacy.* @1.18.3] [w:Diplomacy]
- Tension: `NDiplomacy.TENSION_DECAY_DAILY = 0.005`, `NDiplomacy.WARGOAL_WORLD_TENSION_REDUCTION = -0.5`, `NDiplomacy.TENSION_FACTION_JOIN = 4`, `NDiplomacy.TENSION_JOIN_ATTACKER = 2`, `NDiplomacy.TENSION_SIZE_FACTOR = 1.0`, `NDiplomacy.JOIN_FACTION_LIMIT_CHANGE_AT_WAR = 0.5`, `NCountry.WAR_SUPPORT_TENSION_IMPACT = 0.4`, `NCountry.CIVIL_WAR_INVOLVEMENT_MIN_TENSION = 0.5`, `NAI.DIPLOMACY_FACTION_GLOBAL_TENSION_FACTOR = 0.2`. [v:common/defines/00_defines.lua → NDiplomacy.*, NCountry.*, NAI.* @1.18.3] [w:World tension]
- Operative pressure on tension gates: `NOperatives.OPERATIVE_BASE_DIPLOMATIC_PRESSURE_TENSION_REQUIREMENTS_DRIFT = 0.001`, `NOperatives.DIPLOMATIC_PRESSURE_MAX_TENSION_REQUIREMENTS_DECREASE = 0.2`, `NOperatives.DIPLOMATIC_PRESSURE_OPERATIVE_STACKING_FACTOR = 0.5`. [v:common/defines/00_defines.lua → NOperatives.* @1.18.3] [w:World tension §特工]
- Factions: `NFactions.FACTION_INITIATIVE_CHANGE_RULE_COST = 1`, `NFactions.FACTION_DOCTRINE_SHARING_UNLOCK_COST = 1`, `NFactions.DOCTRINE_SHARING_BASE_MASTERY_GAIN_MONTHLY = 10`, `NFactions.DOCTRINE_SHARING_MONTHLY_MASTERY_GAIN_PER_COMMANDER = 2`, `NFactions.FACTION_ASSIGN_SCIENTIST_COST = 25`, `NFactions.FACTION_SUPREME_COMMANDER_EFFECTIVENESS = 0.2`, `NFactions.FACTION_THEATER_COMMANDER_COUNTRY_LIMIT_BASE = 3`, `NFactions.FACTION_THEATER_COMMANDER_REGION_LIMIT_BASE = 3`, `NFactions.AI_FACTION_POWER_PROJECTION_TRESHOLD = 1000` (sic). [v:common/defines/00_defines.lua → NFactions.* @1.18.3] [w:Faction]
- Faction membership/rule payloads live on ideologies (`join_faction_tension`, `faction_impact_on_world_tension`, `war_impact_on_world_tension`) and leader traits (`generate_wargoal_tension`, `guarantee_tension`). [w:World tension] [w:Faction]

## Modding surfaces

- Tension/wargoal modifiers: `generate_wargoal_tension`, `generate_wargoal_tension_against`, `justify_war_goal_time`, `enemy_justify_war_goal_time`, `justify_war_goal_when_in_major_war_time`; relations: `opinion_gain_monthly(_factor)`, `opinion_gain_monthly_same_ideology(_factor)`, `improve_relations_maintain_cost_factor`. [m:List of modifiers.md → generate_wargoal_tension, enemy_justify_war_goal_time, justify_war_goal_time, opinion_gain_monthly, improve_relations_maintain_cost_factor]
- Faction rules, templates, goals and AI peace/join behavior → Faction modding.md, [[hoi4-country]]. [m:Faction modding.md]
- Ideology-side tension gates and faction impact fields → Ideology modding.md, [[hoi4-ideology]]. [m:Ideology modding.md]
- Operative diplomatic pressure comes from operations → `Intelligence agency.md`, common/operations. [v:common/operations/ @1.18.3]
- NDiplomacy/NFactions defines → Defines.md, [[hoi4-define]]. [m:Defines.md §NDiplomacy]

## Unknowns

- The World tension page's tension formula for peace outcomes (f-values 2/0.5/0/1.25/0.75) conflicts with the local `NDiplomacy.TENSION_ANNEX_NO_CLAIM`, `NDiplomacy.TENSION_ANNEX_CLAIM` and `NDiplomacy.TENSION_PUPPET` defines; both are recorded, the file values are treated as current (see also `Peace conference.md`).
- The tense decay is described both as "0.5/month" and "0.005%/day" on the same page; only the define is certain.
- Guarantee cost is described as 15/30/45 in Diplomacy but the earlier Government page cites the same ladder via a different action list; the 15 base is define-backed.
- Faction pages carry no footnotes; the NDefines above come from the verified tables in NFactions.
