---
name: hoi4-focus
description: Create or modify national focus trees in HOI4. Covers focus tree setup, focus definitions, shared/joint focuses, continuous focuses, AI strategy plans, dynamic icons, prerequisites, and mutual exclusivity.
---

# HOI4 National Focus Modding

This skill covers designing and implementing national focus trees, shared focuses, continuous focuses, and AI strategy plans.

## Focus Tree Definition

Location: `<mod>/common/national_focus/*.txt`

```
focus_tree = {
    id = BHR_focus_tree
    
    country = {
        base = 0
        modifier = {
            add = 10
            tag = BHR
        }
    }
    
    default = no
    continuous_focus_position = { x = 50 y = 1000 }
    initial_show_position = { focus = BHR_first_focus }
    
    shared_focus = BHR_shared_branch    # Include shared focuses
    
    focus = { ... }
    focus = { ... }
}
```

### Key Arguments
- `id`: Unique focus tree identifier (mandatory)
- `country`: MTTH block assigning focus tree to countries (evaluated **once** before game start, never refreshed)
- `default = yes`: Makes this the fallback tree. **Exactly one** default tree must exist.
- `reset_on_civilwar`: The revolter always re-evaluates the focus tree. Same tree = keeps completed focuses.
- `shared_focus`: Loads a shared focus and all its prerequisite-connected shared focuses.
- `continuous_focus_position`: Pixel position of continuous focus palette.

## National Focus Definition

### Minimal Focus
```
focus = {
    id = TAG_focus_name
    x = 5
    y = 0
    icon = GFX_focus_icon_name
    cost = 8
    
    completion_reward = {
        add_manpower = 1000
    }
}
```

### Position System
- `x = 5`, `y = 1`: Absolute position. 1 unit x = 96px, 1 unit y = 130px
- `relative_position_id = TAG_other_focus`: Position relative to another focus (preferred for branches)
- `offset = { x = -5 trigger = { has_dlc = "..." } }`: Conditional position adjustments

**Avoid recursion** (circular relative positions) — it causes crashes.

### Prerequisites and Mutual Exclusivity
```
prerequisite = { focus = TAG_focus_1 focus = TAG_focus_2 }    # OR: at least one
prerequisite = { focus = TAG_focus_1 }                          # AND: separate blocks
prerequisite = { focus = TAG_focus_2 }

mutually_exclusive = { focus = TAG_other_focus }
```

- Prerequisites must be physically positioned **above** the focus for proper line rendering
- Use `has_completed_focus` in `available` for complex boolean logic
- Avoid **duplicate focus IDs** across different trees — breaks prerequisite lines

### Triggers
```
available = {                     # Must be met to take the focus
    stability > 0.1
    294 = { is_owned_by = QAT }
}

allow_branch = {                  # Whether the focus is visible (checked on tree load)
    NOT = { has_completed_focus = TAG_other_branch }
}
```
Use `mark_focus_tree_layout_dirty = yes` to refresh `allow_branch` and `offset`.

```
bypass = { ... }                  # Bypass conditions
bypass_if_unavailable = yes       # Auto-bypass when available is false
available_if_capitulated = yes
cancel_if_invalid = no            # Focus pauses (doesn't cancel) when available becomes false
continue_if_invalid = yes
cancel = { ... }                  # Additional cancel conditions
historical_ai = { ... }           # Prevent AI from picking in historical mode
```

### Effects
```
completion_reward = { ... }       # Granted on completion (focus NOT yet marked complete inside here!)
select_effect = { ... }           # Granted on selection (makes focus uncancelable)
bypass_effect = { ... }           # Granted when bypassed
complete_tooltip = { ... }        # Custom tooltip (effect block, use with hidden_effect)
```

### Dynamic Icons
```
focus = {
    icon = {
        trigger = { has_government = democratic }
        value = GFX_focus_democratic
    }
    icon = {
        value = GFX_focus_fallback
    }
    dynamic = yes
}
```

### AI Will Do
```
ai_will_do = {
    base = 5
    modifier = {
        factor = 3
        tag = POL
    }
}
```
AI picks the focus with the highest randomly generated value. Multiplied by 1.5 for continuing the same branch.

### Search Filters
```
search_filters = { FOCUS_FILTER_MANPOWER FOCUS_FILTER_POLITICAL }
```
Custom filters use sprite `GFX_FOCUS_FILTER_MY_MOD` and localisation key `FOCUS_FILTER_MY_MOD`.

