---
title: Sign Writing Recipes
nextjs:
  metadata:
    title: dinkum-data - Sign Writing Recipes
    description: Query every Sign Writing recipe and its license unlock level.
---

Access every Sign Writing recipe in Dinkum and its license unlock level. {% .lead %}

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
| `img`           | `string`              | Path to the sign's icon, relative to `images/`. |
| `source`        | `string[]?`           | License level required to unlock.               |
| `baseSellPrice` | `number`              | Base sell price in Dinks.                       |
| `buyPrice`      | `number?`             | Purchase price, if purchasable.                 |
| `outputCount`   | `number \| 'Varies'?` | Quantity produced per craft.                    |
| `variants`      | `ResourceVariant[]`   | Ingredient sets that produce this sign.         |
| `buffs`         | `Buffs?`              | Consumable buff effects, if any.                |

---

## Factory

```typescript
import { signWritingRecipes } from 'dinkum-data'

signWritingRecipes() // all 17 recipes
signWritingRecipes(source) // wrap a pre-filtered array
```

---

## Filters

### `bySource(source: string)`

Filter to recipes obtainable from the given source (case-insensitive substring match).

```typescript
signWritingRecipes().bySource('Sign Writing Licence Level 1').get()
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
import { signWritingRecipes } from 'dinkum-data'

// Everything unlocked at the first Sign Writing Licence level
signWritingRecipes().bySource('Sign Writing Licence Level 1').get()
```

---

## Next steps

- See [Licenses](/docs/licenses) for the module tracking the Sign Writing Licence itself
- Browse [Crafting Recipes](/docs/crafting-recipes) for the module sharing this same `Recipe` type
- Read [TypeScript integration](/docs/typescript) for the full `Recipe`/`ResourceVariant` shapes
