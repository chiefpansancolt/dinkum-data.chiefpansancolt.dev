---
title: Weight Calculator
nextjs:
  metadata:
    title: dinkum-data - Weight Calculator
    description: Query items sold by weight for the in-game Weight Calculator.
---

Items sold by weight rather than by unit, with their price per kilogram and weight range, for the Weight Calculator. 13 items are included. {% .lead %}

---

## Type

### `WeightItem`

| Field        | Type     | Description                                    |
| ------------ | -------- | ---------------------------------------------- |
| `id`         | `string` | Stable identifier                              |
| `name`       | `string` | Display name                                   |
| `img`        | `string` | Path to the item's icon, relative to `images/` |
| `pricePerKg` | `number` | Sell price per kilogram in Dinks               |
| `minWeight`  | `number` | Minimum weight in kilograms                    |
| `maxWeight`  | `number` | Maximum weight in kilograms                    |

---

## Factory

```typescript
import { weightItems } from 'dinkum-data'

weightItems() // all 13 items
weightItems(source) // wrap a pre-filtered array
```

---

## Methods

### Sorts

| Method             | Signature                                   | Default  | Description                 |
| ------------------ | ------------------------------------------- | -------- | --------------------------- |
| `sortByPricePerKg` | `sortByPricePerKg(order?: 'asc' \| 'desc')` | `'desc'` | Sort by price per kilogram. |

### Terminal methods

| Method             | Returns                   | Description                         |
| ------------------ | ------------------------- | ----------------------------------- |
| `get()`            | `WeightItem[]`            | All results                         |
| `first()`          | `WeightItem \| undefined` | First result                        |
| `find(id)`         | `WeightItem \| undefined` | Find by `id`                        |
| `findByName(name)` | `WeightItem \| undefined` | Case-insensitive exact name match   |
| `search(query)`    | `WeightItem[]`            | Case-insensitive partial name match |
| `count()`          | `number`                  | Number of results                   |

---

## Examples

```typescript
import { weightItems } from 'dinkum-data'

// Estimate the sell value of a weighed item
const item = weightItems().findByName('Aquamarine')!
const estimate = item.pricePerKg * item.maxWeight

// Most valuable weighed items per kilogram
weightItems().sortByPricePerKg().get()
```

---

## Next steps

- See the [query builder](/docs/query-builder) guide for sort method patterns shared across modules
- Browse [Resources](/docs/resources) for related minerals and other sell-price-bearing items
- Read the [core concepts](/docs/core-concepts) page for the shared `QueryBase` terminal methods used here
