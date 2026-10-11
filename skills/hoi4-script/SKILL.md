---
name: hoi4-script
description: Reference for HOI4's scripting language — effects (commands), triggers (conditions), and scope navigation. Use this skill whenever writing any effect blocks, trigger conditions, or scope chains in focuses, events, decisions, or any other HOI4 mod files. Covers PREV workaround, every/random/any/all pattern, flow control, and common pitfalls.
---

# HOI4 Script Language Reference

This skill covers the three pillars of HOI4's scripting language: effects, triggers, and scopes. These are used together in nearly every modding context — focus rewards, event options, decision effects, on_actions, and more.

---

## Part 1: Scopes

Scopes change which entity effects apply to or triggers check against. Format: `scope = { <contents> }`

### Three Types

| Type | Effects | Triggers | As Target |
|---|---|---|---|
| Dual Scopes | Yes | Yes | Some |
| Effect Scopes | Yes | No | No |
| Trigger Scopes | No | Yes | No |

### Pattern Recognition

| Prefix | Type | Behavior |
|---|---|---|
| `all_` | Trigger | Returns false on first failure |
| `any_` | Trigger | Returns true on first success; `count = N` for at least N |
| `every_` | Effect | Executes on each match in order |
| `random_` | Effect | Executes on one random match |

### Dual Scopes (Work Everywhere)

```
GER = { ... }                    # Specific country by tag
123 = { ... }                    # State by ID
PREV = { ... }                   # Previous scope level
THIS = { ... }                   # Current scope
ROOT = { ... }                   # Root of current context
FROM = { ... }                   # Sender/parent scope
FROM.FROM = { ... }              # Two levels up
event_target:my_target = { ... } # Saved event target
```

### PREV — The Critical Workaround

Non-dual scopes (like `random_country`) can't be used as targets:
```
# WRONG: add_to_faction = random_country

# CORRECT:
random_country = {
    ROOT = { add_to_faction = PREV }
}
```

### Event Scope Chains

When an event fires from an effect block:
- Effect's `ROOT` → Event's `FROM`
- Effect's `FROM` → Event's `FROM.FROM`
- Event targets (saved via `save_event_target_as`) persist through chains

### Limiting Scopes

```
random_owned_state = {
    limit = { is_coastal = yes }
    prioritize = { 123 456 }       # Optional: weight towards these
    include_invisible = yes        # Optional: include hidden (unit leaders/MIO)
    add_building_construction = { type = naval_base level = 1 }
}
```

### Common Effect Scopes

| Scope | Target | In Context Of |
|---|---|---|
| `every_country` / `random_country` | Country | Always |
| `every_other_country` / `random_other_country` | Country | Country |
| `every_enemy_country` / `random_enemy_country` | Country | Country |
| `every_allied_country` / `random_allied_country` | Country | Country |
| `every_neighbor_country` / `random_neighbor_country` | Country | Country |
| `every_subject_country` / `random_subject_country` | Country | Country |
| `every_faction_member` | Country | Country |
| `every_state` / `random_state` | State | Always |
| `every_owned_state` / `random_owned_state` | State | Country |
| `random_owned_controlled_state` | State | Country |
| `every_core_state` / `random_core_state` | State | Country |
| `every_unit_leader` / `random_unit_leader` | Unit Leader | Country |
| `every_army_leader` / `random_army_leader` | Unit Leader | Country |
| `every_navy_leader` / `random_navy_leader` | Unit Leader | Country |
| `every_character` / `random_character` | Character | Country |
| `every_country_division` / `random_country_division` | Division | Country |
| `every_operative` / `random_operative` | Operative | Country |
| `every_military_industrial_organization` | MIO | Country |

### Common Trigger Scopes

