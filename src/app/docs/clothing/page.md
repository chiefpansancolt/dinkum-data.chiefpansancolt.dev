---
title: Clothing
nextjs:
  metadata:
    title: dinkum-data - Clothing
    description: Query wearable clothing by slot, type, set, and Clover's Catalogue availability.
---

Every wearable clothing item in Dinkum, across every slot, with catalogue availability and set membership. {% .lead %}

---

## Clothing

`clothing()` returns a `ClothingQuery` over all 532 items.

### Type

```typescript
interface Clothing {
  id: string
  name: string
  img: string
  source?: string[]
  baseSellPrice: number
  buyPrice?: number
  displayPrice: number | null
  cataloguePrice: number | null
  cloversCatalogue: boolean
  slot: ClothingSlot[]
  type: string
  set: string
}
```

### Field reference

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
| `type`             | `string`         | Clothing type, e.g. `'Hood'`, `'Shirt'`, `'Boots'`                        |
| `set`              | `string`         | Set name, or an empty string if not part of a set                         |

### Filters

| Method             | Signature                    | Description                                                       |
| ------------------ | ---------------------------- | ----------------------------------------------------------------- |
| `bySlot`           | `bySlot(slot: ClothingSlot)` | Filter to clothing that occupies the given slot.                  |
| `byType`           | `byType(type: string)`       | Filter by clothing type (case-insensitive).                       |
| `bySet`            | `bySet(set: string)`         | Filter to clothing belonging to the given set (case-insensitive). |
| `cloversCatalogue` | `cloversCatalogue()`         | Filter to clothing available in Clover's Catalogue.               |

### Sorts

| Method                | Signature                                      | Default  | Description              |
| --------------------- | ---------------------------------------------- | -------- | ------------------------ |
| `sortByBaseSellPrice` | `sortByBaseSellPrice(order?: 'asc' \| 'desc')` | `'desc'` | Sort by base sell price. |

### Terminal methods

| Method              | Returns                 | Description                         |
| ------------------- | ----------------------- | ----------------------------------- |
| `.get()`            | `Clothing[]`            | All results                         |
| `.first()`          | `Clothing \| undefined` | First result                        |
| `.find(id)`         | `Clothing \| undefined` | Find by `id`                        |
| `.findByName(name)` | `Clothing \| undefined` | Case-insensitive exact name match   |
| `.search(query)`    | `Clothing[]`            | Case-insensitive partial name match |
| `.count()`          | `number`                | Number of results                   |

### Examples

```typescript
import { clothing } from 'dinkum-data'

// Every hat, most valuable first
clothing().bySlot('Head').sortByBaseSellPrice().get()

// Everything from the Aurora Set
clothing().bySet('Aurora Set').get()

// Everything currently in Clover's Catalogue
clothing().cloversCatalogue().get()
```

---

## Clothing Slots

`clothingSlots()` and `clothingTypesForSlot()` expose the clothing-slot taxonomy: which `Clothing.type` values are valid for each of the 5 clothing slots (`Head`, `Face`, `Body`, `Legs`, `Feet`). This module has no query builder, since the source data is a fixed grouped object rather than a flat, filterable list. See [core concepts](/docs/core-concepts#modules-without-a-query-builder) for why a handful of modules work this way.

### Type

```typescript
type ClothingSlotTypes = Record<ClothingSlot, string[]>
```

An object with one array of valid `type` strings per slot.

### Functions

```typescript
import { clothingSlots, clothingTypesForSlot } from 'dinkum-data'
```

#### `clothingSlots()`

Returns the full taxonomy, grouped by slot.

```typescript
clothingSlots().Head // ["Hat", "Scarf", "Bow", "Bonnet", "Bandana", "Hood", "Bandage"]
```

#### `clothingTypesForSlot(slot: ClothingSlot)`

Returns the valid `type` values for a single slot.

```typescript
clothingTypesForSlot('Feet') // ["Shoes", "Boots", "Flats"]
```

### Examples

```typescript
import { clothing, clothingTypesForSlot } from 'dinkum-data'

// Build a type dropdown for the Head slot
const headTypes = clothingTypesForSlot('Head')

// Cross-reference with actual clothing data
const hats = clothing()
  .bySlot('Head')
  .get()
  .filter((c) => c.type === 'Hat')
```

---

## Next steps

- See [Furniture & decorations](/docs/furniture) for other placeable items with similar catalogue pricing
- Read [TypeScript integration](/docs/typescript) for the `ClothingSlot` and `CLOTHING_SLOTS` const array
- Browse the [query builder](/docs/query-builder) guide for more filter patterns
