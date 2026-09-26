---
title: Furniture
nextjs:
  metadata:
    title: dinkum-data - Furniture
    description: Query placeable furniture, including catalogue pricing and set membership.
---

Access every placeable furniture item for houses and buildings in Dinkum, including catalogue pricing and set membership. {% .lead %}

---

## Type

### `Furniture`

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

---

## Factory

```typescript
import { furniture } from 'dinkum-data'

furniture() // all 443 items
furniture(source) // wrap a pre-filtered array
```

---

## Filters

| Method             | Signature                  | Description                                                                                       |
| ------------------ | -------------------------- | ------------------------------------------------------------------------------------------------- |
| `bySet`            | `bySet(set: string)`       | Filter to furniture belonging to the given set (case-insensitive). Items with no set never match. |
| `bySource`         | `bySource(source: string)` | Filter to furniture obtainable from the given source (case-insensitive substring match).          |
| `melvinsCatalogue` | `melvinsCatalogue()`       | Filter to furniture available in Melvin's Catalogue.                                              |

```typescript
import { furniture } from 'dinkum-data'

furniture().bySet('Cabin Set').get()
furniture().bySource('Recycling Bin').get()
furniture().melvinsCatalogue().get()
```

---

## Sorts

| Method                | Signature                                      | Default  | Description              |
| --------------------- | ---------------------------------------------- | -------- | ------------------------ |
| `sortByBaseSellPrice` | `sortByBaseSellPrice(order?: 'asc' \| 'desc')` | `'desc'` | Sort by base sell price. |

---

## Terminal methods

| Method              | Returns                  | Description                         |
| ------------------- | ------------------------ | ----------------------------------- |
| `.get()`            | `Furniture[]`            | All results                         |
| `.first()`          | `Furniture \| undefined` | First result                        |
| `.find(id)`         | `Furniture \| undefined` | Find by `id`                        |
| `.findByName(name)` | `Furniture \| undefined` | Case-insensitive exact name match   |
| `.search(query)`    | `Furniture[]`            | Case-insensitive partial name match |
| `.count()`          | `number`                 | Number of results                   |

---

## Examples

```typescript
import { furniture } from 'dinkum-data'

// Everything in Melvin's Catalogue, most valuable first
furniture().melvinsCatalogue().sortByBaseSellPrice().get()

// Everything obtainable from the Recycling Bin
furniture().bySource('Recycling Bin').get()
```

---

## Next steps

- See [Decorations](/docs/decorations) for outdoor placeable items, some of which also appear here
- Browse [Clothing](/docs/clothing) for another catalogue-priced module
- Read the [query builder](/docs/query-builder) guide for more filter patterns