| Scope | Type | Check |
|---|---|---|
| `all_country` / `any_country` | Country | All/any countries |
| `all_other_country` / `any_other_country` | Country | All/any others |
| `all_neighbor_country` / `any_neighbor_country` | Country | All/any neighbors |
| `all_enemy_country` / `any_enemy_country` | Country | All/any at war |
| `all_allied_country` / `any_allied_country` | Country | All/any allies |
| `all_subject_countries` / `any_subject_country` | Country | All/any subjects |
| `all_state` / `any_state` | State | All/any states |
| `all_owned_state` / `any_owned_state` | State | All/any owned |
| `all_core_state` / `any_core_state` | State | All/any cores |
| `all_unit_leader` / `any_unit_leader` | Unit Leader | All/any unit leaders |
| `all_army_leader` / `any_army_leader` | Unit Leader | All/any army leaders |
| `all_character` / `any_character` | Character | All/any characters |

### Flow Control Scopes

```
hidden_effect = { ... }             # Execute without tooltip
effect_tooltip = { ... }            # Show tooltip, don't execute
custom_effect_tooltip = loc_key     # Custom tooltip text
```

---

## Part 2: Triggers (Conditions)

Triggers check the game state and return true or false. They never modify anything.

### Operators

- `=` — Strict equality (not "equal or greater")
- `>` — Greater than
- `<` — Less than

### Flow Control

```
# AND (default — every block is AND):
trigger = { tag = GER has_political_power > 100 }

# OR:
OR = { tag = GER tag = FRA tag = ENG }

# NOT:
NOT = { has_war = yes is_subject = yes }

# Conditional:
if = {
    limit = { has_dlc = "Man the Guns" }
    # triggers checked if limit is true
}
else = { ... }
```

### Commonly Used Country Triggers

```
tag = GER
original_tag = GER
exists = yes
has_government = democratic
stability > 0.5
war_support > 0.3
has_war = yes
has_defensive_war = yes
has_offensive_war = yes
has_capitulated = yes
is_in_faction = yes / is_in_faction_with = TAG
is_faction_leader = yes
is_subject = yes / is_subject_of = TAG
is_major = yes
has_manpower > 100000
num_of_factories > 50
has_political_power > 100
has_army_experience > 50
has_air_experience > 50
has_navy_experience > 50
has_idea = idea_name
has_completed_focus = TAG_focus
has_researched = tech_name
has_dlc = "Man the Guns"
has_game_rule = { rule = rule_name option = option_name }
date > 1939.1.1
is_historical_focus_on = yes
country_exists = TAG
has_country_flag = my_flag
```

### Commonly Used State Triggers

```
is_owned_by = TAG
is_controlled_by = TAG
is_core_of = TAG
is_claimed_by = TAG
state = 123
is_coastal = yes
has_state_flag = my_flag
is_on_continent = europe
manpower > 100000
```

### Commonly Used Diplomatic Triggers

```
has_opinion = { target = TAG value > 50 }
has_opinion_modifier = { target = TAG modifier = modifier_name }
has_guaranteed = TAG
has_military_access = TAG
has_non_aggression_pact = TAG
```

### Custom Trigger Tooltip

```
custom_trigger_tooltip = my_loc_key    # Shows this text instead of raw conditions
```

### Reference Docs

For the complete trigger list:
```
../hoi4/Hearts of Iron IV/documentation/triggers_documentation.html
../hoi4/Hearts of Iron IV/documentation/triggers_documentation.md
../hoi4/Hearts of Iron IV/common/script_enums.txt
```

---

## Part 3: Effects (Commands)

Effects make changes to the game state. They are **one-time** — for continuous effects, use modifiers.

### Where Effects Are Used

- Focus rewards: `completion_reward = { ... }`
- Event options: `option = { ... }`
- Decision effects: `complete_effect = { ... }`
- Country history files
- On actions
- Scripted effects: `common/scripted_effects/`

### Country Effects

