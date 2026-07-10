---
name: hoi4-ideology
description: Create or modify ideologies and sub-ideologies in HOI4. Covers ideology groups, ideology types, party names, colors, and government assignment.
---

# HOI4 Ideology Modding

This skill covers creating and modifying ideology groups and types.

## Ideology File Location

`<mod>/common/ideologies/*.txt`

## Ideology Groups (Top-Level Categories)

The four default groups: `democratic`, `fascism`, `communism`, `neutrality`.

### Sub-Ideologies

```
ideas = {
    communism = {
        stalinism = {
            color = { 255 0 0 }
            can_declare_war_on_same_ideology = yes
            puppet_cost_factor = -0.3
            can_boost_other_ideologies = yes
            min_surrender_limit = -0.1
            
            rules = {
                faction_invite_same_ideology = yes
                join_faction_same_ideology = yes
            }
        }
        
        leninism = {
            color = { 200 50 50 }
            # modifiers
            rules = { ... }
        }
    }
    
    democratic = {
        liberalism = {
            color = { 50 50 255 }
            can_send_volunteers = no
        }
    }
}
```

## Sub-Ideology Arguments

| Argument | Type | Description |
|---|---|---|
| `color` | RGB | UI color for the ideology |
| `can_declare_war_on_same_ideology` | bool | Can declare war on same ideology |
| `can_boost_other_ideologies` | bool | Can use ideology boosting |
| `can_send_volunteers` | bool | Can send volunteers |
| `can_puppet` | bool | Allowed to puppet countries |
| `can_join_factions` | bool | Can join/create factions |
| `can_host_exiles` | bool | Can host governments-in-exile |
| `can_use_kamikaze` | bool | Can use kamikaze strikes |
| `puppet_cost_factor` | float | Puppet cost multiplier |
| `min_surrender_limit` | float | Surrender progress modifier |
| `ai_faction_member_desire` | float | AI willingness to join factions |
| `ai_volunteer_desire` | float | AI willingness to send volunteers |

### Rules Sub-Block

```
rules = {
    faction_invite_same_ideology = yes
    join_faction_same_ideology = yes
    puppet_same_ideology = yes
    guarantee_same_ideology = yes
    lend_lease_same_ideology = yes
    military_access_same_ideology = yes
}
```

## Dynamic Ideologies

```
dynamic_ideology = yes
```

Allows dynamic ideology types defined at runtime (used by some mods for custom governments).

## Character Leader Ideology

In character definitions, the `ideology` field must reference an ideology **type** (sub-ideology), not a group:

```
country_leader = {
    ideology = stalinism        # Correct — a type under 'communism' group
    # NOT: ideology = communism  # Wrong — this is a group, not a type
}
```

## Changing Ideology via Effect

```
set_politics = {
    ruling_party = communism
}
set_popularities = {
    democratic = 10
    communism = 80
    fascism = 10
}
```

Party popularities must add up to exactly 100.

## Localisation

```
l_english:
 communism: "Communism"
 communism_desc: "Description"
 stalinism: "Stalinism"
 stalinism_desc: "Stalinism description"
 
 SCO_communism_party: "SCP"
 SCO_communism_party_long: "Scottish Communist Party"
```

## Common Pitfalls

- **Sub-ideology vs group**: `set_politics.ruling_party` takes an ideology group; `country_leader.ideology` takes a sub-ideology type
- **Popularities must sum to 100**: otherwise they fail to set
- **Missing sub-ideology**: characters with an undefined ideology type won't function properly

## References

This skill covers the essentials. For the complete, unabridged reference with exhaustive details, read the following file(s) under the master skill's references directory:

- `skills/hoi4-mod/references/Ideology modding.md` — Full reference for ideology groups, types, party names, colors, and government assignment.
