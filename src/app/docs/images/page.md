---
title: Image assets
nextjs:
  metadata:
    title: dinkum-data - Image assets
    description: Access 2,000+ bundled PNG image assets for every item in Dinkum, referenced via a relative path on the data itself.
---

The package bundles a PNG image asset for nearly every item: accessible via a relative path stored right on the data. {% .lead %}

---

## How images work

Every entity that extends `Base` (which is almost everything in the package) carries an `img` field: a path relative to the package root, already prefixed with `/images/`.

```typescript
import { crops } from 'dinkum-data'

const watermelon = crops().findByName('Watermelon')!
watermelon.img
// "/images/resources/crops/Watermelon.png"
```

Unlike some other data packages, dinkum-data's items don't have per-upgrade-level image sets: each item has exactly one `img` field. A handful of recipe-shaped modules also embed image paths on their nested `inputs` (ingredients), since each ingredient carries its own icon:

```typescript
import { craftingRecipes } from 'dinkum-data'

const recipe = craftingRecipes().findByName('Rock Path')!
recipe.img // the output item's icon
recipe.variants[0].inputs[0].img // the first ingredient's icon
```

---

## Image organization

Images are organized under `images/<category>/`, generally mirroring the data folder structure:

| Folder              | Contains                                                               |
| ------------------- | ---------------------------------------------------------------------- |
| `animals/`          | Animal sprites and their drops                                         |
| `buffs/`            | Buff and status effect icons                                           |
| `buildings/`        | Building and deed icons                                                |
| `clothing/`         | Clothing item icons                                                    |
| `decorations/`      | Placeable world decoration icons                                       |
| `flowers/`          | Flower icons                                                           |
| `furniture/`        | Furniture icons                                                        |
| `gearAndEquipment/` | Books, cassettes, equipment, tools, vehicles, weapons                  |
| `licences/`         | License icons                                                          |
| `milestones/`       | Milestone icons                                                        |
| `npcs/`             | NPC portraits                                                          |
| `other/`            | Miscellaneous icons                                                    |
| `pedia/`            | Fish, bug, and critter icons                                           |
| `recipes/`          | Cooking, crafting, Food Modeller, and sign-writing recipe icons        |
| `resources/`        | Animal products, crops, foragables, minerals, and other resource icons |
| `trees/`            | Tree icons                                                             |
| `weightItems/`      | Weight-calculator item icons                                           |

Folder names match the `img` path exactly, case-sensitive.

---

## Accessing images

Images are located at `node_modules/dinkum-data/images/...` and are also exposed through the package's `dinkum-data/images/*` export map entry, so bundlers can resolve them directly.

### In a bundler (Webpack, Vite, etc.)

Combine the package name with the relative path from the data:

```typescript
import { crops } from 'dinkum-data'

const watermelon = crops().findByName('Watermelon')!
const imagePath = `dinkum-data${watermelon.img}`
// "dinkum-data/images/resources/crops/Watermelon.png"
```

### In React

```tsx
import { crops } from 'dinkum-data'

function CropCard({ name }: { name: string }) {
  const crop = crops().findByName(name)
  if (!crop) return null

  return (
    <div>
      <img src={`/node_modules/dinkum-data${crop.img}`} alt={crop.name} />
      <h3>{crop.name}</h3>
    </div>
  )
}
```

### Copying images to your public directory

For production use, copy the images directory into your project's public/static folder as a build step. A `postinstall` script keeps this in sync on every install:

```shell
#!/bin/bash
# scripts/copy-game-assets.sh
rm -rf public/images
cp -r node_modules/dinkum-data/images public/images
```

```json
{
  "scripts": {
    "postinstall": "bash scripts/copy-game-assets.sh"
  }
}
```

Since every `img` field is already prefixed with `/images/`, you can then reference it directly with no path rewriting:

```tsx
import { crops } from 'dinkum-data'

const watermelon = crops().findByName('Watermelon')!

;<img src={watermelon.img} alt={watermelon.name} />
// resolves to /images/resources/crops/Watermelon.png in your public/ folder
```

### In Next.js

With the copy step above in place, `img` fields work directly with `next/image`, no helper function needed, since the path is already public-root-relative:

```tsx
import Image from 'next/image'
import { crops } from 'dinkum-data'

function CropIcon({ name }: { name: string }) {
  const crop = crops().findByName(name)
  if (!crop) return null

  return (
    <Image src={crop.img} alt={crop.name} width={32} height={32} unoptimized />
  )
}
```

`unoptimized` is recommended here since these are small, pre-sized game sprites. Next's image optimization pipeline adds overhead without much benefit for icons this size.

---

## Next steps

- See [TypeScript integration](/docs/typescript) for the `Base` interface that every image-bearing entity extends
- Browse any module page (for example [Furniture & decorations](/docs/furniture)) to see real `img` path examples
- See the [core concepts](/docs/core-concepts) page for how entities are shaped across the package
