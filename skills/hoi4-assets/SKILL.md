---
name: hoi4-assets
description: Add or modify game text, localisation, and audio in HOI4 mods. Use this skill whenever the user wants to add names, descriptions, tooltips, or translations for countries, focuses, events, ideas, decisions, or any other game content. Also covers music and sound files.
---

# HOI4 Localisation, Graphics & Audio

Covers game text (localisation), the `gfx/` layout, music, and sound. Sprites/GUI details → [[hoi4-gui]].

## Localisation

`localisation/<language>/*_l_english.yml` — filename **must** contain the language code. **UTF-8 with BOM**, first line `l_english:`, one entry per line:

```
l_english:
 my_key: "Text value"
 my_key_desc: "Description"
```

- Language codes: `english`, `french`, `german`, `spanish`, `braz_por`, `polish`, `russian`, `japanese`, `simp_chinese`, `korean`.
- Keys are ASCII only (letters, digits, `_ . -`); values on a single line in straight quotes; use `\n` for newlines, `\"` for quotes.
- **Override vanilla text without copying files**: put replacement keys in `localisation/<language>/replace/*.yml` — the `replace/` folder takes priority.
- Common key patterns: `TAG` / `TAG_DEF` / `TAG_ADJ` (countries), `idea_id` / `idea_id_desc`, `focus_id` / `focus_id_desc`, `decision_id` / `decision_id_desc`, `event_id.t` / `.desc` / `.a`, `STATE_n`, `VICTORY_POINTS_n`, `character_id[_desc]`. Full table → `Localisation.md`.
- Formatting: color codes `§R...§!`, dynamic commands `[Root.GetName]` → full list in `Localisation.md`.
- Country name priority chain (autonomy > subject > ideology > TAG) → `Country creation.md`.
- Duplicate keys are logged in `logs/text.log`; which value wins is unpredictable — avoid duplicates.

## Scripted Localisation

`common/scripted_localisation/*.txt` — text that changes with game state, used as `[ROOT.my_text]`:

```
defined_text = {
    name = my_text
    text = { trigger = { has_government = democratic } localization_key = loc_demo }
    text = { localization_key = loc_default }     # no trigger = fallback
}
```

## Graphics & Audio

- `gfx/` mirrors the game's structure: `flags/`, `interface/` (icons), `leaders/`, `event_pictures/`, `fonts/`. **Every image needs a spriteType** in `interface/*.gfx` → [[hoi4-gui]].
- `music/` — music tracks and playlists → `Music modding.md`.
- `sound/` — sound effects (`.ogg`), registered in `.asset` files → `Sound modding.md`.
- 3D world models: `Entity modding.md`; visual effects: `Particle modding.md`, `Posteffect modding.md`.

## Fatal Pitfalls

- **Wrong encoding** — the #1 cause of broken localisation (must be UTF-8 **with** BOM)
- **Missing `l_english:` first line** → all entries land in no database
- **Filename without the language code** → file not loaded at all
- **Special characters/spaces in keys, missing closing quote, raw newlines** → rest of the file breaks

## References

Full details in the [[hoi4-mod]] skill's `references/` folder:

- `Localisation.md` — full YAML format, colors, commands, languages
- `Music modding.md`, `Sound modding.md` — audio pipeline
- `Entity modding.md`, `Particle modding.md`, `Posteffect modding.md` — 3D/visual assets
- [[hoi4-gui]] — sprites and GUI
