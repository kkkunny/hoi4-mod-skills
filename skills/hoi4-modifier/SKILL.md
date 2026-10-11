---
name: hoi4-modifier
description: Create and apply modifiers in HOI4 mods. Use this skill whenever the user wants to add stat bonuses or penalties, create dynamic modifiers with variable values, set up ideas that give continuous effects, or understand which modifier keys exist for each scope type.
---

# HOI4 Modifiers

Continuous, persistent effects (`modifier_name = value`). Unlike effects they accumulate; value `0` does nothing, negative values invert.

## What Is NOT a Modifier

- **Research bonuses** — via ideas/focuses: `research_bonus = { category = { research_bonus = 0.1 } }`
- **Equipment bonuses** — via ideas: `equipment_bonus = { tank = { armor_value = 0.05 } }`
- **Opinion modifiers** — `common/opinion_modifiers/`, applied via `add_opinion_modifier`
- **Province/state data** — buildings, resources etc. are their own systems

## Ways to Apply

1. **Ideas / national spirits** — the simplest country modifier: define in `common/ideas/`, apply with `add_ideas` / `add_timed_idea` → [[hoi4-idea]]
2. **Dynamic modifiers** — `common/dynamic_modifiers/`, support per-instance variables:
   ```
   add_dynamic_modifier = {
       modifier = my_dynamic_modifier
       scope = country
       days = 365
       political_power_gain = var:my_var_value
   }
   ```
3. **Country leader traits** — `common/country_leader/` or inline in characters → [[hoi4-character]]
4. **Unit leader traits** — `common/unit_leader/` (the primary way to modify commanders)
5. **Static modifiers** — `common/modifiers/`: global modifiers (hard-coded when they apply, e.g. stability modifiers), state/province modifiers, relation modifiers via `add_opinion_modifier`

## Key Rules

- **Modifiers are accumulative** — the same key from several sources adds up; there are no `if` statements inside a modifier block. Conditional behavior needs dynamic modifiers with variables.
- Read a modifier's current value: `set_variable = { my_var = modifier@political_power_gain }` (countries/states) or `unit_modifier@...` (unit leaders).
- Hidden values: `hidden_modifier = { ... }`; custom tooltip: `custom_modifier_tooltip = loc_key`.
- Scope determines valid keys: country, state, unit leader, character... →
  - `List of modifiers.md` — searchable list of modifier keys
  - `Modifiers.md` — modifier system, application methods, opinion/relation modifiers
- Complete definitions also in the base game: `common/modifier_definitions/*.txt`.

## Fatal Pitfalls

- Modifier key invalid for the scope → silently ignored
- Expecting conditional logic inside a plain modifier → impossible; use a dynamic modifier
- Research/equipment bonuses written as modifiers → not applied

## References

Full details in the [[hoi4-mod]] skill's `references/` folder:

- `Modifiers.md` — modifier system, all application methods, static/opinion modifiers
- `List of modifiers.md` — exhaustive per-scope modifier key list
- [[hoi4-idea]] — typical carrier for country modifiers; [[hoi4-script]] — `modifier@` reads
