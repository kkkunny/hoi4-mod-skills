---
name: hoi4-bookmark
description: Create or modify start date bookmarks and country selection screens in HOI4 mods. Use this skill whenever the user wants to add new starting dates, set up which countries appear on the selection screen, or configure country positions on the bookmark map.
---

# HOI4 Bookmark Modding

Covers start-date bookmarks and the country selection screen.

## File Location

`common/bookmarks/*.txt`

## Minimal Bookmark

```
bookmarks = {
    bookmark = {
        name = BOOKMARK_1936                # localisation keys → [[hoi4-assets]]
        desc = BOOKMARK_1936_DESC
        date = 1936.1.1.12                  # YYYY.M.D.H
        picture = GFX_select_1936
        default = yes                       # default-selected bookmark
        
        selectable_countries = {
            GER = { position = { x = -325 y = 12 } }     # px, relative to center
            ENG = { position = { x = -280 y = -70 } }
        }
        minor_countries = { BHR = { position = { x = 260 y = -120 } } }
    }
}
```

## Key Rules

- **History date blocks interact**: a block at `1939.1.1 = { ... }` in a country history applies only when the start date is **strictly after** the timestamp — content for the bookmark date itself must be outside the block.
- Multiple bookmarks: repeat the `bookmark = { ... }` block inside one `bookmarks = { ... }`; only one gets `default = yes`. Vanilla 1936 is date `1936.1.1.12`.
- **Country selection screen**: countries with their own (non-default) focus tree appear automatically; others only if listed in `minor_countries`/`selectable_countries`. The focus tree must match the country or it won't appear.
- Country entries accept `difficulty` / `custom_difficulty` blocks.
- Portraits on the selection screen come from the country's history/gfx; positions are easiest to copy from vanilla. Full options → `Bookmark modding.md`.

## Fatal Pitfalls

- **Missing localisation** → bookmark shows no name
- **Date block timing** → content at the bookmark's exact timestamp never applies
- **Country missing from selection** → no matching non-default focus tree and not listed manually
- **Wrong picture sprite** → bookmark image missing (sprite must exist in `interface/*.gfx` → [[hoi4-gui]])

## References

Full details in the [[hoi4-mod]] skill's `references/` folder:

- `Bookmark modding.md` — all bookmark fields, positions, difficulty blocks, history interaction
- [[hoi4-focus]] — focus trees for selection; [[hoi4-country]] — history date blocks; [[hoi4-assets]] — bookmark text
