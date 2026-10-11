---
name: hoi4-define
description: Modify game constants and defines in HOI4 mods. Use this skill whenever the user wants to change hardcoded values like focus timing, division size limits, research slots, political power costs, or any other global game setting.
---

# HOI4 Defines Modding

This skill covers modifying game constants through the defines system.

## Defines File Location

`<mod>/common/defines/*.txt`

**Critically: use a define OVERRIDE file, never replace the entire base game defines file.**

## Define Override (The Correct Approach)

Create a new file in your mod: `<mod>/common/defines/00_my_defines.txt`

```
NDefines = {
    NGraphics = {
        COUNTRY_COLOR_SATURATION_MODIFIER = 0.6
        COUNTRY_COLOR_BRIGHTNESS_MODIFIER = 0.8
    }
    
    NFocus = {
        FOCUS_POINT_DAYS = 7
        FOCUS_PROGRESS_PEACE = 1
        FOCUS_PROGRESS_WAR = 1
        MAX_SAVED_FOCUS_PROGRESS = 10
    }
    
    NCountry = {
        MIN_FOCUSES_FOR_CONTINUOUS = 10
        BASE_RESEARCH_SLOTS = 2
    }
    
    NMilitary = {
        MAX_DIVISION_BRIGADE_WIDTH = 5
        MAX_DIVISION_BRIGADE_HEIGHT = 5
        MAX_DIVISION_SUPPORT_WIDTH = 1
        MAX_DIVISION_SUPPORT_HEIGHT = 5
        UNIT_EXP_LEVELS = { 0.1 0.3 0.75 0.9 }
    }
}
```

### Why Override Instead of Replace

- Base game defines are in `common/defines/00_defines.lua`
- Copying the entire file breaks with every game update
- New defines added in patches won't exist in your copy
- Missing defines cause crashes

### Override Priority

Defines load in alphabetical order. Later files override earlier ones for the same key. Use a filename like `zz_my_overrides.txt` to ensure your changes take effect.

## Commonly Modified Defines

### Graphics
```
NGraphics = {
    COUNTRY_COLOR_SATURATION_MODIFIER = 0.6    # Lower = less saturated
    COUNTRY_COLOR_BRIGHTNESS_MODIFIER = 0.8    # Lower = darker
    COUNTRY_FLAG_TEX_WIDTH = 82
    COUNTRY_FLAG_TEX_HEIGHT = 52
    COUNTRY_FLAG_MEDIUM_TEX_WIDTH = 41
    COUNTRY_FLAG_MEDIUM_TEX_HEIGHT = 26
    COUNTRY_FLAG_SMALL_TEX_WIDTH = 10
    COUNTRY_FLAG_SMALL_TEX_HEIGHT = 7
}
```

### Focus
```
NFocus = {
    FOCUS_POINT_DAYS = 7             # Days per 1 cost point
    FOCUS_PROGRESS_PEACE = 1         # Progress per day at peace
    FOCUS_PROGRESS_WAR = 1           # Progress per day at war
    MAX_SAVED_FOCUS_PROGRESS = 10    # Days of progress saved on cancel
    MIN_FOCUSES_FOR_CONTINUOUS = 10  # Focuses needed before continuous
}
```

### Country
```
NCountry = {
    BASE_RESEARCH_SLOTS = 2
    BASE_FUEL_GAIN_PER_OIL = 2
    POPULATION_YEARLY_GROWTH_BASE = 0.015
    MIN_FOCUSES_FOR_CONTINUOUS = 10
}
```

### Military
```
NMilitary = {
    MAX_DIVISION_BRIGADE_WIDTH = 5
    MAX_DIVISION_BRIGADE_HEIGHT = 5
    MAX_DIVISION_SUPPORT_WIDTH = 1
    MAX_DIVISION_SUPPORT_HEIGHT = 5
    UNIT_EXP_LEVELS = { 0.1 0.3 0.75 0.9 }
}
```

### Diplomacy
```
NDiplomacy = {
    BASE_TRUCE_PERIOD = 30            # Days of truce after peace
    BASE_GUARANTEE_COST = 25
    MAX_TRUST_VALUE = 100
    MIN_TRUST_VALUE = -100
}
```

### AI
```
NAI = {
    FOCUS_TREE_CONTINUE_FACTOR = 1.5   # AI bonus for continuing a focus branch
    AI_BASE_POLITICAL_POWER = 0.25
}
```

### Production
```
NProduction = {
    BASE_FACTORY_SPEED = 4.5
    MAX_EQUIPMENT_STOCKPILE = 1000000
}
```

## Finding Defines

The complete base game defines are at:
```
../hoi4/Hearts of Iron IV/common/defines/00_defines.lua
```
This is a Lua file containing all `NDefines` constants.

Useful documentation: `../hoi4/Hearts of Iron IV/documentation/`

## Important Note About Updates

Defines can (and do) get added, removed, or renamed with each major game patch. A define override that references a removed define will cause errors. Check the base game `00_defines.lua` after updates.

## Common Pitfalls

- **Copying the entire defines file**: breaks on game updates. Always use overrides.
- **Wrong category**: defines are organized by category (`NGraphics`, `NFocus`, etc.) — wrong category means the define won't apply
- **Typo in define name**: silently ignored; double-check spelling against base game file
- **Array defines**: values like `UNIT_EXP_LEVELS` must use curly braces `{ 0.1 0.3 0.75 0.9 }`

## References

This skill covers the essentials. For the complete, unabridged reference with exhaustive details, read the following file(s) under the master skill's references directory:

- `skills/hoi4-mod/references/Defines.md` — Full reference for the defines system, categories, and commonly modified values.
