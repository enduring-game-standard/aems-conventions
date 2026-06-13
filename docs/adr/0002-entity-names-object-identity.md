# ADR-0002: The Entity names object-identity; classification is relational; the four-test rubric retires

- **Status:** Accepted (provisional — reasoned, not yet implemented; "we'll see if it breaks")
- **Date:** 2026-06-13
- **Deciders:** Scott (+ Claude)
- **Scope:** AEMS Conventions (what an Entity naming carries vs. what goes on Manifestations; supersedes the four-test rubric in `entity-abstraction.md`)

## Context

`entity-abstraction.md` grew a four-test rubric — Verb, Cross-Game, Substitution, Mechanical
Signature — scored "3 of 4" to decide whether a game concept is an Entity or a Manifestation.
Stress-testing it across DOOM's weapon rack, Chess, Yomi, and four deliberately hostile
cross-game imports (Scan→Cooking Mama, Samus→Scrabble, Master Sword→Animal Crossing,
Estus→Galaga) showed it is not a consistent classifier:

- Two of its tests scale in **opposite directions** on the grain axis — Cross-Game rewards
  generality (everything lumps to `item`), Mechanical Signature rewards specificity (every gun
  splits to its own kind) — so the "3 of 4" vote merely lets whichever tests happen to fire
  decide. DOOM's nine weapons resolve to ~eight sibling weapon Entities, defeating the rubric's
  own anti-proliferation goal.
- Mechanical Signature has **no granularity floor**; it licenses infinite splitting (pump vs.
  double-barrel vs. automatic shotgun).
- The **same test is applied at different depths** in different domains (all healing lumps to
  one Entity; weapons split) with no rule explaining the difference.

The root cause: the rubric makes the Entity answer "what *kind* is this?" Kind is
**relational** (what kind, for whom, for what purpose), and a relation cannot inhere in the
intrinsic, durable Entity slot — so it leaks back onto tags on every attempt.

## Decision

**The Entity names object-identity; classification is relational and lives on Manifestation
tags. The four positive tests are retired.**

- An Entity is a **noun for a thing** (real or established-fiction), medium-free and
  mechanic-free. Its grain is set by **noun-hood**, not by mechanical signature: a
  Manifestation's mechanical quirks never mint a new Entity.
- **Use / role** (`weapon`, `healing-item`, `super-weapon`, `cooking-tool`) is relational — it
  varies by receiver, purpose, and game — so it lives on Manifestation `category`/`type` tags,
  where a receiver freely honors or overrides it. (This is already what universal convention #3
  in the README says; the rubric had reopened it.)
- What survives from `entity-abstraction.md` is its **negative filters**: keep namings free of
  IP-misrepresentation, of medium suffixes (medium transparency), of mechanics (things, not
  MAPS Patterns), and disambiguate ambiguous namings at creation (`ammo-shotgun-shell`, not
  `ammo-shell`).

## Consequences

- Transfer delivers identity, provenance, possession, and condition — **never behavior**.
  Cross-game import is a spectrum: re-instantiation (sword→Skyrim), cosmetic/trophy (Master
  Sword→Animal Crossing), or refusal (Estus→Galaga). Trophies and refusals are correct
  outcomes, not failures. The old "Substitution Test" overpromised and is dropped.
- Agent-entities (`monster`, `boss`) and abilities (`scan`) can be **named for provenance** but
  cannot transfer their substance, which is behavior owned by MAPS/RUNS.
- Determinism for a decomposition agent: "is this a noun for a thing, medium/mechanic-free?" is
  far more answerable than "pick the right abstraction level." Entities and Manifestations live
  as working definitions in local dev files and reach the commons only when a game ships, so
  curation rides the existing dev→release pipeline.
- **Scope boundary.** This ADR governs the *denotation* axis (thing vs. role/mechanic/medium)
  and *grain*. It does **not** govern the *authorship* axis (ownerless `sword` vs. authored
  `master-sword`) — that is one Entity species along a convergence gradient, decided in
  aems-schema ADR-0007 (2026-06-13 Amendment). The axes are orthogonal: a naming can be a thing
  and authored at once.
- `entity-abstraction.md` is rewritten down to this decision.
