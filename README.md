# FP LaTeX Macros

A modular collection of reusable LaTeX commands and definitions for mathematical physics, differential geometry, tensor calculus, and cosmology.

The goal of this repository is to provide a consistent personal LaTeX toolkit that can be reused across scientific papers, notes, lecture material, and technical documents.

Rather than maintaining the same definitions independently in different projects, commonly used commands are collected here and organized into small thematic packages.

See [CHANGELOG.md](CHANGELOG.md) for notable changes and [LICENSE](LICENSE) for the project's licensing notice.

## Repository structure

The intended package layout is described below. `README.md`, `LICENSE`, and `CHANGELOG.md` belong in the repository root, alongside the package files.

| File | Purpose |
| --- | --- |
| `README.md` | Project overview, installation, usage, and development guidelines. |
| [LICENSE](LICENSE) | Licensing notice and maintenance information. |
| [CHANGELOG.md](CHANGELOG.md) | Notable changes and release history. |
| `fp.sty` | Main package and entry point. |
| `fp-base.sty` | Core dependencies and shared definitions. |
| `fp-math.sty` | General mathematical notation. |
| `fp-tensors.sty` | Tensor calculus and differential geometry. |
| `fp-cosmology.sty` | Cosmology and cosmological perturbation theory. |
| `fp-units.sty` | Units and physical constants. |
| `fp-text.sty` | Textual and scientific abbreviations. |

### `fp.sty`

Main package and recommended entry point.

It loads the individual modules required by the complete toolkit, allowing the entire collection to be enabled with

```latex
\usepackage{fp}
```

Individual modules may also be loaded separately when only a subset of the functionality is required.

### `fp-base.sty`

Core dependencies and low-level definitions shared by the other modules.

Typical contents include:

- common mathematical packages;
- basic symbols;
- general-purpose utility commands;
- definitions required by several other modules.

This package should remain as lightweight and general as possible.

### `fp-math.sty`

General mathematical notation that is not specific to physics or cosmology.

Typical contents include:

- mathematical operators;
- ordinary and partial derivatives;
- vectors and matrices;
- delimiters;
- common functions;
- general mathematical shorthand.

Examples may include commands for

```latex
\Tr
\diag
\rank
\pd{f}{x}
\od{f}{x}
```

The purpose of this module is to collect notation that could also be useful in a purely mathematical document.

### `fp-tensors.sty`

Commands related to tensor calculus and differential geometry.

Typical contents include:

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

This module is intended to keep tensor and differential-geometric notation consistent across different projects.

### `fp-cosmology.sty`

Notation specific to cosmology and cosmological perturbation theory.

Typical contents include:

- cosmological density parameters;
- the Hubble parameter;
- scale factor notation;
- cosmological parameters;
- growth functions;
- density contrasts;
- velocity perturbations;
- gravitational potentials;
- background and perturbation quantities.

For example, recurring quantities such as

```latex
\Omega_{\mathrm m}
\Omega_{\mathrm b}
\sigma_8
n_{\mathrm s}
\Lambda\mathrm{CDM}
```

can be represented by consistent semantic commands.

If the collection grows substantially, specialized material such as cosmological perturbation theory may eventually be moved into an additional module.

### `fp-units.sty`

Units and physical constants.

This module is intended to work primarily with `siunitx` and may contain:

- custom astronomical units;
- cosmological units;
- physical constants;
- unit formatting conventions.

Whenever possible, units should be defined through `siunitx` rather than through manually formatted LaTeX commands.

### `fp-text.sty`

Frequently used textual and scientific abbreviations.

Typical examples include:

- names of experiments and missions;
- cosmological model names;
- recurring scientific terminology;
- common textual abbreviations.

For example,

```latex
\LCDM
\wCDM
```

may be used to ensure that model names are always typeset consistently.

## Installation

### Local installation

Place the required `.sty` files in the same directory as the LaTeX document. When using the complete toolkit, include `fp.sty` and all the modules it loads.

Then load the main package with

```latex
\usepackage{fp}
```

### Personal TeX tree

For use across many projects, the package can instead be installed in a personal TeX tree. A possible location is

```text
~/texmf/tex/latex/fp/
```

with the package files stored inside that directory. The appropriate location depends on the TeX distribution and its configuration.

After installation, the package can be used from any document with

```latex
\usepackage{fp}
```

Depending on the TeX distribution, it may be necessary to refresh the filename database.

## Usage

The complete collection can be loaded with

```latex
\documentclass{article}

\usepackage{fp}

\begin{document}

...

\end{document}
```

Alternatively, individual modules can be loaded separately:

```latex
\usepackage{fp-math}
\usepackage{fp-tensors}
\usepackage{fp-cosmology}
```

This can be useful when only a restricted subset of the definitions is needed.

## Design principles

### Semantic command names

Commands should describe the meaning of a quantity rather than merely reproduce its visual appearance.

For example, `\Omatter` is preferable to a very short and ambiguous command such as `\Om` when the additional verbosity improves readability.

Semantic names make the LaTeX source easier to understand and reduce the probability of command-name collisions.

### General-to-specific hierarchy

Definitions should be placed in the most general appropriate module.

