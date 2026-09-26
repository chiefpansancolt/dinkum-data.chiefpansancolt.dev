---
title: Licenses
nextjs:
  metadata:
    title: dinkum-data - Licenses
    description: Query purchasable licenses and their levels.
---

Every purchasable license and its level progression, permit point costs, and unlock descriptions. {% .lead %}

---

## Types

### `License`

```typescript
interface License {
  id: string
  name: string
  img: string
  requirements: string
  levels: LicenseLevel[]
}
```

| Field          | Type             | Description                                       |
| -------------- | ---------------- | ------------------------------------------------- |
| `id`           | `string`         | Stable identifier                                 |
| `name`         | `string`         | Display name                                      |
| `img`          | `string`         | Path to the license's icon, relative to `images/` |
| `requirements` | `string`         | What the license requires to obtain               |
| `levels`       | `LicenseLevel[]` | Every level of the license                        |

### `LicenseLevel`

```typescript
interface LicenseLevel {
  level: number
  skillLevel: number
  permitPointCost: number
  description: string
}
```

| Field             | Type     | Description                              |
| ----------------- | -------- | ---------------------------------------- |
| `level`           | `number` | Level number                             |
| `skillLevel`      | `number` | Skill level required to reach this level |
| `permitPointCost` | `number` | Permit points required to purchase       |
| `description`     | `string` | What this level unlocks                  |

---

## Factory

```typescript
import { licenses } from 'dinkum-data'

licenses() // all 25 licenses
licenses(source) // wrap a pre-filtered array
```

---

## Methods

### `.totalPermitPoints()`

Total permit points required across every level of every license in the current result set.

```typescript
licenses().totalPermitPoints()
```

### Sorts

| Method             | Signature                                   | Default  | Description                                  |
| ------------------ | ------------------------------------------- | -------- | -------------------------------------------- |
| `sortByLevelCount` | `sortByLevelCount(order?: 'asc' \| 'desc')` | `'desc'` | Sort by number of levels, most levels first. |

---

## Terminal methods

| Method              | Returns                | Description                         |
| ------------------- | ---------------------- | ----------------------------------- |
| `.get()`            | `License[]`            | All results                         |
| `.first()`          | `License \| undefined` | First result                        |
| `.find(id)`         | `License \| undefined` | Find by `id`                        |
| `.findByName(name)` | `License \| undefined` | Case-insensitive exact name match   |
| `.search(query)`    | `License[]`            | Case-insensitive partial name match |
| `.count()`          | `number`               | Number of results                   |

---

## Examples

```typescript
import { licenses } from 'dinkum-data'

// Total permit points needed to max out every license in the game
licenses().totalPermitPoints()

// The license with the most levels
licenses().sortByLevelCount().first()

// Look up by name
licenses().findByName('Mining Licence')
```

---

## Next steps

- See [Milestones](/docs/milestones) for another permit-point-earning system
- Browse [Skills](/docs/skills) for the skill levels some license levels require
- Read [core concepts](/docs/core-concepts) for the shared query builder pattern
