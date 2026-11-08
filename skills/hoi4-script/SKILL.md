---
name: hoi4-script
description: Reference for HOI4's scripting language — effects (commands), triggers (conditions), and scope navigation. Use this skill whenever writing any effect blocks, trigger conditions, or scope chains in focuses, events, decisions, or any other HOI4 mod files. Covers PREV workaround, every/random/any/all pattern, flow control, and common pitfalls.
---

# HOI4 Script Language

The three pillars: **scopes** (where), **triggers** (conditions, read-only), **effects** (commands, one-time changes). Used together in every focus reward, event option, decision effect, and on_action. Full lists: `Triggers.md`, `Effect.md`, `Scopes.md`, `Data structures.md`.

## Scopes

| Type | Effects | Triggers | As target |
|---|---|---|---|
| Dual scopes | yes | yes | some |
| Effect scopes (`every_`/`random_`) | yes | no | no |
| Trigger scopes (`all_`/`any_`) | no | yes | no |

```
GER = { ... }            # country by tag       123 = { ... }          # state by ID
PREV / THIS / ROOT / FROM / FROM.FROM            # navigation
event_target:my_target = { ... }                 # saved targets
```

Selection patterns: `every_` runs on each match in order; `random_` picks one; `all_` false on first failure; `any_` true on first success (`count = N` for at least N).

## Scope Rules

- Effect scopes can select entities (`random_owned_state`, `every_country`, ...), trigger scopes only check (`any_owned_state`, ...).
- **Non-dual scopes can't be used as targets** — use the PREV workaround:
  ```
  random_country = {
      ROOT = { add_to_faction = PREV }
  }
  ```
- Limit selections: `limit = { ... }`, `prioritize = { 123 456 }`, `include_invisible = yes`.
- When an effect block fires an event: effect's `ROOT` → event's `FROM`; effect's `FROM` → event's `FROM.FROM`; event targets persist.
- `on_startup` effects start in **None scope**; enter a country/state scope before using scoped effects → [[hoi4-onaction]].
- Scopes can't target non-existent countries (`every_possible_country` is the exception).

## Triggers

- Blocks are **AND by default**; use `OR = { ... }`, `NOT = { ... }`, `if`/`else` for logic.
- `=` means **exact equality**, not "at least". Use `>` / `<`.
- Tooltips: `custom_trigger_tooltip = loc_key`.
- Complete trigger list: `Triggers.md`. In-game docs: `../hoi4/Hearts of Iron IV/documentation/triggers_documentation.*`.

## Effects

- One-time only. For lasting effects use **modifiers** (ideas, dynamic modifiers) → [[hoi4-modifier]].
- Flow control: `if = { limit = { ... } ... } else = { ... }`, `random_list = { 30 = { ... } 70 = { ... } }`, `switch = { trigger = ... }`, `hidden_effect`, `effect_tooltip`, `custom_effect_tooltip`.
- Variables/event targets: `set_variable = { my_var = 100 }`, `add_manpower = var:my_var`, `save_event_target_as = my_target`.
- `custom_effect_tooltip` / `custom_trigger_tooltip` only change tooltip text; they do not execute effects.
- Reusable macros: `common/scripted_effects/*.txt` (call with `my_effect = yes`), triggers in `common/scripted_triggers/*.txt`.
- Complete effect list: `Effect.md`. In-game docs: `documentation/effects_documentation.*`.

## Variables & Fixed Point

- Variables are signed 64-bit **fixed-point with 5 decimal places** (`0.00001`). A 6-decimal literal is not rounded: it shifts the decimal point (`0.619608` → `6.19608`), which clamps colors/ratios. Emit at most 5 decimals.
- **Dual-context statements**: `set_temp_variable`, `add_to_temp_variable`, temp-array operations (`add_to_temp_array`, `clear_temp_array`, `remove_from_temp_array`, ...) work in both triggers and effects. `set_variable`, `add_to_variable`, regular array operations are **effect-only**.
- Temporary variables may not survive across a scripted effect/trigger boundary; set and read them in the same block.

## Fatal Pitfalls

- Effect scope used as a target / mix of effect and trigger scopes → errors
- `=` treated as "≥" → wrong logic
- `check_variable = { x != 0 }` is not valid short syntax → use `NOT = { check_variable = { x = 0 } }` or `compare = not_equals`
- One named scripted effect per state for 1000+ states → file/load bloat; use `every_state` + a global array keyed by `THIS.id`
- State events have ambiguous country/state scope → prefer country events + targets
- `has_completed_focus` is false inside that focus's own `completion_reward` → use flags or hidden events

## References

Full details in the [[hoi4-mod]] skill's `references/` folder:

- `Scopes.md` — every scope, chains, PREV/THIS/FROM/ROOT, event targets
- `Triggers.md` — complete trigger list and operators
- `Effect.md` — complete effect list and arguments
- `Data structures.md` — variables, flags, arrays, script values
