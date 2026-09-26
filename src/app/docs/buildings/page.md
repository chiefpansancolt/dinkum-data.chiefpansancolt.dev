---
title: Buildings
nextjs:
  metadata:
    title: dinkum-data - Buildings
    description: Query building deeds and collectable/movable buildings.
---

Every deed and collectable/movable building available in Dinkum, with construction costs and unlock conditions. {% .lead %}

---

## Type

### `Building`

```typescript
interface Building {
  id: string
  name: string
  img: string
  deedName: string
  size: string
  npc: string
  npcImg: string
  description: string
  buildTime: string
  deedPrice: number
  deedType: DeedType
  inputs: Resource[]
  operatingHours: string[]
  daysClosed: string
}
```

| Field            | Type         | Description                                             |
| ---------------- | ------------ | ------------------------------------------------------- |
| `id`             | `string`     | Stable identifier                                       |
| `name`           | `string`     | Display name                                            |
| `img`            | `string`     | Path to the building's icon, relative to `images/`      |
| `deedName`       | `string`     | Name of the deed item required to place the building    |
| `size`           | `string`     | Footprint, for example `"6x5"`                          |
| `npc`            | `string`     | NPC associated with the building                        |
| `npcImg`         | `string`     | Path to that NPC's portrait                             |
| `description`    | `string`     | Unlock condition or flavor text                         |
| `buildTime`      | `string`     | How many nights construction takes                      |
| `deedPrice`      | `number`     | Purchase price of the deed in Dinks                     |
| `deedType`       | `DeedType`   | `'Collectable'`, `'Movable'`, or `'Reference'`          |
| `inputs`         | `Resource[]` | Materials required to build                             |
| `operatingHours` | `string[]`   | Hours the building's associated shop or service is open |
| `daysClosed`     | `string`     | Days the building is closed, if any                     |

`Resource` is `{ name: string; img: string; count: number }`. See [TypeScript integration](/docs/typescript) for the shared type.

---

## Factory

```typescript
import { buildings } from 'dinkum-data'

buildings() // all 31 buildings
buildings(source) // wrap a pre-filtered array
```

---

## Filters

| Method       | Signature                    | Description                                                                |
| ------------ | ---------------------------- | -------------------------------------------------------------------------- |
| `byDeedType` | `byDeedType(type: DeedType)` | Filter by deed type.                                                       |
| `byNPC`      | `byNPC(npc: string)`         | Filter to buildings tied to the given NPC (exact match, case-insensitive). |

```typescript
import { buildings } from 'dinkum-data'

buildings().byDeedType('Collectable').get()
buildings().byNPC('Nancy').get()
```

---

## Sorts

| Method            | Signature                                  | Default | Description                         |
| ----------------- | ------------------------------------------ | ------- | ----------------------------------- |
| `sortByDeedPrice` | `sortByDeedPrice(order?: 'asc' \| 'desc')` | `'asc'` | Sort by deed price, cheapest first. |

---

## Terminal methods

| Method              | Returns                 | Description                         |
| ------------------- | ----------------------- | ----------------------------------- |
| `.get()`            | `Building[]`            | All results                         |
| `.first()`          | `Building \| undefined` | First result                        |
| `.find(id)`         | `Building \| undefined` | Find by `id`                        |
| `.findByName(name)` | `Building \| undefined` | Case-insensitive exact name match   |
| `.search(query)`    | `Building[]`            | Case-insensitive partial name match |
| `.count()`          | `number`                | Number of results                   |

---

## Examples

```typescript
import { buildings } from 'dinkum-data'

// Every Collectable building, cheapest first
buildings().byDeedType('Collectable').sortByDeedPrice().get()

// Everything tied to Nancy
buildings().byNPC('Nancy').get()

// Look up by name
buildings().findByName('Airport')
```

---

## Next steps

- See [NPCs](/docs/npcs) for the residents tied to these buildings
- Browse [Licenses](/docs/licenses) for the requirements that unlock some buildings
- Read [core concepts](/docs/core-concepts) for the shared query builder pattern
