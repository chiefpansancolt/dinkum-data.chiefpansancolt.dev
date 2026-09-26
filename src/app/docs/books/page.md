---
title: Books
nextjs:
  metadata:
    title: dinkum-data - Books
    description: Query collectible books.
---

Access every collectible book in Dinkum, with every place it can be acquired and its buy/sell prices. {% .lead %}

---

## Type

### `Book`

| Field     | Type           | Description                                    |
| --------- | -------------- | ---------------------------------------------- |
| `id`      | `string`       | Stable identifier                              |
| `name`    | `string`       | Display name                                   |
| `img`     | `string`       | Path to the book's icon, relative to `images/` |
| `details` | `BookDetail[]` | Every acquisition source for this book         |

### `BookDetail`

| Field          | Type               | Description                                                      |
| -------------- | ------------------ | ---------------------------------------------------------------- |
| `aquiredFrom`  | `string`           | Where this copy is obtained                                      |
| `requirements` | `string`           | Requirement to obtain it from this source                        |
| `buyingPrice`  | `number \| 'Gift'` | Purchase price, or `'Gift'` if it can only be received as a gift |
| `sellingPrice` | `number`           | Sell price in Dinks                                              |

---

## Factory

```typescript
import { books } from 'dinkum-data'

books() // all 6 books
books(source) // wrap a pre-filtered array
```

Books have no filter or sort methods, only the standard terminal methods below.

---

## Terminal methods

| Method              | Returns             | Description                         |
| ------------------- | ------------------- | ----------------------------------- |
| `.get()`            | `Book[]`            | All results                         |
| `.first()`          | `Book \| undefined` | First result                        |
| `.find(id)`         | `Book \| undefined` | Find by `id`                        |
| `.findByName(name)` | `Book \| undefined` | Case-insensitive exact name match   |
| `.search(query)`    | `Book[]`            | Case-insensitive partial name match |
| `.count()`          | `number`            | Number of results                   |

---

## Examples

```typescript
import { books } from 'dinkum-data'

books().get()
books().findByName("Adventurer's Journal")
```

---

## Next steps

- See [Cassettes](/docs/cassettes) for another small collectible module
- Browse [Equipment](/docs/equipment) for placeable and wearable equipment
- Read the [core concepts](/docs/core-concepts) page for how the `QueryBase` terminal methods work
