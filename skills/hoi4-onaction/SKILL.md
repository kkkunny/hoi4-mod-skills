---
name: hoi4-onaction
description: Set up game event hooks (on actions) in HOI4 mods. Use this skill whenever the user wants to run effects automatically on game start, monthly, or when wars start, factions join, governments change, states transfer, or any other game event moment.
---

# HOI4 On Actions

This skill covers on actions — game event hooks that trigger effects or events at specific moments.

## On Actions File Location

`<mod>/common/on_actions/*.txt`

## Basic Structure

```
on_actions = {
    on_startup = {
        effect = {
            # Runs when the game loads
        }
    }
}
```

## Commonly Used On Actions

### Time-Based

| On Action | When It Fires |
|---|---|
| `on_startup` | When game/save is loaded |
| `on_monthly` | First day of each month |
| `on_weekly` | Every week |
| `on_daily` | Every day |
| `on_yearly` | First day of each year |

### War-Related

| On Action | When It Fires |
|---|---|
| `on_war_declared` | When a war is declared |
| `on_peace_conference_started` | Peace conference begins |
| `on_peace_conference_ended` | Peace conference concludes |
| `on_civil_war_start` | Civil war begins |
| `on_civil_war_end` | Civil war ends |

### Diplomatic

| On Action | When It Fires |
|---|---|
| `on_faction_created` | Faction is created |
| `on_faction_joined` | Country joins a faction |
| `on_faction_left` | Country leaves a faction |
| `on_puppet` | Country is puppeted |
| `on_puppet_released` | Puppet is released |
| `on_guarantee` | Guarantee is issued |

### Political

| On Action | When It Fires |
|---|---|
| `on_government_change` | Government type changes |
| `on_election` | Election occurs |
| `on_ruling_party_change` | Ruling party changes |
| `on_new_leader` | New country leader |

### State Control

| On Action | When It Fires |
|---|---|
| `on_state_control_changed` | State controller changes |
| `on_state_ownership_changed` | State owner changes |
| `on_annex` | Country is annexed |
| `on_capitulation` | Country capitulates |
| `on_capitulation_immediate` | Capitulation (immediate) |

### Unit/Division

| On Action | When It Fires |
|---|---|
| `on_unit_created` | Unit is created |
| `on_unit_destroyed` | Unit is destroyed |
| `on_navy_damaged` | Navy takes damage |

### Other

| On Action | When It Fires |
|---|---|
| `on_focus_completed` | National focus completed |
| `on_technology_researched` | Technology researched |
| `on_decision_taken` | Decision is taken |
| `on_nuke_drop` | Nuclear bomb dropped |
| `on_operative_created` | Operative is created |
| `on_core_removed` | Core is removed from a country |
| `on_diplomatic_action_taken` | Diplomatic action performed |
| `on_building_completed` | Building construction completes |

## Usage Patterns

### Direct Effects
```
on_startup = {
    effect = {
        random_country = {
            limit = { tag = GER }
            set_country_flag = GER_setup_complete
        }
    }
}
```

### Fire Events
```
on_monthly = {
    events = {
        my_event.0      # Fires for all countries
        my_event.1
    }
}
```

### Random Events
```
on_monthly = {
    random_events = {
        50 = my_event.0        # 50% weight
        30 = my_event.1        # 30% weight
        20 = 0                 # 20% nothing happens
    }
}
```

### Country-Specific Random Events
```
on_monthly = {
    random_country_events = {
        my_event.0 = {
            trigger = {
                tag = GER
                has_war = yes
            }
            weight = 100
        }
    }
}
```

## Scope in On Actions

The ROOT scope depends on the on_action:
- `on_startup`: No specific country (use scopes to target)
- `on_monthly`: Can be country-specific
- Event-based on_actions: FROM is the relevant entity

### FROM in War/Dip On Actions
```
on_war_declared = {
    effect = {
        # ROOT = attacker (usually)
        # FROM = defender (usually)
    }
}
```

## Example: Random AI Path Selection

```
on_startup = {
    effect = {
        random_country = {
            limit = { original_tag = BHR }
            random_list = {
                50 = { set_country_flag = BHR_AI_RANDOM_HISTORICAL }
                50 = { set_country_flag = BHR_AI_RANDOM_ALTERNATE }
            }
        }
    }
}
```
Use flags in `enable` of AI strategy plans to direct AI behavior.

## Event Target Passing in On Actions

Unlike effects that fire events, on_actions that fire random_events do NOT preserve FROM chains from the effect that triggered the on_action.

## Common Pitfalls

- **on_startup ROOT**: Don't assume ROOT is any specific country; use explicit scopes
- **Random events weight**: total can exceed 100 but divide proportionally
- **Performance**: `on_daily` effects run every day for every country — use triggers to limit execution
- **on_actions append**: you don't replace vanilla on_actions — your additions get appended

## References

This skill covers the essentials. For the complete, unabridged reference with exhaustive details, read the following file(s) under the master skill's references directory:

- `skills/hoi4-mod/references/On actions.md` — Full reference for on_action hooks, random events, and scope behavior.
