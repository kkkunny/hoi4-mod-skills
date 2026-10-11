# Research and experience — Mechanics

> **Topic** — research: slots, speed, ahead-of-time; experience: national XP, commanders, unit levels, medals.
> **Scope** — covered: how research progresses and how XP is earned/spent. Not covered: individual tech trees (content), equipment designers (Equipment modding.md), doctrines content.
> **Note** — *Research* and *Experience* are separate wiki pages, merged here because both are "progress currency" systems.
>
> **Sources & version anchors**
> - [w] game wiki *Research* (rev 96% translated) and *Experience* (via the Chinese mirror), snapshot 2026-09-10. Section anchors use mirror titles.
> - [m] modding wiki offline snapshot files in `[[hoi4-mod]]`'s `references/`.
> - [v] vanilla install **1.18.3 "Case Green"** (`launcher-settings.json`), root `/mnt/data/hoi4/Hearts of Iron IV`.
>
> Number resolution: target-version [v] > [m] > [w]; conflicts are listed under `## Unknowns`.

## Player view

- Research slots unlock one tech at a time; base slots are 2 for most countries and 3 for many European ones (`NCountry.BASE_RESEARCH_SLOTS = 2` in the local defines, with countries overriding via `set_research_slots`), plus 1–3 from focuses; each idle slot stores up to 30 saved days. [w:Research §科研槽]
- Every tech cost is a multiple of the 110-day base; research speed modifiers add to speed (time = ⌈N / (1 + B%)⌉, e.g. 110 days at +15% → 96 days); saved days apply after modifiers; research speed cannot drop below 10% of base. [w:Research §科研时间 §科研速度]
- Speed modifiers include difficulty (Civilian +15% … Elite −30%), trade laws (+10% free trade … +1%), computing techs (+3%…+8%), focus-provided limited bonuses (bound to the tech, with a 3-day regret window to move them), license production (+20% for the licensed base tech) and tech sharing (up to +50%, locked in when applied). [w:Research §科研修正]
- Ahead of time: researching before a tech's year applies a 200%/year penalty (AOT = 2); the page gives the exact day-count formulas and worked examples (a full year early ≈ one-third speed). [w:Research §超前惩罚]
- National experience (army/navy/air) is capped at 500 each; it pays for template edits, doctrines/spirits, variants/designers and some decisions; attachés, volunteers, lend-lease and military-high-command advisors add experience. [w:Experience §国家经验]
- Unit experience levels (land): Green −25%, Trained 0, Regular +25%, Veteran +50%, Elite +75%; navy and air wings gain their own stat bonuses (navy damage/defence/sortie, air attack/agility/night penalty/accident chance), and air wings interpolate bonuses with progress. [w:Experience §单位经验]
- New land divisions usually start Trained (or Green if rushed), navy/air start lowest; exercises raise units only to Regular; higher levels require combat; replacing casualties or editing a template costs experience (0% if battalions are unchanged, up to −50% per changed battalion); ships transferred in a peace conference keep 25% XP. [w:Experience §单位经验]
- Medals can be awarded with PP after notable deeds; each medal grants its own division modifiers, +10% officer field experience, and raises the PP cost of the next medal by +25% (base cost 30 PP per medal); awards usually give the divisional commander 100 XP. [w:Experience §勋章 §勋章列表]

## Engine view

