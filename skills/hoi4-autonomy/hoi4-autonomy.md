---
name: hoi4-autonomy
description: Create or modify autonomy states (puppet levels) in HOI4. Covers autonomy definitions, puppet types, autonomy effects, and autonomy-dependent country naming.
---

# HOI4 Autonomy State Modding

This skill covers creating and modifying autonomy states (puppet levels) in Hearts of Iron IV.

## Autonomy File Location

`<mod>/common/autonomous_states/*.txt`

## Autonomy State Definition

```
autonomy_puppet = {
    name = AUTONOMY_PUPPET
    puppeted_name = AUTONOMY_PUPPET_PUPPETED
    
    level = 0.5                          # 0 = free, 1 = fully integrated
    
    # Icon
    icon = GFX_autonomy_puppet
    
    # Default modifier applied to puppet
    default_modifier = {
        civilian_factory_use = 30
        military_factory_use = 50
        extra_trade_to_overlord_factor = 1.0
    }
    
    # Can overlord build in puppet territory
    can_master_build = yes
    can_trade_with_master = yes
    can_trade_with_others = yes
    
    # AI behavior
    ai_submit_wargoal_chance = 0.2
    ai_call_to_war_chance = 0.8
    
    # Overlord actions
    overlord_actions = {
        # What the overlord can do to this puppet
    }
    
    # Subject actions
    subject_actions = {
        # What the subject can do
    }
}
```

## Common Autonomy States (Base Game)

| State ID | Name | Level |
|---|---|---|
| `autonomy_free` | Free | 0.0 |
| `autonomy_dominion` | Dominion | 0.3 |
| `autonomy_colony` | Colony | 0.5 |
| `autonomy_puppet` | Puppet | 0.7 |
| `autonomy_integrated_puppet` | Integrated Puppet | 0.9 |
| `autonomy_annex` | Annexed | 1.0 |

## Assigning Autonomy in History

```
ENG = {
    if = {
        limit = { has_dlc = "Together for Victory" }
        set_autonomy = {
            target = SCO
            autonomous_state = autonomy_integrated_puppet
        }
        else = {
            puppet = SCO
        }
    }
}
```
- Put in the **subject's** history file (not overlord's) to avoid politics reset
- Place **before** `set_popularities` and `set_politics`

## Autonomy in Country Names

Localisation priority for country names includes autonomy states:
```
SCO_autonomy_dominion: "Dominion of Scotland"
SCO_IRE_autonomy_integrated_puppet: "Most Northern Ireland"
```

Format: `TAG_autonomy_state` or `TAG_OVERLORD_autonomy_state`

Generic: `COUNTRY_TAG_autonomy_state`

## Autonomy Effects

```
# In history / effects
set_autonomy = {
    target = SCO
    autonomous_state = autonomy_puppet
}

# Change autonomy level
add_autonomy = 100
add_autonomy_ratio = 0.1
```

## Autonomy-Related Triggers

```
is_subject = yes
is_subject_of = ENG
autonomy_level > 0.5
autonomy_ratio > 0.3
has_autonomy_state = autonomy_puppet
```

## Autonomy Icons

Defined in `<mod>/interface/*.gfx`:
```
spriteType = {
    name = GFX_autonomy_puppet
    texturefile = gfx/interface/autonomy/puppet_icon.dds
}
```

## Common Pitfalls

- **Puppeting placement in history**: putting it in the overlord's file can reset the subject's politics
- **DLC check**: non-TFV games don't have autonomy states — use `puppet = TAG` as fallback
- **Autonomy state must exist**: referencing undefined autonomy state causes errors

## References

This skill covers the essentials. For the complete, unabridged reference with exhaustive details, read the following file(s) under the master skill's references directory:

- `skills/hoi4-mod/references/Autonomy state modding.md` — Full reference for autonomy state definitions, effects, and country naming.