The intended hierarchy runs from `fp-base` through `fp-math` and `fp-tensors` to `fp-cosmology`. Each module should load only the dependencies it actually needs.

Lower-level modules should not depend on more specialized modules. This helps avoid circular dependencies and keeps the package structure predictable.

### Minimal reimplementation

Existing, well-maintained LaTeX packages should be used whenever they already provide the required functionality.

The purpose of this repository is to build consistent higher-level notation on top of facilities such as `amsmath`, `mathtools`, `siunitx`, and the document command interface provided by modern LaTeX.

### Separation of notation and document style

Scientific notation should remain independent of journal-specific formatting.

Commands related to equations, tensors, cosmological quantities, and units belong in this repository. Formatting specific to journals such as MNRAS, JCAP, A&A, or Physical Review should preferably be kept in separate style files.

This separation makes the scientific notation portable between different publication formats.

## Adding a new command

Before adding a command:

1. Check whether an established LaTeX package already provides the required functionality.
2. Determine which module best matches the meaning of the command.
3. Choose a descriptive and reasonably collision-resistant name.
4. Avoid redefining standard LaTeX commands unless absolutely necessary.
5. Keep the definition independent of journal-specific formatting whenever possible.
6. Add a short comment when the purpose of the command is not immediately obvious.
7. Document notable additions or changes under `Unreleased` in [CHANGELOG.md](CHANGELOG.md).

For example:

```latex
% Present-day matter density parameter
\newcommand{\Omatter}{\Omega_{\mathrm m}}
```

Commands with arguments should preferably be used when the notation naturally varies:

```latex
\newcommand{\pd}[2]{%
  \frac{\partial #1}{\partial #2}%
}
```

rather than defining many nearly identical fixed commands.

## Naming conventions

The following conventions are recommended:

- use descriptive command names;
- avoid one-letter commands;
- avoid names already used by standard LaTeX;
- use consistent capitalization;
- group related definitions together;
- prefer semantic names over purely typographical names.

For example:

```latex
\RicciTensor
\RicciScalar
\EinsteinTensor
\Omatter
\Obaryon
```

are generally easier to maintain than a large collection of cryptic abbreviations.

## Compatibility

The package is intended for modern LaTeX distributions.

Dependencies should be kept explicit in the corresponding `.sty` files using, for example,

```latex
\RequirePackage{amsmath}
\RequirePackage{amssymb}
\RequirePackage{mathtools}
\RequirePackage{siunitx}
```

Each module should declare only the dependencies it actually requires, or rely on dependencies explicitly provided by a lower-level module.

## Versioning

This project follows Semantic Versioning using `MAJOR.MINOR.PATCH`:

- **MAJOR** — incompatible changes to existing commands;
- **MINOR** — new backward-compatible commands or modules;
- **PATCH** — backward-compatible fixes and minor internal improvements.

During initial development, versions below `1.0.0` may introduce incompatible changes. Such changes should be clearly documented, including migration instructions where appropriate.

Changes to command names or semantics should be treated carefully because they can affect existing scientific documents.

## Changelog

Notable changes are recorded in [CHANGELOG.md](CHANGELOG.md), using the Keep a Changelog format.

The changelog currently starts with an `Unreleased` section. It records work that has not yet been assigned to a published release.

Entries are grouped under the following headings when applicable:

- **Added** — new features or commands;
- **Changed** — changes to existing behavior;
- **Deprecated** — features or commands scheduled for removal;
- **Removed** — removed features or commands;
- **Fixed** — bug fixes;
- **Security** — security-related fixes.

When preparing a release, move the relevant entries into a versioned section with a release date in `YYYY-MM-DD` format, and keep an `Unreleased` section for subsequent development. Add GitHub release or comparison links once the repository URL and corresponding tags are available.

## Testing

Changes should ideally be checked with one or more small test documents covering the main modules.

A future test suite could include `tests/test-math.tex`, `tests/test-tensors.tex`, `tests/test-cosmology.tex`, `tests/test-units.tex`, and `tests/test-all.tex`.

The complete test document should load

```latex
\usepackage{fp}
```

and exercise representative commands from every module.

## Documentation

As the collection grows, command documentation may be maintained separately from the implementation.

Possible future documentation files include `docs/commands.md`, `docs/math.md`, `docs/tensors.md`, `docs/cosmology.md`, and `docs/units.md`.

A generated command reference may eventually be preferable if the number of definitions becomes large.

## Status

This repository is under active development.

Commands, naming conventions, and module organization may evolve while the package is being consolidated. Backward compatibility will become a stronger requirement once a stable `1.0.0` release is reached.

See [CHANGELOG.md](CHANGELOG.md) for the documented development history.

## License

Copyright 2026 Francesco Pace.

This work may be distributed and/or modified under the conditions of the **LaTeX Project Public License (LPPL), version 1.3c or, at your option, any later version**.

The work has the LPPL maintenance status **maintained**. The **Current Maintainer** is **Francesco Pace**.

See [LICENSE](LICENSE) for the project's licensing notice and scope. The license text is available from the [LaTeX Project](https://www.latex-project.org/lppl.txt).

## Author

Francesco Pace