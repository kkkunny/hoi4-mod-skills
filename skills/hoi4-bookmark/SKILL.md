---
name: hoi4-bookmark
description: Create or modify start date bookmarks and country selection screens in HOI4 mods. Use this skill whenever the user wants to add new starting dates, set up which countries appear on the selection screen, or configure country positions on the bookmark map.
---

# HOI4 Bookmark Modding

This skill covers creating and modifying start date bookmarks and the country selection screen.

## Bookmark File Location

`<mod>/common/bookmarks/*.txt`

## Bookmark Definition

```
bookmarks = {
    bookmark = {
        name = BOOKMARK_1936
        desc = BOOKMARK_1936_DESC
        date = 1936.1.1.12
        picture = GFX_select_1936
        
        default = yes                              # Default bookmark
        
        selectable_countries = {
            # Major countries shown in selection screen
            GER = { position = { x = -325 y = 12 } }
            ENG = { position = { x = -280 y = -70 } }
            SOV = { position = { x = 200 y = 60 } }
            USA = { position = { x = -200 y = 150 } }
            FRA = { position = { x = -260 y = 30 } }
            ITA = { position = { x = -230 y = 105 } }
            JAP = { position = { x = 350 y = 40 } }
        }
        
        minor_countries = {
            # Minor countries shown in smaller section
            BHR = { position = { x = 260 y = -120 } }
        }
    }
}
```

### Key Arguments
- `name`: Localisation key for the bookmark title
- `desc`: Localisation key for the description
- `date`: Start date. Format: `YYYY.M.D.H` (year.month.day.hour)
- `default = yes`: This is the default selected bookmark
- `picture`: Sprite for the bookmark's background/header image

## Country Positions

The `position = { x y }` values determine where country flags appear on the bookmark screen. These are pixel coordinates relative to the bookmark's center.

Country entries can have additional settings:
```
GER = {
    position = { x = -325 y = 12 }
    difficulty = { ... }          # AI difficulty modifiers
    custom_difficulty = { ... }
}
```

## History File Date Setup

The bookmark date interacts with country history date blocks:

```
# In history/countries/TAG - name.txt
# This applies to ALL start dates:
capital = 121
set_stability = 0.7

# This applies only from 1939.1.1 onwards (strictly after):
1939.1.1 = {
    oob = "TAG_1939"
    set_technology = { nuclear_reactor = 1 }
}
```

A date block applies when the start date is **strictly later** than the timestamp.

## Multiple Scenarios

For each bookmark, create history accordingly:
```
bookmarks = {
    bookmark = {
        name = BOOKMARK_1936
        date = 1936.1.1.12
        selectable_countries = { ... }
        default = yes
    }
    
    bookmark = {
        name = BOOKMARK_1939
        date = 1939.9.1.12
        desc = BOOKMARK_1939_DESC
        selectable_countries = { ... }
    }
}
```

## Country Selection Screen

The interesting countries shown when selecting a bookmark:

### Default Focus Tree
Countries with their own focus tree (non-default) appear automatically. Countries using the generic tree appear in the "minor countries" section.

### Setting Up Country to Appear
1. Create a non-default focus tree with `country = { ... }` that matches the country
2. Or manually add to `minor_countries` or `selectable_countries` in the bookmark

## Localisation

```
l_english:
 BOOKMARK_1936: "The Great Depression"
 BOOKMARK_1936_DESC: "Europe is still recovering from the Great War..."
 BOOKMARK_1939: "The Second World War"
 BOOKMARK_1939_DESC: "Germany has invaded Poland..."
```

## Common Pitfalls

- **Missing localisation**: bookmark won't have a name
- **date block timing**: content at `1939.1.1` won't apply when bookmark date IS `1939.1.1` (must be strictly after)
- **Country positions**: hard to get right — use existing positions as reference
- **Focus tree not matching country**: country won't appear in country selection

## References

This skill covers the essentials. For the complete, unabridged reference with exhaustive details, read the following file(s) under the master skill's references directory:

- `skills/hoi4-mod/references/Bookmark modding.md` — Full reference for bookmark definitions, country positions, and start date setup.
