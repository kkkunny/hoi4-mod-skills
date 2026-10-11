---
name: hoi4-ideology
description: Create or modify ideologies and sub-ideologies in HOI4 mods. Use this skill whenever the user wants to add new ideology types, change ideology colors, set diplomatic rules between ideologies, or configure party names and government types.
---

# HOI4 Ideology Modding

Covers ideology groups, sub-ideology types, and their rules.

## File Location

`common/ideologies/*.txt`. Four default groups: `democratic`, `fascism`, `communism`, `neutrality`.

## Minimal Sub-Ideology

```
ideas = {
    communism = {                     # ideology GROUP
        stalinism = {                 # ideology TYPE
            color = { 255 0 0 }
            can_declare_war_on_same_ideology = yes
            puppet_cost_factor = -0.3
            
            rules = {
                faction_invite_same_ideology = yes
                join_faction_same_ideology = yes
            }
        }
    }
}
```

## Key Rules

- **Type vs group**: `set_politics = { ruling_party = communism }` takes a **group**; `country_leader = { ideology = stalinism }` takes a **type**. Mixing them up breaks leaders and politics.
- Common type arguments: `color`, `can_declare_war_on_same_ideology`, `can_boost_other_ideologies`, `can_send_volunteers`, `can_puppet`, `can_join_factions`, `can_host_exiles`, `puppet_cost_factor`, `min_surrender_limit`, `ai_faction_member_desire` — full list in `Ideology modding.md`.
- `rules = { ... }` controls same-ideology interactions: faction invite/join, puppet, guarantee, lend-lease, military access.
- `dynamic_ideology = yes` enables runtime-defined ideology types.
- Popularities must sum to exactly 100 in `set_popularities`, or nothing is set.
- Localisation: `ideology_group`, `ideology_group_desc`, sub-ideology names; party names per country `TAG_ideology_party` / `TAG_ideology_party_long` → [[hoi4-assets]].

## Fatal Pitfalls

- **Group used where a type is required** (leader `ideology`, type-specific triggers) → character/government fails
- **Popularities not summing to 100** → `set_popularities` silently fails
- **Character referencing an undefined type** → character won't function

## References

Full details in the [[hoi4-mod]] skill's `references/` folder:

- `Ideology modding.md` — all type arguments, rules, dynamic ideologies, party/government setup
- [[hoi4-country]] — country starting politics; [[hoi4-character]] — leader ideology; [[hoi4-assets]] — party/ideology localisation
