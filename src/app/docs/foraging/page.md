---
title: Foragables, Trees & Flowers
nextjs:
  metadata:
    title: dinkum-data - Foragables, Trees & Flowers
    description: Query wild-foraged plants, trees, and flowers by biome and growth timing.
---

Access every wild-foraged plant, tree, and flower in Dinkum, with the biomes they grow in and their growth timing. {% .lead %}

---

## Foragables

`foragables()` returns a query over all 42 wild-foraged plants and items.

```typescript
import { foragables } from 'dinkum-data'

// Everything foraged in the Tropics
foragables().byLocation('Tropics').get()

// Look up by name
foragables().findByName('Banana')
```

### Type Definition

```typescript
interface Foragable {
  id: string
  name: string
  img: string
  source?: string[]
  baseSellPrice: number
  buyPrice?: number
  buffs?: Buffs
  locations?: Biome[]
}
```

### Field Reference

| Field           | Type                    | Description                                     |
| --------------- | ----------------------- | ----------------------------------------------- |
| `id`            | `string`                | Stable identifier.                              |
| `name`          | `string`                | Display name.                                   |
| `img`           | `string`                | Path to the item's icon, relative to `images/`. |
| `source`        | `string[] \| undefined` | The plant or object this item is foraged from.  |
| `baseSellPrice` | `number`                | Base sell price in Dinks.                       |
| `buyPrice`      | `number \| undefined`   | Purchase price, if purchasable.                 |
| `buffs`         | `Buffs \| undefined`    | Consumable buff effects, if edible.             |
| `locations`     | `Biome[] \| undefined`  | Biomes the item is found in.                    |

### Filter Methods

| Method       | Signature                  | Description                                                                           |
| ------------ | -------------------------- | ------------------------------------------------------------------------------------- |
| `byLocation` | `byLocation(biome: Biome)` | Filter to foragables found in the given biome. Items with no `locations` never match. |

### Terminal methods

| Method              | Returns                  | Description                         |
| ------------------- | ------------------------ | ----------------------------------- |
| `.get()`            | `Foragable[]`            | All results                         |
| `.first()`          | `Foragable \| undefined` | First result                        |
| `.find(id)`         | `Foragable \| undefined` | Find by `id`                        |
| `.findByName(name)` | `Foragable \| undefined` | Case-insensitive exact name match   |
| `.search(query)`    | `Foragable[]`            | Case-insensitive partial name match |
| `.count()`          | `number`                 | Number of results                   |

---

## Trees

`trees()` returns a query over all 18 trees found across Dinkum's biomes.

```typescript
import { trees } from 'dinkum-data'

// Every tree in Pine Forests, fastest growing first
trees().byLocation('Pine Forests').sortByGrowthPeriod().get()

// Look up by name
trees().findByName('Apple Tree')
```

### Type Definition

```typescript
interface Tree {
  id: string
  name: string
  img: string
  seed?: Seed | Foragable
  itemsDropped: Resource[]
  locations?: Biome[]
  growthPeriod?: number
  regrowth?: number
}
```

`Resource` is `{ name: string; img: string; count: number }`.

### Field Reference

| Field          | Type                             | Description                                         |
| -------------- | -------------------------------- | --------------------------------------------------- |
| `id`           | `string`                         | Stable identifier.                                  |
| `name`         | `string`                         | Display name.                                       |
| `img`          | `string`                         | Path to the tree's icon, relative to `images/`.     |
| `seed`         | `Seed \| Foragable \| undefined` | The seed or foragable this tree grows from, if any. |
| `itemsDropped` | `Resource[]`                     | Items harvested from the tree.                      |
| `locations`    | `Biome[] \| undefined`           | Biomes the tree is found in.                        |
| `growthPeriod` | `number \| undefined`            | Days to reach maturity.                             |
| `regrowth`     | `number \| undefined`            | Days between harvests once mature.                  |

### Filter & Sort Methods

| Method               | Signature                                     | Description                                                                                               |
| -------------------- | --------------------------------------------- | --------------------------------------------------------------------------------------------------------- |
| `byLocation`         | `byLocation(biome: Biome)`                    | Filter to trees found in the given biome. Trees with no `locations` never match.                          |
| `sortByGrowthPeriod` | `sortByGrowthPeriod(order?: 'asc' \| 'desc')` | Sort by growth period in days. Default `'asc'` (fastest first). Trees without a `growthPeriod` sort as 0. |

