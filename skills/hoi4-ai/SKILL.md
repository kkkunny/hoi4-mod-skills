---
name: hoi4-ai
description: Configure AI behavior in Hearts of Iron IV mods. Use this skill whenever the user wants to make the AI follow specific focus orders, prioritize decisions, build division templates, or behave historically. Covers AI strategy plans, ai_will_do blocks, AI focuses, AI templates, and AI personalities.
---

# HOI4 AI Modding

Covers AI strategy plans (focus orders), `ai_will_do` weighting, AI focuses/templates/personalities.

## Minimal AI Strategy Plan

`common/ai_strategy_plans/*.txt` — the primary per-country AI control:

```
SCO_historical = {
    name = "Historical plan for SCO"        # internal name, shown by the aiview console command
    allowed = { tag = SCO }                 # checked ONCE at game start
    enable = { is_historical_focus_on = yes }   # checked daily
    abort = { has_war_with = ENG }          # forces plan to stop
    ai_national_focuses = {                 # exact priority order
        SCO_industry_1
        SCO_politics_1
    }
    focus_factors = {                       # ai_will_do multipliers
        SCO_bad_focus = 0                   # 0 = never pick (unless in ai_national_focuses)
        SCO_good_focus = 3
    }
}
```

## Key Rules

- Multiple plans can be active; `focus_factors` from all active plans multiply together.
- `allowed` = once at start; `enable` = daily; `abort` = continuous. Keep `enable` cheap.
- Ordered focuses stop at the first impossible one; when the list is exhausted the AI falls back to `ai_will_do`.
- **Game rules**: `has_game_rule = { rule = SCO_ai_behavior option = HISTORICAL }` in `enable`; rules defined in `common/game_rules/*.txt`. Random paths: set a flag in `on_startup` → [[hoi4-onaction]].
- **`ai_will_do`** (MTTH block, used in focuses/decisions/events): `base` (default 1), `modifier = { factor = N ... }` (multiply), `add = N`; applied in order; variables allowed.
- Other hook-ups:
  - `common/ai_focuses/*.txt` — AI focus "strategies"; continuous focuses match them via `supports_ai_strategy = <ai_focus_id>`
  - `common/ai_templates/*.txt` — division templates the AI builds (`weight`, `target_width`, `template`, `allowed`)
  - `common/ai_areas/*.txt` — geographic AI planning areas
  - `common/ai_personalities.txt` — global AI personality modifiers
  - `common/ai_strategy/*.txt` — low-level AI strategies
- Full argument lists → `AI focuses.md`.

## Fatal Pitfalls

- **`allowed` used for dynamic conditions** → only evaluated at game start
- **Focus order expectation**: if focus #2 is currently impossible, the AI does not skip ahead — it stalls/falls back
- **Expensive `enable` triggers** → checked daily for every country with the plan

## References

Full details in the [[hoi4-mod]] skill's `references/` folder:

- `AI focuses.md` — strategy plans, focus factors, AI focuses/templates/areas/personalities
- [[hoi4-focus]] — focus lines and `ai_will_do` on focuses; [[hoi4-onaction]] — startup flags
