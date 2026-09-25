---
title: Installation
nextjs:
  metadata:
    title: dinkum-data - Installation
    description: Install dinkum-data and start importing Dinkum game data into your project.
---

Get up and running with dinkum-data in your JavaScript or TypeScript project in under a minute. {% .lead %}

---

## Install the package

Install with your preferred package manager:

### npm

```shell
npm install dinkum-data
```

### Yarn

```shell
yarn add dinkum-data
```

### pnpm

```shell
pnpm add dinkum-data
```

---

## Basic imports

### Named imports

Import individual factory functions and types directly from the package:

```typescript
import { animals, crops, furniture } from 'dinkum-data'

const allAnimals = animals().get()
const summerCrops = crops().bySeason('Summer').get()
const catalogueFurniture = furniture().melvinsCatalogue().get()
```

This is the recommended approach for most use cases. Tree-shaking-friendly bundlers will only include the modules you actually use.

### Namespace import

If you prefer, you can import the entire package as a namespace:

```typescript
import * as dinkum from 'dinkum-data'

const rareFish = dinkum.fish().byRarity('Rare').get()
```

### Type-only imports

When you only need types for annotations, use `import type`:

```typescript
import type { Animal, Crop, Furniture } from 'dinkum-data'

function describe(item: Animal | Crop | Furniture): string {
  return `${item.name} (${item.baseSellPrice} Dinks)`
}
```

---

## Package exports map

The package exposes three entry points through its exports map:

| Entry point            | Description                                                 | Example import                                                  |
| ---------------------- | ----------------------------------------------------------- | --------------------------------------------------------------- |
| `dinkum-data`          | Main entry: every factory function and all TypeScript types | `import { animals } from 'dinkum-data'`                         |
| `dinkum-data/data/*`   | Raw JSON data files backing each module                     | `import cropsData from 'dinkum-data/data/resources/crops.json'` |
| `dinkum-data/images/*` | Bundled PNG image assets for every item                     | Referenced via file path in your bundler or asset pipeline      |

### Main entry point

The main entry point re-exports everything: every module's factory function (`animals()`, `crops()`, `furniture()`, `decorations()`, and around 40 more), plus every exported TypeScript type.

```typescript
import { animals, crops, furniture, decorations, tools } from 'dinkum-data'
```

### Image assets

Every data item ships with an `img` field pointing at a bundled PNG under `dinkum-data/images/`. See the [Image assets](/docs/images) page for usage patterns.

```typescript
const melon = crops().findByName('Watermelon')!
melon.img
// "/images/resources/crops/Watermelon.png"
```

---

## Requirements

- **Node.js** 18 or later
- **TypeScript** 5.0 or later (optional, but recommended)

The package ships with full TypeScript declarations and works in both ESM and CommonJS projects. No additional `@types` package is needed.

---

## Next steps

- Learn the [core concepts](/docs/core-concepts) behind the query builder pattern
- Explore the [query builder](/docs/query-builder) for filtering and looking up data
- See [TypeScript integration](/docs/typescript) for type usage across every module
