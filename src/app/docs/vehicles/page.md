---
title: Vehicles
nextjs:
  metadata:
    title: dinkum-data - Vehicles
    description: Query gliders and other rideable vehicles.
---

Access every glider and other rideable vehicle in Dinkum, with their unlock requirements. {% .lead %}

---

## Type

### `Vehicle`

| Field                  | Type             | Description                                        |
| ---------------------- | ---------------- | -------------------------------------------------- |
| `id`                   | `string`         | Stable identifier                                  |
| `name`                 | `string`         | Display name                                       |
| `img`                  | `string`         | Path to the vehicle's icon, relative to `images/`  |
| `source`               | `string[]`       | Where the vehicle is obtained                      |
| `baseSellPrice`        | `number`         | Base sell price in Dinks                           |
| `buyPrice`             | `number?`        | Purchase price, if purchasable                     |
| `requirementLevel`     | `number \| null` | Skill or license level required, or `null` if none |
| `requirementType`      | `string?`        | Name of the license or skill required              |
| `shinyDiscCount`       | `number?`        | Shiny Discs required to craft                      |
| `berkoniumOreCount`    | `number?`        | Berkonium Ore required to craft                    |
| `windmillCompatable`   | `boolean?`       | Can be powered by a Windmill                       |
| `solarPanelCompatable` | `boolean?`       | Can be powered by a Solar Panel                    |
| `inputs`               | `Resource[]?`    | Other crafting inputs                              |

`Resource` is `{ name: string; img: string; count: number }`. Unlike Tools, Weapons, and Equipment, `Vehicle` has no `buyUnits` field: every vehicle's `buyPrice` is in Dinks.

---

## Factory

```typescript
import { vehicles } from 'dinkum-data'

vehicles() // all 27 vehicles
vehicles(source) // wrap a pre-filtered array
```

---

## Filters

| Method              | Signature                         | Description                                                                                                                 |
| ------------------- | --------------------------------- | --------------------------------------------------------------------------------------------------------------------------- |
| `bySource`          | `bySource(source: string)`        | Filter to vehicles obtainable from the given source (case-insensitive substring match).                                     |
| `byRequirementType` | `byRequirementType(type: string)` | Filter by the license or skill required to unlock the vehicle (case-insensitive). Vehicles with no requirement never match. |

```typescript
import { vehicles } from 'dinkum-data'

vehicles().bySource('Deep Mine').get()
vehicles().byRequirementType('Vehicle Licence').get()
```

---

## Sorts

| Method                | Signature                                      | Default  | Description              |
| --------------------- | ---------------------------------------------- | -------- | ------------------------ |
| `sortByBaseSellPrice` | `sortByBaseSellPrice(order?: 'asc' \| 'desc')` | `'desc'` | Sort by base sell price. |

---

## Terminal methods

| Method              | Returns                | Description                         |
| ------------------- | ---------------------- | ----------------------------------- |
| `.get()`            | `Vehicle[]`            | All results                         |
| `.first()`          | `Vehicle \| undefined` | First result                        |
| `.find(id)`         | `Vehicle \| undefined` | Find by `id`                        |
| `.findByName(name)` | `Vehicle \| undefined` | Case-insensitive exact name match   |
| `.search(query)`    | `Vehicle[]`            | Case-insensitive partial name match |
| `.count()`          | `number`               | Number of results                   |

---

## Examples

```typescript
import { vehicles } from 'dinkum-data'

// Everything found in the Deep Mine
vehicles().bySource('Deep Mine').get()

// Every vehicle that needs a Vehicle Licence
vehicles().byRequirementType('Vehicle Licence').get()
```

---

## Next steps

- See [Tools](/docs/tools), [Weapons](/docs/weapons), and [Equipment](/docs/equipment) for other gear categories
- Browse [Licenses](/docs/licenses) for the license levels vehicles reference
- Read the [query builder](/docs/query-builder) guide for more filter patterns
