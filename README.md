# AEMS Conventions

**Shared, voluntarily adopted answers to the questions the AEMS schema leaves open.**

🏠 **[EGS Overview](https://github.com/enduring-game-standard)** · 📦 **[AEMS](https://github.com/enduring-game-standard/aems-schema)** · 🎯 **[AEMS Conventions](https://github.com/enduring-game-standard/aems-conventions)** · 🔧 **[RUNS](https://github.com/enduring-game-standard/runs-spec)** · 📖 **[RUNS Library](https://github.com/enduring-game-standard/runs-library)** · ⚡ **[WOCS](https://github.com/enduring-game-standard/wocs-protocol)** · 🎼 **[MAPS](https://github.com/enduring-game-standard/maps-notation)** · 🎶 **[MAPS Library](https://github.com/enduring-game-standard/maps-library)** · ❓ **[FAQ](https://github.com/enduring-game-standard/.github/blob/main/profile/FAQ.md)** · 🔤 **[Glossary](https://github.com/enduring-game-standard/.github/blob/main/profile/README.md#glossary)**

---

> **Status**: Design stage. The conventions here are **conceptual examples** —
> illustrations of the *shape* of a shared answer, with no published, verified artifacts
> behind them yet, and possibly wrong. Nothing here is blessed or canonical. When real
> commons artifacts exist, this repo will point at them by reference; it indexes the
> commons, it does not host it (see [ADR-0001](./docs/adr/0001-conventions-index-not-vault.md)).

## What This Repo Is

The [AEMS schema](https://github.com/enduring-game-standard/aems-schema) is the kernel:
four addressable event kinds — Entity, Manifestation, Asset, State — the three parent
references between them, and addressability. Each event's address is its kind, its
publisher's key, and its `d` tag, and the latest event at an address is current.
Compliance is exactly that and nothing more. **Follow the schema and you are using
AEMS.**

This repo is the **conventions layer** above that kernel. A convention is a voluntarily
adopted shared answer to a question the schema deliberately leaves open — what events
*carry*, how they are named, how a community converges on shared namings. **Follow a
convention and you are shaping for interoperability.** Conventions are opinionated by
design: they prescribe, recommend one shape over another, and render verdicts. What is
voluntary is *adopting* them. Ignoring every convention here is still compliant AEMS — the
way a taped-up ball is still soccer. Forking them is expected.

The conventions themselves are prose in this repo, not events. Anyone can write or fork one
without permission. No one ratifies them. A convention earns its standing only when
publishers adopt it.

## Why Conventions?

The schema is silent on what events carry beyond the provenance chain, and that silence
is deliberate. But for two independent clients to read the same deck of cards — or the
same weapon — without writing custom parsing for every publisher, they need shared
assumptions about how those events are structured. Conventions provide them:

- **Discoverability** — consistent tag names and values let clients find related events
  from many publishers.
- **Consistency** — common property names let clients read and process events from
  different publishers without custom parsing.
- **Flexibility** — games and clients adopt, ignore, or fork any convention. Variation
  across implementations is expected, not exceptional.

Conventions standardize *where the data sits*, never what a client does with it. How a
weapon looks on screen is the client's business; how it behaves in play is RUNS's. The
conventions only make the underlying events legible to whoever chooses to read them.

## The Three Tiers

| Tier | Lives in | Binding? |
|------|----------|----------|
| **Protocol** — the four event kinds, the three parent references, and addressability | [aems-schema](https://github.com/enduring-game-standard/aems-schema) | Mandatory for compliance |
| **Conventions** — tag and property vocabularies, naming, namespacing | this repo | Optional, recommended |
| **Ecosystem** — published Entities, Manifestations, decks, content | the commons (Nostr events) | Permissionless |

## Universal Conventions

These patterns apply broadly across AEMS events.

### 1. Reference your Entity legibly

The schema already requires every Manifestation to reference at least one parent Entity —
this is a kernel rule, not a convention, and it holds whether or not you adopt anything
here. The *convention* is to make that reference legible: alongside the Entity's
machine reference, include its human-readable `d` value so a person reading the event
knows the parent without resolving it first.

```json
["entity", "<entity reference>", "<entity d value>"]
```

The tag name `entity` is a placeholder. The schema has not pinned the reference encoding
yet. The readable `d` value helps people, not queries. Relays index only single-letter
tags, and only a tag's first value. A relay filter never matches the third element
([aems-schema ADR-0010](https://github.com/enduring-game-standard/aems-schema/blob/main/docs/adr/0010-what-the-substrate-guarantees.md)).

The loose coupling this reference provides — a Manifestation extends an Entity it does not
own — is what lets fan extensions and new interpretations exist without touching the
original Entity.

### 2. Namespace with umbrella prefixes for readability

Prefix a `d`-tag with the game or creator it is scoped to, so the name reads clearly:

- Manifestations: `minecraft:iron-sword`, `botw:master-sword`, `{creator}:...`
- Style or theme Manifestations: `classic:`, `modern:`, `text:`

Prefixes are **readability convenience, never identity**. An event's address is its kind,
its publisher's key, and its `d`-tag. Two publishers can use the same prefix and their
events stay distinct, because a reference names the publisher's key or the exact event,
not the bare string. There is no blessed or "standard" namespace — a naming becomes
broadly shared only by being widely referenced (convergence by use; see [aems-schema ADR-0007](https://github.com/enduring-game-standard/aems-schema/blob/main/docs/adr/0007-an-entity-event-is-a-naming-not-the-concept.md)),
never by wearing a prefix. A bare, unprefixed `d` (`sword`) is the convention for a naming
intended as broadly shared.

### 3. Put classification on Manifestations

Grouping tags let a client sort events into "all weapons" or "everything from this game."
Carry them on **Manifestations**:

- `["category", "..."]` — broad domain (`weapon`, `enemy`, `playing-card`)
- `["type", "..."]` / `["subtype", "..."]` — hierarchical classification (`melee` / `bladed`)
- `["origin", "..."]` — lore or franchise source (`zelda`, `doom`)
- `["game", "..."]` — specific game (`minecraft`, `doom-classic`)

An Entity carries only its naming — `d`, `name`, and a `content` description. An Entity is
a dictionary entry: it carries the word and its definition, not the shelving codes a
library files it under. Classification is what the *users* of a naming do with it, so it
lives on the Manifestation. Discovery works by finding Manifestations by tag, then reading
up the chain to their parent Entities. See
[entity-abstraction.md](./entity-abstraction.md) for the reasoning.

These tag names have more than one letter, and relays do not index them. A relay answers
a query by kind or by author. The client then filters on `category` or `game` locally,
after it has fetched the events
([aems-schema ADR-0010](https://github.com/enduring-game-standard/aems-schema/blob/main/docs/adr/0010-what-the-substrate-guarantees.md),
item 8). A convention that wants relay-side filtering needs a single-letter tag. As a
conceptual example only, a publisher could add `["t", "weapon"]` alongside `category`.
No convention here adopts a single-letter tag yet (see [Open questions](#open-questions)).

### 4. Reuse common property names

Where the data lives in a Manifestation's `content` — read once a client already has the
event — reuse common property names across domains so a client can render or process
without a per-publisher adapter:

| Domain | Common properties |
|--------|-------------------|
| Combat | `damage`, `durability`, `health`, `speed` |
| Visual | `model`, `image_url`, `texture`, `sprite_front` |
| Audio | `sound_attack`, `sound_death`, `sound_hit` |

A client that knows how to read `image_url` can pull the art from the Manifestations of
any publisher that follows the convention, with no game-specific code.

### 5. Name the thing, and keep namings free of IP misrepresentation

Name an Entity for what a thing *is*. Ownerless concepts like `sword`, `potion`, and
`monster` are Entities. Authored concepts like `master-sword`, `link`, and `cloud` are
Entities too. The protocol treats both the same, and the signature carries the authorship
([aems-schema ADR-0007](https://github.com/enduring-game-standard/aems-schema/blob/main/docs/adr/0007-an-entity-event-is-a-naming-not-the-concept.md),
2026-06-13 Amendment). A use like `healing-item` names what a thing is *for*, so it is a
Manifestation tag value (#3), not an Entity. A game's interpretation is a **Manifestation**. Breath of the Wild's
Master Sword is `botw:master-sword`, and it references both `master-sword` and `sword`.

The convention is to keep namings free of IP *misrepresentation*. Do not pass off a
non-author's naming of an authored concept as the author's. Anyone may name `link`, and
that naming is a rival claim. The author's signed naming is the one a receiver can check
against the author's key. Others build against a signed Entity, so a misleading naming
spreads as far as its references do. See [entity-abstraction.md](./entity-abstraction.md)
for the full guidance.

## Example: Conventions in Use

A generic `sword` Entity and a game-specific Manifestation of it. Kind numbers and the
exact parent-reference encoding are not yet pinned by the schema; examples use named
placeholders.

**Entity** — a naming of the concept

```json
{
  "kind": "<AEMS Entity kind>",
  "tags": [
    ["d", "sword"],
    ["name", "Sword"]
  ],
  "content": {
    "description": "A handheld bladed weapon with a long blade attached to a hilt, used for slashing or thrusting."
  }
}
```

**Manifestation** — one game's interpretation

```json
{
  "kind": "<AEMS Manifestation kind>",
  "tags": [
    ["d", "minecraft:iron-sword"],
    ["entity", "<sword entity reference>", "sword"],
    ["category", "weapon"],
    ["type", "melee"],
    ["subtype", "bladed"],
    ["game", "minecraft"]
  ],
  "content": {
    "material": "iron",
    "damage": 6,
    "durability": 250,
    "model": "https://.../iron_sword.png",
    "sound_attack": "https://.../swing.wav"
  }
}
```

The Entity carries only its naming. The Manifestation is scoped to one game, references
the Entity legibly, classifies itself for discovery on tags, and reuses common property
names in content. A different game publishes its own Manifestation of the same Entity with
entirely different stats, art, and tags.

## Domain Conventions

Per-domain files carry worked examples and recommended patterns:

- [entity-abstraction.md](./entity-abstraction.md) — what an Entity names (object-identity)
  and what belongs on the Manifestation instead. It implements
  [ADR-0002](./docs/adr/0002-entity-names-object-identity.md).
- [playing-cards.md](./playing-cards.md) — a conceptual example for the French-suited
  52-card deck. It covers per-card Entities and where card data sits.

## Open questions

No convention here covers these yet. The schema leaves them open too.

- **Transfer.** An Asset's address includes its owner's key, so a new owner cannot publish
  at the old address. A transfer produces a new Asset under the new owner's key. No
  convention yet links the new Asset to the old one, or to the old Asset's State
  ([aems-schema ADR-0010](https://github.com/enduring-game-standard/aems-schema/blob/main/docs/adr/0010-what-the-substrate-guarantees.md),
  item 7). Until one exists, no document here cites a transfer as evidence.
- **State history.** The kernel promises the current event at an address, not its earlier
  versions. Relays may discard older versions. A history exists only where someone archives
  it, or where a publisher gives each State its own address. Which practice to recommend is
  open (ADR-0010, item 3).
- **State vocabulary.** No convention names what a State carries. Early schema drafts
  sketched `charges_remaining` and `condition` State properties and an `acquired` tag on the
  Asset. Those left the schema and landed nowhere.
- **Relay-side classification.** Classification tags like `category` and `game` filter on
  the client (#3). Which single-letter tag, if any, should carry classification for
  relay-side queries is open (ADR-0010, item 8).
- **Smaller vocabulary gaps.** No convention yet covers typed property values, a display
  name on a Manifestation, or a readable `d` value on `manifestation` and `asset`
  references (#1 covers only the `entity` reference).

---

*Part of the [Enduring Game Standard](https://github.com/enduring-game-standard). See the
[AEMS schema](https://github.com/enduring-game-standard/aems-schema) for the core protocol,
[RUNS](https://github.com/enduring-game-standard/runs-spec) for game execution,
[MAPS](https://github.com/enduring-game-standard/maps-notation) for design notation, and
[WOCS](https://github.com/enduring-game-standard/wocs-protocol) for coordination. Canonical
acronym expansions live in the [EGS Glossary](https://github.com/enduring-game-standard/.github/blob/main/profile/README.md#glossary).*

**MIT License** — Open for use, adaptation, and critique.
