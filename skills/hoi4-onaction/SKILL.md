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

- **Time**: `on_startup` (**new game only — not on save load**, one global pass, None scope); `on_daily` / `on_weekly` / `on_monthly` / `on_yearly` fire **once per country** (root = that country).
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

- **Scope**: varies per on_action. `on_startup` effects start with **no country/state scope**; entering a scope is mandatory before calling scoped effects. War/diplomacy hooks put the relevant entities in `FROM`. Check the reference per hook.
- **Don't annex/delete the current element inside `every_country`** — later countries can be skipped; use `every_possible_country` or iterate a copy.
- On_actions firing `random_events` do **not** preserve `FROM` chains from the triggering effect.
- Common use: random AI path selection at `on_startup` (set a country flag, branch in `ai_strategy_plans` `enable`) → [[hoi4-ai]].
- Effects/triggers syntax → [[hoi4-script]].

## Fatal Pitfalls

- **Assuming a country scope in `on_startup`** → `Invalid Scope ... provided: None`; enter a scope first
- **Relying on `on_startup` for save loads** → it only runs on a new game
- **Annexing the current country inside `every_country`** → later countries can be skipped
- **Expensive `on_daily` effects** → run every day for every country; gate with triggers
- **Assuming `on_monthly` is one global pass** → global scans run once per country; guard with a `global.num_days` variable or one `random_country` pass
- **Repeating a heavy ensure in several files' `on_startup`** → every file's block runs; keep one owner or add a global guard
- **Weights not summing to 100** → fine (proportional), but `0` is the only "nothing happens" entry
- **Expecting to overwrite vanilla on_actions** → additions are appended

## References

Full details in the [[hoi4-mod]] skill's `references/` folder:

- `On actions.md` — complete hook list, per-hook scopes, random event details
- [[hoi4-script]] — effects/triggers; [[hoi4-ai]] — AI flags and plans; [[hoi4-event]] — event definitions
