# AEMS Conventions

The glossary for the AEMS **conventions tier** — the optional-but-recommended layer
that sits on top of the [AEMS schema](https://github.com/enduring-game-standard/aems-schema).
The schema defines four event kinds and the provenance chain between them, and nothing
more (aems-schema ADR-0004); everything about what those events *carry*, and how a
community converges on shared answers, is conventions territory and lives here.

This context **inherits the AEMS glossary** from `aems-schema/CONTEXT.md` — Entity,
Manifestation, Asset, State, Authenticity, and the claims model are defined there and
are not redefined here. This file defines only terms specific to the conventions tier.

## Language

**Convention**:
A voluntarily adopted shared answer to a question the schema deliberately leaves open —
what a context (a game, a league, a community) agrees to assume so that strangers'
events interoperate. A convention is **opinionated by design**: it may prescribe, rank
one shape over another, and render verdicts ("make this an Entity, not a Manifestation").
What is voluntary is *adopting* it, not its strength — follow the schema and you are
*using AEMS*; follow a convention and you are *shaping for interoperability*. Ignoring
every convention is still compliant AEMS, the way a taped-up ball is still soccer
(aems-schema ADR-0009).
_Avoid_: rule, requirement, mandate, spec

**Object-identity**:
What an Entity names — the thing in itself, independent of how any game uses it:
`sword`, `flask`, `gem`, `fireball`. Intrinsic and durable; it is the spine
cross-game recognition rides on, and the only part of a thing that transfers
unchanged. An Entity naming is a real or established-fiction *noun for a thing* —
medium-free and mechanic-free — never a role or an action. Whether the naming is
ownerless (`sword`) or authored (`master-sword`) is a separate axis, carried by the
signature, not a disqualifier — both are Entities (aems-schema ADR-0007). Entity grain
is set by noun-hood, not by mechanical detail: a Manifestation's mechanical quirks never
mint a new Entity — `double-barrel-shotgun` is the object-noun; `doom:super-shotgun`
is its Manifestation.
_Avoid_: type, category, role, kind

**Use-classification**:
What a thing is *for* in a given context — `weapon`, `healing-item`,
`super-weapon`, `cooking-tool`. Relational: it varies by observer, purpose, and
game, so it cannot inhere in the Entity. It lives on **Manifestation tags**
(`category`/`type`/`subtype`), where a receiving game freely honors or overrides it
(the Cooking Mama knife that DOOM reads as a weapon). One thing carries many
use-classifications and none is canonical.
_Avoid_: essence, the kind, the true Entity

## Flagged ambiguities

- **"Convention" (this tier) vs. a kernel MUST (the schema).** The schema's three
  reference edges are mandatory for compliance; a conventions document may *restate*
  them for the reader but can never *own* them. Where this repo phrases a kernel edge as
  a convention it is doing the reader a courtesy, not inventing a requirement
  (aems-schema ADR-0004).

- **Prescriptive ≠ mandatory.** A convention may be as opinionated as it likes — a
  decision procedure that yields verdicts is a perfectly good convention. The schema's
  refusal to adjudicate (e.g. ADR-0007: no registry, convergence by use) constrains the
  *schema*, not what conventions may recommend. Testing a convention against a schema
  ADR's humility is a category error: test a convention for coherence with the schema's
  *vocabulary* and for *internal consistency* instead.

- **Identity (intrinsic) vs. classification (relational).** The recurring trap in
  Entity design is making the Entity answer "what *kind* is this?" Kind is relational —
  what kind, for whom, for what purpose — and a relation cannot sit in the intrinsic,
  durable Entity slot; it leaks back onto tags every time. The Entity names *identity*
  (object-identity); *classification* (use-classification) is relational and
  belongs on Manifestation tags. This is the axis on which `entity-abstraction.md` is
  being re-evaluated.

- **The Manifestation is the pillar seam; the Entity is pillar-neutral.** An Entity is
  pure identity, upstream of every other EGS protocol. A Manifestation is game-paired
  and data-rich, so it is the layer that carries the values RUNS processors read and
  names the role MAPS describes — without notating the mechanic (MAPS) or executing it
  (RUNS). A seam at a boundary, not a dependency: the data co-resides; nothing is
  imported, and the protocols stay independent.
