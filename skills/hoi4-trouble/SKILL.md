---
name: hoi4-trouble
description: Debug and troubleshoot HOI4 mod crashes, errors, and broken content. Use this skill whenever the user encounters crashes, errors in error.log, missing content, broken focus trees, or any mod behavior that isn't working as expected.
---

# HOI4 Troubleshooting

Workflow and quick-reference for debugging broken mods.

## First Steps

1. **Read `error.log`** — Windows: `.../Documents/Paradox Interactive/Hearts of Iron IV/logs/error.log`; Linux: `~/.local/share/Paradox Interactive/Hearts of Iron IV/logs/error.log`; macOS: `~/Library/Application Support/Paradox Interactive/Hearts of Iron IV/logs/error.log`.
2. **Confirm the log is fresh** — check its mtime and that the installed mod matches the current build/commit. A stale log leads to false fixes.
3. Launch with **`-debug`** — more verbose errors; MAP_ERROR no longer blocks loading; enables the Nudge tool.
4. Check `logs/text.log` for duplicate localisation keys — it is separate from `error.log`; duplicate keys are warnings, not load failures.
5. Verify the fix by reloading — for focus trees `reload focus` in the console — then ask for a fresh log before declaring success.

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
| `Invalid Scope ... provided: None` | `on_startup` has no country/state scope — enter one first |
| `Unknown category` / `Invalid idea` | `common/idea_tags` was replaced or only partially supplied |
| `Invalid trigger 'set_variable'` | effect-only command used in a trigger — use a temp variable or restructure |
| `Could not find "X" in window "Y"` | a vanilla `.gui` override deleted a child node; keep it and `hide = yes` |
| `Malformed token: width/height` | wrong `size` keys in `buttonType`/`listboxType` — use `{ x y }` |
| `Tried to get dynamic token that does not exist` | localisation key/file not loaded (BOM/header/language/missing scripted-loc entry) |
| `Unexpected token` at `.txt` line 1 | UTF-8 BOM in a `.txt` file |
| `Malformed token: ns.news.1` | event ID has more than one dot |

## Encoding (most common failure source)

| File | Required |
|---|---|
| all `.txt` (incl. `descriptor.mod`) | UTF-8 **without** BOM; a BOM triggers `Unexpected token` at line 1 |
| `.yml` localisation | UTF-8 **exactly one** BOM, first line `l_<locale>:`; double BOM rejects the whole file |
| `.gui`/`.gfx` | UTF-8 without BOM; wrong file extension = ignored |

## Total-Conversion Overrides (`replace_path`)

`replace_path` unloads the **whole vanilla folder**; it does not merge or patch. Every vanilla file that still references an ID from that folder will error unless the consumer is also replaced or stubbed. Replace a folder only when you can regenerate its whole database, and expect cascades:

| Replaced folder | What breaks | Safe pattern |
|---|---|---|
| `common/idea_tags` | vanilla ideas/focus slots → `Unknown category`, `Invalid idea` | keep all categories/slots; remove only law `slot` lines via a same-name `00_idea.txt` override; never ship a partial category set |
| `common/national_focus` | characters/ideas/scripted_localisation/`ai_strategy_plans` reference focus IDs → `invalid focus`, `has_completed_focus` | keep vanilla focus IDs, or replace/stub every consumer; a blank tree + `load_focus_tree` hides gameplay without deleting IDs |
| `events` | on_actions/decisions/focuses reference event IDs → `Malformed token`, `non-existent event` | stub every referenced event (`hidden = yes`, `trigger = { always = no }`) or replace the consumers |
| `common/decisions` | focuses/ideas/AI/scripted_triggers reference decision IDs | stub decisions and categories with `allowed = { always = no }` / `visible = { always = no }` |
| `common/country_tags` | duplicate tags, missing history/flags, broken `country_tag_aliases` references | generate an empty country for every uncovered tag; preserve aliases; add a `dynamic_tags = yes` block with enough tags |
| `common/ai_strategy` | references removed vanilla ideas/characters → `has_idea` spam | replace it too, or keep vanilla and tolerate the references |
| `common/on_actions` | vanilla hooks disappear; vanilla events/effects that expect them stop | usually keep vanilla (additions append); if replaced, port every hook you rely on |
| `interface/*.gui` (whole-file override) | engine expects vanilla child nodes → `Could not find "X" in window "Y"` | keep every child name; hide instead of delete |

Prefer file-level same-name overrides when changing only a few entries. `replace_path` must be present in **both** the `.mod` file and `descriptor.mod`.

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
- **`replace_path`** in the `.mod` file unloads a whole vanilla folder — only when you can regenerate its full database; see the cascade table above.
- **Version wildcard**: `supported_version = "1.17.*"`.

## References

Full details in the [[hoi4-mod]] skill's `references/` folder:

- `Troubleshooting.md` — error log analysis, crash patterns, debugging techniques
- `Mod structure.md` — mod loading/override semantics and `replace_path`
- `Console commands.md` — full console command list
- [[hoi4-mod]] — mod-wide hard rules
