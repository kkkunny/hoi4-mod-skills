---
name: hoi4-mechanics
description: Explain how Hearts of Iron IV actually works — vanilla mechanics from the player's view and the engine implementation, every statement sourced. Use when a modding decision depends on how vanilla behaves (production, economy, supply, combat, AI ...), when the user asks how a game system works, or when another HOI4 skill routes here for mechanical context. Not a modding how-to — file paths, syntax and write-up live in [[hoi4-mod]].
---

# HOI4 Mechanics — Curated Reference

How vanilla Hearts of Iron IV works, so mods are designed against verified behaviour instead of guesses. What to edit and how → [[hoi4-mod]] and its sub-skills.

## Topic Files

| Topic | File |
|---|---|
| Production: factories, lines, efficiency, resources, conversion, consumer goods | `references/Production.md` |
| Construction: buildings, slots, infrastructure, consumer-goods allocation, repair, conversion | `references/Construction.md` |
| Trade: resources, routes, influence, international market | `references/Trade.md` |
| Fuel: production from oil, storage, army/air/navy consumption | `references/Fuel.md` |
| Logistics: hub/state/air supply, trains, shortage effects | `references/Logistics.md` |
| Division: templates, battalions, support companies, stat aggregation | `references/Division.md` |
| Land battle: width, reserves, damage, tactics, combat factors | `references/Land battle.md` |
| Terrain and weather: per-terrain modifiers, day/night, temperature, weather events | `references/Terrain and weather.md` |
| Attrition: land attrition and air/naval accident formulas | `references/Attrition.md` |
| Air warfare: bases/wings, superiority, missions, air combat, aces | `references/Air warfare.md` |
| Navy: task forces, screening, targeting/hit/damage, carriers, ship stats | `references/Navy.md` |
| Government: political power, stability, war support, command power, attachés | `references/Government.md` |
| Manpower: recruitable population, casualties, reinforcement, garrisons | `references/Manpower.md` |
| Research and experience: research slots/speed/AOT; XP, unit levels, medals | `references/Research and experience.md` |
| Diplomacy: actions/costs, world tension, factions | `references/Diplomacy.md` |
| Puppet: autonomy ladder, gain/loss rates, independence | `references/Puppet.md` |
| Occupation: resistance, compliance, garrisons, laws | `references/Occupation.md` |
| Peace conference: war score, state value, bids | `references/Peace conference.md` |
| Intelligence agency: agency, crypto, spy master | `references/Intelligence agency.md` |
| Land warfare command: theaters, officers, planning, invasions, paradrops | `references/Land warfare command.md` |
| Nuclear bomb: production, delivery, effects, fallout | `references/Nuclear bomb.md` |
| Raid: nuclear/air/paratrooper raids, tiers, effects | `references/Raid.md` |
| Special project: categories, breakthroughs, scientists, management | `references/Special project.md` |
| Ideology: groups, rules, tension gates, change paths | `references/Ideology.md` |
| Balance of power: sides, bands, modifier payloads | `references/Balance of power.md` |

All planned topics compiled. Add a new topic by following this file's format and the citation rules.

## File Structure (four sections)

- **Player view** — what the player sees and does. Sources: game wiki.
- **Engine view** — hidden behaviour, formulas, defines. Sources: game wiki + vanilla install.
- **Modding surfaces** — where the behaviour is exposed (defines, modifiers, files), routed to the matching modding reference or sub-skill. Sources: modding wiki + vanilla install.
- **Unknowns** — unsourced, version-conflicted, or out-of-scope items. Never mixed into the sections above.

## Citation Rules (mandatory)

Every factual sentence in the four sections ends with at least one source tag:

- [w:<game wiki page> §<section>] — game wiki. Original pages: `hoi4.paradoxwikis.com/<Page>`. When the English site is unreachable, read the Chinese mirror (`cooderatom.github.io/hoi4-zh-wiki/<Page>.html`) and keep the tag on the original page; mirror section titles may serve as anchors.
- [m:<snapshot file> §<section or key>] — modding wiki snapshot files at `skills/hoi4-mod/references/`.
- [v:<path relative to the game root> → <key/object> @<version>] — vanilla install; version from `launcher-settings.json`.

Numbers/behaviour conflicts: target-version `[v]` > `[m]` > `[w]`; list both versions instead of silently picking one. Anything without a source goes to Unknowns, labelled `unsourced`.

## Version Anchors

- `[v]` — vanilla install at `/mnt/data/hoi4/Hearts of Iron IV`; the version is detected from `launcher-settings.json` (currently 1.18.3) and stated per file.
- `[w]` — mirror snapshot date recorded per file header.
- `[m]` — snapshot version unverified; known mismatches are recorded per file under Unknowns.

## Workflow

1. **Player view** — frame the user's request in gameplay terms; check the design against what players actually see.
2. **Engine view** — take formulas/defines; if the user's target game version differs from the anchors, re-verify key numbers in that version.
3. **Modding surfaces** — route to the matching modding reference or sub-skill to make the change; mechanics are explained here once, write-up lives there.
4. **Unknowns** — tell the user what is unverified instead of presenting a guess.
