---
name: hoi4-trouble
description: Debug and troubleshoot HOI4 mod crashes, errors, and broken content. Use this skill whenever the user encounters crashes, errors in error.log, missing content, broken focus trees, or any mod behavior that isn't working as expected.
---

# HOI4 Troubleshooting

Workflow and quick-reference for debugging broken mods.

## First Steps

1. **Read `error.log`** — `.../Documents/Paradox Interactive/Hearts of Iron IV/logs/error.log`. It names the file and usually the exact problem.
2. Launch with **`-debug`** — more verbose errors; MAP_ERROR no longer blocks loading; enables the Nudge tool.
3. Check `logs/text.log` for duplicate localisation keys.
4. Verify the fix by reloading — for focus trees `reload focus` in the console.

## Crash/Error Quick Reference

| Symptom | Likely cause |
|---|---|
| Crash on main menu load | duplicate IDs, missing namespace, malformed file |
| Crash loading into a country | map errors (state ID gap, missing `buildings.txt` entries) |
| Crash on civil war | too few dynamic country tags |
| `0xC0000005` CTD | corrupted localisation file (e.g. empty language DB with BOM) |
| "Unexpected token" | bracket mismatch — count braces around the reported line |
| "is not a valid..." | referenced definition missing (tag, namespace, ID) |
| "Duplicate..." | same ID defined twice — harmless for some types, fatal for focuses/events |
| Mod loads but content missing | wrong encoding, filename, or localisation key |

## Encoding (most common failure source)

| File | Required |
|---|---|
| all `.txt` (incl. `descriptor.mod`) | UTF-8 **without** BOM |
| `.yml` localisation | UTF-8 **with** BOM, first line `l_english:` |
| `.gui`/`.gfx` | UTF-8 without BOM; wrong file extension = ignored |

## Frequent Causes by Domain

- **Focus tree**: duplicate focus IDs across trees; recursive `relative_position_id`; `shared_focus` pointing at nothing; missing `_shine` sprite → [[hoi4-focus]]
- **Events**: missing namespace; non-integer event ID → [[hoi4-event]]
- **Ideas**: duplicate idea IDs → [[hoi4-idea]]
- **Characters**: `recruit_character` after `set_politics` or as the last line → [[hoi4-character]]
- **States/map**: non-sequential state IDs; missing `buildings.txt`/`supply_nodes.txt`/`railways.txt` entries; wrong BMP depth → [[hoi4-map]]
- **Script**: effect scope used as a target; trigger/effect scope mix-ups → [[hoi4-script]]
- **Localisation**: missing `l_english:`, spaces in keys, raw newlines, missing closing quote → [[hoi4-assets]]

## Useful Tools

- **Console** (`~`): `event id`, `focus.nocost`, `tag TAG`, `tdebug`, `aiview`, `debug`, `reload focus`, more in `Console commands.md`.
- **`replace_path`** in the `.mod` file to unload vanilla folders when duplicates fight each other.
- **Version wildcard**: `supported_version = "1.17.*"`.

## References

Full details in the [[hoi4-mod]] skill's `references/` folder:

- `Troubleshooting.md` — error log analysis, crash patterns, debugging techniques
- `Console commands.md` — full console command list
- [[hoi4-mod]] — mod-wide hard rules
