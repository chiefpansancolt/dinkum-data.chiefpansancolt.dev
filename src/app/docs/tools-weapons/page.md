---
title: Tools & Weapons
nextjs:
  metadata:
    title: dinkum-data - Tools & Weapons
    description: Query gathering and combat tools and every melee and ranged weapon.
---

Every gathering tool, combat tool, and weapon in Dinkum, with damage, license requirements, and sourcing. {% .lead %}

---

## Tools

`tools()` returns a `ToolQuery` over the 89 gathering and combat tools, with the license and source needed to unlock each.

### Type

```typescript
interface Tool {
  id: string
  name: string
  img: string
  source: string[]
  baseSellPrice: number
  buyPrice?: number
  damage: number | null
  licence: string
  shinyDiscCount?: number
  berkoniumOreCount?: number
  inputs?: Resource[]
  buyUnits?: BuyUnits
}
```

| Field               | Type             | Description                                      |
| ------------------- | ---------------- | ------------------------------------------------ |
| `damage`            | `number \| null` | Damage dealt, or `null` for non-combat tools     |
| `licence`           | `string`         | License required to buy or use the tool          |
| `shinyDiscCount`    | `number?`        | Shiny Discs required to craft                    |
| `berkoniumOreCount` | `number?`        | Berkonium Ore required to craft                  |
| `inputs`            | `Resource[]?`    | Other crafting inputs                            |
| `buyUnits`          | `BuyUnits?`      | `'Dinks'` or `'Permit Points'`, when purchasable |

### Filters & Sorts

| Method         | Signature                               | Description                                                                          |
| -------------- | --------------------------------------- | ------------------------------------------------------------------------------------ |
| `byLicence`    | `byLicence(licence: string)`            | Filter to tools that require the given license (exact match, case-insensitive).      |
| `bySource`     | `bySource(source: string)`              | Filter to tools obtainable from the given source (case-insensitive substring match). |
| `sortByDamage` | `sortByDamage(order?: 'asc' \| 'desc')` | Sort by damage. Defaults to `'desc'` (strongest first). Non-combat tools sort as 0.  |

### Examples

```typescript
import { tools } from 'dinkum-data'

// Every Mining-license tool
tools().byLicence('Mining').get()

// Strongest combat tools
tools().sortByDamage().get()
```

---

## Weapons

`weapons()` returns a `WeaponQuery` over the 32 melee and ranged weapons, with their damage and source.

### Type

```typescript
interface Weapon {
  id: string
  name: string
  img: string
  source: string[]
  baseSellPrice: number
  buyPrice?: number
  damage: number | null
  licenceLevel: number | null
  inputs?: Resource[]
  buyUnits?: BuyUnits
}
```

| Field          | Type             | Description                                      |
| -------------- | ---------------- | ------------------------------------------------ |
| `damage`       | `number \| null` | Damage dealt                                     |
| `licenceLevel` | `number \| null` | License level required, or `null` if none        |
| `inputs`       | `Resource[]?`    | Crafting inputs                                  |
| `buyUnits`     | `BuyUnits?`      | `'Dinks'` or `'Permit Points'`, when purchasable |

### Filters & Sorts

| Method         | Signature                               | Description                                                                            |
| -------------- | --------------------------------------- | -------------------------------------------------------------------------------------- |
| `bySource`     | `bySource(source: string)`              | Filter to weapons obtainable from the given source (case-insensitive substring match). |
| `sortByDamage` | `sortByDamage(order?: 'asc' \| 'desc')` | Sort by damage. Defaults to `'desc'` (strongest first).                                |

### Examples

```typescript
import { weapons } from 'dinkum-data'

// Everything sold by Ted Selly, strongest first
weapons().bySource('Ted Selly').sortByDamage().get()

// The strongest weapon in the game
weapons().sortByDamage().first()
```

---

## Terminal methods

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

- Browse [Equipment, Books, Cassettes & Vehicles](/docs/gear) for other purchasable gear
- See [TypeScript integration](/docs/typescript) for the shared `Resource` and `BuyUnits` types
- Read the [query builder](/docs/query-builder) guide for more on sort methods like `sortByDamage`
