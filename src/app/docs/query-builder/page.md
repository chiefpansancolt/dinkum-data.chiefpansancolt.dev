---
title: Query builder basics
nextjs:
  metadata:
    title: dinkum-data - Query builder basics
    description: Learn how to filter, sort, and look up Dinkum data using the chainable query builder API.
---

The query builder API lets you filter, sort, and look up any module's data with readable, chainable method calls: no manual array manipulation required. {% .lead %}

---

## How it works

Every factory function (`animals()`, `crops()`, `furniture()`, and so on) returns a query builder wrapping an internal array of typed data. Calling a filter or sort method returns a **new** query builder containing only the matching items. You finish the chain with a terminal method (`get()`, `first()`, `find()`, `findByName()`, `search()`, or `count()`) to extract results.

```typescript
import { crops } from 'dinkum-data'

const results = crops()
  .bySeason('Summer') // Filter to summer-plantable crops
  .sortByBaseSellPrice() // Sort by sell price, most valuable first
  .get() // Extract the array
```

---

## Common filter patterns

Filter method names are specific to each module, but a few shapes repeat across many of them:

### Filtering by source

Many modules expose `bySource(source: string)`, a case-insensitive substring match against the item's `source` array:

```typescript
import { furniture, tools } from 'dinkum-data'

const recyclingBinFurniture = furniture().bySource('Recycling Bin').get()
const jimmysBoatTools = tools().bySource("Jimmy's Boat").get()
```

### Filtering by biome

Museum items (fish, bugs, critters) and some resources filter by `Biome`:

```typescript
import { fish, foragables } from 'dinkum-data'

const oceanFish = fish().byBiome('Ocean').get()
const desertPlants = foragables().byBiome('Desert').get()
```

### Filtering by season

Crops and seeds filter by `Season`:

```typescript
import { crops, seeds } from 'dinkum-data'

const springCrops = crops().bySeason('Spring').get()
const yearRoundSeeds = seeds().bySeason('All').get()
```

### Filtering by rarity

Museum items filter by `RarityLevel`:

```typescript
import { bugs } from 'dinkum-data'

const superRareBugs = bugs().byRarity('Super Rare').get()
```

### Filtering by category / type / slot

Modules with a grouping field expose a matching filter, such as `byCategory()` on decorations, `byType()` on animals, or `bySlot()` on clothing:

```typescript
import { decorations, animals, clothing } from 'dinkum-data'

const statues = decorations().byCategory('Statues').get()
const wildAnimals = animals().byType('Wild Animal').get()
const hats = clothing().bySlot('Head').get()
```

See each module's own docs page for its exact filter list. The field reference table on every page lists which of these patterns (and any module-specific filters) apply.

---

## Sort methods

Sort methods follow the pattern `sortByX(order?: 'asc' | 'desc')`, with a sensible default order per field. The most common is `sortByBaseSellPrice`, available on nearly every module with a `baseSellPrice` field:

```typescript
import { weapons } from 'dinkum-data'

const mostValuable = weapons().sortByBaseSellPrice().get() // 'desc' by default
const cheapest = weapons().sortByBaseSellPrice('asc').get()
```

Some modules add domain-specific sorts, like `sortByGrowthPeriod()` on crops and seeds:

```typescript
import { seeds } from 'dinkum-data'

const fastestGrowing = seeds().sortByGrowthPeriod('asc').get()
```

---

## Combining operations

### Branching from a shared base

Since every filter and sort method returns a new instance, you can branch from any intermediate query without affecting the original:

```typescript
import { fish } from 'dinkum-data'

const rareFish = fish().byRarity('Rare')

const rareInOcean = rareFish.byBiome('Ocean').get()
const mostValuable = rareFish.sortByBaseSellPrice().first()
const total = rareFish.count()

// `rareFish` itself is unchanged and still represents all rare fish
```

### Passing a custom source

Every factory function accepts an optional pre-filtered array, letting you wrap your own subset in the same query builder and keep chaining:

```typescript
import { crops, type Crop } from 'dinkum-data'

const springCrops: Crop[] = crops().bySeason('Spring').get()
const cheapSpringCrops = crops(springCrops).sortByBaseSellPrice('asc').get()
```

---

## Collecting results

### As an array

```typescript
const allCrops = crops().get() // Crop[]
const names = crops()
  .get()
  .map((c) => c.name) // string[]
```

### First item only

```typescript
const first = crops().bySeason('Winter').first() // Crop | undefined
```

### By ID, exact name, or partial name

```typescript
const byId = crops().find('watermelon') // Crop | undefined
const exact = crops().findByName('Watermelon') // Crop | undefined
const partial = crops().search('melon') // Crop[]
```

### Count

```typescript
const total = crops().bySeason('Summer').count() // number
```

---

## Common patterns

### Building a lookup table

```typescript
import { tools } from 'dinkum-data'

const toolMap = new Map(
  tools()
    .get()
    .map((t) => [t.id, t]),
)

const metalDetector = toolMap.get('metal_detector')
```

### Conditional filtering

```typescript
import { furniture, type FurnitureQuery } from 'dinkum-data'

function queryFurniture(set?: string, catalogueOnly?: boolean) {
  let query: FurnitureQuery = furniture()

  if (set) {
    query = query.bySet(set)
  }
  if (catalogueOnly) {
    query = query.melvinsCatalogue()
  }

  return query.get()
}
```

### Nested ingredient data

Recipe-shaped modules (cooking, crafting, sign writing, Food Modeller) carry a `variants` array on each recipe, listing the input items needed for each craftable output. Read it straight off the entity, no separate query needed:

```typescript
import { craftingRecipes } from 'dinkum-data'

const recipe = craftingRecipes().findByName('Rock Path')!

for (const variant of recipe.variants) {
  for (const input of variant.inputs) {
    console.log(`${input.count}x ${input.name}`)
  }
}
```

---

## Next steps

- See [TypeScript integration](/docs/typescript) for type-safe querying
- Read the [core concepts](/docs/core-concepts) page for the `QueryBase` pattern in depth
- Browse individual module pages (for example [Crops](/docs/crops), [Tools](/docs/tools)) for module-specific filters
