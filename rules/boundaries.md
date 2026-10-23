# Package Boundaries

## Defining packages

- Always define each package around a single domain capability, named for what it owns (`billing-core`, `user-directory`) - never grab-bag names (`utils`, `common`, `misc`).
  Rationale: "utils" packages become gravity wells that couple every consumer to everything.
- Always keep every package independently buildable and testable; a package that cannot build alone is not a package, it is a folder.
- Never reach into another package's source files; the package manifest and its public entry point are the only legal interface.
- Never split one capability across two packages that only ever change together; that is one package wearing a trenchcoat - merge them.
- Always name packages under a consistent scope (`@acme/`) so boundary lint rules and ownership mapping stay mechanical.
- Always keep packages small enough that one owning team can describe the capability in a single sentence.

## Public API surface

- Always expose exactly one public entry point per package (a root `index.ts`, or an explicit `exports` map in the manifest) and re-export only the intended API.

```json
{
  "name": "@acme/billing",
  "exports": {
    ".": "./src/index.ts"
  }
}
```

- Never deep-import a package's internals (`@acme/billing/src/internal/tax-calc`); if a consumer needs it, promote it to the public API deliberately, in its own reviewed change.
- Never import from another package's build output (`dist/`, `bin/`); resolve through the manifest entry point so the toolchain sees the real edge.
- Always treat every export as a public contract: renaming or removing an export is a breaking change, even when "nobody should be using it".
- Always keep internal modules in a clearly private folder (`internal/`) and never re-export them from the entry point.
- Never export types or functions "just in case"; the smallest sufficient API is the goal - surface area is liability.
- Always document the public API in the package README; if consumers must read source to use a package, the boundary has already failed.
- Never re-export another package's symbols from your entry point (pass-through barrels); consumers must depend on the real owner directly.
- Always keep package names stable; renaming a package is a breaking change for every consumer and goes through the versioning process.

## Manifest honesty

- Always declare every dependency in the package's own manifest; never rely on a hoisted transitive dependency happening to be present.
  Rationale: phantom dependencies break the moment the install tree shifts, in ways CI cannot reproduce locally.
- Always prune unused entries from package manifests during hygiene passes; stale declarations mislead both tooling and reviewers.
- Always keep the `sideEffects` flag (or the ecosystem equivalent) accurate in the manifest; wrong values silently break tree-shaking or drop needed code.
- Always treat the public API as a unit: one version, one changelog, per package.
