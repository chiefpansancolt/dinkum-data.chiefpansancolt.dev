---
title: Crops & Seeds
nextjs:
  metadata:
    title: dinkum-data - Crops & Seeds
    description: Query every farmable crop and plantable seed by season, growth period, and sell price.
---

Access every farmable crop and plantable seed in Dinkum, including growing season, growth timing, and sell prices. {% .lead %}

---

## How crops and seeds relate

Every `Crop` embeds the `Seed` it grows from directly on its own `seed` field, so you can usually work from `crops()` alone. The standalone `seeds()` module exists for querying the full seed catalog on its own terms — it also covers tree and bush seeds that don't have a corresponding `Crop` entry.

```typescript
import { crops } from 'dinkum-data'

const watermelon = crops().findByName('Watermelon')!
watermelon.seed?.growthPeriod
watermelon.seed?.season
```

---

## Crops

`crops()` returns a query over all 17 farmable crops.

```typescript
import { crops } from 'dinkum-data'

// Every crop plantable in Summer, most valuable first
crops().bySeason('Summer').sortByBaseSellPrice().get()

// Look up by name
crops().findByName('Beetroot')
```

### Type Definition

```typescript
interface Crop {
  id: string
  name: string
  img: string
  source?: string[]
  baseSellPrice: number
  buyPrice?: number
  buffs?: Buffs
  seed?: Seed
}
```

### Field Reference

| Field           | Type                    | Description                                     |
| --------------- | ----------------------- | ----------------------------------------------- |
| `id`            | `string`                | Stable identifier.                              |
| `name`          | `string`                | Display name.                                   |
| `img`           | `string`                | Path to the crop's icon, relative to `images/`. |
| `source`        | `string[] \| undefined` | Where the seed is purchased.                    |
| `baseSellPrice` | `number`                | Base sell price in Dinks.                       |
| `buyPrice`      | `number \| undefined`   | Seed purchase price.                            |
| `buffs`         | `Buffs \| undefined`    | Consumable buff effects, if edible.             |
| `seed`          | `Seed \| undefined`     | The seed this crop grows from.                  |

### Filter & Sort Methods

| Method                | Signature                                      | Description                                                                                    |
| --------------------- | ---------------------------------------------- | ---------------------------------------------------------------------------------------------- |
| `bySeason`            | `bySeason(season: Season)`                     | Filter to crops whose seed is plantable in the given season. Crops with no `seed` never match. |
| `sortByBaseSellPrice` | `sortByBaseSellPrice(order?: 'asc' \| 'desc')` | Sort by base sell price. Default `'desc'` (most valuable first).                               |

### Terminal methods

| Method              | Returns             | Description                         |
| ------------------- | ------------------- | ----------------------------------- |
| `.get()`            | `Crop[]`            | All results                         |
| `.first()`          | `Crop \| undefined` | First result                        |
| `.find(id)`         | `Crop \| undefined` | Find by `id`                        |
| `.findByName(name)` | `Crop \| undefined` | Case-insensitive exact name match   |
| `.search(query)`    | `Crop[]`            | Case-insensitive partial name match |
| `.count()`          | `number`            | Number of results                   |

---

## Seeds

`seeds()` returns a query over all 36 plantable seeds — crops, trees, and bushes alike.

```typescript
import { seeds } from 'dinkum-data'

// Every summer seed, fastest growing first
seeds().bySeason('Summer').sortByGrowthPeriod().get()

// Every tree seed
seeds().byCategory('Tree Seed').get()
```

### Type Definition

```typescript
interface Seed {
  id: string
  name: string
  img: string
  source?: string[]
  baseSellPrice: number
  buyPrice?: number
  category: string
  growthPeriod: number
  outputCountMin?: number
  outputCountMax?: number
  regrowth?: number
  season: Season[]
}
```

### Field Reference

| Field            | Type                    | Description                                       |
| ---------------- | ----------------------- | ------------------------------------------------- |
| `id`             | `string`                | Stable identifier.                                |
| `name`           | `string`                | Display name.                                     |
| `img`            | `string`                | Path to the seed's icon, relative to `images/`.   |
| `source`         | `string[] \| undefined` | Where the seed is purchased or obtained.          |
| `baseSellPrice`  | `number`                | Base sell price in Dinks.                         |
| `buyPrice`       | `number \| undefined`   | Purchase price, if purchasable.                   |
| `category`       | `string`                | `'Crop Seed'`, `'Tree Seed'`, `'Bush Seed'`, etc. |
| `growthPeriod`   | `number`                | Days to reach maturity.                           |
| `outputCountMin` | `number \| undefined`   | Minimum yield per harvest.                        |
| `outputCountMax` | `number \| undefined`   | Maximum yield per harvest.                        |
| `regrowth`       | `number \| undefined`   | Days between harvests once mature.                |
| `season`         | `Season[]`              | Seasons the seed can be planted in.               |

### Filter & Sort Methods

| Method               | Signature                                     | Description                                                     |
| -------------------- | --------------------------------------------- | --------------------------------------------------------------- |
| `byCategory`         | `byCategory(category: string)`                | Filter by seed category (exact match, case-insensitive).        |
| `bySeason`           | `bySeason(season: Season)`                    | Filter to seeds plantable in the given season.                  |
| `sortByGrowthPeriod` | `sortByGrowthPeriod(order?: 'asc' \| 'desc')` | Sort by growth period in days. Default `'asc'` (fastest first). |

### Terminal methods

| Method              | Returns             | Description                         |
| ------------------- | ------------------- | ----------------------------------- |
| `.get()`            | `Seed[]`            | All results                         |
| `.first()`          | `Seed \| undefined` | First result                        |
| `.find(id)`         | `Seed \| undefined` | Find by `id`                        |
| `.findByName(name)` | `Seed \| undefined` | Case-insensitive exact name match   |
| `.search(query)`    | `Seed[]`            | Case-insensitive partial name match |
| `.count()`          | `number`            | Number of results                   |

---

## Examples

### Fastest-growing spring crops

```typescript
import { crops } from 'dinkum-data'

const fastSpring = crops()
  .bySeason('Spring')
  .get()
  .sort((a, b) => (a.seed?.growthPeriod ?? 0) - (b.seed?.growthPeriod ?? 0))
```

### Every tree and bush seed together

```typescript
import { seeds } from 'dinkum-data'

const nonCropSeeds = seeds()
  .get()
  .filter((s) => s.category !== 'Crop Seed')
```

### Regrowing crops sorted by value

```typescript
import { crops } from 'dinkum-data'

const regrowers = crops()
  .get()
  .filter((c) => c.seed?.regrowth !== undefined)
  .sort((a, b) => b.baseSellPrice - a.baseSellPrice)
```

## Next steps

- See [Foragables, Trees & Flowers](/docs/foraging) for wild-growing plants, including trees that share the `Seed` shape
- Browse [Recipes](/docs/recipes) for cooking crops into dishes
- Read the [query builder](/docs/query-builder) guide for season and sort filter patterns shared across modules
