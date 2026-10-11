# Puppet — Mechanics

> **Topic** — autonomy system: puppet levels, autonomy gain/loss, independence.
> **Scope** — covered: the master–subject relationship ladder. Not covered: occupation/compliance (`Occupation.md`), peace deals (`Peace conference.md`), focus-driven subject content.
>
> **Sources & version anchors**
> - [w] game wiki *Puppet* (rev 104398, 100% translated), via the Chinese mirror, snapshot 2026-09-10. Section anchors use mirror titles.
> - [m] modding wiki offline snapshot files in `[[hoi4-mod]]`'s `references/`.
> - [v] vanilla install **1.18.3 "Case Green"** (`launcher-settings.json`), root `/mnt/data/hoi4/Hearts of Iron IV`.
>
> Number resolution: target-version [v] > [m] > [w]; conflicts are listed under `## Unknowns`.

## Player view

- A puppet's government is replaced by one installed by the overlord and follows the overlord's ideology; overlords call puppets into wars, represent them in peace conferences (higher-autonomy subjects like dominions may bid themselves), and if the overlord capitulates, subjects without a still-standing major ally also capitulate. [w:Puppet]
- Overlord benefits: import discounts (integrated puppets 80 units per factory, colonies 16, dominions 10), extra trade on unexported resources, and a share of industry from integrated puppets/reichskommissariats (part of civilian and most military factories; dockyards unaffected); puppet divisions join at 70%/90%/100% manpower by level. [w:Puppet §拥有傀儡国的优势]
- Subjects keep a fleet and can receive lend-lease in peacetime, cannot justify or declare wars, and must manage autonomy or risk annexation. [w:Puppet §作为傀儡国的劣势]
- Standard autonomy ladder (0–1000 within each level): Dominion → Colony → Puppet → Integrated Puppet; changing a level costs 50 PP, annexing 300 PP, becoming free 300 PP. [w:Puppet §自治度等级]
- Per-level modifiers (examples): Dominion +25% extra trade / −25% overlord trade cost / +5% peace score / +25% manpower levy; Colony +50%/−50%/−50% tech sharing / +70% levy; Puppet +100%/−90%/−50% / +90% / +10% peace; Integrated Puppet +25% civilian and +75% military industry to overlord / +100% manpower levy / +20% peace. [w:Puppet §自治度等级]
- Fascist ladder (0–1500): Satellite → Reichsprotectorate → Reichskommissariat; Japanese ladder adds Imperial Subject for Manchukuo; special levels exist (supervised state for democracies with +2.0 autonomy/day, collaboration governments at 80% cooperation, personal unions, Nordic Defence Council, Austro-Hungarian subject, EU member, commissariat, warlord subjects, Indonesian federal states). [w:Puppet §自治度等级（法西斯主义） §受监管国 §合作政府]
- Autonomy changes: overlord lend-leases −1 autonomy per 100 IC sent; subject lend-lease +1 per 100 IC; overlord construction in subject states −0.7 per 100 IC spent; exported resources +0.005/day each; war score contribution ≈60%; overlord capitulation grants +50% of the next level; continuous focuses ±0.5/day; attachés ∓0.05/day. [w:Puppet §改变自治度]
- Independence: fast routes are violent (support other ideology, civil war); the peaceful route is raising autonomy via lend-lease until free, or the Together for Victory process (long). [w:Puppet §获得独立]

## Engine view

- Level-change costs: `NDiplomacy.AUTONOMY_LEVEL_CHANGE_PP_COST_BASE = 50`, `NDiplomacy.AUTONOMY_LEVEL_CHANGE_PP_ANNEX = 300`, `NDiplomacy.AUTONOMY_LEVEL_CHANGE_PP_FREE = 300`. [v:common/defines/00_defines.lua → NDiplomacy.* @1.18.3] [w:Puppet §自治度等级]
- Autonomy flows: `NDiplomacy.LL_TO_PUPPET_AUTONOMY_DAILY_FACTOR = -0.01`, `NDiplomacy.LL_TO_OVERLORD_AUTONOMY_DAILY_FACTOR = 0.05`, `NDiplomacy.MASTER_BUILD_AUTONOMY_FACTOR = -0.7`, `NDiplomacy.RESOURCE_SENT_AUTONOMY_DAILY_FACTOR = 0.005`, `NDiplomacy.WAR_SCORE_AUTONOMY_FACTOR = 0.6`, `NDiplomacy.AUTONOMY_FREEDOM_FROM_CAPITULATE = 0.5`, `NDiplomacy.ATTACHE_TO_SUBJECT_EFFECT = -0.05`, `NDiplomacy.ATTACHE_TO_OVERLORD_EFFECT = 0.05`. [v:common/defines/00_defines.lua → NDiplomacy.* @1.18.3] [w:Puppet §改变自治度]

## Modding surfaces

- Autonomy level definitions (rules per level: extra trade, trade cost, tech-sharing factor, manpower levy, industry transfer, peace score share, lend-lease tension cap, recruitable factor, autonomy growth, licence cost, resistance target, ideology drift) → Autonomy state modding.md, [[hoi4-autonomy]]; files under `common/autonomous_states/`. [m:Autonomy state modding.md] [v:common/autonomous_states/ @1.18.3]
- Autonomy modifiers: `autonomy_gain`, `autonomy_gain_global_factor`, `autonomy_gain_warscore`, `autonomy_gain_trade(_factor)`, `autonomy_gain_ll_to_subject(_factor)`, `autonomy_gain_ll_to_overlord`, `subjects_autonomy_gain`. [m:List of modifiers.md → autonomy_gain, autonomy_gain_global_factor, autonomy_gain_warscore, autonomy_gain_trade, subjects_autonomy_gain]
- Continuous focuses (suppression/autonomy push) → `common/continuous_focus/generic.txt` → [[hoi4-focus]]. [v:common/continuous_focus/generic.txt @1.18.3]
- Autonomy defines → Defines.md §NDiplomacy, [[hoi4-define]]. [m:Defines.md §NDiplomacy]

## Unknowns

- The communism puppet discount is quoted twice with different numbers on the same page (35% vs 10% at peace conferences); neither is define-backed (unsourced discrepancy).
- The exact "war score ≈60%" figures are wiki prose; only `WAR_SCORE_AUTONOMY_FACTOR = 0.6` is certain.
- Special-level definitions (Nordic Defence Council, EU member, personal union) are content-driven; their file locations were not traced in this pass.
- Puppet name variants are a large content list; intentionally not transcribed.
