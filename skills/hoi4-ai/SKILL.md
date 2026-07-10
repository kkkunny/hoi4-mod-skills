---
name: hoi4-ai
description: Configure AI behavior in Hearts of Iron IV mods. Use this skill whenever the user wants to make the AI follow specific focus orders, prioritize decisions, build division templates, or behave historically. Covers AI strategy plans, ai_will_do blocks, AI focuses, AI templates, and AI personalities.
---

# HOI4 AI Modding

This skill covers configuring AI behavior: strategy plans, focus selection, AI will do blocks, and AI templates.

## AI Strategy Plans

Location: `<mod>/common/ai_strategy_plans/*.txt`

The primary way to control AI behavior per country:

```
my_ai_plan = {
    name = "Plan Name"               # Internal name (for aiview command)
    desc = "Plan description"        # Internal description
    
    allowed = {                      # Checked ONCE at game start
        tag = BHR
    }
    
    enable = {                       # Checked daily; enables the plan
        is_historical_focus_on = yes
        has_game_rule = {
            rule = BHR_ai_behavior
            option = DEFAULT
        }
    }
    
    abort = {                        # Forces plan to stop if true
        has_war = yes
    }
    
    ai_national_focuses = {          # Focuses in exact priority order
        BHR_industrial_1
        BHR_industrial_2
        BHR_political_1
    }
    
    focus_factors = {                # Multiplier on ai_will_do values
        BHR_bad_focus = 0            # 0 = never pick this
        BHR_good_focus = 3           # 3x more likely
    }
    
    weight = {                       # Overall plan weight multiplier
        base = 1
        modifier = {
            factor = 2
            date > 1939.1.1
        }
    }
}
```

### Key Points
- Multiple AI strategy plans can be active simultaneously
- `ai_national_focuses` takes priority over `ai_will_do` when both are set
- `focus_factors` with value 0 prevents AI from ever picking that focus (unless in `ai_national_focuses`)
- When a plan's ordered focuses are all impossible, AI falls back to `ai_will_do`

### Custom Game Rules Integration

```
has_game_rule = {
    rule = BHR_ai_behavior
    option = HISTORICAL
}
```

Game rules are defined in `<mod>/common/game_rules/*.txt`.

## AI Will Do (MTTH Blocks)

Used in focuses, decisions, events, and other AI selection contexts:

```
ai_will_do = {
    base = 1
    modifier = {
        factor = 2
        has_war = yes
    }
    modifier = {
        add = 5
        has_completed_focus = TAG_prerequisite_focus
    }
}
```

- `base`: starting value (default 1)
- `factor`: multiply current value
- `add`: add to current value
- Modifiers apply in order
- Variables are supported inside modifiers

## AI Focuses

Location: `<mod>/common/ai_focuses/*.txt`

Defines overall AI focus priorities:
```
AI_focus = {
    id = ai_focus_war_production
    name = "War Production"
    ai_strategies = {
        war_production_focus = {
            enable = { has_war = yes }
            abort = { has_war = no }
            weight = 10
        }
    }
}
```

Continuous focuses can match AI strategies with `supports_ai_strategy = ai_focus_id`.

## AI Templates

Location: `<mod>/common/ai_templates/*.txt`

Define what division templates the AI should build:
```
my_template = {
    weight = 100
    target_width = 20
    
    template = {
        regiments = { ... }
        support = { ... }
    }
    
    allowed = {
        has_technology = infantry_weapons_1
    }
}
```

## AI Areas

Location: `<mod>/common/ai_areas/*.txt`

Define geographical areas for AI strategic planning.

## AI Personalities

Location: `<mod>/common/ai_personalities.txt`

Define overall AI personality types:
```
my_personality = {
    modifier = {
        production_factory_efficiency_gain_factor = 0.1
    }
}
```

## Common Pitfalls

- **allowed/abort**: `allowed` checks only once at start; `abort` checks continuously
- **Focus order stops at first impossible**: if focus #2 is taken, AI won't skip to #3 if #2 is impossible
- **AI strategy plans stack**: multiple plans with different `focus_factors` multiply together
- **enable triggers daily**: avoid expensive computations in enable triggers

## References

This skill covers the essentials. For the complete, unabridged reference with exhaustive details, read the following file(s) under the master skill's references directory:

- `skills/hoi4-mod/references/AI focuses.md` — AI strategy plans, focus factors, and AI behavior configuration.
