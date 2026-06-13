# ADR-0001: Conventions index over the commons; never a vault

- **Status:** Accepted (provisional — reasoned, not yet implemented; "we'll see if it breaks")
- **Date:** 2026-06-12
- **Deciders:** Scott (+ Claude)
- **Scope:** AEMS Conventions (the repo's relationship to the commons; its trajectory)

## Context

The repo carries three genres of conventions content: **vocabulary** (what events
carry — tag and property names, namespacing), **namer guidance** (the Entity/Manifestation
rubric), and **curation** (pointing at canonical published artifacts, e.g. the french-52
deck). The curation genre raises the question RUNS already faced in its ADR-0014
amendment: do the canonical artifacts a convention points at live *in* this repo, or on
the commons with the repo merely pointing?

## Decision

**The repo is a curation index over the commons, never a vault.** Canonical AEMS
artifacts (Entities, Manifestations) live on the commons as signed, addressable events;
where a convention names a canonical artifact, this repo records its event reference and
does not host the artifact itself. Vocabulary and namer guidance are prose
contracts/governance — there is nothing for them to host — and are stable across the
repo's eras.

This is forced, not chosen:

- **aems-schema ADR-0007** — convergence is by reference, never by registry. A repo that
  *hosted* the canonical Entities would be a registry, i.e. owning the language. ADR-0007
  explicitly files blessed lists and `std:` curation as conventions-tier work *over the
  commons*, downstream of the no-registry fact.
- **aems-schema ADR-0006** — the substrate is permissionless. A canonical artifact living
  in the repo routes all improvement through whoever controls the repo: the
  studio-as-gatekeeper framing EGS exists to remove.
- Two homes for one artifact is the exact property to avoid; the commons event is the one
  home, and the index points at it.

## Consequences

- **Bootstrap reality.** The commons holds no verified canonical AEMS artifacts yet (the
  french-52 deck is a separate repo whose fidelity is asserted, not verified). So today
  the repo carries **conceptual examples** — illustrations of the *shape* of a convention,
  possibly wrong, explicitly **not** reference artifacts and **not** canonical. They exist
  so the repo teaches something now.
- **No graduation.** A conceptual example holds no privileged claim to become the
  canonical naming. Convergence-by-use (ADR-0007) decides which event references the
  community actually adopts; the index records that observed convergence, it does not
  confer it.
- **Trajectory:** teaching repo of conceptual examples → curation index over commons
  artifacts, with the vocabulary and namer-guidance prose stable across both eras.
