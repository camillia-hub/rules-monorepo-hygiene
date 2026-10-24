# Import Discipline

## Direction

- Always keep imports flowing one direction: apps to features to shared libraries. Lower layers never import upward.
  Rationale: one-directional flow is what keeps the graph buildable, cacheable, and understandable; every upward import is a future cycle.
- Always document the layering where reviewers can see it (repo README or an architecture file) so the rule is enforceable without archaeology.
- Never import from an application into a library; libraries are consumed downstream and stay ignorant of their consumers.

## Cross-package imports

- Always import other packages through their package name (`@acme/billing`), never through relative paths that escape the package.

```ts
// Incorrect
import { Invoice } from '../../billing/src/invoice';

// Correct
import { Invoice } from '@acme/billing';
```

- Never use `../..` paths that cross a package boundary; relative imports are for files inside the same package only.
- Always wire cross-package resolution through the workspace toolchain (workspace protocol, project references, path aliases) - never through ad-hoc folder shortcuts.
- Always add the new edge to the dependency graph in the same change that adds the import (workspace manifest update); undeclared edges defeat affected-graph computation.
- Always mark type-only imports with `import type` so erased imports never hide a real runtime dependency.
- Always share test utilities through a dedicated `*-testing` package; never import a sibling package's test fixtures via relative paths.

## Cycles

- Never introduce cyclic dependencies between packages, including through type-only or indirect chains.
  Rationale: cycles defeat incremental builds, break tree-shaking, and make "what is affected by this change" unanswerable.
- Always break cycles by extracting the shared concept into a lower-level package, or by inverting the dependency behind an interface owned by the lower layer.

```text
Incorrect: billing -> reporting -> billing   (cycle through shared types)
Correct:   billing-types (depends on nothing) <- billing and <- reporting
```

- Never launder a cycle through dynamic `import()` tricks to dodge the cycle detector; fix the layering instead of hiding the dependency.
- Always enforce the acyclic rule in CI with a boundary linter (dependency-cruiser, Nx module boundaries, madge); a rule nobody checks is a wish.
