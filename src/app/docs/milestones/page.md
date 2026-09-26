---
title: Milestones
nextjs:
  metadata:
    title: dinkum-data - Milestones
    description: Query long-term milestone achievements.
---

Every long-term milestone achievement and its level progression. {% .lead %}

---

## Types

### `Milestone`

```typescript
interface Milestone {
  id: string
  name: string
  img: string
  description: string
  levels: MilestoneLevel[]
}
```

| Field         | Type               | Description                                         |
| ------------- | ------------------ | --------------------------------------------------- |
| `id`          | `string`           | Stable identifier                                   |
| `name`        | `string`           | Display name                                        |
| `img`         | `string`           | Path to the milestone's icon, relative to `images/` |
| `description` | `string`           | Flavor text describing the milestone                |
| `levels`      | `MilestoneLevel[]` | Every level of the milestone                        |

### `MilestoneLevel`

```typescript
interface MilestoneLevel {
  level: number
  count: number
  permitPoints: number
  unit?: string
}
```

| Field          | Type      | Description                             |
| -------------- | --------- | --------------------------------------- |
| `level`        | `number`  | Level number                            |
| `count`        | `number`  | Count required to reach this level      |
| `permitPoints` | `number`  | Permit points awarded on completion     |
| `unit`         | `string?` | Unit label for the count, if applicable |

---

## Factory

```typescript
import { milestones } from 'dinkum-data'

milestones() // all 98 milestones
milestones(source) // wrap a pre-filtered array
```

---

## Methods

### `.totalPermitPoints()`

Total permit points awarded across every level of every milestone in the current result set.

```typescript
milestones().totalPermitPoints()
```

### Sorts

| Method             | Signature                                   | Default  | Description                                  |
| ------------------ | ------------------------------------------- | -------- | -------------------------------------------- |
| `sortByLevelCount` | `sortByLevelCount(order?: 'asc' \| 'desc')` | `'desc'` | Sort by number of levels, most levels first. |

---

## Terminal methods

| Method              | Returns                  | Description                         |
| ------------------- | ------------------------ | ----------------------------------- |
| `.get()`            | `Milestone[]`            | All results                         |
| `.first()`          | `Milestone \| undefined` | First result                        |
| `.find(id)`         | `Milestone \| undefined` | Find by `id`                        |
| `.findByName(name)` | `Milestone \| undefined` | Case-insensitive exact name match   |
| `.search(query)`    | `Milestone[]`            | Case-insensitive partial name match |
| `.count()`          | `number`                 | Number of results                   |

---

## Examples

```typescript
import { milestones } from 'dinkum-data'

// Total permit points available from every milestone in the game
milestones().totalPermitPoints()

// Look up by name
milestones().findByName('Alpha Hunter')
```

---

## Next steps

- See [Milestone categories](/docs/milestone-categories) for grouping these into filterable categories
- Browse [Daily milestones](/docs/daily-milestones) for the repeatable, shorter-term counterpart
- Read [core concepts](/docs/core-concepts) for the shared query builder pattern
