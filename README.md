# Changelog based release Action

[![Create release](https://github.com/brainelectronics/changelog-based-release/actions/workflows/release.yaml/badge.svg)](https://github.com/brainelectronics/changelog-based-release/actions/workflows/release.yaml)
![Release](https://img.shields.io/github/v/release/brainelectronics/changelog-based-release?include_prereleases&color=success)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

GitHub Action for creating changelog based GitHub Releases

---

<!-- MarkdownTOC -->

- [Requirements](#requirements)
  - [Tag section](#tag-section)
- [Usage](#usage)
  - [Basic usage](#basic-usage)
  - [Customizing](#customizing)
  - [Outputs](#outputs)
  - [Permissions](#permissions)
- [License](#license)

<!-- /MarkdownTOC -->

## Requirements

As this Action uses the [changelog2version](https://github.com/brainelectronics/changelog2version)
to parse and extract informations from a changelog, the requirements for the
changelog format have to meet certain criterias.

### Tag section

The header of an entry in the changelog has to follow the following format if
not specified differently with the `version-line-regex` and/or `semver-line-regex`
option in the workflow.

This changelog entry shall follow the
[semantic version pattern](https://semver.org/) and shall match the following
pattern:

`## [x.y.z] - yyyy-mm-dd` or `## [x.y.z] - yyyy-mm-ddThh:mm:ss`

The line shall start with two hashtags (`##`) followed by a single space (` `).
The semver version with `x`, `y` and `z` as non-negative integers, seperated
by a dot (`.`) and surrounded by square brackets (`[]`). Followed by a space
(` `), a dash (`-`), another space (` `) and the
[ISO8601](https://www.iso.org/iso-8601-date-and-time-format.html) formatted
date. Additional timestamps after the date, seperated from the date by a
single space (` `) or a capital (`T`), are optional. The semantic version tag
inside the square brackets (`[]`) supports the full semantic versioning scope.

Examples:

- `## [1.2.3] - 2022-11-13`
- `## [1.2.3-alpha-a.b-c-somethinglong+build.1-aef.1-its-okay] - 2022-11-13T12:34:56`

The [changelog](changelog.md) of this repo is an example of how the changelog
shall look like to be parsable with the default changelog2version settings.

## Usage

### Basic usage

With the default values used a GitHub release and tag based on the changelog
located in the project root will be created. For further details check the
[customizing section](#customizing)

See [action.yml](action.yml)

```yaml
name: Create release

on:
  push:
    branches:
      - main

permissions:
  contents: write

jobs:
  release:
    runs-on: ubuntu-latest
    name: Release
    steps:
      - name: Checkout
        uses: actions/checkout@v6
      - name: 'Create changelog based release'
        uses: brainelectronics/changelog-based-release@v1
```

### Customizing

The following are optional as `step.with` keys

| Name                      | Type    | Description                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| ------------------------- | ------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `github-token`            | String  | Secret GitHub Personal Access Token. Defaults to `${{ github.token }}`                                                                                                                                                |
| `changelog-path`          | String  | Path to changelog used for releasing. Default to `changelog.md`                                                                                                                                                       |
| `tag-name-prefix`         | String  | Prefix of tag name before extraced x.y.z from changelog, e.g. `v` to get `v1.2.3`                                                                                                                                     |
| `tag-name-extension`      | String  | Extension of tag name after extracted x.y.z from changelog, e.g. `${{ github.sha }}` to get `1.2.3${{ github.sha }}`. Defaults to `-rc${{ github.run_number }}.dev${{ github.event.number }}` e.g. `1.2.3-rc21.dev`   |
| `release-name-prefix`     | String  | Prefix of release name before extracted x.y.z from changelog, e.g. `Release of ` to get `Release of 1.2.3`                                                                                                            |
| `release-name-extension`  | String  | Extension of release name after extracted x.y.z from changelog, e.g. `-rc` to get `1.2.3-rc`. Defaults to `-rc${{ github.run_number }}.dev${{ github.event.number }}` e.g. `1.2.3-rc21.dev`                           |
| `draft-release`           | Boolean | Indicator of whether or not this release is a draft. Defaults to `false`                                                                                                                                              |
| `prerelease`              | Boolean | Indicator of whether or not is a prerelease. Defaults to `false`                                                                                                                                                      |
| `from-commit-sha`         | String  | Commit SHA to create tag on. Defaults to `${{ github.head_ref }}`                                                                                                                                                      |

```yaml
steps:
  - name: Checkout
    uses: actions/checkout@v6
  - name: 'Create changelog based release'
    uses: brainelectronics/changelog-based-release@v1
    with:
      # note you'll typically need to create a personal access token
      # with permissions to create releases in the other repo
      # or you set the "contents" permissions to "write" as in this example
      github-token:  ${{ secrets.CUSTOM_GITHUB_TOKEN }}
      changelog-path: path/to/changelog.md
      tag-name-prefix: 'v'
      tag-name-extension: '+${{ github.sha }}'
      release-name-prefix: 'Release of '
      release-name-extension: '-rc'
      draft-release: true
      prerelease: true
```

### Outputs

The Action provides the following output variables

| Name                  | Type    | Description                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| --------------------- | ------- | --------------------------------- |
| `latest-description`  | String  | Content of latest changelog entry |
| `latest-version`      | String  | Latest changelog version          |

```yaml
steps:
  - name: Checkout
    uses: actions/checkout@v3
  - name: 'Create changelog based release'
    uses: brainelectronics/changelog-based-release@v1
    id: changelog-release
  - name: Report action output
    run: |
      echo "Latest changelog change description: ${{ steps.changelog-release.outputs.latest-description }}"
      echo "Latest changelog version: ${{ steps.changelog-release.outputs.latest-version }}"
```

### Permissions

This Action requires the following permissions on the GitHub integration token:

```yaml
permissions:
  contents: write
```

[GitHub token permissions](https://docs.github.com/en/actions/security-guides/automatic-token-authentication#permissions-for-the-github_token)
can be set for an individual job, workflow, or for Actions as a whole.

## License

The scripts and documentation in this project are released under the
[MIT License](LICENSE)
