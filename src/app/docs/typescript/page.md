---
title: TypeScript integration
nextjs:
  metadata:
    title: dinkum-data - TypeScript integration
    description: Use the full TypeScript type system with typed exports and shared base types across every module.
---

dinkum-data ships with complete TypeScript declarations: every function, type, and interface is fully typed with no additional packages required. {% .lead %}

---

## Importing types

All types are exported from the main entry point. Use `import type` when you only need the type for annotations:

```typescript
import type {
  Animal,
  Building,
  Clothing,
  Crop,
  Decoration,
  Furniture,
  Recipe,
  Tool,
  Weapon,
} from 'dinkum-data'
```

You can also import types alongside runtime values:

```typescript
import { crops, type Crop } from 'dinkum-data'

function summarize(crop: Crop): string {
  return `${crop.name}: ${crop.baseSellPrice} Dinks`
}

const watermelon = crops().findByName('Watermelon')!
console.log(summarize(watermelon))
```

---

## Common shared types

Several base interfaces are shared across nearly every module. They live in `src/types/common.ts` and are re-exported from the main package.

### `Base`

```typescript
interface Base {
  id: string
  name: string
  img: string
}
```

The minimal shape every entity extends: a stable identifier, display name, and icon path.

### `BaseResource`

```typescript
interface BaseResource extends Base {
  source?: string[]
  baseSellPrice: number
  buyPrice?: number
}
```

Extends `Base` with pricing and sourcing. Most modules' item type (`Crop`, `Furniture`, `Tool`, `Weapon`, and so on) extends `BaseResource` directly, or adds a handful of module-specific fields on top of it.

### `PediaItem`

```typescript
interface PediaItem extends BaseResource {
  biome: Biome[]
  timeFound: TimePeriod[]
  seasons: Season[]
  rarity: RarityLevel
}
```

The shape shared by every Museum-donatable collectible: `Bug`, `Critter`, and `Fish` all extend this.

### `Resource` and `ResourceVariant`

```typescript
interface Resource {
  name: string
  img: string
  count: number
}

interface ResourceVariant {
  id: string
  outputCount?: number
  inputs: Resource[]
}
```

Used by every recipe-shaped module (cooking, crafting, sign writing, Food Modeller) to describe a crafting input and one buildable variant of a recipe.

### `Buffs`

```typescript
interface Buffs {
  length: number
  healthRegenRate?: number
  healthMax?: number
  staminaRegenRate?: number
  staminaMax?: number
  attackLevel?: number
  defenseLevel?: number
  experienceLevel?: number
  fishLevel?: number
  foragingLevel?: number
  miningLevel?: number
  speedLevel?: number
  swimmingLevel?: number
  charged?: boolean
  diligent?: boolean
  sleepless?: boolean
  fastHealthTickSpeedLevel?: number
  coolLevel?: number
}
```

Consumable buff effects, attached to food, drinks, and some equipment.

### `BuyUnits`

```typescript
type BuyUnits = 'Dinks' | 'Permit Points'
```

The currency an item's `buyPrice` is denominated in. Present on any module where an item might be purchased with Permit Points instead of Dinks (some tools, cassettes, equipment).

---

## Enum-like union types

Several fields are typed as a union of string literals, each backed by an exported `const` array of the same values (so you can iterate the valid options at runtime, not just check them at compile time):

```typescript
import { BIOMES, type Biome } from 'dinkum-data'

BIOMES // readonly ['Beach', 'Bushlands', 'Plains', ..., 'Great Bite']
type Biome = (typeof BIOMES)[number]
```

| Const array             | Type                 | Used by                                            |
| ----------------------- | -------------------- | -------------------------------------------------- |
| `BIOMES`                | `Biome`              | Museum items, foragables, minerals, some resources |
| `SEASONS`               | `Season`             | Crops, seeds, calendar                             |
| `TIME_PERIODS`          | `TimePeriod`         | Museum items (time of day found)                   |
| `RARITY_LEVELS`         | `RarityLevel`        | Museum items                                       |
| `WEEKDAYS`              | `Weekday`            | Calendar days                                      |
| `TEMPERAMENTS`          | `Temperament`        | Animals                                            |
| `ANIMAL_TYPES`          | `AnimalType`         | Animals (wild, farm, tamed)                        |
| `DEED_TYPES`            | `DeedType`           | Buildings                                          |
| `LICENSE_TYPES`         | `LicenseType`        | Licenses                                           |
| `CLOTHING_SLOTS`        | `ClothingSlot`       | Clothing, clothing slots                           |
| `DECORATION_CATEGORIES` | `DecorationCategory` | Decorations                                        |

Using the exported const array to build a filter dropdown, for example:

```typescript
import { RARITY_LEVELS } from 'dinkum-data'

RARITY_LEVELS.map((rarity) => ({ label: rarity, value: rarity }))
```

---

## Query builder generics

Every query builder class extends `QueryBase<T>`, constrained to entities with at least `id` and `name`:

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

Module-specific query classes (`CropQuery`, `FurnitureQuery`, `ToolQuery`, and so on) extend this with typed filter and sort methods, each returning `this` module's own query type for chaining:

```typescript
class CropQuery extends QueryBase<Crop> {
  bySeason(season: Season): CropQuery
  sortByBaseSellPrice(order?: 'asc' | 'desc'): CropQuery
}
```

You rarely need to reference these query classes directly, but they're exported if you want to type a function parameter or return value:

```typescript
import { crops, type CropQuery } from 'dinkum-data'

function filterBySeason(query: CropQuery, season: string) {
  return query.bySeason(season as Parameters<CropQuery['bySeason']>[0])
}
```

---

## Next steps

- See [Image assets](/docs/images) for the `img` field shape shared by every entity
- Read the [core concepts](/docs/core-concepts) page for how the query builder pattern uses these types
- Browse the [query builder](/docs/query-builder) guide for filter patterns built on these types