```
add_political_power = 100
add_stability = 0.1
add_war_support = 0.05
add_manpower = 1000
set_stability = 0.7
set_war_support = 0.5
set_research_slots = 4

add_ideas = { idea1 idea2 }
add_timed_idea = { idea = idea_name days = 365 }
remove_ideas = { idea1 }
swap_ideas = { remove_idea = old add_idea = new }

set_technology = { tech_name = 1 }
set_politics = { ruling_party = democratic }
set_popularities = { democratic = 80 communism = 10 fascism = 10 }  # Must sum to 100

set_autonomy = { target = TAG autonomous_state = autonomy_dominion }
create_faction = faction_loc_key
add_to_faction = TAG
declare_war_on = { target = TAG type = annex_everything }

country_event = { id = event.0 days = 7 }
country_event = { id = event.0 days = 0 }       # Immediately

recruit_character = TAG_character
add_country_leader_role = { ... }
add_advisor_role = { ... }

load_focus_tree = { tree = my_tree keep_completed = yes }
unlock_national_focus = TAG_focus               # Bypass without effects
complete_national_focus = TAG_focus             # Full completion

set_country_flag = my_flag
```

### State Effects

```
add_state_core = TAG
remove_state_core = TAG
transfer_state = TAG
set_state_owner = TAG
set_state_controller = TAG
add_claim_by = TAG
add_building_construction = { type = arms_factory level = 1 }
set_state_population = 500000
add_resource = { type = steel amount = 20 }
set_state_category = my_category
set_state_flag = my_flag
```

### Division/Unit Effects

```
create_division = { template_name = "name" }
delete_unit = yes
division_template = { name = "name" regiments = { ... } support = { ... } }
add_equipment_production = {
    equipment = { type = infantry_equipment_0 creator = TAG }
    requested_factories = 1
    progress = 0.2
    efficiency = 100
}
create_equipment_variant = { name = "Variant" type = submarine_1 ... }
```

### Control Flow Effects

```
if = {
    limit = { has_dlc = "Man the Guns" }
    # effects
}
else = { ... }

random_list = {
    30 = { add_manpower = 1000 }
    70 = { add_manpower = 500 }
}

switch = {
    trigger = { has_government = democratic }
    democratic = { ... }
    fascism = { ... }
}

hidden_effect = { ... }
effect_tooltip = { ... }
custom_effect_tooltip = loc_key
```

### Variables and Event Targets

```
set_variable = { my_var = 100 }
multiply_variable = { my_var = 2 }
add_manpower = var:my_var

save_event_target_as = my_target
clear_event_target = my_target
```

### Scripted Effects (Reusable Macros)

In `common/scripted_effects/my_effects.txt`:
```
my_effect = {
    add_political_power = 100
    add_stability = 0.05
}
# Usage: my_effect = yes
```

### Reference Docs

For the complete effect list:
```
../hoi4/Hearts of Iron IV/documentation/effects_documentation.html
../hoi4/Hearts of Iron IV/documentation/effects_documentation.md
```

---

## Common Pitfalls

- **Trigger blocks are AND by default** — use `OR` explicitly when needed
- **`=` means exact equality**, not "equal or greater"
- **Effects are one-time** — use modifiers (ideas, spirits) for lasting effects
- **Non-dual scopes can't be targets** — use PREV as workaround
- **Scopes can't select non-existent countries** (except `every_possible_country`)
- **`has_completed_focus` returns false** inside that focus's own `completion_reward` — use flags or hidden events
- **State events have ambiguous scopes** — prefer country_event with event targets

## References

This skill covers the essentials. For the complete, unabridged reference with exhaustive details, read:

- `skills/hoi4-mod/references/Scopes.md` — Full scope reference: all types, chains, PREV/THIS/FROM/ROOT, event targets
- `skills/hoi4-mod/references/Triggers.md` — Complete trigger list with all operators and conditions
- `skills/hoi4-mod/references/Effect.md` — Complete effect list with all commands
- `skills/hoi4-mod/references/Data structures.md` — Variables, flags, arrays, and script value expressions
