# Government — Mechanics

> **Topic** — politics: political power, stability, war support, command power, ideology/elections, leaders, attachés.
> **Scope** — covered: the political economy of a country. Not covered: focus trees (`hoi4-focus`), idea/law writing (`hoi4-idea`), character files (Character modding.md), occupation/resistance (`Occupation.md`, planned).
>
> **Sources & version anchors**
> - [w] game wiki *Government* (via Chinese mirror `cooderatom.github.io/hoi4-zh-wiki/Government.html`), snapshot 2026-09-10. Section anchors use mirror titles.
> - [m] modding wiki offline snapshot files in `[[hoi4-mod]]`'s `references/`.
> - [v] vanilla install **1.18.3 "Case Green"** (`launcher-settings.json`), root `/mnt/data/hoi4/Hearts of Iron IV`.
>
> Number resolution: target-version [v] > [m] > [w]; conflicts are listed under `## Unknowns`.

## Player view

- Political power (PP) is the currency of internal politics, stored between −500 and 2000; base gain is +2/day on Regular difficulty, modified by difficulty (+50% Civilian … −15% Elite). [w:Government §政治点数 §被动增长]
- PP gain scales with stability: −50% at 0% stability, no modifier at 50%, and +10% at 100% in the stability section (a duplicate passage on the same page says +20%; see Unknowns). [w:Government §稳定度]
- Stability (0–100%) above 50% grants up to +10% PP, +20% factory and dockyard output and −20% consumer-goods factor at 100%; below 50% it penalises down to −20% PP and −50% output at 0%; it also sets the resistance target for occupied territory (0 at 100% stability, +20% at 0%). [w:Government §稳定度]
- Stability is base + permanent modifiers (clamped, with permanent excess conserved as a buffer); war gives −20%, low war support up to −30%, ruling-party popularity up to +15% (+25% with the Political Loyalty army spirit). [w:Government §稳定度]
- War support (0–100%) gates laws and focuses; at maximum it gives up to +30% mobilisation speed, +10% attack/defence on core territory and +50% command power gain; at minimum −50% mobilisation, −95% CP gain, −30% surrender limit; it rises +20% for defensive wars and +10% per attaché, and falls −20% for offensive wars. [w:Government §战争支持度]
- Command power (CP) caps at 80 by default and funds commander recruitment (5 × existing leaders, capped 80), field-marshal promotion (40), traits (15), attachés (50, continuously occupied) and air-supply/ground-crew actions (0.05 per plane; 20 per air region). [w:Government §指挥点数]
- Attachés: sender pays 100 PP and 50 CP (occupied), receives 15% of the host's army/navy/air experience, +10% war support (once) and +0.05 daily autonomy if it is the host's overlord; the host gets +2% division org and +10% planning speed, and −0.05 daily autonomy if it is the sender's overlord. [w:Government §武官]
- Ideology/elections: the ruling party and its leader define the government; democracies hold elections every 1–8 years, a party above 50% support takes power, and elections can fire events. [w:Government §意识形态 §选举]
- National spirits are special ideas granted by focuses/events; leaders may carry country modifiers (examples: Hitler +10% PP and +0.10% weekly war support; Stalin +5% PP and −10% stability; Edward VIII −30% PP). [w:Government §国家精神 §拥有修正的领袖]

## Engine view

