---
title: Minerals
nextjs:
  metadata:
    title: dinkum-data - Minerals
    description: Query every ore and gemstone.
---

Access every ore, gemstone, and other mineral in Dinkum, with the biomes they're found in. {% .lead %}

---

## Type

```typescript
interface Mineral {
  id: string
  name: string
  img: string
  source?: string[]
  baseSellPrice: number
  buyPrice?: number
  locations?: Biome[]
}
```

| Field           | Type        | Description                                        |
| --------------- | ----------- | -------------------------------------------------- |
| `id`            | `string`    | Stable identifier.                                 |
| `name`          | `string`    | Display name.                                      |
| `img`           | `string`    | Path to the mineral's icon, relative to `images/`. |
| `source`        | `string[]?` | How the mineral is obtained, if not simply mined.  |
| `baseSellPrice` | `number`    | Base sell price in Dinks.                          |
| `buyPrice`      | `number?`   | Purchase price, if purchasable.                    |
| `locations`     | `Biome[]?`  | Biomes the mineral is found in.                    |

---

## Factory

```typescript
import { minerals } from 'dinkum-data'

minerals() // all 15 minerals
minerals(source) // wrap a pre-filtered array
```

---

## Filters

### `byLocation(biome: Biome)`

Filter to minerals found in the given biome. Minerals with no `locations` never match.

```typescript
minerals().byLocation('Island Reef').get()
```

---

## Terminal methods

| Method              | Returns                | Description                         |
| ------------------- | ---------------------- | ----------------------------------- |
| `.get()`            | `Mineral[]`            | All results                         |
| `.first()`          | `Mineral \| undefined` | First result                        |
| `.find(id)`         | `Mineral \| undefined` | Find by `id`                        |
| `.findByName(name)` | `Mineral \| undefined` | Case-insensitive exact name match   |
| `.search(query)`    | `Mineral[]`            | Case-insensitive partial name match |
| `.count()`          | `number`               | Number of results                   |

---

## Examples

```typescript
import { minerals } from 'dinkum-data'

// Every mineral found in the Island Reef
minerals().byLocation('Island Reef').get()

// Look up by name
minerals().findByName('Berkonium Ore')
```

---

## Next steps

- See [Paint](/docs/paint) for another simple resource module
- Browse [Relics](/docs/relics) for dig-site collectibles
- Read [TypeScript integration](/docs/typescript) for the shared `Biome` type
