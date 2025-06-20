# Define release version

This action is meant to define what your next version will be.

The assumption is that you're using semver and conventional commits.

- [SemVer](https://semver.org/)
- [Conventional commits](https://www.conventionalcommits.org/en/v1.0.0/)

## How this works

It will take the last git tag, lets say v1.0.0.
Then will will take all the commits after that tag and define what the bump will be.

So say we have v1.0.0 and the next commits in line are:

- feat: My awesome new feature
- fix: Something went horribly wrong
- docs: Updated the README

If there are commits it will always at least be a patch version and create v1.0.1
However, we also see a commit starting with "feat" So this means a feature bump
So it will give you v1.1.0

## Outputs

- previous-version # The version the action is basing the next version on
- release-version # The new version
- bump # Type of bump, either major, minor or patch

## Usage

Example:

```yaml
- name: Define version
  id: version
  uses: LarsNieuwenhuizen/action-define-release-version@v1
```

A simple workflow example to use this action.
Important to note here, in order to actually get the commits and tags we need to checkout the code with tags and commits.
So add the fetch-depth: 0 and fetch-tags: true to the checkout action. Otherwise the action has nothing to use.

```yaml
name: A workflow to release my next version

on:
  workflow_dispatch:

jobs:
  version:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v4
        with:
          fetch-depth: 0
          fetch-tags: true

      - name: Define version
        id: version
        uses: LarsNieuwenhuizen/action-define-release-version@v1

      - name: Use version
        run: |
          echo "Version: ${{ steps.version.outputs.release-version }}"
```

The workflow step summary will also show the next version

### Reusable workflow

I've also wrapped this action in a reusable workflow.
That limits the amount of lines you'll need ;)

[The link to the docs](https://github.com/LarsNieuwenhuizen/workflows?tab=readme-ov-file#define-the-next-version-for-your-apps-based-on-commits)
