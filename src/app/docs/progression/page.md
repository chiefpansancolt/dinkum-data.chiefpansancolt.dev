---
title: Licenses, Milestones & Skills
nextjs:
  metadata:
    title: dinkum-data - Licenses, Milestones & Skills
    description: Query purchasable licenses, long-term milestones, daily milestone tasks, and trainable skills.
---

Every way your island progresses: licenses you buy, milestones you complete, daily tasks you repeat, and skills you train. {% .lead %}

---

## Licenses

`licenses()` returns a `LicenseQuery` over all 25 purchasable licenses and their level progression, permit point costs, and unlock descriptions.

### Type

```typescript
interface License {
  id: string
  name: string
  img: string
  requirements: string
  levels: LicenseLevel[]
}

interface LicenseLevel {
  level: number
  skillLevel: number
  permitPointCost: number
  description: string
}
```

### Field reference

| Field                      | Type             | Description                                       |
| -------------------------- | ---------------- | ------------------------------------------------- |
| `id`                       | `string`         | Stable identifier                                 |
| `name`                     | `string`         | Display name                                      |
| `img`                      | `string`         | Path to the license's icon, relative to `images/` |
| `requirements`             | `string`         | What the license requires to obtain               |
| `levels`                   | `LicenseLevel[]` | Every level of the license                        |
| `levels[].level`           | `number`         | Level number                                      |
| `levels[].skillLevel`      | `number`         | Skill level required to reach this level          |
| `levels[].permitPointCost` | `number`         | Permit points required to purchase                |
| `levels[].description`     | `string`         | What this level unlocks                           |

### Methods

| Method              | Signature                                   | Description                                                                                 |
| ------------------- | ------------------------------------------- | ------------------------------------------------------------------------------------------- |
| `totalPermitPoints` | `totalPermitPoints(): number`               | Total permit points required across every level of every license in the current result set. |
| `sortByLevelCount`  | `sortByLevelCount(order?: 'asc' \| 'desc')` | Sort by number of levels. Defaults to `'desc'` (most levels first).                         |

### Terminal methods

| Method              | Returns                | Description                         |
| ------------------- | ---------------------- | ----------------------------------- |
| `.get()`            | `License[]`            | All results                         |
| `.first()`          | `License \| undefined` | First result                        |
| `.find(id)`         | `License \| undefined` | Find by `id`                        |
| `.findByName(name)` | `License \| undefined` | Case-insensitive exact name match   |
| `.search(query)`    | `License[]`            | Case-insensitive partial name match |
| `.count()`          | `number`               | Number of results                   |

### Examples

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

## Milestones

`milestones()` returns a `MilestoneQuery` over all 98 long-term milestone achievements and their level progression.

### Type

```typescript
interface Milestone {
  id: string
  name: string
  img: string
  description: string
  levels: MilestoneLevel[]
}

interface MilestoneLevel {
  level: number
  count: number
  permitPoints: number
  unit?: string
}
```

### Field reference

| Field                   | Type               | Description                                         |
| ----------------------- | ------------------ | --------------------------------------------------- |
| `id`                    | `string`           | Stable identifier                                   |
| `name`                  | `string`           | Display name                                        |
| `img`                   | `string`           | Path to the milestone's icon, relative to `images/` |
| `description`           | `string`           | Flavour text describing the milestone               |
| `levels`                | `MilestoneLevel[]` | Every level of the milestone                        |
| `levels[].level`        | `number`           | Level number                                        |
| `levels[].count`        | `number`           | Count required to reach this level                  |
| `levels[].permitPoints` | `number`           | Permit points awarded on completion                 |
| `levels[].unit`         | `string?`          | Unit label for the count, if applicable             |

### Methods

| Method              | Signature                                   | Description                                                                                  |
| ------------------- | ------------------------------------------- | -------------------------------------------------------------------------------------------- |
| `totalPermitPoints` | `totalPermitPoints(): number`               | Total permit points awarded across every level of every milestone in the current result set. |
| `sortByLevelCount`  | `sortByLevelCount(order?: 'asc' \| 'desc')` | Sort by number of levels. Defaults to `'desc'` (most levels first).                          |

### Terminal methods

| Method              | Returns                  | Description                         |
| ------------------- | ------------------------ | ----------------------------------- |
| `.get()`            | `Milestone[]`            | All results                         |
| `.first()`          | `Milestone \| undefined` | First result                        |
| `.find(id)`         | `Milestone \| undefined` | Find by `id`                        |
| `.findByName(name)` | `Milestone \| undefined` | Case-insensitive exact name match   |
| `.search(query)`    | `Milestone[]`            | Case-insensitive partial name match |
| `.count()`          | `number`                 | Number of results                   |

