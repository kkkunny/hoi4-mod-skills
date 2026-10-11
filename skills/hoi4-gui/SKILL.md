---
name: hoi4-gui
description: Create and modify the HOI4 user interface — sprites, graphics, GUI layout files, animations, and scripted GUI elements. Use this skill whenever the user wants to add images to the game, set up focus icons, create custom UI windows, add animated sprites, modify the tech tree view, or build scripted GUIs with buttons and dynamic text.
---

# HOI4 GUI & Graphics

Covers sprites, GUI layout files, animations, and scripted GUI. Music/sound and localisation → [[hoi4-assets]].

## Sprite System

**An image in `gfx/` does nothing without a spriteType.** Sprites live in `interface/*.gfx` — create new files, never copy vanilla `.gfx` files.

```
spriteTypes = {
    spriteType = {
        name = GFX_my_sprite
        texturefile = gfx/interface/folder/filename.dds      # forward slashes
    }
    spriteType = {
        name = GFX_multi_frame
        texturefile = gfx/interface/filename.dds
        noOfFrames = 4               # splits horizontally; use sprite:0 ... sprite:3
    }
}
```

- Formats: `.dds` (preferred), `.tga`, `.png`. Flags must be uncompressed 32-bit TGA → [[hoi4-country]].
- Later-loading files (e.g. `zz_*.gfx`) override same-name sprites.
- Animated sprites: `frameAnimatedSpriteType` (`noOfFrames`, `playback_fps`, `looping`).
- **Focus icons** additionally need a shine sprite named exactly `GFX_<name>_shine`; full template + animation params → `National focus modding.md`.

## GUI Layout

`interface/*.gui` (layout) — element types: `containerWindowType`, `iconType`, `buttonType`, `instantTextboxType`, `editBoxType`, `gridboxType`, `listboxType`, `scrollbarType`, `checkboxType`, `trackbarType`.

```
containerWindowType = {
    name = "my_window"
    position = { x = 100 y = 50 }            # px from parent's top-left; % also allowed
    size = { width = 400 height = 300 }
    background = { name = "bg" quadTextureSprite = GFX_my_background }
}
```

- **Partial overrides**: define only your additions/changes — the block `name` must match the vanilla window name exactly; don't copy whole vanilla `.gui` files.
- **New tech branches require GUI edits** in `countrytechtreeview.gui` / `.gfx` → [[hoi4-tech]].
- Fonts: `bitmapfont = { name = my_font path = "gfx/fonts/my_font" sizes = { 16 } }` → `Font modding.md`.

## Scripted GUI

`common/scripted_guis/*.txt` — dynamic elements bound to game state:

```
my_scripted_gui = {
    name = "My Scripted GUI"
    parent_window_name = "country_view"      # must match exactly
    visible = { has_country_flag = my_flag } # false = no evaluations (performance)
    internal = yes                           # only visible to owner
    scripted_buttons = { my_button = { position = { x = 0 y = 0 } available = { ... } click_effect = { ... } } }
    scripted_images = { my_img = { GFX_a = { trigger } GFX_fallback = yes } }
    scripted_text = { my_txt = { text = { trigger = { ... } localization_key = k } text = { localization_key = fallback } } }
}
```

- Parent windows: `country_view`, `diplomacy_view`, `technology_view`, `focus_tree_view`, `decision_view`, `production_view`.
- Focus inlay windows: `common/focus_inlay_windows/*.txt` + `inlay_window = { id = ... }` in the tree.
- All button/image/text arguments → `Scripted GUI modding.md`.

## Fatal Pitfalls

- Image without spriteType → invisible (the most common error)
- Focus shine name not ending in `_shine` → shine never works
- Copying whole vanilla `.gui`/`.gfx` files → breaks on game updates; use partial overrides or new files
- Window name mismatch with `parent_window_name`/vanilla override name → element never appears
- 24-bit TGA flags → fail to load (must be 32-bit)

## References

Full details in the [[hoi4-mod]] skill's `references/` folder:

- `Graphical asset modding.md` — sprite system, animations, image formats
- `Interface modding.md` — complete .gui element reference
- `Scripted GUI modding.md` — all scripted GUI/button/image/text arguments
- `National focus modding.md` — focus icon + shine sprite template; `Font modding.md` — fonts; `Portrait modding.md` — portraits
