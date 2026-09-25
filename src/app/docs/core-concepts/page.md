---
title: Core concepts
nextjs:
  metadata:
    title: dinkum-data - Core concepts
    description: Understand the QueryBase pattern, terminal methods, and the factory function structure shared by every module.
---

Every standard module in dinkum-data follows the same pattern: once you learn it here, you can use any module without reading its docs first. {% .lead %}

---

## One factory function per module

dinkum-data is organized into roughly 40 modules, each covering one category of Dinkum data — crops, animals, furniture, tools, and so on. Every module exports a **factory function** that returns a chainable query builder:

```typescript
import { animals, crops, furniture } from 'dinkum-data'

animals() // AnimalQuery
crops() // CropQuery
furniture() // FurnitureQuery
```

Calling the factory with no arguments wraps the module's full dataset. You can also pass a pre-filtered array back in, which is how you branch a query without losing the original:

```typescript
import { crops } from 'dinkum-data'

const summerCrops = crops().bySeason('Summer').get()

// Wrap an already-filtered array to keep chaining
const cheapSummerCrops = crops(summerCrops).sortByBaseSellPrice('asc').get()
```

---

## The QueryBase pattern

Every standard query builder extends the `QueryBase<T>` abstract class. It provides six **terminal methods** that work identically across every module, plus module-specific filter and sort methods layered on top.

```typescript
abstract class QueryBase<T extends { id: string; name: string }> {
  constructor(protected readonly data: T[])

  get(): T[]
  first(): T | undefined
  find(id: string): T | undefined
  findByName(name: string): T | undefined
  search(query: string): T[]
  count(): number
}
```

The generic constraint `{ id: string; name: string }` means every item has at least an `id` and `name` field, which is what makes `find()`, `findByName()`, and `search()` work identically everywhere.

---

## Terminal methods

Terminal methods end a query chain and return results.

### `get(): T[]`

Returns all matching results as an array. This is the most common terminal method.

```typescript
import { furniture } from 'dinkum-data'

const allFurniture = furniture().get()
const catalogueOnly = furniture().melvinsCatalogue().get()
```

### `first(): T | undefined`

Returns the first result, or `undefined` if the query is empty.

```typescript
import { animals } from 'dinkum-data'

const firstDomesticable = animals().domesticable().first()
```

### `find(id: string): T | undefined`

Finds an item by its exact `id` field.

```typescript
import { tools } from 'dinkum-data'

const byId = tools().find('metal_detector')
```

### `findByName(name: string): T | undefined`

Finds an item by name using a case-insensitive exact match.

```typescript
import { fish } from 'dinkum-data'

const sturgeon = fish().findByName('Sturgeon')
```

### `search(query: string): T[]`

Filters to items whose name contains the given text, case-insensitive. Unlike `findByName`, this returns every partial match rather than a single exact one.

```typescript
import { clothing } from 'dinkum-data'

const hats = clothing().search('hat').get()
```

### `count(): number`

Returns the number of results without needing to separately measure the array.

```typescript
import { crops } from 'dinkum-data'

const summerCropCount = crops().bySeason('Summer').count()
```

---

## Immutable chaining

Every filter and sort method returns a **new instance** of that module's query builder. The original is never mutated, so you can safely branch from any point in a chain:

```typescript
import { fish } from 'dinkum-data'

const rareFish = fish().byRarity('Rare')

// These two queries are completely independent
const rareInOcean = rareFish.byBiome('Ocean').get()
const mostValuable = rareFish.sortByBaseSellPrice().first()

// The original `rareFish` query is unchanged
const allRare = rareFish.get()
```

---

## Modules without a query builder

Four modules don't follow this pattern, because their source data isn't a flat, filterable list:

- **[Calendar](/docs/calendar)** — a fixed 112-day structure, accessed via `calendar()`
- **[Daily Milestones](/docs/progression)** — a fixed set of category-grouped arrays, accessed via `dailyMilestones()`, `dailyMilestonesByCategory()`, and `allDailyMilestones()`
- **[Clothing Slots](/docs/clothing)** — a lookup table via `clothingSlots()` and `clothingTypesForSlot()`
- **[Buff Icons](/docs/buff-icons)** — a lookup table via `buffIcons()`

Each of these still returns plain, fully-typed data — they just don't have `.get()`, `.first()`, and friends, since there's nothing to filter.

---

## Putting it all together

Here's a complete example demonstrating factory functions, chaining, and terminal methods together:

```typescript
import { animals, crops, furniture } from 'dinkum-data'

// Farm animals sorted by sell price
const bestFarmAnimals = animals()
  .byType('Farm Animal')
  .sortByBaseSellPrice()
  .get()

// A single crop, looked up by name
const watermelon = crops().findByName('Watermelon')!

// Everything in Melvin's Catalogue from a specific set
const cabinFurniture = furniture().melvinsCatalogue().bySet('Cabin Set').get()

console.log(`Best farm animal: ${bestFarmAnimals[0]?.name}`)
console.log(`Watermelon sells for ${watermelon.baseSellPrice} Dinks`)
console.log(`Cabin Set has ${cabinFurniture.length} catalogue pieces`)
```

---

## Next steps

- See the [query builder](/docs/query-builder) page for filter and sort patterns shared across modules
- Learn about [TypeScript integration](/docs/typescript) for typed exports
- Browse the [Animals](/docs/animals) page for a first example module
