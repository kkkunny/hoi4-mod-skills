# Nuclear bomb — Mechanics

> **Topic** — nuclear weapons: production, delivery, effects and fallout.
> **Scope** — covered: the nuclear system (Götterdämmerung / 1.15). Not covered: the raid UI itself (`Raid.md`), special project mechanics (`Special project.md`), enrichment technologies (content).
>
> **Sources & version anchors**
> - [w] game wiki *Nuclear_bomb* (rev 102785, 59% translated), via the Chinese mirror, snapshot 2026-09-10. Section anchors use mirror titles.
> - [m] modding wiki offline snapshot files in `[[hoi4-mod]]`'s `references/`.
> - [v] vanilla install **1.18.3 "Case Green"** (`launcher-settings.json`), root `/mnt/data/hoi4/Hearts of Iron IV`.
>
> Number resolution: target-version [v] > [m] > [w]; conflicts are listed under `## Unknowns`. The page cites some defines under a `NMilitary` name; the file's current namespace is `NRaids` (see Unknowns).

## Player view

- Nukes require Engineering techs, special projects (nuclear reactor, fission bomb project, later thermonuclear warhead/naval reactor projects) and a delivery platform; the reactor building enriches a state, the fission project unlocks stockpiling and grants one free test bomb on completion. [w:Nuclear bomb §Preparation]
- Production rates per reactor: standard and civilian reactors make 1 bomb per 852 days (thermonuclear 1278); heavy-water reactors are half speed (1278/2555); the top-bar counter shows stockpile and production progress. [w:Nuclear bomb §Preparation]
- Delivery is integrated into the raids panel: bomber strikes need strategic bomber wings, missile strikes need MRBM/ICBM fired from a rocket site or a nuclear submarine; all targets must be within range. [w:Nuclear bomb §How to drop a nuclear bomb]
- Effects: units in the target province take 10–90% random strength and org damage; thermonuclear weapons hit every province in the state (friendly fire included); province buildings drop to 0 HP; state/shared buildings drop below their current level; a hosting air base loses all stationed planes; the controller loses up to 20% war support, scaled down by VP below 3 and infrastructure below 10. [w:Nuclear bomb §效果]
- The second bomb on a Japanese core state unlocks a special surrender decision (requires Japan below 40 ships and not holding Okinawa/Iwo Jima at the time). [w:Nuclear bomb §效果]
- Fallout: normal bombs apply "nuclear fallout" for 365 days (division speed −30%, resource gain −40%, division defence −25%, repair speeds −40%, max entrenchment −50%, construction −40%); thermonuclear fallout lasts 730 days with harsher values (−65%/−75%/−50%, −75% repairs, −90% entrenchment). [w:Nuclear bomb §State-wide modifiers]

## Engine view

- Production scaling: `NRaids.NUCLEAR_BOMB_PRODUCTION_SCALE = 2555.0` (1 nuclear production = 1 bomb per 7 years; the 852/1278-day rates are this value divided by reactor type) and `NRaids.THERMONUCLEAR_BOMB_PRODUCTION_SCALE = 2555.0`. [v:common/defines/00_defines.lua → NRaids.NUCLEAR_BOMB_PRODUCTION_SCALE, NRaids.THERMONUCLEAR_BOMB_PRODUCTION_SCALE @1.18.3] [w:Nuclear bomb §Preparation]
- Unit damage: `NRaids.NUCLEAR_BOMB_MIN_DAMAGE_PERCENT = 0.1`, `NRaids.NUCLEAR_BOMB_MAX_DAMAGE_PERCENT = 0.9`, `NRaids.THERMONUCLEAR_BOMB_MIN_DAMAGE_PERCENT = 0.6`, `NRaids.THERMONUCLEAR_BOMB_MAX_DAMAGE_PERCENT = 0.9`. [v:common/defines/00_defines.lua → NRaids.NUCLEAR_BOMB_MIN_DAMAGE_PERCENT, NRaids.NUCLEAR_BOMB_MAX_DAMAGE_PERCENT, NRaids.THERMONUCLEAR_BOMB_MIN_DAMAGE_PERCENT, NRaids.THERMONUCLEAR_BOMB_MAX_DAMAGE_PERCENT @1.18.3] [w:Nuclear bomb §效果]
- War-support effect: `NCountry.NUCLEAR_BOMB_DROP_WAR_SUPPORT_EFFECT_MAX_INFRA = 0.2`, `NCountry.NUCLEAR_BOMB_DROP_WAR_SUPPORT_EFFECT_MAX_VP = 3`, `NCountry.THERMONUCLEAR_BOMB_DROP_WAR_SUPPORT_EFFECT_MAX_INFRA = 0.2`, `NCountry.THERMONUCLEAR_BOMB_DROP_WAR_SUPPORT_EFFECT_MAX_VP = 3`. [v:common/defines/00_defines.lua → NCountry.NUCLEAR_BOMB_DROP_WAR_SUPPORT_EFFECT_MAX_INFRA, NCountry.NUCLEAR_BOMB_DROP_WAR_SUPPORT_EFFECT_MAX_VP, NCountry.THERMONUCLEAR_BOMB_DROP_WAR_SUPPORT_EFFECT_MAX_INFRA, NCountry.THERMONUCLEAR_BOMB_DROP_WAR_SUPPORT_EFFECT_MAX_VP @1.18.3] [w:Nuclear bomb §效果]
- Raid-side plumbing (shared with `Raid.md`): `NRaids.NUCLEAR_RAID_CATEGORY_NAME = "nuclear_raids"` links the nuclear button to the raid category. [v:common/defines/00_defines.lua → NRaids.NUCLEAR_RAID_CATEGORY_NAME @1.18.3]

## Modding surfaces

- Enrichment tech: `common/technologies/electronic_mechanical_engineering.txt` (atomic research, +4% research speed). [v:common/technologies/electronic_mechanical_engineering.txt @1.18.3] [w:Nuclear bomb §Preparation]
- Reactors are buildings (`common/buildings/*.txt`) unlocked by special projects → Building modding.md, `Construction.md`; reactor breakthrough gain is a NProject define. [m:Building modding.md]
- Bomb/nuclear production and effects are tuned by NRaids defines → Defines.md, [[hoi4-define]]; production also scales with the `nuclear_production_factor` modifier (percentual; in state scope it applies to the state owner). [m:Defines.md §NRaids] [m:List of modifiers.md → nuclear_production_factor] [m:Modifiers.md → nuclear_production_factor]
- News messages are events/content → [[hoi4-event]]; the delivery UI lives in the raid system. [m:Event modding.md]
- Modifier keys for fallout come from the content state modifiers; they are not part of the snapshot's modifier list (see Unknowns). [w:Nuclear bomb §State-wide modifiers]

## Unknowns

- The wiki cites the damage defines under NMilitary as NUKE_MIN/MAX_DAMAGE_PERCENT (no such keys exist locally); the current names are `NRaids.NUCLEAR_BOMB_MIN_DAMAGE_PERCENT`, `NRaids.NUCLEAR_BOMB_MAX_DAMAGE_PERCENT`, `NRaids.THERMONUCLEAR_BOMB_MIN_DAMAGE_PERCENT`, `NRaids.THERMONUCLEAR_BOMB_MAX_DAMAGE_PERCENT` — a rename between versions (verified by grep).
- The fallout modifier key names are not documented in the snapshot's modifier lists; only the values and durations are transcribed (unsourced key names).
- The Japan surrender event conditions and the "nuclear trifecta" are wiki prose (unsourced in files checked).
