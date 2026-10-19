# Tutorial is unusable under localized (non-English) modpack environments

## Problem description
The tutorial (`Script.java`) locates UI elements — item display names, recipe-tab predicates, plan-menu rows, guide-bar keys, card note targets — by matching **English display text** (exact `equals` / `contains` on rendered strings). Under any localized client (e.g. a Simplified-Chinese modpack with a community localization resource pack), every lookup fails, so the tutorial cannot drive the UI at all.

The same fragility applies in reverse to any client-side string that a localization patch has to touch: because displayed text doubles as an element identity, localizing the UI risks breaking lookups, which forces localizers to treat display text as frozen API surface.

## Environment
- GTNH Planner: 0.2.0-beta.2
- Any non-English modpack/localization (reproduced with the community zh_CN localization of GTNH 2.9.0)

## Steps to reproduce
1. Run the client with a localization that changes item/UI names (e.g. GTNH community Chinese localization pack).
2. Start the in-game tutorial.
3. The tutorial stalls immediately: e.g. `named("iron plate")` can no longer match the localized item name, recipe-tab `contains("chemical")` fails, and plan-menu `rowRect contains "New plan"` finds nothing.

## Expected behavior
The tutorial locates elements through stable identifiers (registry names, unlocalized names, element ids, structural keys) rather than rendered display text, so it keeps working under any localization.

## Actual behavior
All lookups are display-text based; the tutorial is effectively hard-locked to the English client.

## Suggested fix
- Replace display-text lookups with stable keys: item lookup by registry/unlocalized name (e.g. `iron_plate`), recipe tabs by tab id, plan-menu rows by row id, guide-bar keys by section/resource id, card note targets by drawer part id.
- Where text input is required (e.g. search boxes), keep an English fallback or use ids if available.
- This also decouples localization work from tutorial behavior, benefiting any future language.
