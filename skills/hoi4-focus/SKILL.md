---
name: hoi4-focus
description: Create or modify national focus trees in HOI4 mods. Use this skill whenever the user wants to build a focus tree, add focuses, set up focus branches, create shared or joint focuses, configure continuous focuses, or design AI focus selection. Covers focus icons, prerequisites, mutual exclusivity, and AI strategy plans.
---

# HOI4 National Focus Modding

Covers focus trees, focuses, shared/joint/continuous focuses. AI focus orders are configured in [[hoi4-ai]].

## File Locations

| Content | Path |
|---|---|
| Focus trees, focuses | `common/national_focus/*.txt` (filename irrelevant) |
| Continuous focuses | `common/continuous_focus/*.txt` |
| Focus icon sprites | `interface/*.gfx` → [[hoi4-gui]] |

## Minimal Focus Tree

```
focus_tree = {
    id = SCO_focus_tree
    country = { base = 0 modifier = { add = 10 original_tag = SCO } }   # evaluated once, never refreshed
    
    default = no                          # exactly one tree in the game must be default = yes
    continuous_focus_position = { x = 50 y = 1000 }
    
    focus = {
        id = SCO_first_focus
        x = 5 y = 0                       # 1 x unit = 96px, 1 y unit = 130px
        icon = GFX_focus_SCO_first
        cost = 8
        completion_reward = { add_political_power = 100 }
    }
}
```

## Key Rules

- **Positions**: prefer `relative_position_id = TAG_other_focus`. `offset = { x = -5 trigger = { ... } }` for conditional shifts. Circular relative positions **crash**.
- **Prerequisites**: one block per AND, multiple `focus =` inside one block = OR. They must sit physically above the focus. Use `has_completed_focus` for complex logic. Duplicate focus IDs across trees break the lines.
- **Visibility vs availability**: `available` (takable), `allow_branch` (visible), `bypass`, `bypass_if_unavailable`, `cancel_if_invalid` etc. — full argument list in `National focus modding.md`.
- **Effects**: `completion_reward` (focus is **not** yet marked complete inside it — `has_completed_focus` returns false), `select_effect`, `bypass_effect`.
- **Icons**: each focus needs `GFX_<name>` **and** `GFX_<name>_shine` sprites; missing `_shine` is the most common cause of broken shine animation. Templates → `National focus modding.md`.
- **`ai_will_do`** on a focus sets AI preference; ordered AI plans live in `common/ai_strategy_plans/` → [[hoi4-ai]].
- **Shared focuses**: declare `shared_focus = { ... }`, include with `shared_focus = LATIN_name` inside a tree. Joint focuses share completion between countries (`joint_trigger`, `completion_reward_joint_*`).
- **Continuous focuses**: `continuous_focus_palette = { id = ... focus = { daily_cost = 1 modifier = { ... } } }`; `supports_ai_strategy` links to AI focuses.

## Fatal Pitfalls

- `shared_focus` pointing at a non-existent focus → **crash** at main menu
- Tree with only shared focuses and no own focus → **crash**
- Recursive `relative_position_id` → **crash**
- Missing `_shine` sprite → shine silently missing

## References

Full details in the [[hoi4-mod]] skill's `references/` folder:

- `National focus modding.md` — every focus/tree argument, search filters, titlebar styles, icon + shine sprite template, continuous/joint focuses
- [[hoi4-ai]] — AI strategy plans and focus order; [[hoi4-gui]] — sprite setup; [[hoi4-idea]] — focus rewards giving ideas
