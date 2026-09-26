---
title: Tools
nextjs:
  metadata:
    title: dinkum-data - Tools
    description: Query gathering and combat tools by license and source.
---

Access every gathering and combat tool in Dinkum, with the license and source needed to unlock each. {% .lead %}

---

## Type

### `Tool`

| Field               | Type             | Description                                      |
| ------------------- | ---------------- | ------------------------------------------------ |
| `id`                | `string`         | Stable identifier                                |
| `name`              | `string`         | Display name                                     |
| `img`               | `string`         | Path to the tool's icon, relative to `images/`   |
| `source`            | `string[]`       | Where the tool is obtained                       |
| `baseSellPrice`     | `number`         | Base sell price in Dinks                         |
| `buyPrice`          | `number?`        | Purchase price, if purchasable                   |
| `damage`            | `number \| null` | Damage dealt, or `null` for non-combat tools     |
| `licence`           | `string`         | License required to buy or use the tool          |
| `shinyDiscCount`    | `number?`        | Shiny Discs required to craft                    |
| `berkoniumOreCount` | `number?`        | Berkonium Ore required to craft                  |
| `inputs`            | `Resource[]?`    | Other crafting inputs                            |
| `buyUnits`          | `BuyUnits?`      | `'Dinks'` or `'Permit Points'`, when purchasable |

`Resource` is `{ name: string; img: string; count: number }`. See [TypeScript integration](/docs/typescript) for the shared `Resource` and `BuyUnits` types.

---

## Factory

```typescript
import { tools } from 'dinkum-data'

tools() // all 89 tools
tools(source) // wrap a pre-filtered array
```

---

## Filters

| Method      | Signature                    | Description                                                                          |
| ----------- | ---------------------------- | ------------------------------------------------------------------------------------ |
| `byLicence` | `byLicence(licence: string)` | Filter to tools that require the given license (exact match, case-insensitive).      |
| `bySource`  | `bySource(source: string)`   | Filter to tools obtainable from the given source (case-insensitive substring match). |

```typescript
import { tools } from 'dinkum-data'

tools().byLicence('Mining').get()
tools().bySource("John's Goods").get()
```

---

## Sorts

| Method         | Signature                               | Default  | Description                                                                    |
| -------------- | --------------------------------------- | -------- | ------------------------------------------------------------------------------ |
| `sortByDamage` | `sortByDamage(order?: 'asc' \| 'desc')` | `'desc'` | Sort by damage. Tools with no damage (gathering tools, for example) sort as 0. |

---

## Terminal methods

| Method              | Returns             | Description                         |
| ------------------- | ------------------- | ----------------------------------- |
| `.get()`            | `Tool[]`            | All results                         |
| `.first()`          | `Tool \| undefined` | First result                        |
| `.find(id)`         | `Tool \| undefined` | Find by `id`                        |
| `.findByName(name)` | `Tool \| undefined` | Case-insensitive exact name match   |
| `.search(query)`    | `Tool[]`            | Case-insensitive partial name match |
| `.count()`          | `number`            | Number of results                   |

---

## Examples

```typescript
import { tools } from 'dinkum-data'

// Every Mining-license tool
tools().byLicence('Mining').get()

// Strongest combat tools
tools().sortByDamage().get()

// Tools purchasable at John's Goods
tools().bySource("John's Goods").get()
```

---

## Next steps

- See [Weapons](/docs/weapons) for combat-focused gear
- Browse [Equipment](/docs/equipment) for placeable and wearable equipment
- Read the [query builder](/docs/query-builder) guide for more filter patterns
