---
title: Skills
nextjs:
  metadata:
    title: dinkum-data - Skills
    description: Query trainable skills.
---

The 6 trainable skills in Dinkum. {% .lead %}

---

## Type

### `Skill`

```typescript
interface Skill {
  id: string
  name: string
  img: string
  description: string
}
```

| Field         | Type     | Description                                     |
| ------------- | -------- | ----------------------------------------------- |
| `id`          | `string` | Stable identifier                               |
| `name`        | `string` | Display name                                    |
| `img`         | `string` | Path to the skill's icon, relative to `images/` |
| `description` | `string` | What the skill governs and how to unlock it     |

---

## Factory

```typescript
import { skills } from 'dinkum-data'

skills() // all 6 skills
skills(source) // wrap a pre-filtered array
```

---

## Terminal methods

| Method              | Returns              | Description                         |
| ------------------- | -------------------- | ----------------------------------- |
| `.get()`            | `Skill[]`            | All results                         |
| `.first()`          | `Skill \| undefined` | First result                        |
| `.find(id)`         | `Skill \| undefined` | Find by `id`                        |
| `.findByName(name)` | `Skill \| undefined` | Case-insensitive exact name match   |
| `.search(query)`    | `Skill[]`            | Case-insensitive partial name match |
| `.count()`          | `number`             | Number of results                   |

---

## Examples

```typescript
import { skills } from 'dinkum-data'

skills().get()
skills().findByName('Fishing')
```

---

## Next steps

- See [Licenses](/docs/licenses) for the license levels these skills gate
- Browse [Milestones](/docs/milestones) for long-term achievements tied to skill activities
- Read [core concepts](/docs/core-concepts) for the shared query builder pattern