- Research: `NTechnology.BASE_TECH_COST = 110`, `NTechnology.BASE_RESEARCH_POINTS_SAVED = 30`, `NTechnology.MIN_RESEARCH_SPEED = 0.1`, `NTechnology.BASE_YEAR_AHEAD_PENALTY_FACTOR = 2`, `NTechnology.USE_BONUS_REGRET_TIMER = 3`, `NTechnology.LICENSE_PRODUCTION_TECH_BONUS = 0.2`, `NTechnology.MAX_TECH_SHARING_BONUS = 0.5`, `NTechnology.MAX_SUBTECHS = 3`. [v:common/defines/00_defines.lua → NTechnology.* @1.18.3] [w:Research §科研时间 §科研速度]
- National XP caps: `NMilitary.MAX_ARMY_EXPERIENCE = 500`, `NMilitary.MAX_NAVY_EXPERIENCE = 500`, `NMilitary.MAX_AIR_EXPERIENCE = 500`. [v:common/defines/00_defines.lua → NMilitary.MAX_ARMY_EXPERIENCE, NMilitary.MAX_NAVY_EXPERIENCE, NMilitary.MAX_AIR_EXPERIENCE @1.18.3] [w:Experience §国家经验]
- XP generation: `NMilitary.UNIT_EXPERIENCE_PER_COMBAT_HOUR = 0.0001`, `NMilitary.UNIT_EXPERIENCE_PER_TRAINING_DAY = 0.0015`, `NMilitary.TRAINING_EXPERIENCE_SCALE = 62.0`, `NMilitary.FIELD_EXPERIENCE_SCALE = 0.0008`, `NMilitary.FIELD_EXPERIENCE_MAX_PER_DAY = 1.2`, `NMilitary.UNIT_EXPERIENCE_SCALE = 1.0`, `NMilitary.LEADER_EXPERIENCE_SCALE = 1.0`, `NMilitary.XP_GAIN_FACTOR_FOR_MAX_RELATIVE_COMBAT_DAMAGE = 4.0`, `NMilitary.XP_DECAY_RATE_PER_HOUR_IN_COMBAT = 0.03`, `NMilitary.XP_GAIN_PER_OVERRUN_UNIT = 35.0`. [v:common/defines/00_defines.lua → NMilitary.* @1.18.3] [w:Experience §指挥官经验]
- Unit XP rules: `NMilitary.EXPERIENCE_COMBAT_FACTOR = 0.25`, `NMilitary.UNIT_EXPERIENCE_SCALE`, `NMilitary.BATALION_NOT_CHANGED_EXPERIENCE_DROP = 0.0`, `NMilitary.BATALION_CHANGED_EXPERIENCE_DROP = 0.5`, `NNavy.PEACE_ACTION_TRANSFER_NAVY_EXPERIENCE_RETAINED = 0.25`; air wings: `NAir.AIR_WING_XP_MAX = 1000`, `NAir.AIR_WING_XP_LEVELS = { 100, 300, 700, 900 }`. [v:common/defines/00_defines.lua → NMilitary.*, NNavy.*, NAir.* @1.18.3] [w:Experience §单位经验]
- Officers and medals: `NMilitary.FIELD_EXPERIENCE_ON_DIVISION_MULT = 0.04`, `NMilitary.MAX_FIELD_EXPERIENCE_ON_DIVISION = 8000`, `NMilitary.FIELD_EXPERIENCE_ON_DIVISION_PER_MEDAL_MULT = 0.1`, `NMilitary.COST_INCREASE_PER_ACTIVE_MEDAL = 0.25`, `NMilitary.ARMY_LEADER_XP_GAIN_PER_UNIT_IN_COMBAT = 0.1`, `NMilitary.DIVISIONAL_COMMANDER_RANK_XP_THRESHOLD` (array). [v:common/defines/00_defines.lua → NMilitary.* @1.18.3] [w:Experience §勋章]

## Modding surfaces

- Technologies, categories, folders and bonuses → Technology modding.md, [[hoi4-tech]]; research bonuses on focuses/ideas use `research_bonus` blocks → [[hoi4-focus]], [[hoi4-idea]]. [m:Technology modding.md] [m:Idea modding.md]
- Research modifiers: `research_speed_factor`; experience modifiers: `experience_gain_army(_factor)`, `experience_gain_army_unit(_factor)`, `experience_gain_air(_factor)`, `air_training_xp_gain_factor`, `air_mission_xp_gain_factor`, `experience_gain_navy(_factor)`, `experience_gain_navy_unit(_factor)`, `experience_gain_factor` (leader). [m:List of modifiers.md → research_speed_factor, experience_gain_army, experience_gain_air, experience_gain_navy, experience_gain_factor]
- XP spends: variants/designers and modules → Equipment modding.md; template edits → Division modding.md, [[hoi4-tech]]. [m:Equipment modding.md] [m:Division modding.md]
- Medals: definitions live under `common/medals/` and `common/unit_medals/` in the install (verified present), but the offline snapshot has no write-up — no offline reference; treat vanilla files as the source. [v:common/medals/ @1.18.3] [v:common/unit_medals/ @1.18.3]
- Research/XP defines → Defines.md §NTechnology/§NMilitary/§NAir/§NNavy, [[hoi4-define]]. [m:Defines.md §NTechnology]

## Unknowns

- `NCountry.BASE_RESEARCH_SLOTS` is cited from a direct cross-check of 1.18.3 (value 2); the Research page itself only describes slot counts per country (unsourced page-side).
- The Experience page's navy unit-experience table includes a "fighter sortie efficiency" column as printed in the mirror; the wording is mirrored as-is and was not resolved against files (uncertain).
- Medal definitions have no offline documentation; effects/costs above come from the wiki and individual define anchors only.
- Research slot counts per country (2/3/4, +1–3 via focuses) are wiki tables over content files; not transcribed here.
