---
title: Furniture & Decorations
nextjs:
  metadata:
    title: dinkum-data - Furniture & Decorations
    description: Query placeable furniture and 265 world decorations by category, set, and source.
---

Every placeable item for your house and your island: furniture for indoors, and 265 world decorations for everywhere else. {% .lead %}

---

## Furniture

`furniture()` returns a `FurnitureQuery` over all 443 items.

### Type

```typescript
interface Furniture {
  id: string
  name: string
  img: string
  source?: string[]
  baseSellPrice: number
  buyPrice?: number
  displayPrice?: number
  cataloguePrice?: number
  melvinsCatalogue: boolean
  furnitureSet?: string
}
```

### Field reference

| Field              | Type        | Description                                         |
| ------------------ | ----------- | --------------------------------------------------- |
| `id`               | `string`    | Stable identifier                                   |
| `name`             | `string`    | Display name                                        |
| `img`              | `string`    | Path to the item's icon, relative to `images/`      |
| `source`           | `string[]?` | Where the item is obtained                          |
| `baseSellPrice`    | `number`    | Base sell price in Dinks                            |
| `buyPrice`         | `number?`   | Purchase price, if purchasable                      |
| `displayPrice`     | `number?`   | Price shown at Melvin's Catalogue                   |
| `cataloguePrice`   | `number?`   | Actual catalogue purchase price                     |
| `melvinsCatalogue` | `boolean`   | Whether the item is available in Melvin's Catalogue |
| `furnitureSet`     | `string?`   | Set the item belongs to, if any                     |

### Filters

| Method             | Signature                  | Description                                                                                       |
| ------------------ | -------------------------- | ------------------------------------------------------------------------------------------------- |
| `bySet`            | `bySet(set: string)`       | Filter to furniture belonging to the given set (case-insensitive). Items with no set never match. |
| `bySource`         | `bySource(source: string)` | Filter to furniture obtainable from the given source (case-insensitive substring match).          |
| `melvinsCatalogue` | `melvinsCatalogue()`       | Filter to furniture available in Melvin's Catalogue.                                              |

### Sorts

| Method                | Signature                                      | Default  | Description              |
| --------------------- | ---------------------------------------------- | -------- | ------------------------ |
| `sortByBaseSellPrice` | `sortByBaseSellPrice(order?: 'asc' \| 'desc')` | `'desc'` | Sort by base sell price. |

### Terminal methods

| Method              | Returns                  | Description                         |
| ------------------- | ------------------------ | ----------------------------------- |
| `.get()`            | `Furniture[]`            | All results                         |
| `.first()`          | `Furniture \| undefined` | First result                        |
| `.find(id)`         | `Furniture \| undefined` | Find by `id`                        |
| `.findByName(name)` | `Furniture \| undefined` | Case-insensitive exact name match   |
| `.search(query)`    | `Furniture[]`            | Case-insensitive partial name match |
| `.count()`          | `number`                 | Number of results                   |

### Examples

```typescript
import { furniture } from 'dinkum-data'

// Everything in Melvin's Catalogue, most valuable first
furniture().melvinsCatalogue().sortByBaseSellPrice().get()

// Everything obtainable from the Recycling Bin
furniture().bySource('Recycling Bin').get()
```

---

## Decorations

`decorations()` returns a `DecorationQuery` over all 265 items, grouped by category: paths, fences, benches, bridges, lights, statues, and more.

Many of these items also appear in [crafting recipes](/docs/recipes) or in the Furniture module above. This module is a curated index across every decoration category shown on the wiki, not a separate source of truth for those items' prices.

### Type

```typescript
interface Decoration {
  id: string
  name: string
  img: string
  category: DecorationCategory
  source?: string[]
  baseSellPrice: number
  buyPrice?: number
}
```

### Field reference

| Field           | Type                 | Description                                    |
| --------------- | -------------------- | ---------------------------------------------- |
| `id`            | `string`             | Stable identifier                              |
| `name`          | `string`             | Display name                                   |
| `img`           | `string`             | Path to the item's icon, relative to `images/` |
| `category`      | `DecorationCategory` | Decoration category (see below)                |
| `source`        | `string[]?`          | Where the item is obtained                     |
| `baseSellPrice` | `number`             | Base sell price in Dinks                       |
| `buyPrice`      | `number?`            | Purchase price, if purchasable                 |

`DecorationCategory` is a union of 16 string literals, backed by the exported `DECORATION_CATEGORIES` const array (see [TypeScript integration](/docs/typescript#enum-like-union-types) for the pattern): `'Paths & Steps'`, `'Fences & Gates'`, `'Benches'`, `'Bridges'`, `'Flags, Festoons & Arches'`, `'Flower Beds'`, `'Flowers in Pots'`, `'Ladders'`, `'Lights'`, `'Market Stalls & Miscellaneous'`, `'Pergolas'`, `'Plush'`, `'Statues'`, `'Water Fountains & Waterbeds'`, `'Winter Ice Sculpting'`, and `'Festive'`.

### Filters

| Method       | Signature                                  | Description                                                                                |
| ------------ | ------------------------------------------ | ------------------------------------------------------------------------------------------ |
| `byCategory` | `byCategory(category: DecorationCategory)` | Filter to decorations belonging to the given category.                                     |
| `bySource`   | `bySource(source: string)`                 | Filter to decorations obtainable from the given source (case-insensitive substring match). |

### Sorts

| Method                | Signature                                      | Default  | Description              |
| --------------------- | ---------------------------------------------- | -------- | ------------------------ |
| `sortByBaseSellPrice` | `sortByBaseSellPrice(order?: 'asc' \| 'desc')` | `'desc'` | Sort by base sell price. |

### Terminal methods

| Method              | Returns                   | Description                         |
| ------------------- | ------------------------- | ----------------------------------- |
| `.get()`            | `Decoration[]`            | All results                         |
| `.first()`          | `Decoration \| undefined` | First result                        |
| `.find(id)`         | `Decoration \| undefined` | Find by `id`                        |
| `.findByName(name)` | `Decoration \| undefined` | Case-insensitive exact name match   |
| `.search(query)`    | `Decoration[]`            | Case-insensitive partial name match |
| `.count()`          | `number`                  | Number of results                   |

### Examples

```typescript
import { decorations } from 'dinkum-data'

// Every statue, most valuable first
decorations().byCategory('Statues').sortByBaseSellPrice().get()

// Everything obtainable from Franklyn's Lab
decorations().bySource("Franklyn's Lab").get()
```

---

## Next steps

- See [Clothing](/docs/clothing) for wearable items with similar catalogue and set pricing
- Read [TypeScript integration](/docs/typescript) for the general enum-like union type pattern used by `DecorationCategory`
- Browse [Recipes](/docs/recipes) to see how crafting recipes relate to decoration items
