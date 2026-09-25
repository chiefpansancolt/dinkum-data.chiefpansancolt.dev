---
title: Recipes
nextjs:
  metadata:
    title: dinkum-data - Recipes
    description: Query cooking, crafting, Food Modeller, and sign-writing recipes with their ingredients and buffs.
---

Cooking, crafting, Food Modeller, and sign-writing recipes: 413 recipes across four modules, all built on the same shared shape. {% .lead %}

---

## The shared `Recipe` shape

All four recipe modules extend the same base `Recipe` interface:

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

`ResourceVariant` (`{ id, outputCount?, inputs: Resource[] }`) describes one buildable ingredient set for a recipe — most recipes have exactly one variant, but some have several alternate ways to craft the same output. Each `Resource` input carries its own `name`, `img`, and `count`. See [TypeScript integration](/docs/typescript) for the full `Resource`/`ResourceVariant`/`Buffs` shapes.

Every recipe module shares the same terminal methods (`get`, `first`, `find`, `findByName`, `search`, `count`) and, in every case except Food Modeller's filters, a `bySource(source: string)` filter and a `sortByBaseSellPrice(order?)` sort.

---

## Cooking Recipes

`cookingRecipes()` returns a `CookingRecipeQuery` over the 72 cookable recipes. `CookingRecipe` extends `Recipe` with cooking-specific fields:

```typescript
interface CookingRecipe extends Recipe {
  outputCount: number | 'Varies'
  cookingLocation: string[]
  sheilasSellPrice?: number
  tedsSellPrice?: number | null
  jimmysSellPrice?: number
}
```

| Field              | Type              | Description                        |
| ------------------ | ----------------- | ---------------------------------- |
| `cookingLocation`  | `string[]`        | Where the recipe can be cooked     |
| `sheilasSellPrice` | `number?`         | Sell price at Sheila's             |
| `tedsSellPrice`    | `number \| null?` | Sell price at Ted's, if he buys it |
| `jimmysSellPrice`  | `number?`         | Sell price at Jimmy's              |

### Filters

| Method       | Signature                      | Description                                                                       |
| ------------ | ------------------------------ | --------------------------------------------------------------------------------- |
| `byLocation` | `byLocation(location: string)` | Filter to recipes cookable at the given location (exact match, case-insensitive). |

### Examples

```typescript
import { cookingRecipes } from 'dinkum-data'

// Everything cookable on a Campfire, most valuable first
cookingRecipes().byLocation('Campfire').sortByBaseSellPrice().get()
```

---

## Crafting Recipes

`craftingRecipes()` returns a `CraftingRecipeQuery` over the 235 craftable recipes and their unlock sources — the largest recipe module in the package.

### Examples

```typescript
import { craftingRecipes } from 'dinkum-data'

// Everything unlocked through a Building Licence
craftingRecipes().bySource('Building Licence').get()
```

---

## Food Modeller Recipes

`foodModellerRecipes()` returns a `FoodModellerRecipeQuery` over the 89 Food Modeller conversions, which turn food and crops into display furniture.

### Examples

```typescript
import { foodModellerRecipes } from 'dinkum-data'

// Everything the Food Modeller can produce, most valuable first
foodModellerRecipes().sortByBaseSellPrice().get()
```

---

## Sign Writing Recipes

`signWritingRecipes()` returns a `SignWritingRecipeQuery` over the 17 Sign Writing recipes, each gated behind a Sign Writing Licence level stored in `source`.

### Examples

```typescript
import { signWritingRecipes } from 'dinkum-data'

// Everything unlocked at the first Sign Writing Licence level
signWritingRecipes().bySource('Sign Writing Licence Level 1').get()
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

- See [TypeScript integration](/docs/typescript) for the `Resource`, `ResourceVariant`, and `Buffs` shared types
- Browse [Resources](/docs/resources) for Other Craftables, which uses this same `Recipe` shape
- Read the [query builder](/docs/query-builder) guide for more on `bySource` and nested ingredient data
