# Special project — Mechanics

> **Topic** — special projects (Götterdämmerung / 1.15): categories, breakthroughs, facilities/scientists, project management.
> **Scope** — covered: the project pipeline. Not covered: nuclear weapons (`Nuclear bomb.md`), raids (`Raid.md`), the tech/module content each project unlocks.
>
> **Sources & version anchors**
> - [w] game wiki *Special_project* (rev 104156, 96% translated), via the Chinese mirror, snapshot 2026-09-10. Section anchors use mirror titles.
> - [m] modding wiki offline snapshot files in `[[hoi4-mod]]`'s `references/`.
> - [v] vanilla install **1.18.3 "Case Green"** (`launcher-settings.json`), root `/mnt/data/hoi4/Hearts of Iron IV`. Note: special projects are 1.15 content newer than the wiki text in places.
>
> Number resolution: target-version [v] > [m] > [w]; conflicts are listed under `## Unknowns`.

## Player view

- Special projects are long-running programs in an experimental facility of the matching category (Nuclear Research, Naval Engineering, Aerodynamics & Avionics, Land Warfare), led by hired scientists; all need breakthroughs and most add resource/tech/project requirements. [w:Special project §Categories §管理特殊项目]
- Breakthrough gain: +0.12% per research day per assigned slot in the matching category, +0.05% × scientist skill for scientists on basic research (diminishing ×0.5 per extra scientist), +0.01%/day per rocket site (aero) or nuclear reactor (nuclear), +10% for capturing an enemy facility researching that category (−60% per repeat capture). [w:Special project §Breakthroughs]
- Projects run through three phases: establishment, initial research (once, progress kept if stopped) and prototype (repeated iterations until 100%); starting restarts the establishment phase. [w:Special project §管理特殊项目]
- Scientists assigned to a project stop giving passive bonuses; they can be swapped after 10 days; project speed drops to 0.2× under resource shortage and 0.2× under supply shortage (facility needs 3 supply; warning at 66%); an injured scientist zeroes progress; a damaged facility pauses the project until repaired. [w:Special project §研究特殊项目]
- Capturing an enemy facility: your running project loses up to 20%; you gain 20% of the progress difference; the same project's repeat capture gains are reduced 60%. [w:Special project §研究特殊项目]
- Stopping a project takes 10 days during which costs continue and capture still applies; restarting keeps prototype and project progress. [w:Special project §停止特殊项目]
- Project catalog counts: nuclear 6, naval engineering 16, aero/avionics 15, land warfare 9; costs are 1–3 breakthroughs, 45–180 days per prototype with 5–25% progress per iteration, and steel/tungsten/chromium/aluminium/rubber resource costs. [w:Special project §可用特殊项目列表]
- Prototype results are weighted random events (progress swings, scientist XP, research bonuses, facility damage, PP/war-support swings, permanent injuries) — the tables list dozens of outcomes per category. [w:Special project §原型结果]

## Engine view

- Breakthrough rates: `NProject.BREAKTHROUGH_DAILY_TECHNOLOGY_GAIN = 12` (1/100 % per research day), `NProject.BREAKTHROUGH_DAILY_SCIENTIST_SKILL_GAIN = 5` (per skill), `NProject.BASIC_RESEARCH_TECHNOLOGY_BONUS_DIMINISHING_RETURN_FACTOR = 0.5`, `NProject.BREAKTHROUGH_DAILY_ROCKET_SITE_GAIN = 1`, `NProject.BREAKTHROUGH_DAILY_NUCLEAR_REACTOR_GAIN = 1`, `NProject.BASIC_RESEARCH_TECHNOLOGY_BONUS_FACTOR = 0.02`. [v:common/defines/00_defines.lua → NProject.* @1.18.3] [w:Special project §Breakthroughs]
- Capture: `NProject.PROJECT_CAPTURE_BREAKTHROUGH_PROGRESS = 0.1`, `NProject.PROJECT_CAPTURE_DIMINISHING_RETURN = 0.6`, `NProject.PROJECT_LOSS_FACTOR_ON_CAPTURE = 0.2`, `NProject.PROJECT_CAPTURE_GAIN_RATIO = 0.2`. [v:common/defines/00_defines.lua → NProject.* @1.18.3] [w:Special project §研究特殊项目]
- Management: `NProject.DAYS_TO_REMOVE_SCIENTIST = 10`, `NProject.DEFAULT_STOP_PROJECT_DAYS = 10`, `NProject.MINIMUM_PROJECT_SPEED_FACTOR_FROM_RESOURCE_SHORTAGE = 0.2`, `NProject.MINIMUM_PROJECT_SPEED_FACTOR_FROM_SUPPLY = 0.2`, `NProject.NEEDED_SUPPLY_FOR_FULL_SPEED_PROJECT = 3.0`, `NProject.FACILITY_SUPPLY_WARNING_RED_RATIO = 0.66`, `NProject.SCIENTIST_INJURED_FACTOR = 0`, `NProject.DEFAULT_PROJECT_COMPLETION_SCIENTIST_EXPERIENCE_GAIN = 192.0`, `NProject.SCIENTIST_BASIC_RESEARCH_DAILY_XP_GAIN = 0.28`. [v:common/defines/00_defines.lua → NProject.* @1.18.3] [w:Special project §研究特殊项目]
- Structures: `NProject.DEFAULT_COMPLEXITY = 100`, `NProject.PROTOTYPE_PHASE_MAX_PROGRESS = 100`, `NProject.AMOUNT_OF_SUPPORTIVE_SCIENTISTS = 3`, `NProject.SUPPORTIVE_SCIENTISTS_FRACTION = 0.25`, `NProject.SUPPORTIVE_SCIENITST_PROGRESS_BONUS = 0.1` (sic), `NProject.SUPPORTIVE_SCIENTISTS_SHARING_BONUS = 0.05`, `NProject.DISMANTLE_FACILITY_DAYS = 100`. [v:common/defines/00_defines.lua → NProject.* @1.18.3]

## Modding surfaces

- Project and facility definitions live in `common/special_projects/` — no offline doc; vanilla files are the source. [v:common/special_projects/ @1.18.3]
- Facilities are buildings with a supply draw → Building modding.md, `Construction.md`; breakthroughs also come from rocket sites/reactors (`Nuclear bomb.md`). [m:Building modding.md]
- Breakthrough modifiers: `scientist_breakthrough_bonus_factor` (percentual; special-project scientist breakthrough bonus) → [[hoi4-modifier]]. [m:Modifiers.md → scientist_breakthrough_bonus_factor]
- Project outcomes unlock hidden techs, ship modules and buildings → Equipment modding.md, Technology modding.md, [[hoi4-tech]]. [m:Technology modding.md] [m:Equipment modding.md]
- Scientist traits/recruitment are character-like content with NProject defines (`RECRUIT_SCIENTIST_ONE_TRAIT_CHANCE = 0.35`, `RECRUIT_SCIENTIST_COST`, `SCIENTIST_SKILL_LEVEL_*`) → [[hoi4-character]]. [v:common/defines/00_defines.lua → NProject.RECRUIT_SCIENTIST_ONE_TRAIT_CHANCE @1.18.3]

## Unknowns

- The wiki text describes 1.15 features; local 1.18.3 values are used wherever they diverge (all 16 page-cited NProject names were found locally).
- Prototype outcome tables are large randomized content; only counts and representative effects are transcribed.
- The page names no script files; the `common/special_projects/` location was verified locally, not page-cited.
- `SUPPORTIVE_SCIENITST_PROGRESS_BONUS` contains a typo in the game file; cited as-is so grep matches.
