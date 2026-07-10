---
name: hoi4-decision
description: Create decisions, missions, and decision categories in HOI4. Covers category definitions, triggers (allowed/visible/available/potential), cost, effects, AI will do, and targeted decisions.
---

# HOI4 Decision Modding

This skill covers creating decisions, missions, and decision categories in Hearts of Iron IV.

## Decision Categories

Location: `<mod>/common/decisions/categories/*.txt`

Minimal category:
```
my_decision_category = {
}
```

Category with full options:
```
my_decision_category = {
    name = my_category_name         # Localisation override (optional)
    desc = my_category_desc         # Description override (optional)
    picture = GFX_my_category_icon  # Icon override (optional)
    allowed = { tag = BHR }        # Checked ONCE at game start
    visible = { is_subject = no }  # Checked continuously
    
    days_remove = 30               # Days before decisions reappear after removal
    days_re_enable = 90            # Days before category can be opened again
    
    collapse = yes                 # Category starts collapsed in UI
    priority = 100                 # Sort order (higher = first)
}
```

## Decision Definition

Location: `<mod>/common/decisions/*.txt`

```
my_decision_category = {
    my_decision = {
        icon = GFX_my_decision_icon
        
        allowed = { original_tag = BHR }      # Checked ONCE at start
        visible = { is_subject = no }         # Checked continuously
        available = { has_stability > 0.5 }   # Checked continuously (can be taken)
        
        cost = 100                             # Political power cost
        days_remove = 30                       # Days before decision reappears
        days_re_enable = 0                     # Cooldown after taking
        fire_only_once = no                    # Can be taken only once
        
        ai_will_do = {
            base = 1
            modifier = {
                factor = 2
                has_war = yes
            }
        }
        
        activation = {                         # For missions: triggers display
            has_war = yes
        }
        
        available = {
            has_completed_focus = TAG_my_focus
        }
        
        remove_effect = { ... }                # Effect when removed from list
        cancel_trigger = { NOT = { ... } }     # When to automatically cancel
        cancel_effect = { ... }                # Effect when cancelled
        
        on_map_mode = yes                      # Show highlights on map
        state_target = yes                     # Show state selection highlights
        
        complete_effect = {
            add_political_power = -100
            add_stability = 0.05
        }
    }
}
```

## Triggers Detailed

| Trigger | When Checked | Purpose |
|---|---|---|
| `allowed` | Only at game load | DLC/identity checks; restrict to country |
| `visible` | Continuously (if allowed met) | Whether decision appears in list |
| `available` | Continuously | Whether decision can be taken |
| `potential` | Only at game load | Same as allowed, alternate name |
| `activation` | Continuously | For missions: shows goal (doesn't enable) |
| `cancel_trigger` | Continuously | When met, cancels decision |

### For Targeted Decisions (targeting a state)
```
target_trigger = {
    is_on_continent = europe
    OWNER = { has_government = democratic }
}
```
`FROM` = the target, `ROOT` = the decision-taker. `target_trigger` only checks when a state is hovered, for performance.

### Tips
- Put country/DLC checks in `allowed` (not `visible`) for performance
- `visible` does nothing for missions
- `available` + `visible` both must be true for the decision to be clickable

## Missions

Missions are decisions with `activation` — they show a progress/goal but can't be clicked:
```
my_mission = {
    activation = { has_war = yes }
    visible = { tag = BHR }
    complete_effect = { add_stability = 0.1 }
}
```
Missions appear with a completion bar. The `activation` trigger determines if the mission goal is shown.

## Targeted Decisions

Decisions that target a specific state on the map:
```
my_targeted_decision = {
    target_trigger = {
        controller = { tag = ROOT }
        FROM = { is_owned_by = ROOT }
    }
    allowed = { original_tag = BHR }
    state_target = yes
    on_map_mode = yes
    
    complete_effect = {
        add_building_construction = {
            type = arms_factory
            level = 1
        }
    }
}
```

## AI Will Do

```
ai_will_do = {
    base = 1
    modifier = {
        factor = 3
        has_war = yes
    }
    modifier = {
        add = 5
        num_of_factories > 50
    }
}
```
- `base`: starting value
- `factor`: multiply
- `add`: add

## Localisation

In `<mod>/localisation/english/*_l_english.yml` (UTF-8-BOM):
```
l_english:
 my_decision_category: "My Category"
 my_decision_category_desc: "Category description."
 my_decision: "My Decision"
 my_decision_desc: "Decision description."
```

## Common Pitfalls

- **allowed vs visible**: `allowed` checks only once at load — misleading if used for dynamic conditions
- **Missions ignore visible**: use `activation` instead for mission display
- **Cost must be numeric**: PP, command power, etc. must be integer

## References

This skill covers the essentials. For the complete, unabridged reference with exhaustive details, read the following file(s) under the master skill's references directory:

- `skills/hoi4-mod/references/Decision modding.md` — Full reference for decision categories, triggers, costs, effects, and targeted decisions.
