---
name: hoi4-gui
description: Create and modify the Hearts of Iron IV user interface — sprites, .gfx files, .gui layout files, animations, and scripted GUI elements.
---

# HOI4 GUI & Graphics Modding

This skill covers the complete UI domain: sprite definitions, GUI layout files, animations, and scripted GUI elements.

---

## Part 1: The Sprite System

**Placing an image in `gfx/` is not enough.** Every image needs a `spriteType` definition in `interface/*.gfx`.

### SpriteType Definition

Location: `<mod>/interface/*.gfx` (create new files, don't overwrite vanilla)

```
spriteTypes = {
    spriteType = {
        name = GFX_my_sprite
        texturefile = gfx/interface/folder/filename.dds
    }
    
    spriteType = {
        name = GFX_multi_frame
        texturefile = gfx/interface/folder/filename2.dds
        noOfFrames = 4              # Splits horizontally into 4 frames
    }
}
```

- `name` often needs `GFX_` prefix
- `texturefile` uses forward slashes `/`
- Formats: `.dds` (recommended), `.tga`, `.png`
- `noOfFrames = N`: splits horizontally; `GFX_sprite:0` = first frame
- Later-loaded files (alphabetically later, e.g. `zz_*.gfx`) overwrite earlier sprites with the same name
- **Never copy entire vanilla .gfx files** — use new files with late-loading names

### Animated Sprites

```
frameAnimatedSpriteType = {
    name = GFX_my_animation
    texturefile = gfx/interface/my_animation.dds
    noOfFrames = 8
    playback_fps = 24
    looping = yes
}
```

### Focus Icon Shine Animation

The most common animated sprite. Name **must** be `GFX_name_shine`:
```
spriteType = {
    name = GFX_focus_icon_name_shine
    texturefile = gfx/interface/goals/focus_icon.dds
    effectFile = gfx/FX/buttonstate.lua
    animation = {
        animationmaskfile = gfx/interface/goals/focus_icon.dds
        animationtexturefile = gfx/interface/goals/shine_overlay.dds
        animationrotation = -90.0
        animationlooping = no
        animationtime = 0.75
        animationdelay = 0
        animationblendmode = "add"
        animationtype = "scrolling"
        animationrotationoffset = { x = 0.0 y = 0.0 }
        animationtexturescale = { x = 1.0 y = 1.0 }
    }
    animation = {                     # Second shine wave (90°)
        animationmaskfile = gfx/interface/goals/focus_icon.dds
        animationtexturefile = gfx/interface/goals/shine_overlay.dds
        animationrotation = 90.0
        animationlooping = no
        animationtime = 0.75
        animationblendmode = "add"
        animationtype = "scrolling"
        animationrotationoffset = { x = 0.0 y = 0.0 }
        animationtexturescale = { x = 1.0 y = 1.0 }
    }
    legacy_lazy_load = no
}
```

| Animation Param | Description |
|---|---|
| `animationmaskfile` | Shows where it appears |
| `animationtexturefile` | Overlay that animates |
| `animationrotation` | Degrees (-90 CW, 90 CCW) |
| `animationblendmode` | `add`, `multiply`, `overlay` |
| `animationtype` | `scrolling`, `rotating`, `pulsing` |

### Image Formats & Use Cases

| Format | Use |
|---|---|
| `.dds` | Primary: UI, icons, portraits |
| `.tga` | **Required for flags** (uncompressed, 32-bit ARGB) |
| `.png` | Supported for sprites |

Flags: `/gfx/flags/` — `TAG.tga` (82x52), `TAG_democratic.tga` (ideology-specific). Also medium (41x26) and small (10x7).

---

## Part 2: GUI Layout Files

Location: `<mod>/interface/*.gui` and `<mod>/interface/*.gfx`

### Common GUI Elements

| Type | Purpose |
|---|---|
| `containerWindowType` | Container for other elements |
| `iconType` | Static image |
| `buttonType` | Clickable button |
| `instantTextboxType` | Text display |
| `editBoxType` | Text input |
| `gridboxType` | Grid (tech tree, focus tree) |
| `listboxType` | Scrollable list |
| `scrollbarType` | Scrollbar |
| `checkboxType` | Toggle |
| `trackbarType` | Slider |

### Basic Window

```
containerWindowType = {
    name = "my_window"
    position = { x = 100 y = 50 }
    size = { width = 400 height = 300 }
    background = {
        name = "bg"
        quadTextureSprite = GFX_my_background
    }
}
```

### Position and Size

```
position = { x = 100 y = 50 }          # Pixels from parent's top-left
size = { width = 400 height = 300 }    # Pixels
size = { width = 50% height = 100% }   # Percentage of parent
```

### Buttons

```
buttonType = {
    name = "my_button"
    position = { x = 10 y = 10 }
    size = { width = 100 height = 30 }
    quadTextureSprite = GFX_my_button
    tooltip = "my_button_tt"
    buttonFont = "hoi_16mbs"
    buttonText = "my_button_text"
    shortcut = "b"
}
```

### Text

```
instantTextboxType = {
    name = "text"
    font = "hoi_16mbs"
    text = "Default Text"
    format = center              # left, right, center
}
```

### Grid Box

```
gridboxType = {
    name = "my_grid"
    slotsize = { width = 100 height = 100 }
    format = UPPER_LEFT
}
```

### Fonts

```
bitmapfont = {
    name = "my_font"
    path = "gfx/fonts/my_font"
    sizes = { 12 16 24 }
    default_size = 16
}
```
Usage: `font = "my_font_16"`

### Partial Overrides

Define only changed parts — don't copy entire vanilla `.gui` files:
```
containerWindowType = {
    name = "country_view"          # Must match vanilla name exactly
    iconType = {
        name = "my_new_element"
        spriteType = GFX_new_sprite
    }
}
```

### Tech Tree GUI (Mandatory for New Branches)

New technology branches not connected via `path` to existing techs **require** GUI edits in `countrytechtreeview.gui` and `countrytechtreeview.gfx`:
1. gridboxType for the new branch (position, slot size, orientation)
2. Item definitions (icon positions, background boxes, sub-tech placements)
3. New tab button for new folders

---

## Part 3: Scripted GUI

Location: `<mod>/common/scripted_guis/*.txt`

Dynamic interface elements that respond to game state:

```
my_scripted_gui = {
    name = "My Scripted GUI"
    parent_window_name = "my_parent_window"
    
    visible = { has_country_flag = my_gui_flag }
    internal = yes                           # Only visible to owning country
    
    position = { x = 0 y = 0 }
    size = { width = 300 height = 200 }
    movable = yes
    
    scripted_buttons = {
        my_button = {
            name = my_button_loc
            position = { x = 10 y = 10 }
            quadTextureSprite = GFX_my_button
            available = { has_political_power > 100 }
            click_effect = { add_political_power = -100 }
            tooltip = my_button_tt
        }
    }
    
    scripted_images = {
        my_image = {
            position = { x = 20 y = 50 }
            GFX_image_a = { trigger_condition_a }
            GFX_image_b = { trigger_condition_b }
            GFX_fallback = yes              # Always used as fallback
        }
    }
    
    scripted_text = {
        my_text = {
            font = "hoi_16mbs"
            text = {
                trigger = { has_war = yes }
                localization_key = my_war_text
            }
            text = {
                localization_key = default_text
            }
        }
    }
}
```

### Focus Inlay Windows

Defined in `<mod>/common/focus_inlay_windows/*.txt`:
```
inlay_window_id = {
    window_name = my_scripted_gui
    internal = yes
    visible = { has_completed_focus = TAG_unlock }
    
    scripted_buttons = { ... }
    scripted_images = { ... }
}
```

Declared in focus tree:
```
inlay_window = {
    id = inlay_window_id
    position = { x = 4500 y = 1150 }
}
```

### Common Parent Windows

`country_view`, `diplomacy_view`, `technology_view`, `focus_tree_view`, `decision_view`, `production_view`

### Scripted GUI Tips

- `visible = { ... }`: when false, no evaluations performed (use for performance)
- `internal = yes`: only visible to owning country
- Images: first matching trigger wins; `yes` fallback always matches
- Text: multiple `text = { trigger = ... localization_key = ... }` blocks, last one without trigger is fallback

---

## Common Pitfalls

- **Missing sprite definition**: image file exists but no spriteType — most common error
- **Focus shine name**: MUST end with `_shine` — second most common error
- **Copying vanilla .gui/.gfx files**: breaks on updates; use new files or partial overrides
- **Wrong file extension**: `.gui` = layout, `.gfx` = sprites/fonts
- **Window name mismatch**: `parent_window_name` / override name must match exactly
- **Backslashes in paths**: use forward slashes only
- **TGA origin point**: top-left origin flips image; use GIMP/Photoshop
- **24-bit TGA flags**: load with errors; must be 32-bit ARGB

## References

For complete, unabridged references, read:

- `skills/hoi4-mod/references/Graphical asset modding.md` — Full sprite system, .gfx files, animations, image formats
- `skills/hoi4-mod/references/Interface modding.md` — Complete .gui reference: all element types, containers, positioning
- `skills/hoi4-mod/references/Scripted GUI modding.md` — Full scripted GUI reference with all dynamic elements
- `skills/hoi4-mod/references/Portrait modding.md` — Character portrait definitions and leader graphics
- `skills/hoi4-mod/references/Font modding.md` — Font system and bitmap font definitions
