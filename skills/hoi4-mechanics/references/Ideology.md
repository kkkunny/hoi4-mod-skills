# Ideology — Mechanics

> **Topic** — ideologies: the four groups, their rules, tension gates, modifiers and change paths.
> **Scope** — covered: what the ideology groups/mechanics do. Not covered: focus/event content, tension in general (`Diplomacy.md`), party support internals without page numbers.
>
> **Sources & version anchors**
> - [w] game wiki *Ideology* (rev 103695, 100% translated), via the Chinese mirror, snapshot 2026-09-10. Section anchors use mirror titles.
> - [m] modding wiki offline snapshot files in `[[hoi4-mod]]`'s `references/`.
> - [v] vanilla install **1.18.3 "Case Green"** (`launcher-settings.json`), root `/mnt/data/hoi4/Hearts of Iron IV`.
>
> Number resolution: target-version [v] > [m] > [w]; conflicts are listed under `## Unknowns`.

## Player view

- Every country belongs to one of four ideology groups (democratic, communist, fascist, non-aligned), which drives rules, tension gates, intel sharing, peace-conference costs and available occupation laws; sub-ideologies matter mostly for flavour and opinion (+20 same ruling party, +10 same group). [w:Ideology]
- Rules per group: democrats cannot declare war without a wargoal on the same ideology, cannot puppet or build collaboration governments, cannot send volunteers, but can guarantee across ideologies, force-ideology others and host exile governments; communists/fascists/non-aligned can puppet and build collaboration governments, and only communists/fascists can force ideology and send volunteers. [w:Ideology §民主主义 §共产主义 §法西斯主义 §中立主义]
- Tension gates: democratic justify +100%, faction join +80% (50% while defensive), lend-lease +50%, volunteers +50%, guarantees +25%; communist lend-lease +50%; fascist lend-lease +50%; non-aligned justify +50%, faction +40%, lend-lease +60%, volunteers +40%, guarantees +40%. [w:Ideology §民主主义 §共产主义 §法西斯主义 §中立主义]
- Modifier examples: democrats give +20% civil / +5% army / +20% navy / +5% air intel to others, embargo cost −50%, world-tension impact of war −75%, faction join −90%, faction trade opinion +50%; communists +10/+7.5/+12.5/+7.5 intel, faction join tension −10; fascists justify time −80% at war with majors, +15/+10/+10/+10 intel, faction tension −10; non-aligned get ideology-drift defence −30%. [w:Ideology §民主主义 §共产主义 §法西斯主义 §中立主义]
- Peace-conference cost modifiers: democratic annex +30%, liberate −10%, fascist/communist regime change −10%/−10%, puppet +20%; communist liberate +30%, puppet −10%; fascist annex −10%, liberate +30%, puppet −5%. [w:Ideology §民主主义 §共产主义 §法西斯主义]
- Occupation-law access: democrats get Local Autonomy, communists Liberated Workers, fascists Brutal Oppression. [w:Ideology §民主主义 §共产主义 §法西斯主义]
- Changing ideology: a party above 50% support can win an election (peaceful), or via coup, civil war, or the console `set_ruling_party`; the event chain spans "rise of X", sympathizers, class conflict, coups and revolutions with per-event mean times (30–500 days) and thresholds (20–50% support). [w:Ideology §改变意识形态]

## Engine view

- Ideology groups/fields are data: rules (`can_declare_war_without_wargoal`, `can_puppet`, `can_build_collaboration_government`, `can_guarantee_other_ideologies`, `can_force_government`, `can_send_volunteers`, `can_host_government_in_exile`), tension gates (`join_faction_tension`, `generate_wargoal_tension`, `guarantee_tension`, `lend_lease_tension`, `volunteers_tension`), impact scaling (`faction_impact_on_world_tension`, `war_impact_on_world_tension`) and intel sharing percentages. [w:Ideology] [w:Diplomacy] [w:World tension]
- Ideology drift defence is the non-aligned group's −30% (the only drift-related number the page gives; other drift comes from events/focuses). [w:Ideology §中立主义]
- Localisation note: the generic Nazi sub-ideology uses the key `gen_nazism` internally. [w:Ideology §法西斯主义]

## Modding surfaces

- Ideology groups/types, rules, dynamic ideologies and party/government setup → Ideology modding.md, [[hoi4-ideology]]. [m:Ideology modding.md]
- Tension gates and faction impact fields are set on ideology types → `Diplomacy.md`, Faction modding.md. [m:Faction modding.md]
- Country starting politics use these types in history files → [[hoi4-country]]. [m:Character modding.md]

## Unknowns

- The page lists modifier names only in Chinese display form; the exact modifier keys were not enumerated there (cross-checked keys live in `Diplomacy.md`/`Government.md`).
- No party-support drift numbers are given on the page beyond the event chain; drift mechanics are event/content-driven (unsourced on this page).
- The fascist `gen_nazism` localisation quirk is the page's only file-level anchor.
