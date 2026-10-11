---
name: hoi4-define
description: Modify game constants and defines in HOI4 mods. Use this skill whenever the user wants to change hardcoded values like focus timing, division size limits, research slots, political power costs, or any other global game setting.
---

# HOI4 Defines

Covers changing game constants via define override files.

## File Location

`common/defines/*.txt` — **always use an override file, never copy the full base game defines.**

## Minimal Override

```
NDefines = {
    NFocus = {
        FOCUS_POINT_DAYS = 7
    }
    NMilitary = {
        UNIT_EXP_LEVELS = { 0.1 0.3 0.75 0.9 }      # array defines need curly braces
    }
}
```

## Key Rules

- The base game file is `common/defines/00_defines.lua` — copying it breaks with every patch (missing new defines → crashes). Override only the keys you change.
- **Category must be right**: defines are grouped (`NGraphics`, `NFocus`, `NCountry`, `NMilitary`, `NDiplomacy`, `NAI`, `NProduction`, ...). A key under the wrong category won't apply.
- Load order: alphabetical by filename; later files override earlier ones for the same key (use e.g. `zz_my_defines.txt` to be safe).
- **Typo in a define name is silently ignored** — verify spelling against `00_defines.lua`.
- Defines are added/removed/renamed by major patches — re-check after updates.
- Search base game defines: `../hoi4/Hearts of Iron IV/common/defines/00_defines.lua`; commonly changed values → `Defines.md`.

## Fatal Pitfalls

- **Copying the whole defines file** → breaks on game updates
- **Wrong category** → override silently ignored
- **Wrong array syntax** → define not applied

## References

Full details in the [[hoi4-mod]] skill's `references/` folder:

- `Defines.md` — categories, commonly modified defines, override mechanics
- [[hoi4-trouble]] — when a change doesn't seem to apply
