---
title: Weapons
nextjs:
  metadata:
    title: dinkum-data - Weapons
    description: Query every melee and ranged weapon.
---

Access every melee and ranged weapon in Dinkum, with their damage and source. {% .lead %}

---

## Type

### `Weapon`

| Field           | Type             | Description                                      |
| --------------- | ---------------- | ------------------------------------------------ |
| `id`            | `string`         | Stable identifier                                |
| `name`          | `string`         | Display name                                     |
| `img`           | `string`         | Path to the weapon's icon, relative to `images/` |
| `source`        | `string[]`       | Where the weapon is obtained                     |
| `baseSellPrice` | `number`         | Base sell price in Dinks                         |
| `buyPrice`      | `number?`        | Purchase price, if purchasable                   |
| `damage`        | `number \| null` | Damage dealt                                     |
| `licenceLevel`  | `number \| null` | License level required, or `null` if none        |
| `inputs`        | `Resource[]?`    | Crafting inputs                                  |
| `buyUnits`      | `BuyUnits?`      | `'Dinks'` or `'Permit Points'`, when purchasable |

`Resource` is `{ name: string; img: string; count: number }`. See [TypeScript integration](/docs/typescript) for the shared `Resource` and `BuyUnits` types.

---

## Factory

```typescript
import { weapons } from 'dinkum-data'

weapons() // all 32 weapons
weapons(source) // wrap a pre-filtered array
```

---

## Filters

| Method     | Signature                  | Description                                                                            |
| ---------- | -------------------------- | -------------------------------------------------------------------------------------- |
| `bySource` | `bySource(source: string)` | Filter to weapons obtainable from the given source (case-insensitive substring match). |

```typescript
import { weapons } from 'dinkum-data'

weapons().bySource('Ted Selly').get()
```

---

## Sorts

| Method         | Signature                               | Default  | Description     |
| -------------- | --------------------------------------- | -------- | --------------- |
| `sortByDamage` | `sortByDamage(order?: 'asc' \| 'desc')` | `'desc'` | Sort by damage. |

---

## Terminal methods

| Method              | Returns               | Description                         |
| ------------------- | --------------------- | ----------------------------------- |
| `.get()`            | `Weapon[]`            | All results                         |
| `.first()`          | `Weapon \| undefined` | First result                        |
| `.find(id)`         | `Weapon \| undefined` | Find by `id`                        |
| `.findByName(name)` | `Weapon \| undefined` | Case-insensitive exact name match   |
| `.search(query)`    | `Weapon[]`            | Case-insensitive partial name match |
| `.count()`          | `number`              | Number of results                   |

---

## Examples

```typescript
import { weapons } from 'dinkum-data'

// Everything sold by Ted Selly, strongest first
weapons().bySource('Ted Selly').sortByDamage().get()

// The strongest weapon in the game
weapons().sortByDamage().first()
```

---

## Next steps

- See [Tools](/docs/tools) for gathering and utility tools
- Browse [Equipment](/docs/equipment) for placeable and wearable equipment
- Read the [query builder](/docs/query-builder) guide for more filter patterns
