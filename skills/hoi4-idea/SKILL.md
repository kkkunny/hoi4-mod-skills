---
name: hoi4-idea
description: Create national spirits, laws, designers, and ideas in HOI4 mods. Use this skill whenever the user wants to add national spirits, political advisors, design companies, military theorists, timed ideas, or hidden ideas. Covers idea categories, modifiers, and sprites.
---

# HOI4 Idea Modding

This skill covers creating ideas — national spirits, laws, designers, and hidden ideas.

## Idea File Structure

Location: `<mod>/common/ideas/*.txt`

```
ideas = {
    country = {                   # Category for national spirits
        my_idea_1 = {
            # definition
        }
        my_idea_2 = {
            # definition
        }
    }
}
```

Categories are defined in `<mod>/common/idea_tags/*.txt`.

### Key Categories
- `country`: National spirits
- `political_advisor`: Political advisors
- `tank_designer`, `ship_designer`, `air_designer`, `materiel_designer`: Design companies
- `high_command`: High command
- `theorist`: Military theorists
- `chief_army`, `chief_navy`, `chief_air`: Military chiefs
- `hidden`: Hidden ideas (no UI display)
- Economy/Conscription/Trade laws are predefined categories

## National Spirit

```
my_idea = {
    modifier = {
        political_power_gain = 0.10
        stability_weekly = 0.005
    }
    
    picture = GFX_idea_my_spirit
    allowed = { original_tag = GER }              # Checked ONCE at game start
    allowed_civil_war = { original_tag = GER }    # Checked for civil war
    
    available = { has_war = yes }                 # Checked continuously
    
    allowed_difficulty = {                        # AI difficulty restrictions
        difficulty > 0
    }
    
    removal_cost = 100                            # Political power to remove (default 0)
    
    days_of_removal = 90                          # Days before permanent removal
    days_of_removal_if_not_completed = 30
    
    leder = {
        name = my_leader_name
        picture = GFX_my_leader_portrait
        expire = "2015.1.1"
        idea = my_unlocked_idea                   # Unlocked by this spirit
        allowed = { is_ai = no }
    }
}
```

## Adding and Removing Ideas

```
add_ideas = idea_name                             # Single idea
add_ideas = { idea1 idea2 idea3 }                 # Multiple ideas
add_timed_idea = {
    idea = idea_name
    days = 365
}
swap_ideas = {
    remove_idea = old_idea
    add_idea = new_idea
}
show_ideas = { idea_name }                        # Reveal hidden ideas
remove_ideas = { idea_name }
```

## Designers

```
my_designer = {
    modifier = {
        political_power_gain = 0.10
    }
    equipment_bonus = {
        tank = {
            armor_value = 0.05
            reliability = 0.05
        }
    }
    research_bonus = {
        armor = {
            research_bonus = 0.10
        }
    }
}
```

### Equipment Bonuses
Available types match equipment archetypes from `<mod>/common/units/equipment/`. Supported stats: `armor_value`, `reliability`, `defense`, `breakthrough`, `hard_attack`, `soft_attack`, `piercing`, `hardness`, `maximum_speed`, `build_cost_ic`, and others.

### Research Bonuses
Categories match those in `<mod>/common/technology_tags/`.

## Localisation

In `<mod>/localisation/english/*_l_english.yml` (UTF-8-BOM):
```
l_english:
 my_idea: "My National Spirit"
 my_idea_desc: "This spirit improves political power gain."
 my_idea_leader: "Leader's Name"
 my_idea_leader_desc: "Leader's description."
```

Override the localisation key with `name = new_key`.

## Sprites for Ideas

Ideas need a sprite in `<mod>/interface/*.gfx`:
```
spriteType = {
    name = GFX_idea_my_spirit
    texturefile = gfx/interface/ideas/my_spirit.dds
}
```
Use as `picture = GFX_idea_my_spirit`.

Generic `GFX_idea_` sprite is used if no specific picture is set.

## Hidden Ideas

Hidden ideas apply modifiers without appearing in the UI:
- Category: `hidden`
- Still need localisation (name shows in tooltips if referenced)
- Add/remove just like regular ideas
- Setting `picture` is optional for hidden ideas

## Common Pitfalls

- **Duplicate idea names**: same idea ID in multiple files causes unpredictable behavior
- **Missing picture**: ideas still work but show default icon
- **Allowed vs Available**: `allowed` checks only at game start; use `available` for dynamic conditions
- **Tooltip formatting**: use `custom_modifier_tooltip` for complex tooltips

## References

This skill covers the essentials. For the complete, unabridged reference with exhaustive details, read the following file(s) under the master skill's references directory:

- `skills/hoi4-mod/references/Idea modding.md` — Full reference for idea categories, modifiers, localisation, and timed ideas.
