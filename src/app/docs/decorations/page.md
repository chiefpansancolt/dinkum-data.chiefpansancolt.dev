---
title: Decorations
nextjs:
  metadata:
    title: dinkum-data - Decorations
    description: Query 265 placeable world decorations by category.
---

Access every placeable world decoration in Dinkum, grouped by category: paths, fences, benches, bridges, lights, statues, and more. {% .lead %}

---

Many of these items also appear in [Crafting Recipes](/docs/crafting-recipes) or [Furniture](/docs/furniture). This module is a curated index across every decoration category shown on the wiki, not a separate source of truth for those items' prices.

## Type

### `Decoration`

| Field           | Type        | Description                                    |
| --------------- | ----------- | ---------------------------------------------- |
| `id`            | `string`    | Stable identifier                              |
| `name`          | `string`    | Display name                                   |
| `img`           | `string`    | Path to the item's icon, relative to `images/` |
| `category`      | `string`    | Decoration category (see below)                |
| `source`        | `string[]?` | Where the item is obtained                     |
| `baseSellPrice` | `number`    | Base sell price in Dinks                       |
| `buyPrice`      | `number?`   | Purchase price, if purchasable                 |

### `DecorationCategory`

One of `'Paths & Steps'`, `'Fences & Gates'`, `'Benches'`, `'Bridges'`, `'Flags, Festoons & Arches'`, `'Flower Beds'`, `'Flowers in Pots'`, `'Ladders'`, `'Lights'`, `'Market Stalls & Miscellaneous'`, `'Pergolas'`, `'Plush'`, `'Statues'`, `'Water Fountains & Waterbeds'`, `'Winter Ice Sculpting'`, or `'Festive'`. This is a union type backed by an exported `DECORATION_CATEGORIES` const array; see [TypeScript integration](/docs/typescript) for the general pattern.

---

## Factory

```typescript
import { decorations } from 'dinkum-data'

decorations() // all 265 items
decorations(source) // wrap a pre-filtered array
```

---

## Filters

| Method       | Signature                                  | Description                                                                                |
| ------------ | ------------------------------------------ | ------------------------------------------------------------------------------------------ |
| `byCategory` | `byCategory(category: DecorationCategory)` | Filter to decorations belonging to the given category.                                     |
| `bySource`   | `bySource(source: string)`                 | Filter to decorations obtainable from the given source (case-insensitive substring match). |

```typescript
import { decorations } from 'dinkum-data'

decorations().byCategory('Statues').get()
decorations().bySource('Blueprint').get()
```

---

## Sorts

| Method                | Signature                                      | Default  | Description              |
| --------------------- | ---------------------------------------------- | -------- | ------------------------ |
| `sortByBaseSellPrice` | `sortByBaseSellPrice(order?: 'asc' \| 'desc')` | `'desc'` | Sort by base sell price. |

---

## Terminal methods

| Method              | Returns                   | Description                         |
| ------------------- | ------------------------- | ----------------------------------- |
| `.get()`            | `Decoration[]`            | All results                         |
| `.first()`          | `Decoration \| undefined` | First result                        |
| `.find(id)`         | `Decoration \| undefined` | Find by `id`                        |
| `.findByName(name)` | `Decoration \| undefined` | Case-insensitive exact name match   |
| `.search(query)`    | `Decoration[]`            | Case-insensitive partial name match |
| `.count()`          | `number`                  | Number of results                   |

---

## Examples

```typescript
import { decorations } from 'dinkum-data'

// Every statue, most valuable first
decorations().byCategory('Statues').sortByBaseSellPrice().get()

// Everything obtainable from Franklyn's Lab
decorations().bySource("Franklyn's Lab").get()
```

---

## Next steps

- See [Furniture](/docs/furniture) and [Crafting Recipes](/docs/crafting-recipes) for the items this module cross-references
- Read [TypeScript integration](/docs/typescript) for how `DecorationCategory` and other enum-like types work
- Read the [query builder](/docs/query-builder) guide for more filter patterns
