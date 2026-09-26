---
title: Milestone Categories
nextjs:
  metadata:
    title: dinkum-data - Milestone Categories
    description: Query the categories used to group milestones for filtering.
---

The 9 milestone categories used to group the game's long-term milestones for filtering. {% .lead %}

---

## Type

### `MilestoneCategory`

```typescript
interface MilestoneCategory {
  id: string
  name: string
}
```

| Field  | Type     | Description                                                           |
| ------ | -------- | --------------------------------------------------------------------- |
| `id`   | `string` | The substring every milestone in this category shares in its own `id` |
| `name` | `string` | Display name                                                          |

A milestone belongs to a category when its `id` contains the category's `id` as a substring. For example, every fishing milestone's `id` contains `"fish"`.

---

## Factory

```typescript
import { milestoneCategories } from 'dinkum-data'

milestoneCategories() // all 9 categories
milestoneCategories(source) // wrap a pre-filtered array
```

---

## Terminal methods

| Method              | Returns                          | Description                         |
| ------------------- | -------------------------------- | ----------------------------------- |
| `.get()`            | `MilestoneCategory[]`            | All results                         |
| `.first()`          | `MilestoneCategory \| undefined` | First result                        |
| `.find(id)`         | `MilestoneCategory \| undefined` | Find by `id`                        |
| `.findByName(name)` | `MilestoneCategory \| undefined` | Case-insensitive exact name match   |
| `.search(query)`    | `MilestoneCategory[]`            | Case-insensitive partial name match |
| `.count()`          | `number`                         | Number of results                   |

---

## Examples

```typescript
import { milestoneCategories, milestones } from 'dinkum-data'

// Build a category filter dropdown
const categories = milestoneCategories().get()

// Every milestone in the "Fishing" category
const fishing = milestoneCategories().findByName('Fishing')!
const fishingMilestones = milestones()
  .get()
  .filter((m) => m.id.includes(fishing.id))
```

---

## Next steps

- See [Milestones](/docs/milestones) for the full milestone list these categories group
- Browse [Daily milestones](/docs/daily-milestones) for the repeatable, shorter-term counterpart
- Read [core concepts](/docs/core-concepts) for the shared query builder pattern
