---
title: Trophies
nextjs:
  metadata:
    title: dinkum-data - Trophies
    description: Query competition trophies from Bug Catching and Fish Catching Competitions.
---

Access every trophy awarded from Bug Catching and Fish Catching Competitions in Dinkum. {% .lead %}

---

## Type

`Trophy` is an alias for `BaseResource`, with no extra fields.

```typescript
interface Trophy {
  id: string
  name: string
  img: string
  source?: string[]
  baseSellPrice: number
  buyPrice?: number
}
```

| Field           | Type        | Description                                       |
| --------------- | ----------- | ------------------------------------------------- |
| `id`            | `string`    | Stable identifier.                                |
| `name`          | `string`    | Display name.                                     |
| `img`           | `string`    | Path to the trophy's icon, relative to `images/`. |
| `source`        | `string[]?` | The competition the trophy is awarded from.       |
| `baseSellPrice` | `number`    | Base sell price in Dinks.                         |
| `buyPrice`      | `number?`   | Purchase price, if purchasable.                   |

Trophies have no filter methods, only the standard terminal methods below.

---

## Factory

```typescript
import { trophies } from 'dinkum-data'

trophies() // all 6 trophies
trophies(source) // wrap a pre-filtered array
```

---

## Terminal methods

| Method              | Returns               | Description                         |
| ------------------- | --------------------- | ----------------------------------- |
| `.get()`            | `Trophy[]`            | All results                         |
| `.first()`          | `Trophy \| undefined` | First result                        |
| `.find(id)`         | `Trophy \| undefined` | Find by `id`                        |
| `.findByName(name)` | `Trophy \| undefined` | Case-insensitive exact name match   |
| `.search(query)`    | `Trophy[]`            | Case-insensitive partial name match |
| `.count()`          | `number`              | Number of results                   |

---

## Examples

```typescript
import { trophies } from 'dinkum-data'

trophies().get()
trophies().findByName('Gold Bug Comp Trophy')
```

---

## Next steps

- See [Relics](/docs/relics) for another collectible resource module
- Read [core concepts](/docs/core-concepts) for the shared `QueryBase` terminal methods
