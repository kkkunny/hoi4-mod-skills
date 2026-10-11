---
name: hoi4-onaction
description: Set up game event hooks (on actions) in HOI4 mods. Use this skill whenever the user wants to run effects automatically on game start, monthly, or when wars start, factions join, governments change, states transfer, or any other game event moment.
---

# HOI4 On Actions

Covers `on_actions` — hooks that run effects/events at specific game moments.

## File Location

`common/on_actions/*.txt`. Your additions **append** to vanilla on_actions (you don't replace them).

## Minimal On Action

```
on_actions = {
    on_startup = {
        effect = {
            random_country = {
                limit = { original_tag = SCO }
                set_country_flag = SCO_setup
            }
        }
    }
}
```

## Common Hooks

- **Time**: `on_startup` (game/save load), `on_daily`, `on_weekly`, `on_monthly`, `on_yearly`
- **War/peace**: `on_war_declared`, `on_peace_conference_started/ended`, `on_civil_war_start/end`, `on_capitulation`
- **Diplomacy**: `on_faction_created/joined/left`, `on_puppet`, `on_puppet_released`, `on_guarantee`
- **Politics**: `on_government_change`, `on_election`, `on_ruling_party_change`, `on_new_leader`
- **States/units**: `on_state_control_changed`, `on_state_ownership_changed`, `on_unit_created/destroyed`, `on_annex`
- **Other**: `on_focus_completed`, `on_technology_researched`, `on_decision_taken`, `on_nuke_drop`, `on_building_completed`

Complete list with scope details → `On actions.md`.

## Firing Patterns

```
# Direct effects
on_monthly = { effect = { ... } }

# Fire events for all / random / weighted-random
on_monthly = {
    events = { my_event.0 }
    random_events = { 50 = my_event.0  30 = my_event.1  20 = 0 }      # weights, 0 = nothing
    random_country_events = { my_event.0 = { trigger = { tag = GER } weight = 100 } }
}
```

- **Scope**: varies per on_action — `on_startup` has no specific ROOT (target explicitly); war/diplomacy hooks put the relevant entities in `FROM`. Check the reference per hook.
- On_actions firing `random_events` do **not** preserve `FROM` chains from the triggering effect.
- Common use: random AI path selection at `on_startup` (set a country flag, branch in `ai_strategy_plans` `enable`) → [[hoi4-ai]].
- Effects/triggers syntax → [[hoi4-script]].

## Fatal Pitfalls

- **Assuming a ROOT country in `on_startup`** → effects hit the wrong/no scope
- **Expensive `on_daily` effects** → run every day for every country; gate with triggers
- **Weights not summing to 100** → fine (proportional), but `0` is the only "nothing happens" entry
- **Expecting to overwrite vanilla on_actions** → additions are appended

## References

Full details in the [[hoi4-mod]] skill's `references/` folder:

- `On actions.md` — complete hook list, per-hook scopes, random event details
- [[hoi4-script]] — effects/triggers; [[hoi4-ai]] — AI flags and plans; [[hoi4-event]] — event definitions
