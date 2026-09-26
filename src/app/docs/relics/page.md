---
title: Relics
nextjs:
  metadata:
    title: dinkum-data - Relics
    description: Query every dig-site relic and its locations.
---

Access every relic dug up from junk piles and sold to John or Franklyn in Dinkum. {% .lead %}

---

## Type

```typescript
interface Relic {
  id: string
  name: string
  img: string
  baseSellPrice: number
  buyPrice?: number
  locations: string[]
  johnsSellPrice: number
  franklynsSellPrice?: number
}
```

| Field                | Type       | Description                                      |
| -------------------- | ---------- | ------------------------------------------------ |
| `id`                 | `string`   | Stable identifier.                               |
| `name`               | `string`   | Display name.                                    |
| `img`                | `string`   | Path to the relic's icon, relative to `images/`. |
| `baseSellPrice`      | `number`   | Base sell price in Dinks.                        |
| `buyPrice`           | `number?`  | Purchase price, if purchasable.                  |
| `locations`          | `string[]` | Junk piles or sources the relic can be dug from. |
| `johnsSellPrice`     | `number`   | Sell price at John's Goods.                      |
| `franklynsSellPrice` | `number?`  | Sell price to Franklyn, if he buys it.           |

---

## Factory

```typescript
import { relics } from 'dinkum-data'

relics() // all 15 relics
relics(source) // wrap a pre-filtered array
```

---

## Filters

### `byLocation(location: string)`

Filter to relics found at the given location (exact match, case-insensitive).

```typescript
relics().byLocation('Old Barrel').get()
```

---

## Sorts

### `sortByJohnsSellPrice(order?: 'asc' | 'desc')`

Sort by John's sell price. Defaults to `'desc'` (most valuable first).

---

## Other methods

### `uniqueLocations()`

Every distinct dig-site location across the current result set, alphabetically sorted. Useful for building a location filter without hand-maintaining a separate list.

```typescript
relics().uniqueLocations()
// ["Car Relic", "Crab Pot", "John's Goods", "Old Barrel", "Satellite", "Wheelie Bin"]
```

---

## Terminal methods

| Method              | Returns              | Description                         |
| ------------------- | -------------------- | ----------------------------------- |
| `.get()`            | `Relic[]`            | All results                         |
| `.first()`          | `Relic \| undefined` | First result                        |
| `.find(id)`         | `Relic \| undefined` | Find by `id`                        |
| `.findByName(name)` | `Relic \| undefined` | Case-insensitive exact name match   |
| `.search(query)`    | `Relic[]`            | Case-insensitive partial name match |
| `.count()`          | `number`             | Number of results                   |

---

## Examples

```typescript
import { relics } from 'dinkum-data'

// Every relic from an Old Barrel, most valuable first
relics().byLocation('Old Barrel').sortByJohnsSellPrice().get()
```

---

## Next steps

- See [Trophies](/docs/trophies) for another collectible resource module
- Browse [Other Craftables](/docs/other-craftables) for processed goods
- Read [query builder basics](/docs/query-builder) for more on chaining filters and sorts
