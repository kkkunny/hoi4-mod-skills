---
name: hoi4-character
description: Create country leaders, advisors, generals, and admirals in HOI4 mods. Use this skill whenever the user wants to add new characters, set up character portraits, assign leader traits, recruit unit commanders, or configure advisor roles and bonuses.
---

# HOI4 Character Modding

Covers country leaders, advisors, unit commanders (corps/field marshal/navy), traits, and portraits.

## Checklist

1. Define the character in `common/characters/*.txt`
2. Localisation: `character_id` + `character_id_desc` → [[hoi4-assets]]
3. Portrait sprites in `interface/*.gfx` → [[hoi4-gui]]
4. **Recruit** in `history/countries/TAG*.txt`: `recruit_character = SCO_ronald_mcdonald` — must **not** be the last line of the file
5. Add roles later via `add_country_leader_role` / `add_advisor_role` / `add_corps_commander_role` / `add_field_marshal_role` / `add_naval_commander_role`

## Minimal Character

```
characters = {
    SCO_ronald_mcdonald = {
        name = SCO_ronald_mcdonald
        portraits = {
            civilian = { large = GFX_SCO_ronald_mcdonald small = GFX_SCO_ronald }
            army = { large = GFX_SCO_ronald_army }
            navy = { large = GFX_SCO_ronald_navy }
        }
        country_leader = {
            ideology = socialism        # must be an ideology TYPE, not a group
            traits = { veteran }
            expire = "1965.1.1"
        }
        advisor = {
            slot = political_advisor
            idea = my_advisor_idea      # the idea carries the effect → [[hoi4-idea]]
        }
        corps_commander = { skill = 4 attack_skill = 3 defense_skill = 4 planning_skill = 3 logistics_skill = 2 }
    }
}
```

## Roles

| Role | Block | Add later via |
|---|---|---|
| Country leader | `country_leader` | `add_country_leader_role` |
| Advisor | `advisor` (`slot = political_advisor` etc.) | `add_advisor_role` |
| Corps commander | `corps_commander` | `add_corps_commander_role` |
| Field marshal | `field_marshal` | `add_field_marshal_role` |
| Navy leader | `navy_leader` | `add_naval_commander_role` |
| High command / theorist | `high_command`, `theorist` | slot bound (see reference) |

Unit leader skills: `skill`, `attack_skill`, `defense_skill`, `planning_skill`, `logistics_skill` (1–10). Advisor effects come from the idea (`idea = ...`), not the character block.

## Key Rules

- **Country leaders must be recruited before `set_politics`** in history, or the default leader stays.
- `ideology` uses a **type** (e.g. `stalinism`), never a group (`communism`). Types are defined in `common/ideologies/` → [[hoi4-ideology]].
- Traits: defined inline via `traits = { ... }`; trait definitions in `common/country_leader/` and `common/unit_leader/`, each carrying modifiers → [[hoi4-modifier]].
- Portraits: categories `civilian` (leaders/advisors), `army`, `navy`; `large` full portrait, `small` icon slot. No portrait = random from `portraits/*.txt` pools.
- Generic characters: `generate_character = { roles = { corps_commander } skill = { min = 2 max = 4 } }` in `history/general/*.txt`.
- Full argument lists → `Character modding.md`.

## Fatal Pitfalls

- **Recruit missing** → character never appears
- **Recruited after `set_politics`** → leader never takes over
- **`recruit_character` as last line** → recruitment fails
- **Ideology group instead of type** → character won't become leader

## References

Full details in the [[hoi4-mod]] skill's `references/` folder:

- `Character modding.md` — all role blocks, traits, portraits, generic characters, recruitment
- `Portrait modding.md` — portrait assets; `Namelist modding.md` — name lists in `common/names/`
- [[hoi4-idea]] — advisor effects; [[hoi4-ideology]] — ideology types
