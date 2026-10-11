---
name: hoi4-autonomy
description: Create or modify autonomy states and puppet levels in HOI4 mods. Use this skill whenever the user wants to add new puppet types, change what overlords or subjects can do, modify autonomy effects, or set up country names based on autonomy levels.
---

# HOI4 Autonomy State Modding

Covers autonomy states (puppet levels), their effects, and subjects in history.

## File Location

`common/autonomous_states/*.txt`

## Minimal Autonomy State

```
autonomy_puppet = {
    level = 0.7                          # 0 = free, 1 = fully integrated
    icon = GFX_autonomy_puppet           # sprite in interface/*.gfx
    default_modifier = {
        civilian_factory_use = 30
        military_factory_use = 50
        extra_trade_to_overlord_factor = 1.0
    }
    can_master_build = yes
    can_trade_with_master = yes
    ai_call_to_war_chance = 0.8
    overlord_actions = { ... }           # actions the overlord may take
    subject_actions = { ... }            # actions the subject may take
}
```

Base game levels: `autonomy_free` (0.0), `autonomy_dominion` (0.3), `autonomy_colony` (0.5), `autonomy_puppet` (0.7), `autonomy_integrated_puppet` (0.9), `autonomy_annex` (1.0).

## Assigning in History

Put this in the **subject's** history file, **before** `set_popularities`/`set_politics` (putting it in the overlord's file can reset the subject's politics):

```
ENG = {
    if = {
        limit = { has_dlc = "Together for Victory" }
        set_autonomy = { target = SCO autonomous_state = autonomy_integrated_puppet }
        else = { puppet = SCO }          # non-TFV fallback
    }
}
```

- Adjust at runtime: `add_autonomy = 100`, `add_autonomy_ratio = 0.1`.
- Triggers: `is_subject`, `is_subject_of`, `autonomy_level`, `autonomy_ratio`, `has_autonomy_state`.
- Country names per autonomy: `TAG_autonomy_state` / `TAG_OVERLORD_autonomy_state` → priority chain in `Localisation.md`.
- Overlord/subject actions, full modifier list → `Autonomy state modding.md`.

## Fatal Pitfalls

- **Autonomy assigned in the overlord's history** → subject's politics can reset
- **Referencing an undefined autonomy state** → errors
- **No DLC fallback** → without Together for Victory, autonomy states don't exist; use `puppet = TAG`

## References

Full details in the [[hoi4-mod]] skill's `references/` folder:

- `Autonomy state modding.md` — all autonomy arguments, overlord/subject actions, effects, naming
- [[hoi4-country]] — history files; [[hoi4-assets]] — autonomy name localisation
