# Shared Library Versioning

## Semver discipline

- Always version published shared libraries with strict semantic versioning: breaking change is a major, a feature is a minor, a fix is a patch.
  Rationale: consumers can upgrade safely only when the version number tells the truth about the change.
- Always document every release in a changelog; the changelog is the contract review for consumers.
- Never ship a breaking change in a minor or patch release; "small" breaking changes break the same builds as big ones.
- Never mutate or republish an already-published version; publish a new patch - registries and caches are append-only in practice.

## Consuming shared libraries

- Always upgrade dependents promptly after a shared-library release; never let production paths accumulate more than two major versions of the same library.
- Never run conflicting versions of one shared library across packages without an explicit documented reason and a named owner.
- Always run the full affected test graph on any shared-library change; a green build of the library alone proves nothing about its consumers.
- Always keep peer-dependency ranges honest and minimal; an over-wide peer range is an undeclared breaking-change vector.

## Change process

- Always deprecate before deleting: mark the old API, give consumers a migration window, then remove it in the next major release.
- Always mark deprecations with the tooling's machine-readable attribute (`[Obsolete]`, `@deprecated` JSDoc) including the replacement - not just prose comments.

```ts
// Correct: machine-readable deprecation with a pointer
/** @deprecated Use priceWithOptions instead. Will be removed in v3. */
export function price(order: Order): Money;
```

- Never publish breaking API edits under a patch version to dodge review; version honestly or not at all.
- Always prefer additive changes (new optional parameters, new exports) over signature changes; extension beats mutation.

```ts
// Incorrect: breaking signature change
export function price(order: Order, currency: string): Money;

// Correct: additive, backwards-compatible
export function price(order: Order, options?: { currency?: string }): Money;
```

- Always announce breaking-change majors to consumers with a migration guide before release, not after.
- Never bump versions by hand in a feature PR; use the release tooling so tags, changelogs, and manifests stay in lockstep.
- Always run consumer contract tests against the release candidate before publishing a shared library.
