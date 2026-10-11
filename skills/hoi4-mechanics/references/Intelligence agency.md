# Intelligence agency — Mechanics

> **Topic** — intelligence: agency, upgrades, cryptography, operatives, spy master.
> **Scope** — covered: the agency subsystem (La Résistance). Not covered: individual operations/missions content, occupation resistance (`Occupation.md`).
>
> **Sources & version anchors**
> - [w] game wiki *Intelligence_agency* (rev 98816, 98% translated), via the Chinese mirror, snapshot 2026-09-10. Section anchors use mirror titles.
> - [m] modding wiki offline snapshot files in `[[hoi4-mod]]`'s `references/`.
> - [v] vanilla install **1.18.3 "Case Green"** (`launcher-settings.json`), root `/mnt/data/hoi4/Hearts of Iron IV`.
>
> Number resolution: target-version [v] > [m] > [w]; conflicts are listed under `## Unknowns`.

## Player view

- Most agent actions require an intelligence agency: 5 civilian factories for 30 days; every upgrade also costs factories and 30 days (a "Shadowy Gentleman" advisor cuts the time by 15% → 25 days). [w:Intelligence agency §机构升级]
- Upgrades are split into five branches: Intelligence (civil/army/navy/air intel +25% each), Defence (counter-intelligence, anti-partisan), Operations (steal blueprints, portable radio coordination, invisible ink, plastic explosives, suicide pills), Operative Training (local recruitment, commando training, interrogation, diplomatic training, psychological warfare) and Cryptology Department. [w:Intelligence agency §机构升级]
- Cryptography: cumulative decryption capacity must reach the enemy's cryptology level: 12000 + 4250 × level (level 7 = 41750); capacity is split evenly across simultaneous targets; being at peace gives −50%; breaking a cipher gives passive bonuses (air detection +5%, interception mission efficiency +25%, +10% per broken intel type) and a 30-day public bonus (+15% invasion defence/breakthrough, +50% planning and preparation speed, +50% broken-cipher intel). [w:Intelligence agency §密码学]
- Spy master: requires ≥3 agency upgrades, independence or high-autonomy subject status and faction membership; 50 PP to become spy master, one per faction; extra operative slots come from semi-independent faction members by industry: +0.25 for 10–50 factories, +0.5 for ≥50 (military+civilian, dockyards excluded). [w:Intelligence agency §特工大师]
- Operatives are the field agents; they gain experience and traits via missions and operations, whose strength scales with network size. [w:Intelligence agency §特工 §任务 §行动]
- Agencies use one of nine generic emblems and default names; some countries have predefined agencies. [w:Intelligence agency §机构名称与标志]

## Engine view

