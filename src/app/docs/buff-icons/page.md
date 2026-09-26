---
title: Buff Icons
nextjs:
  metadata:
    title: dinkum-data - Buff Icons
    description: Look up the icon image path for each buff tier used across Dinkum items.
---

The icon image path for each buff tier, matching the fields of the `Buffs` type. Useful for rendering a buff's icon next to its value without hand-maintaining a separate icon map. {% .lead %}

---

## Why this module is different

This module has no `QueryBase` query builder, since the source data is a fixed object rather than a flat, filterable list. `buffIcons()` returns that object directly.

---

## Type

### `BuffIcons`

An object with one icon path per buff tier field: `length`, `healthRegenRate`, `attackLevel1` through `attackLevel3`, `charged`, `coolLevel1`, `coolLevel2`, and so on. Every tiered buff field gets a `1`/`2`/`3` suffix per level, matching how tiers are shown in-game. See [TypeScript integration](/docs/typescript) for the full `Buffs` interface these icon fields mirror.

---

## Factory

```typescript
import { buffIcons } from 'dinkum-data'

buffIcons().length // "/images/buffs/Full_Buff.png"
buffIcons().attackLevel1 // "/images/other/Attack_Buff.png"
```

---

## Examples

```typescript
import { buffIcons, cookingRecipes } from 'dinkum-data'

const icons = buffIcons()
const recipe = cookingRecipes().first()!

// Render the icon for a recipe's buff effect
if (recipe.buffs?.healthRegenRate) {
  const icon = icons.healthRegenRate
}
```

---

## Next steps

- See [TypeScript integration](/docs/typescript) for the `Buffs` interface these icon fields correspond to
- Read the [core concepts](/docs/core-concepts) page for why Buff Icons and a few other modules skip the standard `QueryBase` pattern
- Browse [Cooking Recipes](/docs/cooking-recipes) for the recipes that carry `buffs` data
