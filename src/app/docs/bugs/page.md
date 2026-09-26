---
title: Bugs
nextjs:
  metadata:
    title: dinkum-data - Bugs
    description: Query every Museum-donatable bug by biome, season, time found, and rarity.
---

Access every Museum-donatable bug in Dinkum, with biome, rarity, season, and time-of-day data. {% .lead %}

---

## Type

### `Bug`

`Bug` is an alias for `PediaItem`. See [TypeScript integration](/docs/typescript) for the full shared shape.

| Field           | Type           | Description                                                                           |
| --------------- | -------------- | ------------------------------------------------------------------------------------- |
| `id`            | `string`       | Stable identifier                                                                     |
| `name`          | `string`       | Display name                                                                          |
| `img`           | `string`       | Path to the bug's icon, relative to `images/`                                         |
| `source`        | `string[]?`    | Where the bug is obtained                                                             |
| `baseSellPrice` | `number`       | Base sell price in Dinks                                                              |
| `buyPrice`      | `number?`      | Purchase price, if purchasable                                                        |
| `biome`         | `Biome[]`      | Biomes the bug is found in                                                            |
| `timeFound`     | `TimePeriod[]` | Times of day the bug is active: `'Morning'`, `'Day'`, `'Evening'`, `'Night'`, `'All'` |
| `seasons`       | `Season[]`     | Seasons the bug is active                                                             |
| `rarity`        | `RarityLevel`  | `'Common'`, `'Uncommon'`, `'Rare'`, `'Very Rare'`, or `'Super Rare'`                  |

---

## Factory

```typescript
import { bugs } from 'dinkum-data'

bugs() // all 50 bugs
bugs(source) // wrap a pre-filtered array
```

---

## Filters

| Method     | Signature                       | Description                                         |
| ---------- | ------------------------------- | --------------------------------------------------- |
| `byBiome`  | `byBiome(biome: Biome)`         | Filter to bugs found in the given biome.            |
| `byRarity` | `byRarity(rarity: RarityLevel)` | Filter by rarity.                                   |
| `bySeason` | `bySeason(season: Season)`      | Filter to bugs active in the given season.          |
| `byTime`   | `byTime(time: TimePeriod)`      | Filter to bugs active during the given time period. |

```typescript
import { bugs } from 'dinkum-data'

bugs().byBiome('Pine Forests').get()
bugs().byRarity('Super Rare').get()
bugs().bySeason('Summer').get()
bugs().byTime('Night').get()
```

---

## Sorts

| Method                | Signature                                      | Default  | Description              |
| --------------------- | ---------------------------------------------- | -------- | ------------------------ |
| `sortByBaseSellPrice` | `sortByBaseSellPrice(order?: 'asc' \| 'desc')` | `'desc'` | Sort by base sell price. |

---

## Terminal methods

| Method              | Returns            | Description                         |
| ------------------- | ------------------ | ----------------------------------- |
| `.get()`            | `Bug[]`            | All results                         |
| `.first()`          | `Bug \| undefined` | First result                        |
| `.find(id)`         | `Bug \| undefined` | Find by `id`                        |
| `.findByName(name)` | `Bug \| undefined` | Case-insensitive exact name match   |
| `.search(query)`    | `Bug[]`            | Case-insensitive partial name match |
| `.count()`          | `number`           | Number of results                   |

---

## Examples

```typescript
import { bugs } from 'dinkum-data'

// Super rare bugs active at night in Autumn
bugs().byRarity('Super Rare').byTime('Night').bySeason('Autumn').get()

// Most valuable bugs
bugs().sortByBaseSellPrice().get()
```

---

## Next steps

- See [Critters](/docs/critters) and [Fish](/docs/fish) for the other two Museum categories
- Browse [Animals](/docs/animals) for wildlife outside the Museum
- Read [TypeScript integration](/docs/typescript) for the shared `PediaItem` shape
