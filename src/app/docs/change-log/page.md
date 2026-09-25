---
title: Change Log
nextjs:
  metadata:
    title: dinkum-data - Change Log
    description: See what has changed from release to release in dinkum-data.
---

This page summarizes recent releases. See the full [CHANGELOG.md](https://github.com/chiefpansancolt/dinkum-data/blob/main/CHANGELOG.md) on GitHub for the complete history. {% .lead %}

---

## Version [1.0.2](https://github.com/chiefpansancolt/dinkum-data/releases/tag/1.0.2)

### Fixed

Renamed 9 image files that were literally named with the 3-character string `%27` instead of a real
apostrophe (`Inv_Hunter%27s_Helmet.png` → `Inv_Hunter's_Helmet.png`, and 8 others across clothing,
furniture, books, milestones, critters, and cooking recipes). Every `img` field already referenced
the correct URL-encoded `%27s`, so browsers decoded the request to a filename that never existed on
disk, producing broken images. No data files needed to change: only the assets.

## Version [1.0.1](https://github.com/chiefpansancolt/dinkum-data/releases/tag/1.0.1)

### Fixed

The published `1.0.0` package was built from a stale `dist/` that predated the `decorations()` and
`foodModellerRecipes()` modules being wired into the main entry point, so neither function (nor
their types) was actually importable, even though the underlying JSON data shipped correctly.
Republished with a fresh build that includes both, and added a `prepublishOnly` build script so a
stale `dist/` can't be published again.

## Version [1.0.0](https://github.com/chiefpansancolt/dinkum-data/releases/tag/1.0.0)

A full reconciliation pass against the [Dinkum Wiki](https://dinkum.fandom.com/wiki/Dinkum_Wiki),
covering every existing module plus a brand-new `decorations()` module and the 1st Anniversary
Update ("The Great Bite").

### Added

- `decorations()`: new module covering 265 placeable world decorations across 16 categories.
- `foodModellerRecipes()`: new module for Food Modeller display conversions.
- Full content from the 1st Anniversary Update ("The Great Bite") patch notes, including the Great
  Bite biome, the Night Merchant NPC and calendar event, the Scooter vehicle, 20 new tools, 4 new
  equipment items, 12 new clothing items, 4 new decorations, and 2 new furniture items.
- Dozens of missing items found and added across nearly every module during the wiki reconciliation
  pass.

### Fixed

- Corrected a systematic ~1.6x stale-price bug affecting 19 wild animal `researchReward` values.
- Corrected stale or incorrect prices and sources across minerals, other-craftables, cassettes,
  cooking recipes, paint, vehicles, furniture, buildings, clothing, and animal products.
- Fixed several name typos and missing source lists.

## Version [0.2.0](https://github.com/chiefpansancolt/dinkum-data/releases/tag/0.2.0)

### Added

- `clothingSlots()` / `clothingTypesForSlot()`: the clothing-slot taxonomy.
- `buffIcons()`: the icon image path for each buff tier.
- `milestoneCategories()`: the 9 milestone categories used to group milestones for filtering.
- `relics().uniqueLocations()`: every distinct relic dig-site location, derived from the data.

## Version [0.1.0](https://github.com/chiefpansancolt/dinkum-data/releases/tag/0.1.0)

Initial release: structured JSON data, TypeScript types, image assets, and a chainable query builder
API for Dinkum, sourced from the [Dinkum Wiki](https://dinkum.fandom.com/wiki/Dinkum_Wiki).

---

## Keeping up to date

This package tracks Dinkum updates closely: see the [Data source](/) section on the getting started
page for where the data comes from, and watch the [GitHub repository](https://github.com/chiefpansancolt/dinkum-data)
for release notifications.
