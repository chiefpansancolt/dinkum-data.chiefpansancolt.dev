---
title: NPCs
nextjs:
  metadata:
    title: dinkum-data - NPCs
    description: Query every resident NPC with their birthdays and gift preferences.
---

Every resident NPC in Dinkum, with their occupation, move-in requirements, and food preferences. {% .lead %}

---

## Type

### `NPC`

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

---

## Factory

```typescript
import { npcs } from 'dinkum-data'

npcs() // all 23 NPCs
npcs(source) // wrap a pre-filtered array
```

---

## Filters

| Method         | Signature                    | Description                                                                             |
| -------------- | ---------------------------- | --------------------------------------------------------------------------------------- |
| `byOccupation` | `byOccupation(text: string)` | Filter to NPCs whose occupation description contains the given text (case-insensitive). |

```typescript
import { npcs } from 'dinkum-data'

npcs().byOccupation('quest giver').get()
```

---

## Terminal methods

| Method              | Returns            | Description                         |
| ------------------- | ------------------ | ----------------------------------- |
| `.get()`            | `NPC[]`            | All results                         |
| `.first()`          | `NPC \| undefined` | First result                        |
| `.find(id)`         | `NPC \| undefined` | Find by `id`                        |
| `.findByName(name)` | `NPC \| undefined` | Case-insensitive exact name match   |
| `.search(query)`    | `NPC[]`            | Case-insensitive partial name match |
| `.count()`          | `number`           | Number of results                   |

---

## Examples

```typescript
import { npcs } from 'dinkum-data'

// Every NPC whose occupation mentions the Town Hall
npcs().byOccupation('Town Hall').get()

// Look up by name
npcs().findByName('Fletch')
```

---

## Next steps

- See [Buildings](/docs/buildings) for the buildings some NPCs are tied to
- Browse [Calendar](/docs/calendar) for these NPCs' birthdays and calendar events
- Read [core concepts](/docs/core-concepts) for the shared query builder pattern
