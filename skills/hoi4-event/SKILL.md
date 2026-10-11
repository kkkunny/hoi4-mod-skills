---
name: hoi4-event
description: Create and fire events in HOI4 mods. Use this skill whenever the user wants to add country events, news popups, hidden events, event chains, or trigger events from focuses, decisions, or on_actions. Covers MTTH, event options, pictures, and AI chance.
---

# HOI4 Event Modding

This skill covers creating and firing events in Hearts of Iron IV.

## Event Types

| Type | ROOT Scope | Note |
|---|---|---|
| `country_event` | Country | Most common type |
| `news_event` | Country | Has pop-up; can be disabled |
| `state_event` | Country + State | Avoid due to scope confusion |
| `unit_leader_event` | Country + Unit Leader | Distinct appearance |
| `operative_leader_event` | Country + Operative | Distinct appearance |

**There is nearly no difference between event types** besides appearance, default scopes, and news_event's pop-up toggle.

Recommendation: use `country_event` with [event targets](Event targets) instead of `state_event` whenever possible.

## Event Creation

### Namespace

Must be declared **before** any events using it:
```
add_namespace = my_event
```
Then events use `id = my_event.0`, `id = my_event.1`, etc.

Dotted namespaces work: `add_namespace = my_event.subtopic` → `id = my_event.subtopic.1`

**The ID after the namespace must be an integer.**

### Basic Event Structure

```
add_namespace = my_event

country_event = {
    id = my_event.0
    title = my_event.0.t
    desc = my_event.0.desc
    picture = GFX_my_sprite
    
    trigger = {
        tag = GER
        has_political_power > 100
    }
    
    mean_time_to_happen = {
        days = 30
    }
    
    option = {
        name = my_event.0.a
        add_political_power = 50
    }
    option = {
        name = my_event.0.b
        add_stability = -0.05
    }
}
```

### Dynamic Title/Description

```
title = {
    text = my_event.0.t.a
    trigger = { tag = ENG }
}
title = {
    text = my_event.0.t.b
}
```
First matching trigger wins.

### Hidden Events
```
is_triggered_only = yes    # No MTTH, fired only via effect
hidden = yes               # No popup, effects still execute
```

### Event Picture

Sprite defined in `<mod>/interface/*.gfx`:
```
spriteType = {
    name = GFX_my_sprite
    texturefile = gfx/event_pictures/my_event_picture.dds
}
```
Used as: `picture = GFX_my_sprite`

### Mean Time to Happen (MTTH)

```
mean_time_to_happen = {
    days = 30
    modifier = {
        factor = 0.5
        has_war = yes
    }
}
```
- `days = 30`: base 30-day average
- `factor = 0.5`: half the time (faster)
- `factor = 2.0`: double the time (slower)

### Is Triggered Only
```
is_triggered_only = yes    # Fire only via effect, no MTTH
```

## Event Options

```
option = {
    name = my_event.0.a              # Localisation key for button text
    trigger = { has_stability > 0.3 } # Condition for this option to appear
    add_political_power = 50
    ai_chance = {
        base = 10
        modifier = {
            factor = 5
            has_war = yes
        }
    }
}
```

### AI Chance
AI picks based on: (ai_chance * factor1 * factor2 * ...) for each option. Higher = more likely.

## Firing Events

### Via Effect
```
country_event = { id = my_event.0 days = 7 }
country_event = { id = my_event.0 days = 0 }   # Fire immediately
country_event = { id = my_event.0 hours = 12 }
```

### Event Targets (Scope Passing)
When fired from an effect block:
- `ROOT` becomes `FROM` inside the event
- Previous `FROM` becomes `FROM.FROM`
- [Event targets](https://hoi4.paradoxwikis.com/Event_targets) (saved as event_target:) are preserved

### Delayed Events and Non-Existing Countries
A country that doesn't exist can still receive delayed events. The timer won't tick until the country exists — effectively a "backlog" system.

## Localisation

In `<mod>/localisation/english/*_l_english.yml` (UTF-8-BOM):
```
l_english:
 my_event.0.t: "My Event Title"
 my_event.0.desc: "This is the event description."
 my_event.0.a: "Accept this"
 my_event.0.b: "Decline this"
```

## Common Pitfalls

- **Missing namespace**: event ID becomes a malformed token and won't work
- **Scope confusion**: state_event scopes are ambiguous between country and state
- **Hidden events still need** `is_triggered_only = yes` to prevent MTTH requirements

## References

This skill covers the essentials. For the complete, unabridged reference with exhaustive details, read the following file(s) under the master skill's references directory:

- `skills/hoi4-mod/references/Event modding.md` — Full reference for event types, triggers, options, pictures, and namespace setup.
