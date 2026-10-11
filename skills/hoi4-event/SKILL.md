---
name: hoi4-event
description: Create and fire events in HOI4 mods. Use this skill whenever the user wants to add country events, news popups, hidden events, event chains, or trigger events from focuses, decisions, or on_actions. Covers MTTH, event options, pictures, and AI chance.
---

# HOI4 Event Modding

Covers event files, event firing, options, pictures, and chains.

## File Location

`events/*.txt` (filename irrelevant). Base game files are in the game install under `events/`.

## Minimal Event

```
add_namespace = sco_events            # declare BEFORE any event using it

country_event = {
    id = sco_events.0                 # part after the namespace must be an integer
    title = sco_events.0.t
    desc = sco_events.0.desc
    picture = GFX_sco_event_picture
    
    is_triggered_only = yes           # fired only via effect, no MTTH
    
    option = {
        name = sco_events.0.a
        add_political_power = 50
        ai_chance = { base = 10 modifier = { factor = 5 has_war = yes } }
    }
}
```

## Event Types

`country_event`, `news_event` (pop-up, can be disabled), `state_event`, `unit_leader_event`, `operative_leader_event`. They differ only in appearance and default scopes. **Prefer `country_event` + event targets over `state_event`** (its country/state scope is ambiguous).

## Key Rules

- **MTTH or triggered**: without `is_triggered_only = yes`, an event requires `mean_time_to_happen = { days = 30 modifier = { factor = 0.5 ... } }` and `trigger = { ... }`.
- **Hidden events**: `hidden = yes` (no pop-up, effects still run) — still needs `is_triggered_only = yes` if it should not fire on its own.
- **Firing**: `country_event = { id = ns.0 days = 7 }` (or `days = 0` / `hours = 12`) from any effect block.
- **Scope chain when fired from an effect**: the effect's `ROOT` becomes the event's `FROM`, previous `FROM` becomes `FROM.FROM`; event targets are preserved. Scope rules → [[hoi4-script]].
- **Dynamic title/desc**: multiple `title = { text = ... trigger = { ... } }` blocks — first match wins.
- **AI options**: `ai_chance` weights options proportionally.
- **Delayed events to non-existent countries** queue up and fire once the country exists.
- **Pictures** need a spriteType in `interface/*.gfx` → [[hoi4-gui]].
- **Localisation keys**: `ns.0.t`, `ns.0.desc`, `ns.0.a`, `ns.0.b` → [[hoi4-assets]].

## Fatal Pitfalls

- **Missing namespace** → event ID becomes a malformed token
- **Non-integer ID after the namespace** → several events collapse into one
- **`state_event` scope ambiguity** → wrong scope; use `country_event` with event targets

## References

Full details in the [[hoi4-mod]] skill's `references/` folder:

- `Event modding.md` — all event arguments, MTTH details, options, news toggles, event chains
- [[hoi4-script]] — effects/triggers/scopes; [[hoi4-onaction]] — firing events on game hooks
