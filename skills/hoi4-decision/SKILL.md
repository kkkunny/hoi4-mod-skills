---
name: hoi4-decision
description: Create decisions, missions, and decision categories in HOI4 mods. Use this skill whenever the user wants to add clickable decisions, mission objectives, targeted state decisions, or organize decisions into categories in the decision tab.
---

# HOI4 Decision Modding

Covers decision categories, decisions, missions, and targeted decisions.

## File Locations

- Categories: `common/decisions/categories/*.txt`
- Decisions: `common/decisions/*.txt`
- Icons: `interface/*.gfx` → [[hoi4-gui]]; text keys → [[hoi4-assets]]

## Minimal Decision

```
my_decision_category = {
    my_decision = {
        icon = GFX_my_decision_icon
        
        allowed = { original_tag = SCO }        # checked ONCE at game load
        visible = { is_subject = no }           # checked continuously
        available = { has_stability > 0.5 }     # checked continuously — clickable condition
        
        cost = 100                              # political power; must be an integer
        days_remove = 30                        # days before it reappears
        fire_only_once = no
        
        complete_effect = { add_stability = 0.05 }
    }
}
```

Categories accept `allowed`, `visible`, `days_remove`/`days_re_enable`, `collapse`, `priority`, and optional `name`/`desc`/`picture` overrides.

## Trigger Reference

| Trigger | When checked | Purpose |
|---|---|---|
| `allowed` / `potential` | once at load | restrict to country, DLC/identity checks |
| `visible` | continuously | appears in the decision list |
| `available` | continuously | can actually be taken right now |
| `activation` | continuously | missions: shows the goal, cannot be clicked |
| `cancel_trigger` | continuously | auto-cancel; runs `cancel_effect` |
| `remove_effect` | settlement | effect when removed from the list |

Both `visible` and `available` must be true for a decision to be clickable.

## Missions and Targets

- **Missions** = decisions with `activation`, shown with a completion bar; `visible` does nothing for them.
- **Targeted decisions** add `target_trigger` + `state_target = yes` / `on_map_mode = yes`. Inside `target_trigger`: `FROM` = target, `ROOT` = decision-taker; only checked while a state is hovered (performance).
- `ai_will_do` weights AI picking: `base`, `factor`, `add`.

## Fatal Pitfalls

- **`allowed` used for dynamic conditions** → checks once at load, never updates
- **`visible` used to gate missions** → ignored; use `activation`
- **Non-numeric cost** → decision fails to load

## References

Full details in the [[hoi4-mod]] skill's `references/` folder:

- `Decision modding.md` — all category/decision arguments, costs, cooldowns, targeted decision scopes
- [[hoi4-script]] — `complete_effect` / trigger syntax; [[hoi4-ai]] — AI weighting context
