---
title: Paint
nextjs:
  metadata:
    title: dinkum-data - Paint
    description: Query every paint color used for customizing buildings and vehicles.
---

Access every paint color in Dinkum, used for customizing buildings and vehicles. {% .lead %}

---

## Type

`Paint` is an alias for `BaseResource`, with no extra fields.

```typescript
interface Paint {
  id: string
  name: string
  img: string
  source?: string[]
  baseSellPrice: number
  buyPrice?: number
}
```

| Field           | Type        | Description                                      |
| --------------- | ----------- | ------------------------------------------------ |
| `id`            | `string`    | Stable identifier.                               |
| `name`          | `string`    | Display name.                                    |
| `img`           | `string`    | Path to the paint's icon, relative to `images/`. |
| `source`        | `string[]?` | Where the paint is obtained.                     |
| `baseSellPrice` | `number`    | Base sell price in Dinks.                        |
| `buyPrice`      | `number?`   | Purchase price, if purchasable.                  |

---

## Factory

```typescript
import { paint } from 'dinkum-data'

paint() // all 12 colors
paint(source) // wrap a pre-filtered array
```

---

## Filters

### `bySource(source: string)`

Filter to paint obtainable from the given source (case-insensitive substring match). Colors with no source never match.

```typescript
paint().bySource('Deep Mine').get()
```

---

## Terminal methods

| Method              | Returns              | Description                         |
| ------------------- | -------------------- | ----------------------------------- |
| `.get()`            | `Paint[]`            | All results                         |
| `.first()`          | `Paint \| undefined` | First result                        |
| `.find(id)`         | `Paint \| undefined` | Find by `id`                        |
| `.findByName(name)` | `Paint \| undefined` | Case-insensitive exact name match   |
| `.search(query)`    | `Paint[]`            | Case-insensitive partial name match |
| `.count()`          | `number`             | Number of results                   |

---

## Examples

```typescript
import { paint } from 'dinkum-data'

// Every paint color found in the Deep Mine
paint().bySource('Deep Mine').get()
```

---

## Next steps

- See [Minerals](/docs/minerals) for another simple resource module
- Browse [Vehicles](/docs/vehicles) for a category paint is used to customize
- Read [TypeScript integration](/docs/typescript) for the shared `BaseResource` type
