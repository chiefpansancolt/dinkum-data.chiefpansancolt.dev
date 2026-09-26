---
title: Clothing
nextjs:
  metadata:
    title: dinkum-data - Clothing
    description: Query wearable clothing by slot, type, set, and Clover's Catalogue availability.
---

Access every wearable clothing item in Dinkum across every slot, including catalogue availability and set membership. {% .lead %}

---

## Type

### `Clothing`

| Field              | Type             | Description                                                               |
| ------------------ | ---------------- | ------------------------------------------------------------------------- |
| `id`               | `string`         | Stable identifier                                                         |
| `name`             | `string`         | Display name                                                              |
| `img`              | `string`         | Path to the item's icon, relative to `images/`                            |
| `source`           | `string[]?`      | Where the item is obtained                                                |
| `baseSellPrice`    | `number`         | Base sell price in Dinks                                                  |
| `buyPrice`         | `number?`        | Purchase price, if purchasable                                            |
| `displayPrice`     | `number \| null` | Price shown at Clover's Catalogue, or `null` if unavailable               |
| `cataloguePrice`   | `number \| null` | Actual catalogue purchase price, or `null` if unavailable                 |
| `cloversCatalogue` | `boolean`        | Whether the item is available in Clover's Catalogue                       |
| `slot`             | `ClothingSlot[]` | Slots the item occupies: `'Head'`, `'Face'`, `'Body'`, `'Legs'`, `'Feet'` |
| `type`             | `string`         | Clothing type, for example `'Hood'`, `'Shirt'`, `'Boots'`                 |
| `set`              | `string`         | Set name, or an empty string if not part of a set                         |

The exact `type` values valid for each `slot` are enforced by the separate [Clothing Slots](/docs/clothing-slots) module.

---

## Factory

```typescript
import { clothing } from 'dinkum-data'

clothing() // all 532 items
clothing(source) // wrap a pre-filtered array
```

---

## Filters

| Method             | Signature                    | Description                                                       |
| ------------------ | ---------------------------- | ----------------------------------------------------------------- |
| `bySlot`           | `bySlot(slot: ClothingSlot)` | Filter to clothing that occupies the given slot.                  |
| `byType`           | `byType(type: string)`       | Filter by clothing type (case-insensitive).                       |
| `bySet`            | `bySet(set: string)`         | Filter to clothing belonging to the given set (case-insensitive). |
| `cloversCatalogue` | `cloversCatalogue()`         | Filter to clothing available in Clover's Catalogue.               |

```typescript
import { clothing } from 'dinkum-data'

clothing().bySlot('Head').get()
clothing().byType('Hood').get()
clothing().bySet('Aurora Set').get()
clothing().cloversCatalogue().get()
```

---

## Sorts

| Method                | Signature                                      | Default  | Description              |
| --------------------- | ---------------------------------------------- | -------- | ------------------------ |
| `sortByBaseSellPrice` | `sortByBaseSellPrice(order?: 'asc' \| 'desc')` | `'desc'` | Sort by base sell price. |

---

## Terminal methods

| Method              | Returns                 | Description                         |
| ------------------- | ----------------------- | ----------------------------------- |
| `.get()`            | `Clothing[]`            | All results                         |
| `.first()`          | `Clothing \| undefined` | First result                        |
| `.find(id)`         | `Clothing \| undefined` | Find by `id`                        |
| `.findByName(name)` | `Clothing \| undefined` | Case-insensitive exact name match   |
| `.search(query)`    | `Clothing[]`            | Case-insensitive partial name match |
| `.count()`          | `number`                | Number of results                   |

---

## Examples

```typescript
import { clothing } from 'dinkum-data'

// Every hat, most valuable first
clothing().bySlot('Head').sortByBaseSellPrice().get()

// Everything from the Aurora set
clothing().bySet('Aurora Set').get()

// Everything currently in Clover's Catalogue
clothing().cloversCatalogue().get()
```

---

## Next steps

- See [Clothing Slots](/docs/clothing-slots) for the slot-to-type lookup table
- Browse [Furniture](/docs/furniture) for another catalogue-priced module
- Read the [query builder](/docs/query-builder) guide for more filter patterns
