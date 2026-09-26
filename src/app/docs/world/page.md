---
title: Buildings & NPCs
nextjs:
  metadata:
    title: dinkum-data - Buildings & NPCs
    description: Query building deeds and every resident NPC with their birthdays and gift preferences.
---

Everything you can build on your island, and everyone who might come live there. {% .lead %}

---

## Buildings

`buildings()` returns a `BuildingQuery` over all 31 deeds and collectable/movable buildings, with their construction costs.

### Type

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

### Field reference

| Field            | Type         | Description                                             |
| ---------------- | ------------ | ------------------------------------------------------- |
| `id`             | `string`     | Stable identifier                                       |
| `name`           | `string`     | Display name                                            |
| `img`            | `string`     | Path to the building's icon, relative to `images/`      |
| `deedName`       | `string`     | Name of the deed item required to place the building    |
| `size`           | `string`     | Footprint, e.g. `"6x5"`                                 |
| `npc`            | `string`     | NPC associated with the building                        |
| `npcImg`         | `string`     | Path to that NPC's portrait                             |
| `description`    | `string`     | Unlock condition or flavour text                        |
| `buildTime`      | `string`     | How many nights construction takes                      |
| `deedPrice`      | `number`     | Purchase price of the deed in Dinks                     |
| `deedType`       | `DeedType`   | `'Collectable'`, `'Movable'`, or `'Reference'`          |
| `inputs`         | `Resource[]` | Materials required to build                             |
| `operatingHours` | `string[]`   | Hours the building's associated shop or service is open |
| `daysClosed`     | `string`     | Days the building is closed, if any                     |

`Resource` is `{ name: string; img: string; count: number }`. See [TypeScript integration](/docs/typescript) for the shared type.

### Filters

| Method       | Signature                    | Description                                                                |
| ------------ | ---------------------------- | -------------------------------------------------------------------------- |
| `byDeedType` | `byDeedType(type: DeedType)` | Filter by deed type.                                                       |
| `byNPC`      | `byNPC(npc: string)`         | Filter to buildings tied to the given NPC (exact match, case-insensitive). |

### Sorts

| Method            | Signature                                  | Default | Description         |
| ----------------- | ------------------------------------------ | ------- | ------------------- |
| `sortByDeedPrice` | `sortByDeedPrice(order?: 'asc' \| 'desc')` | `'asc'` | Sort by deed price. |

### Terminal methods

| Method              | Returns                 | Description                         |
| ------------------- | ----------------------- | ----------------------------------- |
| `.get()`            | `Building[]`            | All results                         |
| `.first()`          | `Building \| undefined` | First result                        |
| `.find(id)`         | `Building \| undefined` | Find by `id`                        |
| `.findByName(name)` | `Building \| undefined` | Case-insensitive exact name match   |
| `.search(query)`    | `Building[]`            | Case-insensitive partial name match |
| `.count()`          | `number`                | Number of results                   |

### Examples

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

## NPCs

`npcs()` returns an `NPCQuery` over all 23 resident NPCs, with their occupation, move-in requirements, and food preferences.

### Type

```typescript
interface NPC {
  id: string
  name: string
  img: string
  occupation: string
  requirements: {
    visit: string
    moveIn: string
  }
  foodPreferences: {
    likes: string
    likesImg: string
    dislikes: string
    dislikesImg: string
  }
}
```

### Field reference

| Field                         | Type     | Description                                       |
| ----------------------------- | -------- | ------------------------------------------------- |
| `id`                          | `string` | Stable identifier                                 |
| `name`                        | `string` | Display name                                      |
| `img`                         | `string` | Path to the NPC's portrait, relative to `images/` |
| `occupation`                  | `string` | Their role in town                                |
| `requirements.visit`          | `string` | Requirement to visit the NPC                      |
| `requirements.moveIn`         | `string` | Requirement for the NPC to move in                |
| `foodPreferences.likes`       | `string` | Favorite food                                     |
| `foodPreferences.likesImg`    | `string` | Path to that food's icon                          |
| `foodPreferences.dislikes`    | `string` | Disliked food                                     |
| `foodPreferences.dislikesImg` | `string` | Path to that food's icon                          |

### Filters

| Method         | Signature                    | Description                                                                             |
| -------------- | ---------------------------- | --------------------------------------------------------------------------------------- |
| `byOccupation` | `byOccupation(text: string)` | Filter to NPCs whose occupation description contains the given text (case-insensitive). |

### Terminal methods

| Method              | Returns            | Description                         |
| ------------------- | ------------------ | ----------------------------------- |
| `.get()`            | `NPC[]`            | All results                         |
| `.first()`          | `NPC \| undefined` | First result                        |
| `.find(id)`         | `NPC \| undefined` | Find by `id`                        |
| `.findByName(name)` | `NPC \| undefined` | Case-insensitive exact name match   |
| `.search(query)`    | `NPC[]`            | Case-insensitive partial name match |
| `.count()`          | `number`           | Number of results                   |

### Examples

```typescript
import { npcs } from 'dinkum-data'

// Every NPC whose occupation mentions the Town Hall
npcs().byOccupation('Town Hall').get()

// Look up by name
npcs().findByName('Fletch')
```

---

## Next steps

- See [Calendar](/docs/calendar) for NPC birthdays and the events tied to them
- Read the [query builder](/docs/query-builder) guide for more filter patterns
- Browse [Licenses, milestones & skills](/docs/progression) for how NPCs like Fletch relate to licenses
