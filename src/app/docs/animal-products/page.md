---
title: Animal Products
nextjs:
  metadata:
    title: dinkum-data - Animal Products
    description: Query items dropped or produced by animals.
---

Access every item dropped or produced by an animal in Dinkum, with the biomes each source animal is found in. {% .lead %}

---

## Type

```typescript
interface AnimalProduct {
  id: string
  name: string
  img: string
  source?: string[]
  baseSellPrice: number
  buyPrice?: number
  buffs?: Buffs
  locations?: Biome[]
}
```

| Field           | Type        | Description                                     |
| --------------- | ----------- | ----------------------------------------------- |
| `id`            | `string`    | Stable identifier.                              |
| `name`          | `string`    | Display name.                                   |
| `img`           | `string`    | Path to the item's icon, relative to `images/`. |
| `source`        | `string[]?` | The animal(s) that produce this item.           |
| `baseSellPrice` | `number`    | Base sell price in Dinks.                       |
| `buyPrice`      | `number?`   | Purchase price, if purchasable.                 |
| `buffs`         | `Buffs?`    | Consumable buff effects, if edible.             |
| `locations`     | `Biome[]?`  | Biomes where the source animal is found.        |

See [TypeScript integration](/docs/typescript) for the shared `Buffs` and `Biome` shapes.

---

## Factory

```typescript
import { animalProducts } from 'dinkum-data'

animalProducts() // all 36 items
animalProducts(source) // wrap a pre-filtered array
```

---

## Filters

### `byLocation(biome: Biome)`

Filter to products found in the given biome. Products with no `locations` never match.

```typescript
animalProducts().byLocation('Bushlands').get()
```

---

## Terminal methods

| Method              | Returns                      | Description                         |
| ------------------- | ---------------------------- | ----------------------------------- |
| `.get()`            | `AnimalProduct[]`            | All results                         |
| `.first()`          | `AnimalProduct \| undefined` | First result                        |
| `.find(id)`         | `AnimalProduct \| undefined` | Find by `id`                        |
| `.findByName(name)` | `AnimalProduct \| undefined` | Case-insensitive exact name match   |
| `.search(query)`    | `AnimalProduct[]`            | Case-insensitive partial name match |
| `.count()`          | `number`                     | Number of results                   |

---

## Examples

```typescript
import { animalProducts } from 'dinkum-data'

// Every animal product found in Bushlands
animalProducts().byLocation('Bushlands').get()

// Look up by name
animalProducts().findByName('Alpha Antler')
```

---

## Next steps

- See [Animals](/docs/animals) for the source animals these products come from
- Browse [Minerals](/docs/minerals) for another resource module
- Read [TypeScript integration](/docs/typescript) for the shared `Buffs` and `Biome` types
