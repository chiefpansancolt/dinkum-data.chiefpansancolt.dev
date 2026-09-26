---
title: Daily Milestones
nextjs:
  metadata:
    title: dinkum-data - Daily Milestones
    description: Access the repeatable daily task pool, grouped by category.
---

The repeatable daily task pool, grouped by category: Day One, Travel, NPC, Fishing, Farming, Foraging, Logging, Mining, Excavation, Bug Catching, Crafting, Hunting, Trapping, and Dinks. 120 tasks are included across all categories. {% .lead %}

---

This module has no `QueryBase` query builder, since the source data is a fixed set of grouped arrays rather than one flat, filterable list. See [core concepts](/docs/core-concepts#modules-without-a-query-builder) for why a handful of modules work this way.

## Types

### `DailyMilestone`

```typescript
interface DailyMilestone {
  id: string
  name: string
  permitPoints: number
}
```

| Field          | Type     | Description                         |
| -------------- | -------- | ----------------------------------- |
| `id`           | `string` | Stable identifier                   |
| `name`         | `string` | Task description                    |
| `permitPoints` | `number` | Permit points awarded on completion |

### `DailyMilestones`

An object with one array of `DailyMilestone[]` per category: `dayOneMilestones`, `travelMilestones`, `npcMilestones`, `fishingMilestones`, `farmingMilestones`, `foragingMilestones`, `loggingMilestones`, `miningMilestones`, `excavationMilestones`, `bugCatchingMilestones`, `craftingMilestones`, `huntingMilestones`, `trappingMilestones`, and `dinksMilestones`.

---

## Functions

```typescript
import {
  allDailyMilestones,
  dailyMilestones,
  dailyMilestonesByCategory,
} from 'dinkum-data'
```

### `dailyMilestones()`

Return the raw daily-milestone data, grouped by category.

```typescript
dailyMilestones().fishingMilestones
```

### `dailyMilestonesByCategory(category: keyof DailyMilestones)`

Return the daily milestones for a single category.

```typescript
dailyMilestonesByCategory('fishingMilestones')
```

### `allDailyMilestones()`

Return every daily milestone across all categories, flattened into a single array.

```typescript
allDailyMilestones() // DailyMilestone[120]
```

---

## Examples

```typescript
import { allDailyMilestones, dailyMilestonesByCategory } from 'dinkum-data'

// All fishing-related daily tasks
dailyMilestonesByCategory('fishingMilestones')

// Total permit points available across every daily task
allDailyMilestones().reduce((sum, m) => sum + m.permitPoints, 0)
```

---

## Next steps

- See [Milestones](/docs/milestones) for the long-term, non-repeatable counterpart
- Browse [Milestone categories](/docs/milestone-categories) for the related category taxonomy used by long-term milestones
- Read [core concepts](/docs/core-concepts) for why some modules skip the query builder pattern
