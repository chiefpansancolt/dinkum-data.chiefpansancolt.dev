---
title: Crops
nextjs:
  metadata:
    title: dinkum-data - Crops
    description: Query every farmable crop by season, growth period, and sell price.
---

Access every farmable crop in Dinkum, along with the seed each one grows from. {% .lead %}

Every `Crop` embeds the `Seed` it grows from directly on its own `seed` field. For the full seed catalog, including tree and bush seeds that don't have a corresponding `Crop` entry, see the standalone [Seeds](/docs/seeds) page.

---

## Type

### `Crop`

| Field           | Type        | Description                                    |
| --------------- | ----------- | ---------------------------------------------- |
| `id`            | `string`    | Stable identifier                              |
| `name`          | `string`    | Display name                                   |
| `img`           | `string`    | Path to the crop's icon, relative to `images/` |
| `source`        | `string[]?` | Where the seed is purchased                    |
| `baseSellPrice` | `number`    | Base sell price in Dinks                       |
| `buyPrice`      | `number?`   | Seed purchase price                            |
| `buffs`         | `Buffs?`    | Consumable buff effects, if edible             |
| `seed`          | `Seed?`     | The seed this crop grows from                  |

---

## Factory

```typescript
import { crops } from 'dinkum-data'

crops() // all 17 crops
crops(source) // wrap a pre-filtered array
```

---

## Filters

| Method     | Signature                  | Description                                                                                    |
| ---------- | -------------------------- | ---------------------------------------------------------------------------------------------- |
| `bySeason` | `bySeason(season: Season)` | Filter to crops whose seed is plantable in the given season. Crops with no `seed` never match. |

```typescript
import { crops } from 'dinkum-data'

crops().bySeason('Summer').get()
```

---

## Sorts

| Method                | Signature                                      | Default  | Description              |
| --------------------- | ---------------------------------------------- | -------- | ------------------------ |
| `sortByBaseSellPrice` | `sortByBaseSellPrice(order?: 'asc' \| 'desc')` | `'desc'` | Sort by base sell price. |

---

## Terminal methods

| Method              | Returns             | Description                         |
| ------------------- | ------------------- | ----------------------------------- |
| `.get()`            | `Crop[]`            | All results                         |
| `.first()`          | `Crop \| undefined` | First result                        |
| `.find(id)`         | `Crop \| undefined` | Find by `id`                        |
| `.findByName(name)` | `Crop \| undefined` | Case-insensitive exact name match   |
| `.search(query)`    | `Crop[]`            | Case-insensitive partial name match |
| `.count()`          | `number`            | Number of results                   |

---

## Examples

```typescript
import { crops } from 'dinkum-data'

// Every crop plantable in Summer, most valuable first
crops().bySeason('Summer').sortByBaseSellPrice().get()

// Look up by name
crops().findByName('Beetroot')
```

---

## Next steps

- See [Seeds](/docs/seeds) for the full seed catalog, including trees and bushes
- Browse [Cooking Recipes](/docs/cooking-recipes) for what harvested crops can become
- Read the [query builder](/docs/query-builder) guide for more filter patterns
