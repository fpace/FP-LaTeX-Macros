# fp-macros

A modular collection of reusable LaTeX commands and definitions for mathematical physics, differential geometry, tensor calculus, cosmology, units, and scientific text.

The goal of this repository is to provide a consistent personal LaTeX toolkit that can be reused across scientific papers, notes, lecture material, and technical documents.

Rather than maintaining the same definitions independently in different projects, commonly used commands are collected here and organized into small thematic packages.

See [CHANGELOG.md](CHANGELOG.md) for notable changes and [LICENSE](LICENSE) for licensing information.

## Repository structure

The package is organized as follows:

| File | Purpose |
| --- | --- |
| `README.md` | Project overview, installation, usage, and development guidelines. |
| `LICENSE` | Licensing notice and maintenance information. |
| `AUTHORS` | List of maintainers and contributing people. |
| `CHANGELOG.md` | Notable changes and release history. |
| `fp-macros.sty` | Main package and aggregate entry point. |
| `fp-base.sty` | Core dependencies, project metadata, shared state, options, messages, and internal helpers. |
| `fp-math.sty` | General mathematical notation. |
| `fp-tensors.sty` | Tensor calculus and differential geometry. |
| `fp-cosmology.sty` | Cosmology and cosmological perturbation theory. |
| `fp-units.sty` | Units and physical constants. |
| `fp-text.sty` | Textual and scientific abbreviations. |

The dependency hierarchy is intentionally one-directional:

```text
fp-macros
├── fp-base
├── fp-math
│   └── fp-base
├── fp-tensors
│   └── fp-math
│       └── fp-base
├── fp-cosmology
│   └── fp-tensors
│       └── fp-math
│           └── fp-base
├── fp-units
│   └── fp-base
└── fp-text
    └── fp-base
```

Lower-level modules must not depend on more specialized modules.

## Package roles

### `fp-macros.sty`

`fp-macros.sty` is the main aggregate entry point.

Loading

```latex
\usepackage{fp-macros}
```

enables the complete toolkit.

It forwards package options to `fp-base`, loads all component modules, and emits a single package banner. Internal module banners are suppressed while the aggregate package is being loaded.

### `fp-base.sty`

`fp-base.sty` is the shared core of the project.

It owns:

- project-wide metadata;
- the `journal` / `nojournal` option state;
- common messages and error handling;
- internal loading-state helpers;
- banner helpers;
- core mathematical dependencies;
- shared low-level definitions used by more than one module.

The project version, date, package name, and description are managed centrally in this file.

The core should remain lightweight and general.

### `fp-math.sty`

General mathematical notation that is not specific to tensor calculus, physics, or cosmology.

Typical contents may include:

- mathematical operators;
- ordinary and partial derivatives;
- vectors and matrices;
- delimiters;
- common mathematical functions;
- general mathematical shorthand.

The purpose of this module is to collect notation that could also be useful in a purely mathematical document.

### `fp-tensors.sty`

Commands related to tensor calculus and differential geometry.

Typical contents may include:

- metric tensors;
- covariant derivatives;
- Christoffel symbols;
- Riemann and Ricci tensors;
- curvature scalars;
- Einstein tensors;
- Lie derivatives;
- index notation;
- symmetrization and antisymmetrization;
- geometrical shorthand.

This module depends on `fp-math`.

### `fp-cosmology.sty`

Notation specific to cosmology and cosmological perturbation theory.

Typical contents may include:

- cosmological density parameters;
- Hubble quantities;
- scale-factor notation;
- cosmological parameters;
- growth functions;
- density contrasts;
- velocity perturbations;
- gravitational potentials;
- background and perturbation quantities.

This module depends on `fp-tensors`.

### `fp-units.sty`

Units and physical constants.

Typical contents may include:

- astronomical units;
- cosmological units;
- physical constants;
- unit-formatting conventions.

In normal mode this module may use `siunitx`. In journal mode it must remain usable without requiring `siunitx`.

### `fp-text.sty`

Frequently used textual and scientific abbreviations.

Typical contents may include:

- names of experiments and missions;
- cosmological model names;
- recurring scientific terminology;
- common textual abbreviations.

This module has no additional external dependencies beyond `fp-base`.

## Installation

### Local installation

Place the required `.sty` files in the same directory as the LaTeX document.

For the complete toolkit, copy all package files and load

```latex
\usepackage{fp-macros}
```

### Personal TeX tree

For use across multiple projects, the package can be installed in a personal TeX tree, for example:

```text
~/texmf/tex/latex/fp-macros/
```

Place the package files in that directory. Depending on the TeX distribution, it may be necessary to refresh the filename database.

## Usage

### Complete toolkit

The default mode is `nojournal`:

