---
title: Trees
nextjs:
  metadata:
    title: dinkum-data - Trees
    description: Query every tree by biome, growth period, and regrowth timing.
---

Access every tree found across Dinkum's biomes, including the seed or foragable each grows from and their regrowth timing. {% .lead %}

---

## Type

### `Tree`

| Field          | Type                 | Description                                        |
| -------------- | -------------------- | -------------------------------------------------- |
| `id`           | `string`             | Stable identifier                                  |
| `name`         | `string`             | Display name                                       |
| `img`          | `string`             | Path to the tree's icon, relative to `images/`     |
| `seed`         | `Seed \| Foragable?` | The seed or foragable this tree grows from, if any |
| `itemsDropped` | `Resource[]`         | Items harvested from the tree                      |
| `locations`    | `Biome[]?`           | Biomes the tree is found in                        |
| `growthPeriod` | `number?`            | Days to reach maturity                             |
| `regrowth`     | `number?`            | Days between harvests once mature                  |

`Resource` is `{ name: string; img: string; count: number }`. See [TypeScript integration](/docs/typescript) for the shared type.

---

## Factory

```typescript
import { trees } from 'dinkum-data'

trees() // all 18 trees
trees(source) // wrap a pre-filtered array
```

---

## Filters

| Method       | Signature                  | Description                                                                      |
| ------------ | -------------------------- | -------------------------------------------------------------------------------- |
| `byLocation` | `byLocation(biome: Biome)` | Filter to trees found in the given biome. Trees with no `locations` never match. |

```typescript
import { trees } from 'dinkum-data'

trees().byLocation('Pine Forests').get()
```

---

## Sorts

| Method               | Signature                                     | Default | Description                                                              |
| -------------------- | --------------------------------------------- | ------- | ------------------------------------------------------------------------ |
| `sortByGrowthPeriod` | `sortByGrowthPeriod(order?: 'asc' \| 'desc')` | `'asc'` | Sort by growth period in days. Trees without a `growthPeriod` sort as 0. |

---

## Terminal methods

| Method              | Returns             | Description                         |
| ------------------- | ------------------- | ----------------------------------- |
| `.get()`            | `Tree[]`            | All results                         |
| `.first()`          | `Tree \| undefined` | First result                        |
| `.find(id)`         | `Tree \| undefined` | Find by `id`                        |
| `.findByName(name)` | `Tree \| undefined` | Case-insensitive exact name match   |
| `.search(query)`    | `Tree[]`            | Case-insensitive partial name match |
| `.count()`          | `number`            | Number of results                   |

---

## Examples

```typescript
import { trees } from 'dinkum-data'

// Every tree in Pine Forests, fastest growing first
trees().byLocation('Pine Forests').sortByGrowthPeriod().get()

// Look up by name
trees().findByName('Apple Tree')
```

---

## Next steps

- See [Foragables](/docs/foragables) and [Flowers](/docs/flowers) for other wild-growing content
- Browse [Seeds](/docs/seeds) for the seeds trees can grow from
- Read the [query builder](/docs/query-builder) guide for more filter patterns
