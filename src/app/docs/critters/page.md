---
title: Critters
nextjs:
  metadata:
    title: dinkum-data - Critters
    description: Query every Museum-donatable critter by biome, season, time found, and rarity.
---

Access every Museum-donatable critter in Dinkum, with biome, rarity, season, and time-of-day data. {% .lead %}

---

## Type

### `Critter`

`Critter` is an alias for `PediaItem`. See [TypeScript integration](/docs/typescript) for the full shared shape.

| Field           | Type           | Description                                                          |
| --------------- | -------------- | -------------------------------------------------------------------- |
| `id`            | `string`       | Stable identifier                                                    |
| `name`          | `string`       | Display name                                                         |
| `img`           | `string`       | Path to the critter's icon, relative to `images/`                    |
| `source`        | `string[]?`    | Where the critter is obtained                                        |
| `baseSellPrice` | `number`       | Base sell price in Dinks                                             |
| `buyPrice`      | `number?`      | Purchase price, if purchasable                                       |
| `biome`         | `Biome[]`      | Biomes the critter is found in                                       |
| `timeFound`     | `TimePeriod[]` | Times of day the critter is active                                   |
| `seasons`       | `Season[]`     | Seasons the critter is active                                        |
| `rarity`        | `RarityLevel`  | `'Common'`, `'Uncommon'`, `'Rare'`, `'Very Rare'`, or `'Super Rare'` |

---

## Factory

```typescript
import { critters } from 'dinkum-data'

critters() // all 25 critters
critters(source) // wrap a pre-filtered array
```

---

## Filters

| Method     | Signature                       | Description                                             |
| ---------- | ------------------------------- | ------------------------------------------------------- |
| `byBiome`  | `byBiome(biome: Biome)`         | Filter to critters found in the given biome.            |
| `byRarity` | `byRarity(rarity: RarityLevel)` | Filter by rarity.                                       |
| `bySeason` | `bySeason(season: Season)`      | Filter to critters active in the given season.          |
| `byTime`   | `byTime(time: TimePeriod)`      | Filter to critters active during the given time period. |

```typescript
import { critters } from 'dinkum-data'

critters().byBiome('Beach').get()
critters().byRarity('Rare').get()
critters().bySeason('Winter').get()
critters().byTime('Day').get()
```

---

## Sorts

| Method                | Signature                                      | Default  | Description              |
| --------------------- | ---------------------------------------------- | -------- | ------------------------ |
| `sortByBaseSellPrice` | `sortByBaseSellPrice(order?: 'asc' \| 'desc')` | `'desc'` | Sort by base sell price. |

---

## Terminal methods

| Method              | Returns                | Description                         |
| ------------------- | ---------------------- | ----------------------------------- |
| `.get()`            | `Critter[]`            | All results                         |
| `.first()`          | `Critter \| undefined` | First result                        |
| `.find(id)`         | `Critter \| undefined` | Find by `id`                        |
| `.findByName(name)` | `Critter \| undefined` | Case-insensitive exact name match   |
| `.search(query)`    | `Critter[]`            | Case-insensitive partial name match |
| `.count()`          | `number`               | Number of results                   |

---

## Examples

```typescript
import { critters } from 'dinkum-data'

// Rare critters found on the Beach
critters().byBiome('Beach').byRarity('Rare').get()

// Most valuable critters
critters().sortByBaseSellPrice().get()
```

---

## Next steps

- See [Bugs](/docs/bugs) and [Fish](/docs/fish) for the other two Museum categories
- Browse [Animals](/docs/animals) for wildlife outside the Museum
- Read [TypeScript integration](/docs/typescript) for the shared `PediaItem` shape
