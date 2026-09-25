---
title: Resources
nextjs:
  metadata:
    title: dinkum-data - Resources
    description: Query animal products, minerals, paint, relics, trophies, and processed craftables.
---

Everything from animal drops to mined minerals to processed craftables from the Crusher, Stone Grinder, and Grain Mill. {% .lead %}

---

## Animal Products

`animalProducts()` returns an `AnimalProductQuery` over the 36 items dropped or produced by animals.

### Type

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

| Field       | Type        | Description                             |
| ----------- | ----------- | --------------------------------------- |
| `source`    | `string[]?` | The animal(s) that produce this item    |
| `locations` | `Biome[]?`  | Biomes where the source animal is found |
| `buffs`     | `Buffs?`    | Consumable buff effects, if edible      |

### Filters

| Method       | Signature                  | Description                                                                         |
| ------------ | -------------------------- | ----------------------------------------------------------------------------------- |
| `byLocation` | `byLocation(biome: Biome)` | Filter to products found in the given biome. Items with no `locations` never match. |

### Examples

```typescript
import { animalProducts } from 'dinkum-data'

// Every animal product found in Bushlands
animalProducts().byLocation('Bushlands').get()

// Look up by name
animalProducts().findByName('Alpha Antler')
```

---

## Minerals

`minerals()` returns a `MineralQuery` over the 15 ores, gemstones, and other minerals.

### Type

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

| Field       | Type        | Description                                      |
| ----------- | ----------- | ------------------------------------------------ |
| `source`    | `string[]?` | How the mineral is obtained, if not simply mined |
| `locations` | `Biome[]?`  | Biomes the mineral is found in                   |

### Filters

| Method       | Signature                  | Description                                                                         |
| ------------ | -------------------------- | ----------------------------------------------------------------------------------- |
| `byLocation` | `byLocation(biome: Biome)` | Filter to minerals found in the given biome. Items with no `locations` never match. |

### Examples

```typescript
import { minerals } from 'dinkum-data'

// Every mineral found in the Island Reef
minerals().byLocation('Island Reef').get()

minerals().findByName('Berkonium Ore')
```

---

## Paint

`paint()` returns a `PaintQuery` over the 12 paint colors used for customizing buildings and vehicles. `Paint` is an alias for `BaseResource` — no extra fields beyond `id`, `name`, `img`, `source`, `baseSellPrice`, and `buyPrice`.

### Filters

| Method     | Signature                  | Description                                                                                                             |
| ---------- | -------------------------- | ----------------------------------------------------------------------------------------------------------------------- |
| `bySource` | `bySource(source: string)` | Filter to paint obtainable from the given source (case-insensitive substring match). Colors with no source never match. |

### Examples

```typescript
import { paint } from 'dinkum-data'

// Every paint color found in the Deep Mine
paint().bySource('Deep Mine').get()
```

---

## Relics

`relics()` returns a `RelicQuery` over the 15 relics dug up from junk piles and sold to John or Franklyn.

### Type

```typescript
interface Relic {
  id: string
  name: string
  img: string
  baseSellPrice: number
  buyPrice?: number
  locations: string[]
  johnsSellPrice: number
  franklynsSellPrice?: number
}
```

| Field                | Type       | Description                                     |
| -------------------- | ---------- | ----------------------------------------------- |
| `locations`          | `string[]` | Junk piles or sources the relic can be dug from |
| `johnsSellPrice`     | `number`   | Sell price at John's Goods                      |
| `franklynsSellPrice` | `number?`  | Sell price to Franklyn, if he buys it           |

### Filters & Sorts

| Method                 | Signature                                       | Description                                                                            |
| ---------------------- | ----------------------------------------------- | -------------------------------------------------------------------------------------- |
| `byLocation`           | `byLocation(location: string)`                  | Filter to relics found at the given location (exact match, case-insensitive).          |
| `sortByJohnsSellPrice` | `sortByJohnsSellPrice(order?: 'asc' \| 'desc')` | Sort by John's sell price. Defaults to `'desc'` (most valuable first).                 |
| `uniqueLocations`      | `uniqueLocations()`                             | Every distinct dig-site location across the current result set, alphabetically sorted. |

### Examples

```typescript
import { relics } from 'dinkum-data'

// Every relic from an Old Barrel, most valuable first
relics().byLocation('Old Barrel').sortByJohnsSellPrice().get()

// Build a location filter without hand-maintaining a list
relics().uniqueLocations()
// ["Car Relic", "Crab Pot", "John's Goods", "Old Barrel", "Satellite", "Wheelie Bin"]
```

---

## Trophies

`trophies()` returns a `TrophyQuery` over the 6 trophies awarded from Bug Catching and Fish Catching Competitions. `Trophy` is an alias for `BaseResource` — no filters beyond the standard terminal methods.

### Examples

```typescript
import { trophies } from 'dinkum-data'

trophies().get()
trophies().findByName('Gold Bug Comp Trophy')
```

---

## Other Craftables

`otherCraftables()` returns an `OtherCraftableQuery` over the 32 miscellaneous craftable resources that don't fit the recipe modules — processed goods from the Crusher, Stone Grinder, and Grain Mill. It uses the same shared `Recipe` type as [Recipes](/docs/recipes).

### Type

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

### Filters

| Method     | Signature                  | Description                                                                                                                 |
| ---------- | -------------------------- | --------------------------------------------------------------------------------------------------------------------------- |
| `bySource` | `bySource(source: string)` | Filter to craftables obtainable from the given source (case-insensitive substring match). Items with no source never match. |

### Examples

```typescript
import { otherCraftables } from 'dinkum-data'

// Everything produced by the Crusher
otherCraftables().bySource('Crusher').get()
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

- See [TypeScript integration](/docs/typescript) for the shared `Biome` and `BaseResource` types used across these modules
- Browse [Recipes](/docs/recipes) for the `Recipe` type shared with Other Craftables
- Read the [query builder](/docs/query-builder) guide for more on `bySource` and location-based filters
