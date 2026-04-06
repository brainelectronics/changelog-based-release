# Changelog
All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

<!--
## [x.y.z] - yyyy-mm-dd
### Added
### Changed
### Removed
### Fixed
-->
<!--
RegEx for release version from file
r"^\#\# \[\d{1,}[.]\d{1,}[.]\d{1,}\] \- \d{4}\-\d{2}-\d{2}$"
-->

## Released
## [1.1.0] - 2026-04-06
### Added
- `.pre-commit-config` with `yamllint`

### Changed
- Use `softprops/action-gh-release` over deprecated and archived `actions/create-release`
- Update `setup-python` action from `v4` to `v6`
- Update `checkout` action from `v3` to `v6`
- Update `changelog2version` from `0.9.0` to `0.12.1`

### Fixed
- Solved all `yamllint` warnings

## [1.0.0] - 2022-11-12
### Added
- This changelog file
- [`.gitignore`](.gitignore) file
- [`requirements.txt`](requirements.txt) file to install all required packages
- [GitHub CI test workflow](.github/workflows/main.yml) to test [`action.yaml`](action.yaml)
- [GitHub CI release workflow](.github/workflows/release.yml) to create a tag and release on every run on `main`
- Usage description in [`README`](README.md)

### Changed
- [`action.yaml`](action.yaml) file updated with steps to run `changelog2version` and create a tagged release

## [0.1.0] - 2022-11-11
### Fixed
- The above `0.1.0` section is required due to a bug in `changelog2version`

<!-- Links -->
[Unreleased]: https://github.com/brainelectronics/changelog-based-release/compare/1.1.0...main

[1.1.0]: https://github.com/brainelectronics/changelog-based-release/tree/1.1.0
[1.0.0]: https://github.com/brainelectronics/changelog-based-release/tree/1.0.0
[0.1.0]: https://github.com/brainelectronics/changelog-based-release/tree/0.1.0

<!-- [ref-issue-1]: https://github.com/brainelectronics/changelog-based-release/issues/1 -->
