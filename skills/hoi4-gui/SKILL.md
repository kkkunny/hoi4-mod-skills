---
name: hoi4-gui
description: Create and modify the HOI4 user interface — sprites, graphics, GUI layout files, animations, scripted GUI elements, custom panels, legends, scrollable lists, pie-chart UI, and tooltip text. Use this skill whenever the user wants to add images to the game, set up focus icons, create custom UI windows, add animated sprites, modify the tech tree view, or build scripted GUIs with buttons and dynamic text.
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
- `GFX_buildings_strip`: `icon_frame` is **1-based** (`icon_frame = N` uses frame N-1). Extending building icons needs both the DDS and `noOfFrames` in a later-sorting `zz_*.gfx`; an off-by-one shows the previous building's icon.
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

- **New UI**: put it in a new `.gui` with a uniquely named independent container, then mount it with a scripted GUI. If you must override a vanilla `.gui`, copy the whole file and keep every vanilla child node name — hide nodes with `hide = yes` instead of deleting them, or the engine logs `Could not find "X" in window "Y"`.
- `size` syntax differs by element: `containerWindowType`/`gridBoxType` use `{ width height }`; `buttonType`/`listboxType` use `{ x y }`. A mismatch logs `Malformed token: width/height` and the whole file fails to load.
- `position` is measured from `orientation`; switching to `upper_left` changes the origin, so recompute coordinates instead of keeping center-based offsets.
- **Scrollable grid/list**: the container needs a real `background`, `clipping = yes`, pixel `size`, and content taller than the viewport. Set the grid to the full content height, not `100%`; add `verticalScrollbar`, `scroll_wheel_factor`, `smooth_scrolling`, and `margin`. A grid equal to the viewport makes the wheel do nothing.
- **New tech branches require GUI edits** in `countrytechtreeview.gui` / `.gfx` → [[hoi4-tech]].
- Fonts: `bitmapfont = { name = my_font path = "gfx/fonts/my_font" sizes = { 16 } }` → `Font modding.md`.

## Scripted GUI

`common/scripted_guis/*.txt` — dynamic elements bound to game state. The `scripted_gui` wrapper and `window_name` are mandatory; `window_name` must match an **independent** `containerWindowType` (not nested) in `interface/*.gui`.

```text
scripted_gui = {
    my_gui = {
        window_name = "my_container"             # independent container in interface/*.gui
        context_type = player_context            # player_context / selected_country_context / selected_state_context
        parent_window_token = politics_tab       # or parent_window_name = state_info_window
        visible = { has_country_flag = my_flag } # false = no evaluations
        effects = { my_button_click = { add_political_power = 50 } }          # <element>_click
        triggers = { my_button_click_enabled = { has_political_power > 50 } } # <element>_click_enabled
        properties = { my_icon = { image = "[my_icon_sloc]" } }
    }
}
```

- `parent_window_token` / `parent_window_name` attaches the window; a nested vanilla container needs the `_instance` suffix. Common tokens: `politics_tab`, `selected_country_view`, `selected_state_view`, `decision_tab`, `production_tab`, `top_bar`.
- `context_type` supplies the scope: `ROOT` is always the player country; `selected_country_context` makes `THIS` the selected country, `selected_state_context` the selected state. Without it, dynamic loc/effects have no scope.
- Large scripted GUIs evaluate every tick; gate `properties` with a dirty variable so they update only when state changes → `Scripted GUI modding.md`.
- Focus inlay windows are different: `common/focus_inlay_windows/*.txt` use `scripted_buttons` / `scripted_images`, and the tree declares `inlay_window = ...`.
- Full arguments → `Scripted GUI modding.md`; inlay fields → `National focus modding.md`.

## Tooltips & Scripted Localisation

- `pdx_tooltip` takes a plain loc key. A plain `.gui` element has no country/state scope, so a scripted-loc key resolves to `[KEY]` there.
- To show dynamic text in a tooltip, put the element inside a scripted GUI container with `context_type` (see above).
- Vanilla controls (leader portraits, `pol_faction_icon`, `elections`, population/manpower) handle their own tooltip; a direct `pdx_tooltip` is ignored. Cover them with a transparent `iconType` overlay inside the scripted GUI.
- Non-interactive text: use `instantTextBoxType`, not `buttonType`; a transparent `iconType` can carry the tooltip without adding a click.

## Fatal Pitfalls

- Image without spriteType → invisible (the most common error)
- Focus shine name not ending in `_shine` → shine never works
- Copying whole vanilla `.gfx` files → breaks on updates; add new sprites instead
- Overriding a vanilla `.gui` and deleting child nodes → `Could not find "X" in window "Y"`; keep the node and `hide = yes`
- Missing `window_name`, or a nested container used as `window_name` → scripted GUI never applies
- Scripted-loc tooltip in a plain `.gui` → renders `[KEY]`; mount it through a scripted GUI context
- Wrong `size` keys in `buttonType`/`listboxType` → the whole `.gui` fails to parse
- Scrollable grid at `height = 100%` or without a background → wheel does nothing
- `icon_frame` treated as 0-based → each building shows the previous icon
- 24-bit TGA flags → fail to load (must be 32-bit)

## References

Full details in the [[hoi4-mod]] skill's `references/` folder:

- `Graphical asset modding.md` — sprite system, animations, image formats
- `Interface modding.md` — complete .gui element reference
- `Scripted GUI modding.md` — window assignment, contexts, effects/triggers, properties, dynamic lists
- `Building modding.md` — building definitions, `icon_frame`, `GFX_buildings_strip` override details
- `National focus modding.md` — focus icon + shine sprite template, focus inlay `scripted_buttons`/`scripted_images`; `Font modding.md` — fonts; `Portrait modding.md` — portraits
