# Manpower — Mechanics

> **Topic** — manpower: available vs used, training limits, casualties, reinforcement, exile/occupation modifiers, garrisons.
> **Scope** — covered: the manpower economy. Not covered: division composition (`Division.md`), occupation laws (future `Occupation.md`), ship/air wing costs (Equipment modding.md).
>
> **Sources & version anchors**
> - [w] game wiki *Manpower* (via Chinese mirror `cooderatom.github.io/hoi4-zh-wiki/Manpower.html`, rev 97% translated), snapshot 2026-09-10. Section anchors use mirror titles. The page has no footnote section.
> - [m] modding wiki offline snapshot files in `[[hoi4-mod]]`'s `references/`.
> - [v] vanilla install **1.18.3 "Case Green"** (`launcher-settings.json`), root `/mnt/data/hoi4/Hearts of Iron IV`.
>
> Number resolution: target-version [v] > [m] > [w]; conflicts are listed under `## Unknowns`.

## Player view

- Manpower is shown top-left (hover for breakdown); available manpower is the sum of recruitable population in controlled states (with off-map storage when states are capped), used manpower is split between army, air and navy. [w:Manpower §可用人力 §合计人力]
- Training capacity: a country can train at most 75% of its fielded manpower or 100,000, whichever is higher; divisions can start partial training as soon as some manpower arrives. [w:Manpower §陆军]
- Unit manpower examples: infantry battalion 1000, engineer support company 300; destroyer I 250, carrier III 5.5K naval service manpower; tactical-bomber wings need 40 per plane and fighter wings 20 per plane. [w:Manpower §陆军 §空军与海军]
- Recruitable population is raised by conscription laws and focuses; non-core states have a heavy penalty (a specific advisor improves −98% to −96%), occupation policy matters, states gain monthly population growth, and events/focuses can grant fixed manpower. [w:Manpower §可用人力]
- Manpower stored in states can be lost if the state is occupied; disbanding/compressing units returns manpower immediately, stored off-map when all states are full. [w:Manpower §可用人力]
- Losses come from casualties in combat and accidents and are removed from the unit immediately; a division destroyed/encircled returns 20% of its remaining manpower and loses the rest. [w:Manpower §损失]
- Reinforcement arrives in chunks of 10% of the template's manpower and requires a continuous path to the capital; ships refill lost crew at 5% per day; exercises and attrition cause no casualties. [w:Manpower §损失]

## Engine view

