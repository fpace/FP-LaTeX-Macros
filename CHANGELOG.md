# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Added - 2026-09-15

- Added the modular package structure:
  - `fp-base.sty`;
  - `fp-math.sty`;
  - `fp-tensors.sty`;
  - `fp-cosmology.sty`;
  - `fp-units.sty`;
  - `fp-text.sty`.
- Added `journal` and `nojournal` package modes.
- Added support for `journal=true` and `journal=false`.
- Added shared package-option handling in `fp-base.sty`.
- Added centralized project metadata for package name, version, date, and description.
- Added shared package and module banner messages.
- Added internal loading-state management so that dependency banners are suppressed during aggregate and transitive module loading.
- Added selective loading of individual modules with automatic dependency loading.
- Added explicit handling of unknown package options.
- Added differentiated dependency loading between `journal` and `nojournal` modes.

### Changed - 2026-09-11

- Renamed the main package from `fp` to `fp-macros` to avoid conflict with the existing LaTeX `fp` package.
- Made `fp-macros.sty` the aggregate entry point rather than the owner of shared state.
- Moved shared infrastructure, options, metadata, messages, and core dependencies to `fp-base.sty`.
- Renamed component files to the shorter module names `fp-math.sty`, `fp-tensors.sty`, `fp-cosmology.sty`, `fp-units.sty`, and `fp-text.sty`.
- Updated the README to describe the implemented package architecture, loading modes, dependency policy, metadata management, banner behavior, and testing strategy.

### Fixed - 2026-09-11

- Corrected the initial `expl3` option-definition and boolean-condition syntax.
- Corrected unknown-option handling.
- Removed reliance on internal LaTeX metadata variables.
- Ensured that aggregate loading emits a single public banner.
- Ensured that direct module loading emits only the banner of the requested module.
- Ensured that transitive dependencies remain silent when loaded internally.

### Changed - 2026-09-10

- Updated `README.md` to describe the whole project.

### Added - 2026-09-08

- Added the initial project documentation.
- Added LaTeX Project Public License 1.3c licensing information.

<!--
When preparing a release:

1. Move the entries under [Unreleased] to a new version section.
2. Add the release date in ISO format: YYYY-MM-DD.
3. Add a new empty [Unreleased] section.
4. Add or update comparison links once the GitHub repository URL is known.

Use the following headings where appropriate:

- Added
- Changed
- Deprecated
- Removed
- Fixed
- Security
-->
