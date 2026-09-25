---
title: Getting Started
nextjs:
  metadata:
    title: dinkum-data - Getting Started
    description: Getting started with dinkum-data, a comprehensive, fully-typed dataset for Dinkum with structured JSON data, image assets, and a chainable query builder API.
---

Documentation for the dinkum-data npm package: a comprehensive, fully-typed dataset for Dinkum with structured JSON data, 2,000+ bundled image assets, and a chainable query builder API for animals, farming, gear, recipes, and more. {% .lead %}

{% quick-links %}

{% quick-link title="Installation" icon="installation" href="/docs/installation" description="Install the package and start querying Dinkum data in your project." /%}

{% quick-link title="Core concepts" icon="presets" href="/docs/core-concepts" description="Learn the factory function and query builder pattern shared by every module." /%}

{% quick-link title="Query builder" icon="plugins" href="/docs/query-builder" description="Filter, sort, and look up crops, animals, gear, and more with chainable methods." /%}

{% quick-link title="TypeScript" icon="theming" href="/docs/typescript" description="Full TypeScript support with typed exports for every module." /%}

{% /quick-links %}

---

## Quick start

### Install the package

```shell
npm install dinkum-data
```

### Import and query data

```typescript
import { animals, fish, licenses, seeds } from 'dinkum-data'

// Domesticable animals
animals().domesticable().get()

// Find a specific fish
fish().findByName('Sturgeon')

// Rare fish, most valuable first
fish().byRarity('Rare').sortByBaseSellPrice().get()

// Spring-plantable seeds, fastest growing first
seeds().bySeason('Spring').sortByGrowthPeriod().get()

// Total permit points needed to max every license
licenses().totalPermitPoints()
```

---

## What's included

The package provides fully-typed data and query builders for nearly 40 modules across the game:

- **Wildlife & Museum**: Wild, farm, and tameable animals; Museum-donatable bugs, critters, and fish
- **Farming & Foraging**: Crops, seeds, foragables, trees, and flowers
- **Resources & Materials**: Animal products, minerals, paint, relics, trophies, and processed craftables
- **Recipes**: Cooking, crafting, Food Modeller conversions, and sign-writing recipes
- **Gear & Equipment**: Books, cassettes, equipment, tools, vehicles, and weapons
- **Clothing, Furniture & Decorations**: Wearable clothing, placeable furniture, and 265 world decorations
- **World**: Buildings and deeds, and every resident NPC
- **Progression**: Licenses, milestones, daily milestones, and skills
- **Calendar**: The full 112-day calendar with birthdays and events
- **Calculators & Reference**: The weight calculator dataset and buff icon lookups

---

## Data source

All game data is sourced from the [Dinkum Wiki](https://dinkum.fandom.com/wiki/Dinkum_Wiki) and kept up-to-date with each game update.

---

## Getting help

### Submit an issue

Found a bug or have a feature request? Open an issue on the [GitHub repository](https://github.com/chiefpansancolt/dinkum-data/issues).

### View on GitHub

Check out the source code at [github.com/chiefpansancolt/dinkum-data](https://github.com/chiefpansancolt/dinkum-data).

### Contributing

Bug reports, feature requests, and pull requests are welcome. See [how to contribute](/docs/how-to-contribute) for details.

This project is not affiliated with, endorsed, or sponsored by James Bendon or KRAFTON. Dinkum is a trademark of its respective owners.