- Agency fundamentals: `NOperatives.AGENCY_CREATION_FACTORIES = 5`, `NOperatives.AGENCY_CREATION_DAYS = 30`, `NOperatives.AGENCY_UPGRADE_DAYS = 30`, `NOperatives.AGENCY_UPGRADE_PER_OPERATIVE_SLOT = 5`, `NOperatives.MAX_OPERATIVE_SLOT_FROM_AGENCY_UPGRADES = 1`, `NOperatives.AGENCY_OPERATIVE_RECRUITMENT_TIME = 30`. [v:common/defines/00_defines.lua → NOperatives.* @1.18.3] [w:Intelligence agency §机构升级]
- Spy master: `NOperatives.BECOME_SPYMASTER_PP_COST = 50`, `NOperatives.BECOME_SPYMASTER_FI_COST = 1`, `NOperatives.BECOME_SPYMASTER_MIN_UPGRADES = 3`, `NOperatives.OPERATIVE_SLOTS_FROM_FACTION_MEMBERS_FOR_SPY_MASTER = { 0.0, 0.0, 0.25, 10.0, 0.5, 50.0 }` (threshold pairs: 0.25 slot for 10–50 factories, 0.5 for ≥50). [v:common/defines/00_defines.lua → NOperatives.* @1.18.3] [w:Intelligence agency §特工大师]
- Cryptology: `NOperatives.CRYPTO_BASE_CRYPTO_LEVEL = 12000`, `NOperatives.CRYPTO_CRYPTO_LEVEL_PER_CRYPTO_UPGRADE = 4250`, `NOperatives.CRYPTO_CRYPTO_ACTIVE_BONUS_DURATION = 30`, `NOperatives.CYRPTO_ACTIVE_BONUS_ACTIVATION_PROGRESS_RATIO = 0.5` (sic — typo in the game file). [v:common/defines/00_defines.lua → NOperatives.* @1.18.3] [w:Intelligence agency §密码学]
- Counter-intelligence and network model: `NOperatives.BASE_COUNTER_INTELLIGENCE_RATING = 0.0`, `NOperatives.AGENCY_DEFENSE_EFFECT_ON_HOSTILE_ACTION_COST = 0.2`, `NOperatives.INTEL_NETWORK_BASE_STRENGTH_TARGET_COUNTERINTELLIGENCE_FACTOR = -10.0`, `NOperatives.INTEL_NETWORK_BASE_STRENGTH_TARGET_COUNTERINTELLIGENCE_OFFSET = 90`, `NOperatives.INTEL_NETWORK_STRENGTH_TARGET_OFFSET_PER_OPERATIVE = 15.0`, `NOperatives.INTEL_NETWORK_STRENGTH_DECAY_WHEN_ABOVE_TARGET = -2.5`, `NOperatives.INTEL_NETWORK_GAIN_FROM_ADJACENCY_FACTOR = 0.5`, `NOperatives.INTEL_NETWORK_GAIN_DECAY_PER_STEP_FACTOR = 0.5`, `NOperatives.INTEL_NETWORK_MIN_VP_TO_TARGET = 15`, `NOperatives.INTEL_NETWORK_MIN_STRENGTH_TO_TARGET = 101.0`. [v:common/defines/00_defines.lua → NOperatives.* @1.18.3]
- Intel caps by source: `NIntel.STATIC_INTEL_SOURCE_BROKEN_CYPHER_MAXIMUMS = { 60, 60, 60, 60 }`, `NIntel.STATIC_INTEL_SOURCE_RADAR_MAXIMUMS = { 10, 10, 20, 20 }`, `NIntel.STATIC_INTEL_SOURCE_INTEL_NETWORK_MAXIMUMS = { 30, 30, 40, 30 }`, `NIntel.DYNAMIC_INTEL_SOURCE_EVENT_MAXIMUMS = { 40, 40, 40, 40 }`, `NIntel.DYNAMIC_INTEL_SOURCE_LAND_COMBAT_MAXIMUMS = { 0, 30, 5, 10 }`, `NIntel.DYNAMIC_INTEL_SOURCE_NAVAL_COMBAT_MAXIMUMS = { 10, 0, 40, 20 }`, `NIntel.DYNAMIC_INTEL_SOURCE_AIR_COMBAT_MAXIMUMS = { 0, 0, 0, 25 }`. [v:common/defines/00_defines.lua → NIntel.* @1.18.3]
- The crypto passive/active bonuses and the −50% peace penalty live in `common/modifiers/00_static_modifiers.txt`. [v:common/modifiers/00_static_modifiers.txt @1.18.3] [w:Intelligence agency §密码学]

## Modding surfaces

- Agency upgrades and their effects are tuned by NOperatives defines and the static modifiers file; the upgrade definitions themselves have no offline doc in the snapshot — vanilla files are the source. [v:common/modifiers/00_static_modifiers.txt @1.18.3]
- Operatives, missions and operations: `common/operations/*.txt` — no offline write-up; the master skill lists spy operations as online-only. [v:common/operations/ @1.18.3]
- Operation-related modifiers (cost/outcome/risk keys, resistance interactions) → `Occupation.md`, List of modifiers.md. [m:List of modifiers.md → boost_resistance_factor, operation_make_resistance_contacts_cost]
- Intel-driven bonuses that feed other systems (naval detection, planning) are documented with those systems (`Navy.md`, `Land warfare command.md`). [m:List of modifiers.md → naval_strike_targetting_factor]

## Unknowns

- The Intelligence agency page details only agency/crypto/spy-master numbers; missions/operations have their own wiki pages and no offline reference here (unsourced scope note).
- Upgrade definitions and their factory costs/prerequisites are content; not transcribed beyond the branches listed.
- The `CYRPTO_...` define name is misspelled in the game file; cited as-is so grep matches.
- Cipher-break bonus durations and values beyond the ones listed come from the page's crypto section; only the defines above were file-verified.