- PP bounds and gain: `NCountry.POLITICAL_POWER_LOWER_CAP = -500`, `NCountry.POLITICAL_POWER_UPPER_CAP = 2000`, `NPolitics.BASE_POLITICAL_POWER_INCREASE = 2`. [v:common/defines/00_defines.lua → NCountry.POLITICAL_POWER_LOWER_CAP, NCountry.POLITICAL_POWER_UPPER_CAP, NPolitics.BASE_POLITICAL_POWER_INCREASE @1.18.3] [w:Government §政治点数]
- Stability/war-support defaults: `NCountry.DEFAULT_STABILITY = 0.5`, `NCountry.MIN_STABILITY = 0`, `NCountry.MAX_STABILITY = 1`, `NCountry.BASE_STABILITY_WAR_FACTOR = -0.2`, `NCountry.BASE_STABILITY_PARTY_POPULARITY_FACTOR = 0.15`, `NCountry.DEFAULT_WAR_SUPPORT = 0.5`, `NCountry.MIN_WAR_SUPPORT = 0`, `NCountry.MAX_WAR_SUPPORT = 1`. [v:common/defines/00_defines.lua → NCountry.* @1.18.3] [w:Government §稳定度 §战争支持度]
- Stability feeds the resistance target: `NResistance.RESISTANCE_TARGET_MODIFIER_PER_STABILITY_LOSS = 0.2`. [v:common/defines/00_defines.lua → NResistance.RESISTANCE_TARGET_MODIFIER_PER_STABILITY_LOSS @1.18.3] [w:Government §稳定度]
- Command power: `NCountry.BASE_MAX_COMMAND_POWER = 80`, leader costs `NPolitics.ARMY_LEADER_COST = 5`, `NPolitics.ARMY_LEADER_MAX_COST = 80`, `NPolitics.NAVY_LEADER_COST = 5`, `NPolitics.NAVY_LEADER_MAX_COST = 80`, `NMilitary.UNIT_LEADER_ASSIGN_TRAIT_COST = 15`, `NMilitary.PROMOTE_LEADER_CP_COST = 40`; air supply `NAir.MISSION_COMMAND_POWER_COSTS[11] = 0.05`, ground crews `NAir.AIR_MORE_GROUND_CREWS_COST = 20`. [v:common/defines/00_defines.lua → NCountry.BASE_MAX_COMMAND_POWER, NPolitics.*, NMilitary.*, NAir.* @1.18.3] [w:Government §指挥点数]
- Attachés: `NDiplomacy.BASE_SEND_ATTACHE_COST = 100`, `NDiplomacy.BASE_SEND_ATTACHE_CP_COST = 50`, `NCountry.ATTACHE_XP_SHARE = 0.15`, `NDiplomacy.ATTACHE_TO_OVERLORD_EFFECT = 0.05`, `NDiplomacy.ATTACHE_TO_SUBJECT_EFFECT = -0.05`, `NAI.DIPLOMACY_ACCEPT_ATTACHE_OPINION_TRASHHOLD = 20` (sic — typo in the game file), `NAI.DIPLOMACY_ACCEPT_ATTACHE_BASE = 50`. [v:common/defines/00_defines.lua → NDiplomacy.*, NCountry.ATTACHE_XP_SHARE, NAI.DIPLOMACY_ACCEPT_ATTACHE_* @1.18.3] [w:Government §武官]
- Static modifier hooks exist for the stability/war-support bands: `stability_good_modifier`, `stability_bad_modifier`, `war_support_good_modifier`, `war_support_bad_modifier`, `war_support_during_war`, `attache_sent` in `common/modifiers/00_static_modifiers.txt`. [v:common/modifiers/00_static_modifiers.txt @1.18.3] [w:Government §稳定度 §战争支持度]
- Receive-attaché bonuses ship as the `received_attache` idea (+2% division org, +10% planning speed) in `common/ideas/zzz_generic.txt`. [v:common/ideas/zzz_generic.txt @1.18.3] [w:Government §武官]

## Modding surfaces

- Politics/human modifiers: `political_power_gain`, `political_power_factor`, `political_power_cost`, `stability_factor`, `stability_weekly(_factor)`, `party_popularity_stability_factor`, `war_stability_factor`, `defensive_war_stability_factor`, `offensive_war_stability_factor`, `war_support_factor`, `war_support_weekly(_factor)`. [m:List of modifiers.md → political_power_gain, political_power_factor, stability_factor, war_support_factor, war_support_weekly, party_popularity_stability_factor]
- Cost modifiers: `political_advisor_cost_factor`, `economy_cost_factor`, `trade_laws_cost_factor`, `mobilization_laws_cost_factor`, `army_chief_cost_factor`, `army_leader_cost_factor`, `navy_leader_cost_factor`, `military_leader_cost_factor`, `promote_cost_factor`. [m:List of modifiers.md → political_advisor_cost_factor, economy_cost_factor, army_leader_cost_factor, promote_cost_factor]
- Laws/advisors themselves are ideas → [[hoi4-idea]], Idea modding.md; leader traits and ruler modifiers → Character modding.md, [[hoi4-character]]; ideologies/parties and elections → [[hoi4-ideology]]. [m:Idea modding.md] [m:Character modding.md]
- CP/PP costs live in NPolitics/NMilitary/NDiplomacy defines → Defines.md, [[hoi4-define]]. [m:Defines.md §NPolitics]

## Unknowns

- The Government page contradicts itself on stability→PP at 100% (+10% in the stability section vs +20% in the PP section); both are recorded as printed (unsourced resolution).
- The attaché AI acceptance base is cited by the page as a "−50 reluctance" while the local define is `NAI.DIPLOMACY_ACCEPT_ATTACHE_BASE = 50` (base acceptance); the mapping is not stated (unsourced).
- Election mechanics have no dedicated modding reference in the snapshot beyond [[hoi4-ideology]]; election event files are content (common/events) and out of scope here.
