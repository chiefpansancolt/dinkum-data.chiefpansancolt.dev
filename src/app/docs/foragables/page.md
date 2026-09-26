---
title: Foragables
nextjs:
  metadata:
    title: dinkum-data - Foragables
    description: Query every wild-foraged plant and item by biome.
---

Access every wild-foraged plant and item in Dinkum, with the biomes they're found in. {% .lead %}

---

## Type

### `Foragable`

| Field           | Type        | Description                                    |
| --------------- | ----------- | ---------------------------------------------- |
| `id`            | `string`    | Stable identifier                              |
| `name`          | `string`    | Display name                                   |
| `img`           | `string`    | Path to the item's icon, relative to `images/` |
| `source`        | `string[]?` | The plant or object this item is foraged from  |
| `baseSellPrice` | `number`    | Base sell price in Dinks                       |
| `buyPrice`      | `number?`   | Purchase price, if purchasable                 |
| `buffs`         | `Buffs?`    | Consumable buff effects, if edible             |
| `locations`     | `Biome[]?`  | Biomes the item is found in                    |

---

## Factory

```typescript
import { foragables } from 'dinkum-data'

foragables() // all 42 items
foragables(source) // wrap a pre-filtered array
```

---

## Filters

| Method       | Signature                  | Description                                                                           |
| ------------ | -------------------------- | ------------------------------------------------------------------------------------- |
| `byLocation` | `byLocation(biome: Biome)` | Filter to foragables found in the given biome. Items with no `locations` never match. |

```typescript
import { foragables } from 'dinkum-data'

foragables().byLocation('Tropics').get()
```

---

## Terminal methods

| Method              | Returns                  | Description                         |
| ------------------- | ------------------------ | ----------------------------------- |
| `.get()`            | `Foragable[]`            | All results                         |
| `.first()`          | `Foragable \| undefined` | First result                        |
| `.find(id)`         | `Foragable \| undefined` | Find by `id`                        |
| `.findByName(name)` | `Foragable \| undefined` | Case-insensitive exact name match   |
| `.search(query)`    | `Foragable[]`            | Case-insensitive partial name match |
| `.count()`          | `number`                 | Number of results                   |

---

## Examples

```typescript
import { foragables } from 'dinkum-data'

// Everything foraged in the Tropics
foragables().byLocation('Tropics').get()

// Look up by name
foragables().findByName('Banana')
```

---

## Next steps

- See [Trees](/docs/trees) and [Flowers](/docs/flowers) for other wild-growing content
- Browse [Crops](/docs/crops) and [Seeds](/docs/seeds) for cultivated plants
- Read the [query builder](/docs/query-builder) guide for more filter patterns
