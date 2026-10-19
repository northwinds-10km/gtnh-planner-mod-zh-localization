# GTNH Planner — Simplified Chinese Localization

Complete Simplified Chinese localization of **GTNH Planner** — the Minecraft mod
side of [gtnhplanner.com](https://gtnhplanner.com) (a fork of PlanNH by Sbancuz,
rebuilt and renamed; see [jackwrichards/gtnh-planner-mod](https://github.com/jackwrichards/gtnh-planner-mod)),
version 0.2.0-beta.2. Produced as a bytecode constant-patch (no source rebuild required).

This project localizes the mod's own UI. In-game item/fluid names (coils, materials,
fuels, etc.) come from the game registry and require the official GTNH localization
resource pack to display in Chinese; the two work together for a fully Chinese
experience.

## Coverage

~1,200+ strings across the entire in-game UI:

- Board & cards (main canvas, drawers, wires, recipe cards, NEI integration)
- Notes / boxes / arrows (context menus, style pickers, tooltips)
- Settings panel, minimap, pinned list, keybinds
- Task system (task panel, task board, task editing, comments, assignments)
- Solver hints & warnings (BoardSession message family)
- Machine control pages (heating coils, casings, solenoids, solenoid coils, field restriction coils — calibrated to official GTNH material names)
- Power panel & cloud sync error messages (cloud/session package)
- Quest picker, plan menu, library view

Both variants of the multi-release jar are patched (base + `META-INF/versions/25/`).

## Translation quality

All terminology is calibrated against the official GTNH localization project
(GTNH 汉化组) language files, so item/machine/material names match what Chinese
players see everywhere else in the pack. Examples: Kanthal = 坎塔尔合金,
Trinium = 三元金属, Field Restriction Coil = 力场约束线圈, DTPF = 超维度等离子锻炉.

Logic/serialization strings (registry names, part keys, recipe keys, protocol
fields) are deliberately left untouched — verified by bytecode reference analysis.

## Files

| File | Description |
|---|---|
| `gtnhplanner-0.2.0-beta.2-zh-v7.jar` | The localized mod (install by replacing the original jar in `mods/`) |
| `translations-v1.json` … `translations-v7.json` | Per-version patch rule archives (source of truth) |
| `zh-CN-flat-mapping.json` | Flat en→zh mapping (convenience for lang-file conversion) |
| `issue-*.md` | Issue reports filed upstream (EN + ZH reference copies) |

## Testing

GTNH 2.9.0-RC-2 / Java 25, multiple in-game regression passes (board ops, solver,
cloud sync, task system, power panel, NEI integration). See the issue reports in the issue-*.md files` for bugs found during testing (filed upstream as #515 comment / #519 / #520).

## Upstreaming

Offered to the author (jackwrichards/gtnh-planner-mod) under MIT, in whichever
form preferred:

- **Option A**: source-level i18n refactor — move hardcoded strings into
  en_US/zh_CN lang files; this repo's mapping data converts directly.
- **Option B**: integrate the mapping data at your own pace.

Terminology reference data comes from the GTNH community localization project
(GTNH 汉化组); those files remain under their own license and are not redistributed
here — only our own translation rules are.

## License

MIT, matching upstream (gtnh-planner-mod).