### Titlebar Styles
```
text_icon = example_style
```
Styles defined in root-level `style = { ... }` blocks with sprites for `unavailable`, `available`, `current`, `completed` states.

## Shared Focuses

Shared focuses can be used across multiple focus trees:

```
shared_focus = {
    id = LATIN_focus_name
    x = 0 y = 0
    icon = GFX_icon
    completion_reward = { add_political_power = 100 }
}
```

Include in a tree with: `shared_focus = LATIN_focus_name` (loads all prerequisite-connected shared focuses).

**Setting `shared_focus` to a non-existent focus causes a crash.**
**No non-shared focuses + shared focus = crash.**

### Joint Focuses
Joint focuses share completion between countries:
```
joint_focus = {
    id = LATIN_focus_name_2
    joint_trigger = { is_latin_american_country = yes }
    completion_reward = { ... }                        # All joint countries
    completion_reward_joint_originator = { ... }       # Only the completer
    completion_reward_joint_member = { ... }           # Other joint countries
}
```

## Continuous Focuses

Location: `<mod>/common/continuous_focus/*.txt`

```
continuous_focus_palette = {
    id = BHR_focus_palette
    country = { factor = 0 modifier = { tag = BHR add = 10 } }
    position = { x = 50 y = 1000 }
    
    focus = {
        id = BHR_focus_name_1
        icon = GFX_focus_icon_name_1
        daily_cost = 1
        available = { has_war = no }
        enable = { ... }
        modifier = { stability_weekly = 0.005 }
        supports_ai_strategy = ai_focus_peaceful
    }
}
```

## AI Strategy Plans

Location: `<mod>/common/ai_strategy_plans/*.txt`

```
BHR_historical = {
    name = "Historical plan for BHR"
    allowed = { tag = BHR }                          # Checked ONCE at game start
    enable = {                                       # Checked daily
        is_historical_focus_on = yes
        has_game_rule = { rule = BHR_ai_behavior option = DEFAULT }
    }
    abort = { ... }                                  # Removes plan if true
    ai_national_focuses = {                           # Focuses in priority order
        BHR_focus_name_1 BHR_focus_name_2
    }
    focus_factors = {                                 # ai_will_do multipliers
        BHR_focus_name_3 = 0
        BHR_focus_name_5 = 2
    }
    weight = { ... }                                  # Overall plan weight
}
```

### Custom Game Rules
For random AI paths, set a random country flag in `on_startup` and check it in `enable`.

## Focus Icons (Sprite Setup)

Location: `<mod>/interface/*.gfx`

Focus needs two sprites:
1. Regular: `GFX_focus_icon_name`
2. Shine: `GFX_focus_icon_name_shine` (with `_shine` suffix — this is **mandatory**)

Shine sprite template:
```
spriteType = {
    name = GFX_focus_icon_name_shine
    texturefile = gfx/interface/goals/filename.dds
    effectFile = gfx/FX/buttonstate.lua
    animation = {
        animationmaskfile = gfx/interface/goals/filename.dds
        animationtexturefile = gfx/interface/goals/shine_overlay.dds
        animationrotation = -90.0
        animationlooping = no
        animationtime = 0.75
        animationblendmode = "add"
        animationtype = "scrolling"
        animationrotationoffset = { x = 0.0 y = 0.0 }
        animationtexturescale = { x = 1.0 y = 1.0 }
    }
    animation = {                                       # Second wave
        animationmaskfile = gfx/interface/goals/filename.dds
        animationtexturefile = gfx/interface/goals/shine_overlay.dds
        animationrotation = 90.0
        animationlooping = no
        animationtime = 0.75
        animationblendmode = "add"
        animationtype = "scrolling"
        animationrotationoffset = { x = 0.0 y = 0.0 }
        animationtexturescale = { x = 1.0 y = 1.0 }
    }
    legacy_lazy_load = no
}
```

## Key Pitfalls

- **Focus not marked complete** inside `completion_reward` — `has_completed_focus` returns false
- **Duplicate focus IDs** across different trees break lines
- **Recursive `relative_position_id`** causes crashes
- **Missing `_shine` sprite** is the most common reason shine animation doesn't work
- **`shared_focus` referencing non-existent focus** crashes at main menu
- **allow_branch** doesn't inherit from parent — child focuses with their own `allow_branch` must re-check parent conditions

## References

This skill covers the essentials. For the complete, unabridged reference with exhaustive details, read the following file(s) under the master skill's references directory:

- `skills/hoi4-mod/references/National focus modding.md` — Full reference for focus trees, shared focuses, continuous focuses, and AI strategy plans.
