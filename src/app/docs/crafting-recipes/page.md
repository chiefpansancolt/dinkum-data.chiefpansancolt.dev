---
title: Crafting Recipes
nextjs:
  metadata:
    title: dinkum-data - Crafting Recipes
    description: Query every craftable item and its unlock source.
---

Access every craftable recipe in Dinkum and its unlock source. It's the largest recipe module in the package, with 235 recipes. {% .lead %}

---

## Type

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
| `source`        | `string[]?`           | Where the recipe is unlocked.                   |
| `baseSellPrice` | `number`              | Base sell price in Dinks.                       |
| `buyPrice`      | `number?`             | Purchase price, if purchasable.                 |
| `outputCount`   | `number \| 'Varies'?` | Quantity produced per craft.                    |
| `variants`      | `ResourceVariant[]`   | Ingredient sets that produce this item.         |
| `buffs`         | `Buffs?`              | Consumable buff effects, if any.                |

`ResourceVariant` is `{ id: string; outputCount?: number; inputs: Resource[] }`. See [TypeScript integration](/docs/typescript) for the full shape.

---

## Factory

```typescript
import { craftingRecipes } from 'dinkum-data'

craftingRecipes() // all 235 recipes
craftingRecipes(source) // wrap a pre-filtered array
```

---

## Filters

### `bySource(source: string)`

Filter to recipes obtainable from the given source (case-insensitive substring match).

```typescript
craftingRecipes().bySource('Building Licence').get()
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
import { craftingRecipes } from 'dinkum-data'

// Everything unlocked through a Building Licence
craftingRecipes().bySource('Building Licence').get()
```

---

## Next steps

- See [Other Craftables](/docs/other-craftables) for the module sharing this same `Recipe` type
- Browse [Cooking Recipes](/docs/cooking-recipes) for another recipe-shaped module
- Read [TypeScript integration](/docs/typescript) for the full `Recipe`/`ResourceVariant` shapes
