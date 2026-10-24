# Ownership

## Clear owners

- Always assign every package exactly one owning team, recorded in `CODEOWNERS` (or the repo's ownership manifest) and in the package manifest.
  Rationale: code with no owner rots; code with many owners deadlocks. One owner, many contributors.

```text
# CODEOWNERS
/packages/billing-core/    @acme/team-billing
/packages/user-directory/  @acme/team-identity
```

- Never merge changes to a package without approval from its owning team; ownership is enforced through required review, not politeness.
- Always route cross-team changes as pull requests to the owning team, never as edits slipped into an unrelated feature branch.
- Always keep the ownership manifest current with the package tree; a new package without an owner entry must fail CI.
- Never leave a package ownerless after a re-org; transfer ownership in the same change that moves the people.
- Always keep ownership at team granularity, never at an individual; people rotate, teams persist.
- Never let ownership metadata live only in a wiki; if it is not in the repo, it does not route reviews.

## Responsibilities of owners

- Always keep the package documented by its owner: purpose, public API, support channel, and deprecation status in its README.
- Always respond to consumer questions and review requests within the team's published SLA; an unresponsive owner forces forks, and forks fragment the monorepo.
- Always document the escalation path for an unresponsive owner (for example the platform or architecture group); escalating beats forking.
- Never abandon a package in place: archive it with a migration pointer, or hand ownership to a named successor.
- Always treat generated code as owned by the generator's owner, not by whoever last ran the generator.
- Always apply higher scrutiny to dependency and API-surface changes in owned packages than to internal refactors; those are the changes consumers feel.
- Always hand off a package with a working build and green tests; transferring broken code is not a transfer.

## Contribution rules for non-owners

- Always keep external contributions small and backwards-compatible; large redesigns start as a proposal discussed with the owner.
- Never add dependencies to a package you do not own; propose them instead - dependencies are part of the owner's maintenance burden.
- Never approve your own cross-team contribution on behalf of the owning team; the owner's review is the control, not a formality.
- Always respect the owning team's roadmap: when you need a capability urgently, offer it as a reviewed contribution rather than a copy-paste fork.
- Always involve the owning team early when a contribution will touch the public API; late surprises get rejected, not reviewed.

```text
Incorrect: copying billing/src/invoice.ts into your package "to move fast"
Correct:   exporting the needed helper from @acme/billing via a small owner-reviewed PR
```
