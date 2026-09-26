---
title: Clothing Slots
nextjs:
  metadata:
    title: dinkum-data - Clothing Slots
    description: Look up which clothing types belong to each clothing slot.
---

The clothing-slot taxonomy: which `Clothing.type` values are valid for each of the 5 clothing slots (`Head`, `Face`, `Body`, `Legs`, `Feet`). {% .lead %}

---

This module has no query builder, since the source data is a fixed grouped object rather than a flat, filterable list. See [core concepts](/docs/core-concepts#modules-without-a-query-builder) for why a handful of modules work this way.

## Type

### `ClothingSlotTypes`

`Record<ClothingSlot, string[]>`: an object with one array of valid `type` strings per slot.

---

## Functions

```typescript
import { clothingSlots, clothingTypesForSlot } from 'dinkum-data'
```

### `clothingSlots()`

Return the full taxonomy, grouped by slot.

```typescript
clothingSlots().Head // ['Hat', 'Scarf', 'Bow', 'Bonnet', 'Bandana', 'Hood', 'Bandage']
```

### `clothingTypesForSlot(slot: ClothingSlot)`

Return the valid `type` values for a single slot.

```typescript
clothingTypesForSlot('Feet') // ['Shoes', 'Boots', 'Flats']
```

---

## Examples

```typescript
import { clothing, clothingTypesForSlot } from 'dinkum-data'

// Build a type dropdown for the Head slot
const headTypes = clothingTypesForSlot('Head')

// Cross-reference with actual clothing data
const hats = clothing()
  .bySlot('Head')
  .get()
  .filter((c) => c.type === 'Hat')
```

---

## Next steps

- See [Clothing](/docs/clothing) for the full clothing item data
- Read the [core concepts](/docs/core-concepts) page for the other modules that skip the query builder pattern