```latex
\documentclass{article}

\usepackage{fp-macros}

\begin{document}

...

\end{document}
```

This is equivalent to

```latex
\usepackage[nojournal]{fp-macros}
```

or

```latex
\usepackage[journal=false]{fp-macros}
```

### Journal-compatible mode

For documents intended for journal submission, use

```latex
\usepackage[journal]{fp-macros}
```

or equivalently

```latex
\usepackage[journal=true]{fp-macros}
```

Journal mode minimizes optional external dependencies and avoids relying on packages that may conflict with journal classes or submission environments.

The purpose of `journal` is compatibility, not journal-specific typography. Formatting specific to MNRAS, JCAP, A&A, Physical Review, or other journals should remain outside this package.

### Loading individual modules

Individual modules may be loaded directly when only a subset of the toolkit is needed:

```latex
\usepackage{fp-math}
```

```latex
\usepackage{fp-tensors}
```

```latex
\usepackage{fp-cosmology}
```

```latex
\usepackage{fp-units}
```

```latex
\usepackage{fp-text}
```

The global options can also be passed when a module is loaded directly:

```latex
\usepackage[journal]{fp-cosmology}
```

Options are forwarded to `fp-base`, which owns the shared package state.

Dependencies are loaded automatically. For example, loading `fp-cosmology` also loads `fp-tensors`, `fp-math`, and `fp-base`.

## Loading banners

The package uses a shared banner mechanism defined in `fp-base`.

When the aggregate package is loaded,

```latex
\usepackage{fp-macros}
```

only the `fp-macros` banner is emitted. Banners from modules loaded internally are suppressed.

When a module is loaded directly, for example

```latex
\usepackage{fp-cosmology}
```

only the public banner for `fp-cosmology` is emitted; banners from its internal dependencies are suppressed.

This keeps the LaTeX log informative without producing unnecessary noise.

## Package options

The currently supported global options are:

| Option | Meaning |
| --- | --- |
| `journal` | Enable journal-compatible mode. |
| `journal=true` | Equivalent to `journal`. |
| `journal=false` | Disable journal-compatible mode. |
| `nojournal` | Disable journal-compatible mode; this is the default behavior. |

Unknown options generate an explicit package error.

## Dependencies

Dependencies are intentionally split between the shared core and the modules that require them.

| Component | `journal` | `nojournal` |
| --- | --- | --- |
| `fp-base` | `l3keys2e`, `amsmath`, `amssymb` | `l3keys2e`, `amsmath`, `amssymb` |
| `fp-math` | `bm` | `bm`, `mathtools` |
| `fp-tensors` | no additional external package | `tensor` |
| `fp-cosmology` | no additional external package | no additional external package |
| `fp-units` | no additional external package | `siunitx` |
| `fp-text` | no additional external package | no additional external package |

Each module should declare only the dependencies it actually needs or rely on dependencies explicitly provided by a lower-level module.

Packages that primarily control document style or layout should not be loaded automatically by `fp-macros`.

In particular, packages such as `geometry`, `hyperref`, `cleveref`, `caption`, `subcaption`, `titlesec`, `fontspec`, and `unicode-math` belong to the document configuration rather than to this macro collection.

## Project metadata

Project-wide metadata are defined centrally in `fp-base.sty`.

The managed values are:

```latex
\c_fp_macros_name_tl
\c_fp_macros_date_tl
\c_fp_macros_version_tl
\c_fp_macros_description_tl
```

When preparing a new release, update the corresponding constant definitions in `fp-base.sty`. The aggregate package and the individual modules reuse the same project date and version.

This keeps release metadata synchronized across the complete package family.

## Design principles

### Semantic command names

Commands should describe the meaning of a quantity rather than merely reproduce its visual appearance.

For example, a descriptive command such as

```latex
\Omatter
```

is generally preferable to a short and ambiguous command name when the additional verbosity improves readability and reduces the risk of collisions.

### General-to-specific hierarchy

Definitions should be placed in the most general appropriate module.

The intended conceptual hierarchy is:

```text
fp-base
   ↓
fp-math
   ↓
fp-tensors
   ↓
fp-cosmology
```

`fp-units` and `fp-text` are parallel specialized modules built on `fp-base`.

Lower-level modules should never depend on higher-level modules.

### Minimal reimplementation

Existing, well-maintained LaTeX packages should be used when they already provide suitable low-level functionality.

The purpose of `fp-macros` is to provide consistent higher-level scientific notation rather than to reimplement established packages.

### Separation of notation and document style

Scientific notation should remain independent of document layout and journal-specific formatting.

Commands related to mathematics, tensors, cosmological quantities, units, and recurring scientific text belong here. Page geometry, fonts, captions, references, headings, and other presentation choices generally do not.

### Journal compatibility

