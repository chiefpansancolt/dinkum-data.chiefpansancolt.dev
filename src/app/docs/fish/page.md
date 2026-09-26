---
title: Fish
nextjs:
  metadata:
    title: dinkum-data - Fish
    description: Query every Museum-donatable and cookable fish by biome, season, time found, and rarity.
---

Access every Museum-donatable and cookable fish in Dinkum, with biome, rarity, season, and time-of-day data. {% .lead %}

---

## Type

### `Fish`

Extends `PediaItem` with cooked-value fields. See [TypeScript integration](/docs/typescript) for the full shared shape.

| Field           | Type           | Description                                                          |
| --------------- | -------------- | -------------------------------------------------------------------- |
| `id`            | `string`       | Stable identifier                                                    |
| `name`          | `string`       | Display name                                                         |
| `img`           | `string`       | Path to the fish's icon, relative to `images/`                       |
| `source`        | `string[]?`    | Where the fish is caught                                             |
| `baseSellPrice` | `number`       | Base (raw) sell price in Dinks                                       |
| `buyPrice`      | `number?`      | Purchase price, if purchasable                                       |
| `biome`         | `Biome[]`      | Biomes the fish is found in                                          |
| `timeFound`     | `TimePeriod[]` | Times of day the fish is active                                      |
| `seasons`       | `Season[]`     | Seasons the fish is active                                           |
| `rarity`        | `RarityLevel`  | `'Common'`, `'Uncommon'`, `'Rare'`, `'Very Rare'`, or `'Super Rare'` |
| `cookedPrice`   | `number`       | Sell price once cooked                                               |
| `cookedPieces`  | `number`       | Number of cooked pieces yielded                                      |

---

## Factory

```typescript
import { fish } from 'dinkum-data'

fish() // all 45 fish
fish(source) // wrap a pre-filtered array
```

---

## Filters

| Method     | Signature                       | Description                                         |
| ---------- | ------------------------------- | --------------------------------------------------- |
| `byBiome`  | `byBiome(biome: Biome)`         | Filter to fish found in the given biome.            |
| `byRarity` | `byRarity(rarity: RarityLevel)` | Filter by rarity.                                   |
| `bySeason` | `bySeason(season: Season)`      | Filter to fish available in the given season.       |
| `byTime`   | `byTime(time: TimePeriod)`      | Filter to fish caught during the given time period. |

```typescript
import { fish } from 'dinkum-data'

fish().byBiome('Ocean').get()
fish().byRarity('Rare').get()
fish().bySeason('Summer').get()
fish().byTime('Morning').get()
```

---

## Sorts

| Method                | Signature                                      | Default  | Description                             |
| --------------------- | ---------------------------------------------- | -------- | --------------------------------------- |
| `sortByBaseSellPrice` | `sortByBaseSellPrice(order?: 'asc' \| 'desc')` | `'desc'` | Sort by raw (uncooked) base sell price. |
| `sortByCookedPrice`   | `sortByCookedPrice(order?: 'asc' \| 'desc')`   | `'desc'` | Sort by cooked sell price.              |

---

## Terminal methods

| Method              | Returns             | Description                         |
| ------------------- | ------------------- | ----------------------------------- |
| `.get()`            | `Fish[]`            | All results                         |
| `.first()`          | `Fish \| undefined` | First result                        |
| `.find(id)`         | `Fish \| undefined` | Find by `id`                        |
| `.findByName(name)` | `Fish \| undefined` | Case-insensitive exact name match   |
| `.search(query)`    | `Fish[]`            | Case-insensitive partial name match |
| `.count()`          | `number`            | Number of results                   |

---

## Examples

```typescript
import { fish } from 'dinkum-data'

// Every summer fish, best cooked sell price first
fish().bySeason('Summer').sortByCookedPrice().get()

// Rare ocean fish
fish().byBiome('Ocean').byRarity('Rare').get()
```

---

## Next steps

- See [Bugs](/docs/bugs) and [Critters](/docs/critters) for the other two Museum categories
- Browse [Cooking Recipes](/docs/cooking-recipes) for what cooked fish can be turned into
- Read [TypeScript integration](/docs/typescript) for the shared `PediaItem` shape