### Terminal methods

| Method              | Returns             | Description                         |
| ------------------- | ------------------- | ----------------------------------- |
| `.get()`            | `Tree[]`            | All results                         |
| `.first()`          | `Tree \| undefined` | First result                        |
| `.find(id)`         | `Tree \| undefined` | Find by `id`                        |
| `.findByName(name)` | `Tree \| undefined` | Case-insensitive exact name match   |
| `.search(query)`    | `Tree[]`            | Case-insensitive partial name match |
| `.count()`          | `number`            | Number of results                   |

---

## Flowers

`flowers()` returns a query over all 46 flowers found across Dinkum's biomes.

```typescript
import { flowers } from 'dinkum-data'

// Every flower in Bushlands, fastest-growing first
flowers().byLocation('Bushlands').sortByGrowthPeriod().get()

// Look up by name
flowers().findByName('Billy Button')
```

### Type Definition

```typescript
interface Flower {
  id: string
  name: string
  img: string
  source?: string[]
  baseSellPrice: number
  buyPrice?: number
  seed?: Seed | Foragable
  itemsDropped?: Resource[]
  locations: Biome[]
  conditions?: string
  growthPeriod?: number
  regrowth?: number
}
```

### Field Reference

| Field           | Type                             | Description                                           |
| --------------- | -------------------------------- | ----------------------------------------------------- |
| `id`            | `string`                         | Stable identifier.                                    |
| `name`          | `string`                         | Display name.                                         |
| `img`           | `string`                         | Path to the flower's icon, relative to `images/`.     |
| `source`        | `string[] \| undefined`          | Where the flower is obtained.                         |
| `baseSellPrice` | `number`                         | Base sell price in Dinks.                             |
| `buyPrice`      | `number \| undefined`            | Purchase price, if purchasable.                       |
| `seed`          | `Seed \| Foragable \| undefined` | The seed or foragable this flower grows from, if any. |
| `itemsDropped`  | `Resource[] \| undefined`        | Items produced when harvested.                        |
| `locations`     | `Biome[]`                        | Biomes the flower is found in.                        |
| `conditions`    | `string \| undefined`            | Special growing conditions.                           |
| `growthPeriod`  | `number \| undefined`            | Days to reach maturity.                               |
| `regrowth`      | `number \| undefined`            | Days between harvests once mature.                    |

### Filter & Sort Methods

| Method               | Signature                                     | Description                                                                                                 |
| -------------------- | --------------------------------------------- | ----------------------------------------------------------------------------------------------------------- |
| `byLocation`         | `byLocation(biome: Biome)`                    | Filter to flowers found in the given biome.                                                                 |
| `sortByGrowthPeriod` | `sortByGrowthPeriod(order?: 'asc' \| 'desc')` | Sort by growth period in days. Default `'asc'` (fastest first). Flowers without a `growthPeriod` sort as 0. |

### Terminal methods

| Method              | Returns               | Description                         |
| ------------------- | --------------------- | ----------------------------------- |
| `.get()`            | `Flower[]`            | All results                         |
| `.first()`          | `Flower \| undefined` | First result                        |
| `.find(id)`         | `Flower \| undefined` | Find by `id`                        |
| `.findByName(name)` | `Flower \| undefined` | Case-insensitive exact name match   |
| `.search(query)`    | `Flower[]`            | Case-insensitive partial name match |
| `.count()`          | `number`              | Number of results                   |

---

## Examples

### Everything foraged or grown in the Tropics

```typescript
import { foragables, trees, flowers } from 'dinkum-data'

const tropicalForagables = foragables().byLocation('Tropics').get()
const tropicalTrees = trees().byLocation('Tropics').get()
const tropicalFlowers = flowers().byLocation('Tropics').get()
```

### Fastest-growing trees across every biome

```typescript
import { trees } from 'dinkum-data'

const fastestTrees = trees().sortByGrowthPeriod().get()
```

## Next steps

- See [Crops & Seeds](/docs/crops) for the shared `Seed` shape trees and flowers can grow from
- Browse [Animals](/docs/animals) for the wildlife sharing these same biomes
- Read the [query builder](/docs/query-builder) guide for biome and growth-period filter patterns
