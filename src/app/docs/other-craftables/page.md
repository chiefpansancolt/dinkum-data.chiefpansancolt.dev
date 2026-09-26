---
title: Other Craftables
nextjs:
  metadata:
    title: dinkum-data - Other Craftables
    description: Query processed goods from the Crusher, Stone Grinder, and Grain Mill.
---

Access every miscellaneous craftable resource in Dinkum that doesn't fit the recipe modules, like processed goods from the Crusher, Stone Grinder, and Grain Mill. {% .lead %}

---

## Type

This module uses the same shared `Recipe` type as [Crafting Recipes](/docs/crafting-recipes).

```typescript
interface Recipe {
  id: string
  name: string
  img: string
  source?: string[]
  baseSellPrice: number
  buyPrice?: number
  outputCount?: number | 'Varies'
  variants: ResourceVariant[]
  buffs?: Buffs
}
```

| Field           | Type                  | Description                                     |
| --------------- | --------------------- | ----------------------------------------------- |
| `id`            | `string`              | Stable identifier.                              |
| `name`          | `string`              | Display name.                                   |
| `img`           | `string`              | Path to the item's icon, relative to `images/`. |
| `source`        | `string[]?`           | The machine or process that produces it.        |
| `baseSellPrice` | `number`              | Base sell price in Dinks.                       |
| `buyPrice`      | `number?`             | Purchase price, if purchasable.                 |
| `outputCount`   | `number \| 'Varies'?` | Quantity produced per craft.                    |
| `variants`      | `ResourceVariant[]`   | Ingredient sets that produce this item.         |
| `buffs`         | `Buffs?`              | Consumable buff effects, if any.                |

See [TypeScript integration](/docs/typescript) for the shared `ResourceVariant`/`Resource`/`Buffs` shapes.

---

## Factory

```typescript
import { otherCraftables } from 'dinkum-data'

otherCraftables() // all 32 items
otherCraftables(source) // wrap a pre-filtered array
```

---

## Filters

### `bySource(source: string)`

Filter to craftables obtainable from the given source (case-insensitive substring match). Items with no source never match.

```typescript
otherCraftables().bySource('Crusher').get()
```

---

## Terminal methods

| Method              | Returns               | Description                         |
| ------------------- | --------------------- | ----------------------------------- |
| `.get()`            | `Recipe[]`            | All results                         |
| `.first()`          | `Recipe \| undefined` | First result                        |
| `.find(id)`         | `Recipe \| undefined` | Find by `id`                        |
| `.findByName(name)` | `Recipe \| undefined` | Case-insensitive exact name match   |
| `.search(query)`    | `Recipe[]`            | Case-insensitive partial name match |
| `.count()`          | `number`              | Number of results                   |

---

## Examples

```typescript
import { otherCraftables } from 'dinkum-data'

// Everything produced by the Crusher
otherCraftables().bySource('Crusher').get()
```

---

## Next steps

- See [Crafting Recipes](/docs/crafting-recipes) for the module sharing this same `Recipe` type
- Browse [Minerals](/docs/minerals) for the raw ores these craftables are processed from
- Read [TypeScript integration](/docs/typescript) for the full `Recipe`/`ResourceVariant`/`Resource` shapes
