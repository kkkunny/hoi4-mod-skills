---
name: hoi4-character
description: Create country leaders, advisors, generals, and admirals in HOI4 mods. Use this skill whenever the user wants to add new characters, set up character portraits, assign leader traits, recruit unit commanders, or configure advisor roles and bonuses.
---

# HOI4 Character Modding

This skill covers creating characters: country leaders, political advisors, corps commanders, field marshals, admirals, and more.

## Quick Checklist

1. Create character in `<mod>/common/characters/*.txt`
2. Define **name** localisation in `<mod>/localisation/english/*_l_english.yml`
3. Define **portrait** sprites in `<mod>/interface/*.gfx`
4. **Recruit** the character in `<mod>/history/countries/TAG*.txt` (must NOT be last line)
5. If not starting with a role, add it later via `add_country_leader_role`, `add_corps_commander_role`, etc.

## Basic Character Definition

```
characters = {
    SCO_ronald_mcdonald = {
        name = SCO_ronald_mcdonald
        
        portraits = {
            civilian = {
                large = GFX_SCO_ronald_mcdonald
                small = GFX_SCO_advisor_ronald
            }
            army = {
                large = GFX_SCO_ronald_army
                small = GFX_SCO_ronald_army_small
            }
            navy = {
                large = GFX_SCO_ronald_navy
            }
        }
        
        country_leader = {
            ideology = socialism        # Must be ideology TYPE, not group
            traits = { scary_clown }
            expire = "1965.1.1"
        }
        
        advisor = {
            slot = political_advisor
            idea = my_advisor_idea
            allowed = { has_completed_focus = SCO_my_focus }
            visible = { has_country_flag = SCO_advisor_visible }
            traits = { my_trait }
        }
        
        corps_commander = {
            skill = 4
            attack_skill = 3
            defense_skill = 4
            planning_skill = 3
            logistics_skill = 2
            traits = { veteran }
        }
        
        field_marshal = {
            skill = 5
            traits = { old_guard }
        }
        
        navy_leader = {
            skill = 3
            traits = { superior_tactician }
        }
    }
}
```

## Character Roles

| Role | Block | Effect to add later |
|---|---|---|
| Country leader | `country_leader = { ... }` | `add_country_leader_role` |
| Political advisor | `advisor = { ... }` | `add_advisor_role` |
| Corps commander | `corps_commander = { ... }` | `add_corps_commander_role` |
| Field marshal | `field_marshal = { ... }` | `add_field_marshal_role` |
| Navy leader | `navy_leader = { ... }` | `add_naval_commander_role` |
| High command | `high_command = { ... }` | Use slot-based approach |
| Theorist | `theorist = { ... }` | Use slot-based approach |

## Country Leader

```
country_leader = {
    ideology = socialism         # Ideology TYPE (e.g., socialism, stalinism, despotism)
    traits = { trait1 trait2 }
    expire = "1965.1.1"         # Optional: death/retirement date
    id = -1                     # Leader ID for sub-ideology (optional)
}
```

**Must be recruited BEFORE `set_politics`** in country history, otherwise the default leader will stay.

The `ideology` must be an ideology **type** (e.g., `socialism`, `democratic`, `stalinism`), not an ideology **group** (e.g., `communism`, `democratic`). Check types in `<mod>/common/ideologies/*.txt`.

## Unit Leader Skills

| Attribute | Range | Description |
|---|---|---|
| `skill` | 1-10 | Base skill level |
| `attack_skill` | 1-10 | Offensive capability |
| `defense_skill` | 1-10 | Defensive capability |
| `planning_skill` | 1-10 | Planning bonus |
| `logistics_skill` | 1-10 | Supply usage reduction |

## Character Traits

Traits are defined either:
1. Directly in character (inline): `traits = { trait_name }`
2. In `<mod>/common/country_leader/*.txt` (country leader traits)
3. In `<mod>/common/unit_leader/*.txt` (unit leader traits)

Traits apply modifiers:
```
my_trait = {
    political_power_gain = 0.10
    # etc.
}
```

## Portrait Sprites

Portrait categories:
- `civilian`: Used for country leaders and advisors
- `army`: Used for corps commanders and field marshals
- `navy`: Used for admirals

Each category has:
- `large`: Full portrait (country leaders, unit leader details)
- `small`: Icon portrait (advisor slots, unit list)

If no portrait is defined, the game randomly generates one from `portraits/*.txt` pool.

Sprite definition in `<mod>/interface/*.gfx`:
```
spriteTypes = {
    spriteType = {
        name = GFX_SCO_ronald_mcdonald
        texturefile = gfx/leaders/SCO/ronald_mcdonald.dds
    }
}
```

Create a new `.gfx` file rather than overwriting base game files.

## Recruitment

In `<mod>/history/countries/TAG*.txt`:
```
recruit_character = SCO_ronald_mcdonald
```
- Must be **before** `set_politics` for country leaders
- Must **not** be the last line in the file (at least one line after)

## Generic Characters

Generated in `<mod>/history/general/*.txt` using `generate_character`:
```
generate_character = {
    roles = { corps_commander }
    ideology = democratic
    skill = { min = 2 max = 4 }
}
```

## Localisation

In `<mod>/localisation/english/*_l_english.yml`:
```
l_english:
 SCO_ronald_mcdonald: "Ronald McDonald"
 SCO_ronald_mcdonald_desc: "A description of Ronald McDonald."
```

## Common Pitfalls

- **Character not appearing**: forgot `recruit_character` in history
- **Country leader not becoming ruler**: recruited after `set_politics`
- **Recruit on last line**: recruitment fails if no line follows it
- **Ideology type vs group**: `country_leader.ideology` must be a type (e.g., `stalinism`), not a group (e.g., `communism`)
- **Missing advisor from UI**: advisor's `allowed` or `visible` condition isn't met

## References

This skill covers the essentials. For the complete, unabridged reference with exhaustive details, read the following file(s) under the master skill's references directory:

- `skills/hoi4-mod/references/Character modding.md` — Full reference for character definitions, roles, portraits, traits, and recruitment.