A command that is part of the public interface should, whenever practical, remain usable in both `journal` and `nojournal` modes.

Features that require optional dependencies available only in `nojournal` mode should be designed carefully and documented explicitly.

## Adding a new command

Before adding a command:

1. Check whether an established LaTeX package already provides the required functionality.
2. Determine which module best matches the meaning of the command.
3. Prefer a descriptive and reasonably collision-resistant public name.
4. Avoid redefining standard LaTeX commands unless there is a compelling reason.
5. Keep the definition independent of journal-specific formatting whenever possible.
6. Avoid making a public macro depend unnecessarily on a `nojournal`-only package.
7. Use the `fp_...` namespace for internal `expl3` functions and variables.
8. Add a short comment when the purpose of the command is not immediately obvious.
9. Add tests for new public behavior.
10. Document notable additions or changes under `Unreleased` in [CHANGELOG.md](CHANGELOG.md).

Commands with arguments should generally be preferred over families of nearly identical fixed commands when the notation naturally varies.

## Naming conventions

Recommended conventions are:

- use descriptive public command names;
- avoid one-letter command names;
- avoid names already used by LaTeX or common packages;
- use consistent capitalization;
- group related definitions together;
- prefer semantic names over purely typographical names;
- use `fp_`-prefixed names for internal `expl3` control sequences;
- use the appropriate `expl3` scope/type prefixes for internal variables, such as `\g_..._bool`, `\c_..._tl`, and `\l_...`.

Examples of intended semantic public names include:

```latex
\RicciTensor
\RicciScalar
\EinsteinTensor
\Omatter
\Obaryon
```

## Compatibility

The package requires a modern LaTeX format:

```latex
\NeedsTeXFormat{LaTeX2e}[2023-11-01]
```

The package is designed for use with pdfLaTeX, LuaLaTeX, and XeLaTeX.

Compatibility with journal classes is one of the reasons for providing the `journal` option.

## Versioning

This project follows Semantic Versioning using `MAJOR.MINOR.PATCH`:

- **MAJOR** — incompatible changes to the public interface;
- **MINOR** — new backward-compatible commands, options, or modules;
- **PATCH** — backward-compatible fixes and internal improvements.

During initial development, versions below `1.0.0` may introduce incompatible changes. Such changes should be documented clearly.

Changes to public command names or semantics should be treated carefully because they can affect existing scientific documents.

## Changelog

Notable changes are recorded in [CHANGELOG.md](CHANGELOG.md), using the Keep a Changelog structure.

Until the first public release is prepared, ongoing work remains under `Unreleased`.

When preparing a release:

1. move the relevant entries from `Unreleased` to a versioned section;
2. add the release date in ISO format (`YYYY-MM-DD`);
3. create a new empty `Unreleased` section;
4. add GitHub comparison links once the repository URL and tags are known.

## Testing

Changes should be checked with small test documents covering both aggregate and selective loading.

A useful test layout is:

```text
tests/
├── test-all.tex
├── test-math.tex
├── test-tensors.tex
├── test-cosmology.tex
├── test-units.tex
├── test-text.tex
├── test-journal.tex
└── test-options.tex
```

Tests should cover at least:

- `\usepackage{fp-macros}`;
- `\usepackage[journal]{fp-macros}`;
- direct loading of each module;
- transitive dependency loading;
- `journal` and `nojournal` behavior;
- suppression of internal dependency banners;
- handling of unknown package options;
- representative public commands as they are added.

Where practical, tests should be run with pdfLaTeX, LuaLaTeX, and XeLaTeX.

## Documentation

As the collection grows, command documentation may be maintained separately from the implementation.

Possible future documentation files include:

```text
docs/
├── commands.md
├── math.md
├── tensors.md
├── cosmology.md
├── units.md
└── text.md
```

A generated command reference may eventually be preferable if the number of definitions becomes large.

## Status

This repository is under active development.

The package architecture is being consolidated before the public command set is expanded. Commands and naming conventions may therefore evolve before the first stable `1.0.0` release.

See [CHANGELOG.md](CHANGELOG.md) for the documented development history.


## Minimum requirements

`fp-macros` requires:

- LaTeX kernel 2020-02-02 or newer;
- a compatible LaTeX3 programming layer;
- `l3keys2e` when used with LaTeX kernels older than 2022-06-01.

The package has been tested with:

- LaTeX kernel 2021-11-15;
- expl3 2022-01-21.

## License

Copyright 2026 Francesco Pace.

This work may be distributed and/or modified under the conditions of the **LaTeX Project Public License (LPPL), version 1.3c or, at your option, any later version**.

The work has the LPPL maintenance status **maintained**. The **Current Maintainer** is **Francesco Pace**.

See [LICENSE](LICENSE) for the licensing notice and scope.

## Author

Francesco Pace
