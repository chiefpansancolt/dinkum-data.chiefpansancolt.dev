---
title: Cooking Recipes
nextjs:
  metadata:
    title: dinkum-data - Cooking Recipes
    description: Query cookable dishes with their ingredients, buffs, and NPC sell prices.
---

Access every cookable recipe in Dinkum, with ingredients, buffs, and where each dish sells for the most. {% .lead %}

---

## Type

`CookingRecipe` extends the shared `Recipe` type with a few cooking-specific fields.

```typescript
interface CookingRecipe {
  id: string
  name: string
  img: string
  baseSellPrice: number
  outputCount: number | 'Varies'
  variants: ResourceVariant[]
  buffs?: Buffs
  cookingLocation: string[]
  sheilasSellPrice?: number
  tedsSellPrice?: number | null
  jimmysSellPrice?: number
}
```

| Field              | Type                 | Description                                     |
| ------------------ | -------------------- | ----------------------------------------------- |
| `id`               | `string`             | Stable identifier.                              |
| `name`             | `string`             | Display name.                                   |
| `img`              | `string`             | Path to the dish's icon, relative to `images/`. |
| `baseSellPrice`    | `number`             | Base sell price in Dinks.                       |
| `outputCount`      | `number \| 'Varies'` | Quantity produced per craft.                    |
| `variants`         | `ResourceVariant[]`  | Ingredient sets that produce this recipe.       |
| `buffs`            | `Buffs?`             | Consumable buff effects.                        |
| `cookingLocation`  | `string[]`           | Where the recipe can be cooked.                 |
| `sheilasSellPrice` | `number?`            | Sell price at Sheila's.                         |
| `tedsSellPrice`    | `number \| null?`    | Sell price at Ted's, if he buys it.             |
| `jimmysSellPrice`  | `number?`            | Sell price at Jimmy's.                          |

See [TypeScript integration](/docs/typescript) for the shared `Recipe`/`ResourceVariant`/`Resource` shapes this extends.

---

## Factory

```typescript
import { cookingRecipes } from 'dinkum-data'

cookingRecipes() // all 72 recipes
cookingRecipes(source) // wrap a pre-filtered array
```

---

## Filters

### `byLocation(location: string)`

Filter to recipes that can be cooked at the given location (exact match, case-insensitive).

```typescript
cookingRecipes().byLocation('Campfire').get()
```

---

## Sorts

### `sortByBaseSellPrice(order?: 'asc' | 'desc')`

Sort by base sell price. Defaults to `'desc'` (most valuable first).

---

## Terminal methods

| Method              | Returns                      | Description                         |
| ------------------- | ---------------------------- | ----------------------------------- |
| `.get()`            | `CookingRecipe[]`            | All results                         |
| `.first()`          | `CookingRecipe \| undefined` | First result                        |
| `.find(id)`         | `CookingRecipe \| undefined` | Find by `id`                        |
| `.findByName(name)` | `CookingRecipe \| undefined` | Case-insensitive exact name match   |
| `.search(query)`    | `CookingRecipe[]`            | Case-insensitive partial name match |
| `.count()`          | `number`                     | Number of results                   |

---

## Examples

```typescript
import { cookingRecipes } from 'dinkum-data'

// Everything cookable on a Campfire, most valuable first
cookingRecipes().byLocation('Campfire').sortByBaseSellPrice().get()
```

---

## Data notes

As of the Cooking Table review, Mighty Sandwich's `baseSellPrice` is a placeholder `0`. Neither its wiki page nor the Cooking Table recipe table list a real sell price for it yet.

---

## Next steps

- See [Crafting Recipes](/docs/crafting-recipes) for the module sharing the same `Recipe` base
- Browse [Food Modeller Recipes](/docs/food-modeller-recipes) for another recipe-shaped module
- Read [TypeScript integration](/docs/typescript) for the full `Recipe`/`ResourceVariant` shapes
