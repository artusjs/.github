## Artus.js Organization Configuration

## Features

- Organization's README
- Reusing Workflow


## Reusing Workflow

> for more detail, see [GitHub Docs](https://docs.github.com/en/actions/using-workflows/reusing-workflows#using-inputs-and-secrets-in-a-reusable-workflow)

### CI Workflow

1. create `.github/workflows/ci.yml` at your repository:

```yaml
name: CI

on:
  push:
    branches: [ master, main ]

  pull_request:
    branches: [ master, main, next, beta, "*.x" ]

  schedule:
    - cron: '0 2 * * *'

  workflow_dispatch:

jobs:
  Job:
    name: Node.js
    uses: artusjs/.github/.github/workflows/node-test.yml@master
    # pass these inputs only if you need to custom
    # with:
    #   os: 'ubuntu-latest, macos-latest, windows-latest'
    #   version: '16, 18'
```

2. Update `npm scripts` at your `package.json`:

```json
{
  "name": "your-project",
  "scripts": {
    "lint": "eslint .",
    "test": "mocha",
    "ci": "c8 npm test"
  },
}
```

### Release Workflow

1. create `.github/workflows/release.yml` at your repository:

```yaml
name: Release
on:
  # comment this if you want to release by manual trigger action
  push:
    branches: [ master, main, next, beta, "*.x" ]

  # manual trigger action at GitHub Actions Tab
  workflow_dispatch:

jobs:
  release:
    name: Node.js
    uses: artusjs/.github/.github/workflows/node-release.yml@master
```

2. note the following:
- it will release 1.0.0 at master for the first release, so if you don't want this, init your code at `beta` branch, so it will release `1.0.0-beta.1`.
- if you don't want unexpected release, don't use `fix:` / `feat:`, just use `chore:` / `docs:` / `style:` / `test:`
- see https://semantic-release.gitbook.io/semantic-release/#commit-message-format for more detail.
