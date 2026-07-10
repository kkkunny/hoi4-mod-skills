---
name: hoi4-modifier
description: Create and apply modifiers in HOI4. Covers static modifiers, dynamic modifiers, global modifiers, national modifiers, and how to apply modifiers through ideas, traits, and other systems.
---

# HOI4 Modifier Reference

This skill covers modifiers — the primary way to create consistent, long-lasting effects in Hearts of Iron IV.

## What Are Modifiers

Modifiers are continuous effects applied to a scope (country, state, unit leader). Unlike effects (one-time changes), modifiers persist and accumulate.

Format: `modifier_name = <numeric_value>`

```
political_power_gain = 0.2
political_power_gain = -0.05
# Total: +0.15 above base
```

A modifier with value 0 always does nothing. Negative values reverse the effect.

## Reading Modifier Values via Variables

```
set_variable = { my_var = modifier@political_power_gain }    # Country/state
set_variable = { my_var = unit_modifier@army_defence_factor } # Unit leaders
```

## What Is NOT a Modifier

- **Research bonuses** — applied through ideas with `research_bonus = { ... }`
- **Equipment bonuses** — applied through ideas with `equipment_bonus = { ... }`
- **Opinion modifiers** — defined in `common/opinion_modifiers/`, applied with `add_opinion_modifier`

## Ways to Apply Modifiers

### 1. Ideas (Country Modifiers)

The simplest way. Apply via effects:
```
add_ideas = idea_name
add_timed_idea = { idea = idea_name days = 365 }
```

### 2. Dynamic Modifiers

Defined in `common/dynamic_modifiers/`. Support variables:
```
# Definition
my_dynamic_modifier = {
    icon = 0
    color = { 255 0 0 }
    enable = { always = yes }
}

# Application
add_dynamic_modifier = {
    modifier = my_dynamic_modifier
    scope = country
    days = 365
    political_power_gain = var:my_var_value
}
```

### 3. Country Leader Traits
```
# Defined in common/country_leader/ or inside character definitions
# Applied through characters
```

### 4. Unit Leader Traits
Primary way to modify unit leaders. Applied to commanders/admirals.

### 5. Static Modifiers

#### Global Modifiers
Located in `common/modifiers/`. Hard-coded on *when* they apply (e.g., stability bonuses), but customizable in *what* they apply.

#### State (Provincial) Modifiers
Located in `common/modifiers/`. Applied to specific states or provinces.

#### Relation Modifiers
```
TAG = {
    add_opinion_modifier = {
        target = OTHER
        modifier = my_opinion_modifier
    }
}
```

## Common Country Modifiers

```
# Political
political_power_gain = 0.10          # +10% PP gain
stability_weekly = 0.005             # +0.5% stability per week
war_support_weekly = 0.005           # +0.5% war support per week

# Research
research_speed_factor = 0.05         # +5% research speed

# Production
industrial_capacity_factory = 0.10
production_factory_efficiency_gain_factor = 0.10
production_factory_max_efficiency_factor = 0.05

# Military
army_experience_gain_factor = 0.10
army_defence_factor = 0.10
army_attack_factor = 0.10
army_speed_factor = 0.05
division_training_time_factor = -0.10
conscription = 0.025                  # +2.5% recruitable population

# Construction
production_speed_buildings_factor = 0.10
line_change_production_efficiency_factor = 0.05

# Resources
extraction_resource_efficiency_factor = 0.10
synthetic_oil_factor = 0.10

# Trade
trade_opinion_factor = 0.10
trade_deal_opinion_factor = 0.10
```

## Common State Modifiers

```
local_resources_factor = 0.10
local_factories = 0.10
local_manpower = 0.10
local_building_slots_factor = 0.10
buildings_max_level_factor = 0.10
state_max_factories_factor = 0.10
minimum_training_level = 10
```

## Common Unit Leader Modifiers

```
army_defence_factor = 0.10
attack_skill = 1
defense_skill = 1
planning_skill = 1
logistics_skill = 1
morale_boost_factor = 0.10
```

## Modifier Block Format

```
modifier = {
    political_power_gain = 0.1
    disabled_ideas = 1               # Boolean: 1 = enabled
}
```

### Hidden Modifiers
```
modifier = {
    hidden_modifier = {
        political_power_gain = 0.1       # Won't show in tooltip
    }
    custom_modifier_tooltip = my_loc     # Custom tooltip text
}
```

## Complete List of Modifiers

For the complete reference, consult:
- `../hoi4/Hearts of Iron IV/common/modifier_definitions/*.txt`
- `../hoi4/Hearts of Iron IV/documentation/` (official docs)

## Common Pitfalls

- **Modifiers are accumulative**: `0` does nothing, negative works as reverse
- **No if statements in modifiers**: use dynamic modifiers with variables for conditional behavior
- **Research/equipment bonuses**: not modifiers — use idea-research_bonus or idea-equipment_bonus

## References

This skill covers the essentials. For the complete, unabridged reference with exhaustive details, read the following file(s) under the master skill's references directory:

- `skills/hoi4-mod/references/Modifiers.md` — Full reference for the modifier system and how modifiers are applied.
- `skills/hoi4-mod/references/List of modifiers.md` — Comprehensive list of all available modifiers in HOI4.
