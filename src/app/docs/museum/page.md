---
title: 'Museum: Bugs, Critters & Fish'
nextjs:
  metadata:
    title: dinkum-data - Museum
    description: Query every Museum-donatable bug, critter, and fish by biome, season, time found, and rarity.
---

Access every Museum-donatable bug, critter, and fish in Dinkum, with biome, rarity, season, and time-of-day data. {% .lead %}

---

## Shared shape

All three modules on this page — `bugs()`, `critters()`, and `fish()` — extend the same base type, `PediaItem`:

```typescript
interface PediaItem {
  id: string
  name: string
  img: string
  source?: string[]
  baseSellPrice: number
  buyPrice?: number
  biome: Biome[]
  timeFound: TimePeriod[]
  seasons: Season[]
  rarity: RarityLevel
}
```

`TimePeriod` is `'Morning' | 'Day' | 'Evening' | 'Night' | 'All'`, and `RarityLevel` is `'Common' | 'Uncommon' | 'Rare' | 'Very Rare' | 'Super Rare'`.

All three query builders share the same core filter set: `byBiome(biome)`, `byRarity(rarity)`, `bySeason(season)`, and `byTime(time)`, plus `sortByBaseSellPrice(order?)` and the standard terminal methods (`get`, `first`, `find`, `findByName`, `search`, `count`).

---

## Bugs

`bugs()` returns a query over all 50 Museum-donatable bugs. `Bug` is a plain alias for `PediaItem` — no extra fields.

```typescript
import { bugs } from 'dinkum-data'

// Super rare bugs active at night in Autumn
bugs().byRarity('Super Rare').byTime('Night').bySeason('Autumn').get()

// Most valuable bugs
bugs().sortByBaseSellPrice().get()

// Bugs found in Pine Forests
bugs().byBiome('Pine Forests').get()
```

### Terminal methods

| Method              | Returns            | Description                         |
| ------------------- | ------------------ | ----------------------------------- |
| `.get()`            | `Bug[]`            | All results                         |
| `.first()`          | `Bug \| undefined` | First result                        |
| `.find(id)`         | `Bug \| undefined` | Find by `id`                        |
| `.findByName(name)` | `Bug \| undefined` | Case-insensitive exact name match   |
| `.search(query)`    | `Bug[]`            | Case-insensitive partial name match |
| `.count()`          | `number`           | Number of results                   |

---

## Critters

`critters()` returns a query over all 25 Museum-donatable critters. Like `Bug`, `Critter` is a plain alias for `PediaItem`.

```typescript
import { critters } from 'dinkum-data'

// Rare critters found on the Beach
critters().byBiome('Beach').byRarity('Rare').get()

// Most valuable critters
critters().sortByBaseSellPrice().get()

// Critters active during the day
critters().byTime('Day').get()
```

### Terminal methods

| Method              | Returns                | Description                         |
| ------------------- | ---------------------- | ----------------------------------- |
| `.get()`            | `Critter[]`            | All results                         |
| `.first()`          | `Critter \| undefined` | First result                        |
| `.find(id)`         | `Critter \| undefined` | Find by `id`                        |
| `.findByName(name)` | `Critter \| undefined` | Case-insensitive exact name match   |
| `.search(query)`    | `Critter[]`            | Case-insensitive partial name match |
| `.count()`          | `number`               | Number of results                   |

---

## Fish

`fish()` returns a query over all 45 Museum-donatable and cookable fish. `Fish` extends `PediaItem` with two extra cooked-value fields:

```typescript
interface Fish extends PediaItem {
  cookedPrice: number
  cookedPieces: number
}
```

| Field          | Type     | Description                      |
| -------------- | -------- | -------------------------------- |
| `cookedPrice`  | `number` | Sell price once cooked.          |
| `cookedPieces` | `number` | Number of cooked pieces yielded. |

Fish adds one extra sort method on top of the shared set:

| Method                | Signature                                      | Default  | Description                             |
| --------------------- | ---------------------------------------------- | -------- | --------------------------------------- |
| `sortByBaseSellPrice` | `sortByBaseSellPrice(order?: 'asc' \| 'desc')` | `'desc'` | Sort by raw (uncooked) base sell price. |
| `sortByCookedPrice`   | `sortByCookedPrice(order?: 'asc' \| 'desc')`   | `'desc'` | Sort by cooked sell price.              |

```typescript
import { fish } from 'dinkum-data'

// Every summer fish, best cooked sell price first
fish().bySeason('Summer').sortByCookedPrice().get()

// Rare ocean fish
fish().byBiome('Ocean').byRarity('Rare').get()
```

### Terminal methods

| Method              | Returns             | Description                         |
| ------------------- | ------------------- | ----------------------------------- |
| `.get()`            | `Fish[]`            | All results                         |
| `.first()`          | `Fish \| undefined` | First result                        |
| `.find(id)`         | `Fish \| undefined` | Find by `id`                        |
| `.findByName(name)` | `Fish \| undefined` | Case-insensitive exact name match   |
| `.search(query)`    | `Fish[]`            | Case-insensitive partial name match |
| `.count()`          | `number`            | Number of results                   |

---

## Next steps

- See [Animals](/docs/animals) for wildlife outside the Museum
- Browse [Foragables, Trees & Flowers](/docs/foraging) for the biomes bugs, critters, and fish share
- Read [core concepts](/docs/core-concepts) for how the shared `PediaItem` shape fits the wider query builder pattern
