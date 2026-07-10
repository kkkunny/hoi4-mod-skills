---
name: hoi4-trouble
description: Debug and troubleshoot HOI4 mod crashes, errors, and broken content. Use this skill whenever the user encounters crashes, errors in error.log, missing content, broken focus trees, or any mod behavior that isn't working as expected.
---

# HOI4 Troubleshooting

This skill covers debugging and fixing common Hearts of Iron IV modding issues.

## Primary Debugging Tool: error.log

Location: `.../Documents/Paradox Interactive/Hearts of Iron IV/logs/error.log`

**Always check this first** when something doesn't work. Enable debug mode via launch options (`-debug`) for more verbose errors.

## Debug Mode

Add `-debug` to launch options for:
- More verbose error reporting
- Map errors don't prevent game loading (MAP_ERROR only crashes in normal mode)
- Access to Nudge tool

## Common Crash Causes

| Symptom | Likely Cause |
|---|---|
| Crash on main menu load | Duplicate IDs, missing namespace, malformed file |
| Crash loading into country | Map errors (MAP_ERROR), state ID gaps, missing buildings.txt entries |
| Crash on civil war | Insufficient dynamic country tags |
| `map/cities.txt` in crash log | Map error (often state ID or province issues) |
| CTD with error 0xC0000005 | Corrupted localisation file (empty language DB with BOM) |

## Common Error Categories

### Encoding Issues
- `.txt` files must be UTF-8 without BOM
- `.yml` localisation files must be UTF-8 with BOM
- `descriptor.mod` must be UTF-8 without BOM
- Wrong encoding → malformed tokens, missing content

### Localisation Issues
- Missing `l_english:` first line
- Special characters in localisation keys
- Multi-line values (use `\n`)
- Missing closing quote
- Error: `Expected colon(:) at line <...>`

### Scope Issues
- Using effect scopes in trigger blocks (or vice versa)
- Non-dual scopes as targets
- State events having ambiguous country/state scopes

### Focus Tree Issues
- Duplicate focus IDs across trees → broken prerequisite lines
- Recursive `relative_position_id` → crash
- Missing `_shine` sprite → no shine animation
- `shared_focus` referencing non-existent focus → crash
- No non-shared focuses but shared focus present → crash

### State/Map Issues
- State IDs not sequential (gaps) → crash
- Missing `buildings.txt` entries for new provinces → crash
- Missing `supply_nodes.txt`/`railways.txt` for new maps → crash
- 8-bit BMP saved with wrong colormap → corrupted map

### Character Issues
- `recruit_character` as last line in file → fails
- Country leader recruited after `set_politics` → leader doesn't appear
- Wrong ideology type in `country_leader.ideology` → character won't become leader

### Events
- Missing namespace declaration → event IDs as malformed tokens
- Non-integer event ID → multiple events treated as same event

### Ideas
- Duplicate idea IDs → unpredictable behavior
- Missing picture → works but shows default icon

## Useful Techniques

### Search in Multiple Files
Most text editors (VS Code, Notepad++, Sublime) support searching entire directories. Use this to find all references to a tag, state ID, or localisation key.

### Replace Path
Add to `.mod` file to completely unload vanilla folders:
```
replace_path = "history/states"
replace_path = "map/strategicregions"
```
Useful for total overhaul mods. Only unloads folder contents, not subfolders.

### Version Wildcards
```
supported_version = "1.17.*"
```
Works for all 1.17.x patches.

### Debug Console
Press `~` (backtick) to open the console. Useful commands:
- `event id` — fire an event
- `focus.nocost` — instant focus completion
- `tag TAG` — switch to a country
- `tdebug` — show tooltip debug info
- `aiview` — see AI strategy info
- `tag_color` — test country colors
- `debug` — toggle debug mode
- `reload focus` — reload focus trees from files

## Error Log Patterns

### "Unexpected token"
Usually a bracket mismatch. Count opening and closing braces. Check the line mentioned and those immediately before.

### "is not a valid" errors
Missing definition. Check that namespaces, tags, and IDs are all properly defined.

### "Duplicate" errors
Two definitions with the same name. Some duplicates are harmless, but focus/event ID duplicates cause real issues.

## Quick Diagnostics Checklist

1. Check `error.log` — it tells you exactly what's wrong
2. Verify encoding: `.txt` = UTF-8 no BOM, `.yml` = UTF-8 with BOM
3. Check bracket matching in files you recently edited
4. Verify all referenced IDs exist (focuses, events, characters, ideas)
5. Verify state/technology/national focus IDs don't overlap with vanilla
6. Check `text.log` for duplicate localisation keys
7. For map issues: use debug mode + Nudge

## References

This skill covers the essentials. For the complete, unabridged reference with exhaustive details, read the following file(s) under the master skill's references directory:

- `skills/hoi4-mod/references/Troubleshooting.md` — Full reference for debugging techniques, error log analysis, and common crash fixes.
