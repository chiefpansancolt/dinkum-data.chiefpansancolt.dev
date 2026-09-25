---
title: Equipment, Books, Cassettes & Vehicles
nextjs:
  metadata:
    title: dinkum-data - Equipment, Books, Cassettes & Vehicles
    description: Query placeable equipment, collectible books, music cassettes, and gliders and other vehicles.
---

Placeable equipment, collectible books, music cassettes, and every glider and rideable vehicle in Dinkum. {% .lead %}

---

## Equipment

`equipment()` returns an `EquipmentQuery` over the 62 wearable and placeable equipment items, including license/skill requirements and Windmill/Solar Panel compatibility.

### Type

```typescript
interface Equipment {
  id: string
  name: string
  img: string
  source: string[]
  baseSellPrice: number
  buyPrice?: number
  description: string
  requirementLevel: number | null
  requirementType?: string
  shinyDiscCount?: number
  berkoniumOreCount?: number
  windmillCompatable?: boolean
  solarPanelCompatable?: boolean
  inputs?: Resource[]
  buyUnits?: BuyUnits
}
```

| Field                  | Type             | Description                                        |
| ---------------------- | ---------------- | -------------------------------------------------- |
| `description`          | `string`         | What the item does                                 |
| `requirementLevel`     | `number \| null` | Skill or license level required, or `null` if none |
| `requirementType`      | `string?`        | Name of the license or skill required              |
| `windmillCompatable`   | `boolean?`       | Can be powered by a Windmill                       |
| `solarPanelCompatable` | `boolean?`       | Can be powered by a Solar Panel                    |

### Filters & Sorts

| Method                 | Signature                                      | Description                                                                                        |
| ---------------------- | ---------------------------------------------- | -------------------------------------------------------------------------------------------------- |
| `bySource`             | `bySource(source: string)`                     | Filter to equipment obtainable from the given source (case-insensitive substring match).           |
| `byRequirementType`    | `byRequirementType(type: string)`              | Filter by the license or skill required to unlock the item. Items with no requirement never match. |
| `windmillCompatible`   | `windmillCompatible()`                         | Filter to equipment compatible with the Windmill.                                                  |
| `solarPanelCompatible` | `solarPanelCompatible()`                       | Filter to equipment compatible with the Solar Panel.                                               |
| `sortByBaseSellPrice`  | `sortByBaseSellPrice(order?: 'asc' \| 'desc')` | Sort by base sell price. Defaults to `'desc'` (most valuable first).                               |

### Examples

```typescript
import { equipment } from 'dinkum-data'

// Everything craftable at the Crafting Table
equipment().bySource('Crafting Table').get()

// Every Windmill-powered device
equipment().windmillCompatible().get()
```

---

## Books

`books()` returns a `BookQuery` over the 6 collectible books, with every place they can be acquired and their buy/sell prices.

### Type

```typescript
interface Book {
  id: string
  name: string
  img: string
  details: BookDetail[]
}

interface BookDetail {
  aquiredFrom: string
  requirements: string
  buyingPrice: number | 'Gift'
  sellingPrice: number
}
```

| Field          | Type               | Description                                                      |
| -------------- | ------------------ | ---------------------------------------------------------------- |
| `details`      | `BookDetail[]`     | Every acquisition source for this book                           |
| `aquiredFrom`  | `string`           | Where this copy is obtained                                      |
| `requirements` | `string`           | Requirement to obtain it from this source                        |
| `buyingPrice`  | `number \| 'Gift'` | Purchase price, or `'Gift'` if it can only be received as a gift |
| `sellingPrice` | `number`           | Sell price in Dinks                                              |

Books have no filter methods — only the standard terminal methods below.

### Examples

```typescript
import { books } from 'dinkum-data'

books().get()
books().findByName("Adventurer's Journal")
```

---

## Cassettes

`cassettes()` returns a `CassetteQuery` over the 15 music cassettes and their purchase price and source.

### Type

```typescript
interface Cassette {
  id: string
  name: string
  img: string
  buyPrice: number
  buyUnits: string
  source: string[]
}
```

`buyUnits` is the currency the purchase price is in: `'Dinks'` or `'Permit Points'`.

### Filters & Sorts

| Method           | Signature                                 | Description                                                                              |
| ---------------- | ----------------------------------------- | ---------------------------------------------------------------------------------------- |
| `bySource`       | `bySource(source: string)`                | Filter to cassettes obtainable from the given source (case-insensitive substring match). |
| `sortByBuyPrice` | `sortByBuyPrice(order?: 'asc' \| 'desc')` | Sort by buy price. Defaults to `'asc'` (cheapest first).                                 |

### Examples

```typescript
import { cassettes } from 'dinkum-data'

// Every cassette sold on Jimmy's Boat, cheapest first
cassettes().bySource("Jimmy's Boat").sortByBuyPrice().get()
```

---

## Vehicles

`vehicles()` returns a `VehicleQuery` over the 27 gliders and other rideable vehicles, with their unlock requirements.

### Type

```typescript
interface Vehicle {
  id: string
  name: string
  img: string
  source: string[]
  baseSellPrice: number
  buyPrice?: number
  requirementLevel: number | null
  requirementType?: string
  shinyDiscCount?: number
  berkoniumOreCount?: number
  windmillCompatable?: boolean
  solarPanelCompatable?: boolean
  inputs?: Resource[]
}
```

Shares the same `requirementLevel`/`requirementType`/`shinyDiscCount`/`berkoniumOreCount`/`windmillCompatable`/`solarPanelCompatable` fields as Equipment above. Note: unlike Equipment, Vehicle has no `buyUnits` field — every vehicle's `buyPrice` is in Dinks.

### Filters & Sorts

| Method                | Signature                                      | Description                                                                                              |
| --------------------- | ---------------------------------------------- | -------------------------------------------------------------------------------------------------------- |
| `bySource`            | `bySource(source: string)`                     | Filter to vehicles obtainable from the given source (case-insensitive substring match).                  |
| `byRequirementType`   | `byRequirementType(type: string)`              | Filter by the license or skill required to unlock the vehicle. Vehicles with no requirement never match. |
| `sortByBaseSellPrice` | `sortByBaseSellPrice(order?: 'asc' \| 'desc')` | Sort by base sell price. Defaults to `'desc'` (most valuable first).                                     |

### Examples

```typescript
import { vehicles } from 'dinkum-data'

// Everything found in the Deep Mine
vehicles().bySource('Deep Mine').get()

// Every vehicle that needs a Vehicle Licence
vehicles().byRequirementType('Vehicle Licence').get()
```

---

## Terminal methods

Every query builder on this page shares the same terminal methods from `QueryBase`:

| Method              | Returns          | Description                         |
| ------------------- | ---------------- | ----------------------------------- |
| `.get()`            | `T[]`            | All results                         |
| `.first()`          | `T \| undefined` | First result                        |
| `.find(id)`         | `T \| undefined` | Find by `id`                        |
| `.findByName(name)` | `T \| undefined` | Case-insensitive exact name match   |
| `.search(query)`    | `T[]`            | Case-insensitive partial name match |
| `.count()`          | `number`         | Number of results                   |

---

## Next steps

- Browse [Tools & Weapons](/docs/tools-weapons) for combat and gathering gear
- See [TypeScript integration](/docs/typescript) for the shared `Resource` and `BuyUnits` types
- Read the [query builder](/docs/query-builder) guide for more on compatibility filters like `windmillCompatible`