### Examples

```typescript
import { milestones } from 'dinkum-data'

// Total permit points available from every milestone in the game
milestones().totalPermitPoints()

// Look up by name
milestones().findByName('Alpha Hunter')
```

---

## Milestone Categories

`milestoneCategories()` returns a `MilestoneCategoryQuery` over the 9 categories used to group milestones for filtering. A milestone belongs to a category when its `id` contains the category's `id` as a substring (e.g. every fishing milestone's `id` contains `"fish"`).

### Type

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

### Terminal methods

| Method              | Returns                          | Description                         |
| ------------------- | -------------------------------- | ----------------------------------- |
| `.get()`            | `MilestoneCategory[]`            | All results                         |
| `.first()`          | `MilestoneCategory \| undefined` | First result                        |
| `.find(id)`         | `MilestoneCategory \| undefined` | Find by `id`                        |
| `.findByName(name)` | `MilestoneCategory \| undefined` | Case-insensitive exact name match   |
| `.search(query)`    | `MilestoneCategory[]`            | Case-insensitive partial name match |
| `.count()`          | `number`                         | Number of results                   |

### Examples

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

## Skills

`skills()` returns a `SkillQuery` over the 6 trainable skills in Dinkum.

### Type

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

### Terminal methods

| Method              | Returns              | Description                         |
| ------------------- | -------------------- | ----------------------------------- |
| `.get()`            | `Skill[]`            | All results                         |
| `.first()`          | `Skill \| undefined` | First result                        |
| `.find(id)`         | `Skill \| undefined` | Find by `id`                        |
| `.findByName(name)` | `Skill \| undefined` | Case-insensitive exact name match   |
| `.search(query)`    | `Skill[]`            | Case-insensitive partial name match |
| `.count()`          | `number`             | Number of results                   |

### Examples

```typescript
import { skills } from 'dinkum-data'

skills().get()
skills().findByName('Fishing')
```

---

## Daily Milestones

The repeatable daily task pool, grouped by category (Day One, Travel, NPC, Fishing, Farming, Foraging, Logging, Mining, Excavation, Bug Catching, Crafting, Hunting, Trapping, Dinks). 120 tasks are included across all categories.

This module has no query builder, since the source data is a fixed set of grouped arrays rather than one flat, filterable list — see [core concepts](/docs/core-concepts#modules-without-a-query-builder).

### Type

```typescript
interface DailyMilestone {
  id: string
  name: string
  permitPoints: number
}

interface DailyMilestones {
  dayOneMilestones: DailyMilestone[]
  travelMilestones: DailyMilestone[]
  npcMilestones: DailyMilestone[]
  fishingMilestones: DailyMilestone[]
  farmingMilestones: DailyMilestone[]
  foragingMilestones: DailyMilestone[]
  loggingMilestones: DailyMilestone[]
  miningMilestones: DailyMilestone[]
  excavationMilestones: DailyMilestone[]
  bugCatchingMilestones: DailyMilestone[]
  craftingMilestones: DailyMilestone[]
  huntingMilestones: DailyMilestone[]
  trappingMilestones: DailyMilestone[]
  dinksMilestones: DailyMilestone[]
}
```

### Functions

```typescript
import {
  allDailyMilestones,
  dailyMilestones,
  dailyMilestonesByCategory,
} from 'dinkum-data'
```

#### `dailyMilestones()`

Returns the raw daily-milestone data, grouped by category.

```typescript
dailyMilestones().fishingMilestones
```

#### `dailyMilestonesByCategory(category: keyof DailyMilestones)`

Returns the daily milestones for a single category.

```typescript
dailyMilestonesByCategory('fishingMilestones')
```

#### `allDailyMilestones()`

Returns every daily milestone across all categories, flattened into a single array.

```typescript
allDailyMilestones() // DailyMilestone[120]
```

### Examples

```typescript
import { allDailyMilestones, dailyMilestonesByCategory } from 'dinkum-data'

// All fishing-related daily tasks
dailyMilestonesByCategory('fishingMilestones')

// Total permit points available across every daily task
allDailyMilestones().reduce((sum, m) => sum + m.permitPoints, 0)
```

---

## Next steps

- See [Buildings & NPCs](/docs/world) for the NPCs (like Fletch) tied to license purchases
- Read the [core concepts](/docs/core-concepts) page for why Daily Milestones has no query builder
- Browse the [query builder](/docs/query-builder) guide for filter patterns used across the QueryBase modules on this page
