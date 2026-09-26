---
title: Cassettes
nextjs:
  metadata:
    title: dinkum-data - Cassettes
    description: Query music cassettes and their purchase price and source.
---

Access every music cassette in Dinkum, with its purchase price and source. {% .lead %}

---

## Type

### `Cassette`

| Field      | Type       | Description                                                       |
| ---------- | ---------- | ----------------------------------------------------------------- |
| `id`       | `string`   | Stable identifier                                                 |
| `name`     | `string`   | Display name                                                      |
| `img`      | `string`   | Path to the cassette's icon, relative to `images/`                |
| `buyPrice` | `number`   | Purchase price                                                    |
| `buyUnits` | `string`   | Currency the purchase price is in: `'Dinks'` or `'Permit Points'` |
| `source`   | `string[]` | Where the cassette is sold                                        |

---

## Factory

```typescript
import { cassettes } from 'dinkum-data'

cassettes() // all 15 cassettes
cassettes(source) // wrap a pre-filtered array
```

---

## Filters

| Method     | Signature                  | Description                                                                              |
| ---------- | -------------------------- | ---------------------------------------------------------------------------------------- |
| `bySource` | `bySource(source: string)` | Filter to cassettes obtainable from the given source (case-insensitive substring match). |

```typescript
import { cassettes } from 'dinkum-data'

cassettes().bySource("Jimmy's Boat").get()
```

---

## Sorts

| Method           | Signature                                 | Default | Description        |
| ---------------- | ----------------------------------------- | ------- | ------------------ |
| `sortByBuyPrice` | `sortByBuyPrice(order?: 'asc' \| 'desc')` | `'asc'` | Sort by buy price. |

---

## Terminal methods

| Method              | Returns                 | Description                         |
| ------------------- | ----------------------- | ----------------------------------- |
| `.get()`            | `Cassette[]`            | All results                         |
| `.first()`          | `Cassette \| undefined` | First result                        |
| `.find(id)`         | `Cassette \| undefined` | Find by `id`                        |
| `.findByName(name)` | `Cassette \| undefined` | Case-insensitive exact name match   |
| `.search(query)`    | `Cassette[]`            | Case-insensitive partial name match |
| `.count()`          | `number`                | Number of results                   |

---

## Examples

```typescript
import { cassettes } from 'dinkum-data'

// Every cassette sold on Jimmy's Boat, cheapest first
cassettes().bySource("Jimmy's Boat").sortByBuyPrice().get()
```

---

## Next steps

- See [Books](/docs/books) for another small collectible module
- Browse [Equipment](/docs/equipment) for the Boom Box these cassettes play in
- Read the [query builder](/docs/query-builder) guide for more filter patterns
