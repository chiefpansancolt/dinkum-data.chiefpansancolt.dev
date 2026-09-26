---
title: Calendar
nextjs:
  metadata:
    title: dinkum-data - Calendar
    description: Access the full 112-day Dinkum calendar with NPC birthdays and seasonal events.
---

The full Dinkum calendar: 4 seasons of 28 days each (112 days total), with every birthday and event embedded on the day it occurs. {% .lead %}

---

## Why this module is different

Unlike other modules, calendar days have no `id` or `name` field, so `CalendarQuery` does not extend `QueryBase<T>`. Instead of `find()` and `findByName()`, it addresses days by season and day number, and adds calendar-specific lookups for birthdays and events.

---

## Type

### `CalendarDay`

| Field       | Type              | Description                                       |
| ----------- | ----------------- | ------------------------------------------------- |
| `day`       | `number`          | Day of the season, 1-28                           |
| `season`    | `Season`          | `'Summer'`, `'Autumn'`, `'Winter'`, or `'Spring'` |
| `weekday`   | `Weekday`         | Day of the week                                   |
| `events`    | `CalendarEvent[]` | Events occurring on this day                      |
| `birthdays` | `Birthday[]`      | NPC birthdays occurring on this day               |

### `Birthday`

| Field       | Type     | Description                  |
| ----------- | -------- | ---------------------------- |
| `day`       | `number` | Day of the season            |
| `season`    | `Season` | Season the birthday falls in |
| `character` | `string` | NPC name                     |
| `likes`     | `string` | Their favorite gift          |

### `CalendarEvent`

| Field      | Type     | Description                            |
| ---------- | -------- | -------------------------------------- |
| `name`     | `string` | Event name                             |
| `startDay` | `number` | First day of the season the event runs |
| `endDay`   | `number` | Last day of the season the event runs  |
| `season`   | `Season` | Season the event occurs in             |
| `emoji`    | `string` | Emoji shown alongside the event name   |

---

## Factory

```typescript
import { calendar } from 'dinkum-data'

calendar() // a CalendarQuery over all 112 days
```

---

## Methods

### `get(): CalendarDay[]`

Returns every day in the calendar, in season order (Summer, Autumn, Winter, Spring).

```typescript
calendar().get() // CalendarDay[112]
```

### `count(): number`

Returns the number of days (always 112).

### `bySeason(season: Season): CalendarDay[]`

Returns every day in the given season (always 28 days).

```typescript
calendar().bySeason('Summer')
```

### `getDay(season: Season, day: number): CalendarDay | undefined`

Returns a single day by season and day number, or `undefined` if it doesn't exist.

```typescript
calendar().getDay('Summer', 22)
```

### `getAllBirthdays(): Birthday[]`

Returns every birthday across the whole calendar.

### `getAllEvents(): CalendarEvent[]`

Returns every event across the whole calendar.

### `getBirthday(character: string): Birthday | undefined`

Returns the birthday for the given character (case-insensitive), or `undefined` if not found.

```typescript
calendar().getBirthday('Fletch')
```

---

## Examples

```typescript
import { calendar } from 'dinkum-data'

// Every day in Winter
calendar().bySeason('Winter')

// What's happening on Summer day 22
calendar().getDay('Summer', 22)
// { day: 22, season: 'Summer', events: [{ name: 'Fish Catching Competition', ... }], ... }

// When is Fletch's birthday
calendar().getBirthday('Fletch')
// { day: 1, season: 'Summer', character: 'Fletch', likes: 'Bush Lime' }

// Every event in the game
calendar().getAllEvents()
```

---

## Next steps

- See the [core concepts](/docs/core-concepts) page for why Calendar and a few other modules skip the standard `QueryBase` pattern
- Browse [NPCs](/docs/npcs) for the NPC records these birthdays reference
- See [Daily Milestones](/docs/daily-milestones) for the related repeatable task pool
