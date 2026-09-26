---
title: Food Modeller Recipes
nextjs:
  metadata:
    title: dinkum-data - Food Modeller Recipes
    description: Query Food Modeller conversions from food and crops into display furniture.
---

Access every Food Modeller conversion in Dinkum, turning food and crops into display furniture. {% .lead %}

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

| Field           | Type                  | Description                                             |
| --------------- | --------------------- | ------------------------------------------------------- |
| `id`            | `string`              | Stable identifier.                                      |
| `name`          | `string`              | Display name.                                           |
| `img`           | `string`              | Path to the display item's icon, relative to `images/`. |
| `source`        | `string[]?`           | Where the recipe is obtained.                           |
| `baseSellPrice` | `number`              | Base sell price in Dinks.                               |
| `buyPrice`      | `number?`             | Purchase price, if purchasable.                         |
| `outputCount`   | `number \| 'Varies'?` | Quantity produced per conversion.                       |
| `variants`      | `ResourceVariant[]`   | Item sets that produce this display piece.              |
| `buffs`         | `Buffs?`              | Consumable buff effects, if any.                        |

---

## Factory

```typescript
import { foodModellerRecipes } from 'dinkum-data'

foodModellerRecipes() // all 89 recipes
foodModellerRecipes(source) // wrap a pre-filtered array
```

---

## Filters

### `bySource(source: string)`

Filter to recipes obtainable from the given source (case-insensitive substring match).

```typescript
foodModellerRecipes().bySource('Food Modeller').get()
```

---

## Sorts

### `sortByBaseSellPrice(order?: 'asc' | 'desc')`

Sort by base sell price. Defaults to `'desc'` (most valuable first).

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
import { foodModellerRecipes } from 'dinkum-data'

// Everything the Food Modeller can produce, most valuable first
foodModellerRecipes().sortByBaseSellPrice().get()
```

---

## Next steps

- See [Furniture](/docs/furniture) for the module tracking the display furniture these conversions produce
- Browse [Crafting Recipes](/docs/crafting-recipes) for the module sharing this same `Recipe` type
- Read [TypeScript integration](/docs/typescript) for the full `Recipe`/`ResourceVariant` shapes
