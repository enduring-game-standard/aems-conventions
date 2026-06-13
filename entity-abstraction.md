# Entity Naming Conventions

These conventions answer the question the AEMS schema leaves open: **what does an Entity naming
carry, and what belongs on the Manifestation instead?** They are prescriptive — adopt them and
your namings interoperate across publishers; the schema is satisfied either way (a taped-up
ball is still soccer). The reasoning trail is in
[ADR-0002](./docs/adr/0002-entity-names-object-identity.md) and
[aems-schema ADR-0007](https://github.com/enduring-game-standard/aems-schema/blob/main/docs/adr/0007-an-entity-event-is-a-naming-not-the-concept.md).

## The one rule

> **An Entity names object-identity — *what a thing is*. Everything about *what a thing is for*,
> or *how it behaves*, lives elsewhere.**

An Entity is a **noun for a thing**: `sword`, `potion`, `flask`, `fireball`, `gem`. It is the
durable spine cross-game recognition rides on — the only part of a thing that transfers
unchanged. Three things are *not* object-identity and never belong on an Entity:

| Not an Entity | What it actually is | Where it lives |
|---|---|---|
| **Use / role** — `weapon`, `healing-item`, `super-weapon`, `cooking-tool` | what the thing is *for*, which varies by game and receiver | Manifestation `category`/`type` tags |
| **Mechanic / interaction pattern** — `consumable`, `pickup`, `scan` (the ability) | a rule — *how* a verb resolves | a [MAPS Pattern](https://github.com/enduring-game-standard/maps-notation) |
| **Medium** — `-card`, `-die`, `-token`, `-tile` | the delivery mechanism, an accident of substance | Manifestation property |

## Why use, mechanic, and medium are not identity

**Use is relational.** A kitchen knife is a cooking tool or a weapon depending on who holds it;
import Cooking Mama's knife into DOOM and DOOM reads it as a weapon. One thing carries many
uses and none is canonical, so use cannot inhere in the Entity — it goes on Manifestation tags,
where a receiving game honors or overrides it.

**A mechanic is a rule.** `consumable` (used up on use) and `pickup` (acquired by contact)
describe how a verb resolves, not what a thing is — a potion can be drunk *or* thrown. Rules
are MAPS Patterns. The chess piece is the Entity; the L-shaped Knight move is a Pattern. You
can mail someone the Knight; you cannot mail them how the Knight moves.

**Medium is an accident.** The Ace of Spades is the same whether card, pixel, or clay tile; a
movement resource is the same whether a board game's "Speed Card" or a die roll. Never suffix a
`d`-tag with the medium — unless the thing has *no identity apart from it*. Standard playing
cards and chess pieces are the exception: strip a card from the Ace of Spades and nothing
recognizable remains, so rank-and-suit *is* the identity.

## Grain: set by noun-hood, not mechanical detail

A thing's Entity grain is fixed by whether it is a noun for a thing — not by how mechanically
distinct it is. A Manifestation's mechanical quirks never mint a new Entity. DOOM's nine weapons
are nine **Manifestations** of a handful of object-noun Entities (`shotgun`, `pistol`,
`chaingun`, `rocket-launcher`, …) — the super shotgun being a beefier double-barrel does not
make `super-shotgun` an Entity. The object-noun `double-barrel-shotgun` is the identity;
`doom:super-shotgun` is its Manifestation.

Disambiguate ambiguous namings at creation, when it is cheap:

| Use | Avoid | Why |
|---|---|---|
| `ammo-shotgun-shell` | `ammo-shell` | shotgun shell vs. artillery shell |
| `projectile-rocket` | `rocket` | the projectile, or the weapon? |

## A Manifestation references every object-identity it fills

Recognition is voluntary and receiver-side: a Manifestation references the object-identity nouns
the thing *is*, coarse and fine, and a receiving game binds the finest rung it understands.
`botw:master-sword` references both `master-sword` (the specific identity) and `sword` (the
genus) — a Zelda-aware game binds `master-sword`, a generic game binds `sword`, Scrabble binds
nothing. A genuinely dual-natured object (a sword-cane) references two identities (`cane`,
`sword`).

This is distinct from **roles**, which are many and go on tags: `minecraft:oak-plank` references
the object-identity `plank`, and carries `category: building-block` and `category: resource`. A
building game queries the building-block tag; a crafting game queries the resource tag. Multiple
Entity references mean multiple *identities*; multiple roles mean multiple *tags*.

## Authorship is a gradient, not a category

There is **one species of Entity: a signed naming.** Ownerless concepts (`sword`, `potion`) and
authored concepts (`master-sword`, `link`, `cloud`) are the two ends of a single
convergence-concentration gradient, carried by the **signature**, not by structure:

- Ownerless namings are many and interchangeable; the commons converges on one **by use**.
- An authored concept's authoritative naming is the one signed by its author's key — Nintendo's
  `link`. Anyone else's `link` is a rival claim a receiver may refuse; the timestamp and
  signature prove who named it. Convergence concentrates on the author because the world
  recognizes the author, not because the protocol enforces it.

This is what answers *"Cloud is the same every time, yet represented differently every time"*:
one `cloud` Entity (gravity concentrated on Square), many per-game Manifestations. A concept may
**migrate** along the gradient over time — Sherlock Holmes, Dracula, and Robin Hood drifted from
authored toward folklore — with no change to the data model. Keep namings free of IP
*misrepresentation* (don't pass off a non-author's naming as authoritative), but an authored
naming is a perfectly good Entity. See
[aems-schema ADR-0007](https://github.com/enduring-game-standard/aems-schema/blob/main/docs/adr/0007-an-entity-event-is-a-naming-not-the-concept.md).

## What transfer delivers

AEMS transfers **identity, provenance, possession, and condition — never behavior or
fitness-for-use.** A receiving game reads "this player provably possesses X, descending from
naming Y, in condition Z," and decides what to do. Cross-game import is a spectrum:

- **Re-instantiation** — the receiver shares the use (Master Sword → Skyrim: a working sword).
- **Cosmetic / trophy** — the receiver knows the object but has no use for it (Master Sword →
  Animal Crossing: a wall display).
- **Refusal / inert token** — the receiver shares no identity rung (Estus Flask → Galaga:
  nothing, absent a designer's arbitrary fiat).

Trophies and refusals are correct, the way a chess Knight does nothing at a poker table. And
**agent-entities** (`monster`, `boss`, a character like Samus) and **abilities** (`scan`) can be
named for provenance but cannot transfer their substance — behavior, which is MAPS/RUNS.
Importing Samus delivers her identity and a skin, not her moveset; importing Scan delivers the
orb, not the reveal-power.

## Thin Entity layers are correct

The more a game's identity lives in its mechanics rather than its objects, the thinner its AEMS
layer and the richer its MAPS layer. Chess is six piece-nouns and a deep rulebook; that thin
Entity layer is the right answer, not a sign the conventions failed.

## Open questions

1. **Constitutive object-identity.** A chess Knight's identity is arguably its movement rule
   (a MAPS Pattern), not its shape — unlike Cloud, whose identity is *not* his moveset. Does
   object-identity hold for constitutive pieces, or do they fuse identity with mechanic?
2. **Agent-entity granularity.** Is `monster` the right grain for all hostile creatures, or do
   `melee-enemy` / `ranged-enemy` warrant separate namings? Since agents transfer only as
   provenance plus a skin, the stakes are low — but the grain is unsettled.

---

*Part of [AEMS Conventions](./README.md). See [ADR-0002](./docs/adr/0002-entity-names-object-identity.md)
and [aems-schema ADR-0007](https://github.com/enduring-game-standard/aems-schema/blob/main/docs/adr/0007-an-entity-event-is-a-naming-not-the-concept.md).*

**MIT License** — Open for use, adaptation, and critique.
