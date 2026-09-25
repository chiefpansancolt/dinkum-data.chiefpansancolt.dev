---
title: Animals
nextjs:
  metadata:
    title: dinkum-data - Animals
    description: Query Dinkum's wild, farm, and tameable animals by type, biome, temperament, and more.
---

Access every wild, farm, and tameable animal in Dinkum, including their drops, produce, and domestication details. {% .lead %}

## Quick start

```typescript
import { animals } from 'dinkum-data'

// All domesticable animals, most valuable first
animals().domesticable().sortByBaseSellPrice().get()

// Every aggressive animal in the Desert
animals().byTemperament('Aggressive').byHabitat('Desert').get()

// Look up by name
animals().findByName('Bush Devil')
```

## Type Definition

Each animal object has the following shape:

```typescript
interface Animal {
  id: string
  name: string
  img: string
  temperament: Temperament
  habitat?: string[]
  health?: number
  drops: Resource[]
  produces?: Resource[]
  researchReward?: number
  buyPrice?: number
  baseSellPrice?: number
  maxSellPrice?: number
  source?: string
  type: AnimalType
  domesticable?: boolean
}
```

`Resource` is `{ name: string; img: string; count: number }`.

### Field Reference

| Field            | Type                      | Description                                                               |
| ---------------- | ------------------------- | ------------------------------------------------------------------------- |
| `id`             | `string`                  | Stable identifier.                                                        |
| `name`           | `string`                  | Display name.                                                             |
| `img`            | `string`                  | Path to the animal's icon, relative to `images/`.                         |
| `temperament`    | `Temperament`             | `'Passive'`, `'Neutral'`, or `'Aggressive'`.                              |
| `habitat`        | `string[] \| undefined`   | Biomes the animal is found in; omitted for animals with no fixed habitat. |
| `health`         | `number \| undefined`     | Hit points.                                                               |
| `drops`          | `Resource[]`              | Items dropped when defeated.                                              |
| `produces`       | `Resource[] \| undefined` | Items produced over time when domesticated.                               |
| `researchReward` | `number \| undefined`     | Research points awarded on first encounter.                               |
| `buyPrice`       | `number \| undefined`     | Purchase price, for animals bought rather than caught.                    |
| `baseSellPrice`  | `number \| undefined`     | Base sell price in Dinks.                                                 |
| `maxSellPrice`   | `number \| undefined`     | Maximum sell price at full growth or quality.                             |
| `source`         | `string \| undefined`     | Where the animal is obtained, if not simply encountered in the wild.      |
| `type`           | `AnimalType`              | `'Wild Animal'`, `'Farm Animal'`, or `'Tammed Animal'`.                   |
| `domesticable`   | `boolean \| undefined`    | Whether the animal can be tamed and kept on the farm.                     |

## Query Methods

The `animals()` function returns an `AnimalQuery` instance. All filter and sort methods return a new `AnimalQuery`, so you can chain them in any order.

### Inherited Methods

These terminal methods are available on every query builder:

| Method              | Returns               | Description                                |
| ------------------- | --------------------- | ------------------------------------------ |
| `.get()`            | `Animal[]`            | Return all results as an array.            |
| `.first()`          | `Animal \| undefined` | Return the first result.                   |
| `.find(id)`         | `Animal \| undefined` | Find an animal by exact ID.                |
| `.findByName(name)` | `Animal \| undefined` | Find an animal by name (case-insensitive). |
| `.search(query)`    | `Animal[]`            | Filter to names containing the query text. |
| `.count()`          | `number`              | Return the number of results.              |

### Filter Methods

| Method          | Signature                                 | Description                                                                                |
| --------------- | ----------------------------------------- | ------------------------------------------------------------------------------------------ |
| `byType`        | `byType(type: AnimalType)`                | Filter by animal type.                                                                     |
| `byTemperament` | `byTemperament(temperament: Temperament)` | Filter by temperament.                                                                     |
| `byHabitat`     | `byHabitat(habitat: string)`              | Filter to animals found in the given habitat. Animals with no `habitat` field never match. |
| `domesticable`  | `domesticable()`                          | Filter to animals that can be domesticated.                                                |

### Sort Methods

| Method                | Signature                                      | Default  | Description                                                           |
| --------------------- | ---------------------------------------------- | -------- | --------------------------------------------------------------------- |
| `sortByBaseSellPrice` | `sortByBaseSellPrice(order?: 'asc' \| 'desc')` | `'desc'` | Sort by base sell price. Animals without a `baseSellPrice` sort as 0. |

## Examples

### All domesticable animals, most valuable first

```typescript
import { animals } from 'dinkum-data'

animals().domesticable().sortByBaseSellPrice().get()
```

### Every aggressive animal in the Desert

```typescript
const desertAggro = animals()
  .byTemperament('Aggressive')
  .byHabitat('Desert')
  .get()
```

### Wild animals with a research reward

```typescript
const researchable = animals()
  .byType('Wild Animal')
  .get()
  .filter((a) => a.researchReward !== undefined)
```

### Look up a specific animal

```typescript
const bushDevil = animals().findByName('Bush Devil')
```

## Next steps

- See [Museum: Bugs, Critters & Fish](/docs/museum) for other Dinkum wildlife
- Browse [Foragables, Trees & Flowers](/docs/foraging) for the plants animals share biomes with
- Read [Resources & Materials](/docs/resources) for what animal drops turn into once collected
