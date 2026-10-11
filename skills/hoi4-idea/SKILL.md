---
name: hoi4-idea
description: Create national spirits, laws, designers, and ideas in HOI4 mods. Use this skill whenever the user wants to add national spirits, political advisors, design companies, military theorists, timed ideas, or hidden ideas. Covers idea categories, modifiers, and sprites.
---

# HOI4 Idea Modding

Covers ideas: national spirits, laws, designers, advisors, hidden ideas.

## File Locations

- Ideas: `common/ideas/*.txt`; categories: `common/idea_tags/*.txt`
- Sprites: `interface/*.gfx` → [[hoi4-gui]]; localisation: `idea_id` + `idea_id_desc` → [[hoi4-assets]]
- Modifiers used inside ideas → [[hoi4-modifier]]

## Minimal Idea

```
ideas = {
    country = {                       # category: national spirits
        my_spirit = {
            modifier = { political_power_gain = 0.10 }
            picture = GFX_idea_my_spirit
            
            allowed = { original_tag = SCO }      # checked ONCE at game start
            allowed_civil_war = { original_tag = SCO }
            available = { has_war = no }          # checked continuously
            removal_cost = 100                    # PP to remove; 0 = cannot be removed
        }
    }
}
```

## Categories

`country` (national spirits), `political_advisor`, `high_command`, `theorist`, `chief_army`/`chief_navy`/`chief_air`, `tank_designer`/`ship_designer`/`air_designer`/`materiel_designer`, `hidden` (modifiers with no UI). Economy/Conscription/Trade laws are predefined categories. Custom categories → `common/idea_tags/`.

## Key Rules

- **Apply/remove**: `add_ideas = id` (or `{ a b }`), `add_timed_idea = { idea = id days = 365 }`, `swap_ideas = { remove_idea = old add_idea = new }`, `show_ideas = { id }`, `remove_ideas = { id }`.
- **Designers** use `equipment_bonus = { tank = { armor_value = 0.05 } }` and `research_bonus = { armor = { research_bonus = 0.10 } }` — types must match equipment archetypes / tech categories. Full field lists in `Idea modding.md`.
- **Timed removal**: `days_of_removal`, `days_of_removal_if_not_completed`.
- **Hidden ideas**: category `hidden`, still add/remove like any idea.
- **Generic sprite**: `GFX_idea_` is used when no picture is set (idea still works).

## Fatal Pitfalls

- **Duplicate idea IDs** across files → unpredictable behavior
- **`allowed` used for dynamic conditions** → checks only at game start; use `available`
- **Referenced sprite missing** → default icon, not an error (check visuals)

## References

Full details in the [[hoi4-mod]] skill's `references/` folder:

- `Idea modding.md` — all categories, idea arguments, designer bonuses, timed/hidden ideas, law slots
- [[hoi4-modifier]] — modifier keys; [[hoi4-focus]] / [[hoi4-event]] — granting ideas
- [[hoi4-mechanics]] — mechanics behind economy laws: consumer goods, factory output, trade, fuel — its `references/Production.md`, `Trade.md`, `Fuel.md`; politics and manpower — its `Government.md`, `Manpower.md`
