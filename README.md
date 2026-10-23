# Monorepo Hygiene Rules

A Camillia rules pack that enforces monorepo hygiene as hard constraints: clean package boundaries, one-directional imports, honest versioning.

## Rules at a glance

- **Package boundaries** (`rules/boundaries.md`) - single-capability packages, one public entry point per package, no deep imports, minimal deliberate API surface.
- **Import discipline** (`rules/imports.md`) - one-directional layering, package-name imports only, no cross-package relative paths, no cyclic dependencies.
- **Shared library versioning** (`rules/versioning.md`) - strict semver, mandatory changelogs, deprecate-before-delete, additive changes over breaking edits.
- **Ownership** (`rules/ownership.md`) - exactly one owning team per package, owner review required, documented support and deprecation status.

## Install

Install from the Camillia Community Hub. On install, the rule files are placed under `.workspace/rules/` in your workspace and apply to every agent session there.

## License

MIT - see [LICENSE](LICENSE). Copyright 2026 Camillia contributors.