- Recruitable population model: `NCountry.LOCAL_MANPOWER_ACCESSIBLE_NON_CORE_FACTOR = 0.02` (non-core access base), `NCountry.MAX_NON_CORE_MANPOWER_FACTOR = 1.0` (clamp), with manpower gain modifier keys `conscription`, `conscription_factor`, `non_core_manpower`, `weekly_manpower`, `local_manpower`, `local_non_core_manpower`. [v:common/defines/00_defines.lua → NCountry.LOCAL_MANPOWER_ACCESSIBLE_NON_CORE_FACTOR, NCountry.MAX_NON_CORE_MANPOWER_FACTOR @1.18.3] [m:List of modifiers.md → conscription, conscription_factor, non_core_manpower, weekly_manpower, local_manpower, local_non_core_manpower] [w:Manpower §可用人力]
- Reinforcement: `NCountry.REINFORCEMENT_MANPOWER_CHUNK = 0.1` (10% chunks), `NCountry.REINFORCEMENT_MANPOWER_DELIVERY_SPEED = 10.0` (travel speed modifier). [v:common/defines/00_defines.lua → NCountry.REINFORCEMENT_MANPOWER_CHUNK, NCountry.REINFORCEMENT_MANPOWER_DELIVERY_SPEED @1.18.3] [w:Manpower §损失]
- Training limits: `NProduction.MIN_POSSIBLE_TRAINING_MANPOWER = 100000`, `NProduction.MIN_FIELD_TO_TRAINING_MANPOWER_RATIO = 0.75`. [v:common/defines/00_defines.lua → NProduction.MIN_POSSIBLE_TRAINING_MANPOWER, NProduction.MIN_FIELD_TO_TRAINING_MANPOWER_RATIO @1.18.3] [w:Manpower §陆军]
- Casualty/XP coupling: `NCountry.WAR_SUPPORT_FROM_CASUALTIES = 0.025`, `NMilitary.EXPERIENCE_LOSS_FACTOR = 1.00` (share of experienced soldiers lost with manpower). [v:common/defines/00_defines.lua → NCountry.WAR_SUPPORT_FROM_CASUALTIES, NMilitary.EXPERIENCE_LOSS_FACTOR @1.18.3] [w:Manpower §损失]
- Exile/government-in-exile manpower: `NCountry.COUNTRY_MANPOWER_CAPITULATED_FREE_POOL_FACTOR = 0.1`, `NCountry.COUNTRY_MANPOWER_CAPITULATED_CORE_GAIN_FACTOR = 0.001`, `NCountry.COUNTRY_MANPOWER_CAPITULATED_NON_CORE_GAIN_FACTOR = 0.001`, plus `NCountry.GIE_MANPOWER_TOTAL_MAX_FACTOR = 0.5`, `NCountry.GIE_MANPOWER_RATO_OF_MAX_START_PENALTY = 0.5` (sic — typo in the game file), `NCountry.GIE_MANPOWER_GAIN_PENALTY_MAX = 0.95`, `NCountry.GIE_VETERAN_MANPOWER_CORE_GAIN_FACTOR = 0.01`, `NCountry.GIE_VETERAN_MANPOWER_NON_CORE_GAIN_FACTOR = 0.005`, `NCountry.GIE_EXILE_TRANSFER_ON_LEADER_CAPITULATION_MANPOWER_FACTOR = 0.1`. [v:common/defines/00_defines.lua → NCountry.COUNTRY_MANPOWER_CAPITULATED_FREE_POOL_FACTOR, NCountry.COUNTRY_MANPOWER_CAPITULATED_CORE_GAIN_FACTOR, NCountry.COUNTRY_MANPOWER_CAPITULATED_NON_CORE_GAIN_FACTOR, NCountry.GIE_MANPOWER_TOTAL_MAX_FACTOR, NCountry.GIE_MANPOWER_RATO_OF_MAX_START_PENALTY, NCountry.GIE_MANPOWER_GAIN_PENALTY_MAX, NCountry.GIE_VETERAN_MANPOWER_CORE_GAIN_FACTOR, NCountry.GIE_VETERAN_MANPOWER_NON_CORE_GAIN_FACTOR, NCountry.GIE_EXILE_TRANSFER_ON_LEADER_CAPITULATION_MANPOWER_FACTOR @1.18.3]
- Garrison manpower: `NCountry.GARRISON_MANPOWER_REINFORCEMENT_SPEED = 2000.0`, `NCountry.GARRISON_STR_POW_MANPOWER = 2`, `NCountry.GARRISON_MANPOWER_LOST_BY_ATTACK = 0.016`, `NCountry.GARRISON_MANPOWER_MIN_DELIVERY_SPEED = 0.7`. [v:common/defines/00_defines.lua → NCountry.GARRISON_MANPOWER_REINFORCEMENT_SPEED, NCountry.GARRISON_STR_POW_MANPOWER, NCountry.GARRISON_MANPOWER_LOST_BY_ATTACK, NCountry.GARRISON_MANPOWER_MIN_DELIVERY_SPEED @1.18.3]
- Foreign manpower (lend/ask): `NCountry.FOREIGN_MANPOWER_MIN_THRESHOLD = 5000`, `NCountry.MANPOWER_BUFFER_TO_NOT_GIVE_MAJOR = 0.6`, `NCountry.MANPOWER_BUFFER_TO_NOT_GIVE_MINOR = 0.3`, `NCountry.FOREIGN_MANPOWER_AI_COOLDOWN_DAYS = 30`. [v:common/defines/00_defines.lua → NCountry.* @1.18.3]
- Other anchors: `NCountry.MIN_MANPOWER_RATIO = 0.15` (alert threshold), `NCountry.MAX_INTELLIGENCE_MANPOWER_DATA_DEVIATION = 0.4`. [v:common/defines/00_defines.lua → NCountry.MIN_MANPOWER_RATIO, NCountry.MAX_INTELLIGENCE_MANPOWER_DATA_DEVIATION @1.18.3]

## Modding surfaces

- Conscription laws are ideas in `common/ideas/_manpower.txt` → [[hoi4-idea]], Idea modding.md; recruitable share is set by the law's `conscription`/`conscription_factor`. [m:Idea modding.md] [v:common/ideas/_manpower.txt @1.18.3] [m:List of modifiers.md → conscription, conscription_factor]
- Unit manpower needs come from sub-unit definitions → Unit modding.md; ship/air wing service manpower from their equipment → Equipment modding.md, [[hoi4-tech]]. [m:Unit modding.md] [m:Equipment modding.md]
- Occupation/non-core manpower penalties interact with occupation laws and `local_non_core_manpower` (state modifier) → State modding.md, future `Occupation.md`. [m:State modding.md] [m:List of modifiers.md → local_non_core_manpower]
- Manpower defines → Defines.md §NCountry/§NProduction, [[hoi4-define]]. [m:Defines.md §NCountry]

## Unknowns

- The Manpower page has no footnote section, so none of its numbers were verifiable against defines directly; where a define exists it is cited separately above (the 75%/100k training cap, 10% chunks).
- Wounded/field-hospital recovery numbers are not on this page (they live with field hospital company data); garrison manpower is only partly documented here — see `Occupation.md` later.
- The claimed 20% manpower refund on destroyed divisions is wiki prose (unsourced in files checked).
- The advisor improving non-core recruitable penalty "−98% → −96%" is a wiki example without a define anchor (unsourced).
