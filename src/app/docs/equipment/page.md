---
title: Equipment
nextjs:
  metadata:
    title: dinkum-data - Equipment
    description: Query wearable and placeable equipment.
---

Access every wearable and placeable equipment item in Dinkum, including license and skill requirements and Windmill/Solar Panel compatibility. {% .lead %}

---

## Type

### `Equipment`

| Field                  | Type             | Description                                        |
| ---------------------- | ---------------- | -------------------------------------------------- |
| `id`                   | `string`         | Stable identifier                                  |
| `name`                 | `string`         | Display name                                       |
| `img`                  | `string`         | Path to the item's icon, relative to `images/`     |
| `source`               | `string[]`       | Where the item is obtained                         |
| `baseSellPrice`        | `number`         | Base sell price in Dinks                           |
| `buyPrice`             | `number?`        | Purchase price, if purchasable                     |
| `description`          | `string`         | What the item does                                 |
| `requirementLevel`     | `number \| null` | Skill or license level required, or `null` if none |
| `requirementType`      | `string?`        | Name of the license or skill required              |
| `shinyDiscCount`       | `number?`        | Shiny Discs required to craft                      |
| `berkoniumOreCount`    | `number?`        | Berkonium Ore required to craft                    |
| `windmillCompatable`   | `boolean?`       | Can be powered by a Windmill                       |
| `solarPanelCompatable` | `boolean?`       | Can be powered by a Solar Panel                    |
| `inputs`               | `Resource[]?`    | Other crafting inputs                              |
| `buyUnits`             | `BuyUnits?`      | `'Dinks'` or `'Permit Points'`, when purchasable   |

`Resource` is `{ name: string; img: string; count: number }`. See [TypeScript integration](/docs/typescript) for the shared `Resource` and `BuyUnits` types.

---

## Factory

```typescript
import { equipment } from 'dinkum-data'

equipment() // all 62 items
equipment(source) // wrap a pre-filtered array
```

---

## Filters

| Method                 | Signature                         | Description                                                                                                           |
| ---------------------- | --------------------------------- | --------------------------------------------------------------------------------------------------------------------- |
| `bySource`             | `bySource(source: string)`        | Filter to equipment obtainable from the given source (case-insensitive substring match).                              |
| `byRequirementType`    | `byRequirementType(type: string)` | Filter by the license or skill required to unlock the item (case-insensitive). Items with no requirement never match. |
| `windmillCompatible`   | `windmillCompatible()`            | Filter to equipment compatible with the Windmill.                                                                     |
| `solarPanelCompatible` | `solarPanelCompatible()`          | Filter to equipment compatible with the Solar Panel.                                                                  |

```typescript
import { equipment } from 'dinkum-data'

equipment().bySource('Crafting Table').get()
equipment().byRequirementType('Irrigation Licence').get()
equipment().windmillCompatible().get()
equipment().solarPanelCompatible().get()
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
| `.get()`            | `Equipment[]`            | All results                         |
| `.first()`          | `Equipment \| undefined` | First result                        |
| `.find(id)`         | `Equipment \| undefined` | Find by `id`                        |
| `.findByName(name)` | `Equipment \| undefined` | Case-insensitive exact name match   |
| `.search(query)`    | `Equipment[]`            | Case-insensitive partial name match |
| `.count()`          | `number`                 | Number of results                   |

---

## Examples

```typescript
import { equipment } from 'dinkum-data'

// Everything craftable at the Crafting Table
equipment().bySource('Crafting Table').get()

// Every Windmill-powered device
equipment().windmillCompatible().get()

// Most valuable equipment
equipment().sortByBaseSellPrice().get()
```

---

## Next steps

- See [Tools](/docs/tools) and [Weapons](/docs/weapons) for other gear categories
- Browse [Vehicles](/docs/vehicles) for gliders and other rideable equipment
- Read the [query builder](/docs/query-builder) guide for more filter patterns
