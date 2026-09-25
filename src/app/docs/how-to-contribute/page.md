---
title: How to contribute
nextjs:
  metadata:
    title: dinkum-data - How to contribute
    description: See what ways you can contribute to dinkum-data.
---

First and foremost, thank you for taking the time to contribute to dinkum-data: any help is appreciated to make dinkum-data better and stronger! {% .lead %}

---

## Code of Conduct

This project and everyone participating in it is governed by the [dinkum-data
Code of Conduct](https://github.com/chiefpansancolt/dinkum-data/blob/main/.github/CODE_OF_CODUCT.md).
By participating, you are expected to uphold this code.

## How can I contribute

### Reporting bugs

#### Before submitting a bug report

This section guides you through submitting a bug report for dinkum-data. Following these
guidelines helps maintainers and the community understand your report, reproduce the behavior, and
find related reports.

Before creating a bug report, please check the [open bug issues](https://github.com/chiefpansancolt/dinkum-data/issues?q=is%3Aopen+is%3Aissue+label%3Abug)
as you might find out that you don't need to create one. When you are creating a bug report,
please include as many details as possible: the item or module involved, the expected vs. actual
values, and a link to the relevant [Dinkum Wiki](https://dinkum.fandom.com/wiki/) page if the data
looks stale.

> Note: If you find a closed issue that seems like it is the same thing you're experiencing, open a new issue and include a link to the original issue in the body of your new one.

### Suggesting enhancements

This section guides you through submitting an enhancement suggestion for dinkum-data, including
new query filters, new data modules, and improvements to existing ones.

Before creating an enhancement suggestion, please check the [open enhancement issues](https://github.com/chiefpansancolt/dinkum-data/issues?q=is%3Aopen+is%3Aissue+label%3Aenhancement)
as you might find out that you don't need to create one.

## Code contributions

Looking to contribute to dinkum-data? Look for tickets tagged `help-wanted`, or add data for a
recent game update: see the [Development guide](https://github.com/chiefpansancolt/dinkum-data/blob/main/.github/DEVELOPMENT.md)
for the module layout and data conventions.

- [Help wanted issues](https://github.com/chiefpansancolt/dinkum-data/issues?q=is%3Aopen+is%3Aissue+label%3A%22help+wanted%22): issues which could use help from the community to complete

### Development setup

```bash
git clone https://github.com/<your-username>/dinkum-data.git
cd dinkum-data
pnpm install
```

| Command              | Description                                  |
| -------------------- | -------------------------------------------- |
| `pnpm build`         | Build the package with tsup                  |
| `pnpm lint`          | Run TypeScript type-checking and ESLint      |
| `pnpm format`        | Format all files with Prettier               |
| `pnpm format:check`  | Check formatting without writing changes     |
| `pnpm test`          | Run the Jest test suite                      |
| `pnpm test:coverage` | Run the test suite with coverage             |
| `pnpm sample`        | Run the sample scripts against the built API |

## Pull requests

Please follow these steps to have your contribution considered by the maintainers:

- Follow all instructions in [the pull request template](https://github.com/chiefpansancolt/dinkum-data/blob/main/.github/PULL_REQUEST_TEMPLATE.md)
- Add or update tests for any data or query builder change
- After you submit your pull request, verify that all status checks are passing

**Ensure all merge conflicts are fixed and CI is passing.**

While the prerequisites above must be satisfied prior to having your pull request reviewed, the
reviewer(s) may ask you to complete additional design work, tests, or other changes before your
pull request can be ultimately accepted.
