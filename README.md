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
four event kinds — Entity, Manifestation, Asset, State — and the provenance chain between
them. Compliance is exactly that and nothing more. **Follow the schema and you are using
AEMS.**

This repo is the **conventions layer** above that kernel. A convention is a voluntarily
adopted shared answer to a question the schema deliberately leaves open — what events
*carry*, how they are named, how a community converges on shared namings. **Follow a
convention and you are shaping for interoperability.** Conventions are opinionated by
design: they prescribe, recommend one shape over another, and render verdicts. What is
voluntary is *adopting* them. Ignoring every convention here is still compliant AEMS — the
way a taped-up ball is still soccer. Forking them is expected.

Because AEMS events are plain-text Nostr events, conventions are discoverable by relay
query and publishable by anyone without permission. No one ratifies them; a convention
earns its standing only by being adopted.

## Why Conventions?

The schema is silent on what events carry beyond the provenance chain, and that silence
is deliberate. But for two independent clients to read the same deck of cards — or the
same weapon — without writing custom parsing for every publisher, they need shared
assumptions about how those events are structured. Conventions provide them:

- **Discoverability** — consistent tags and namespacing make entities queryable across
  clients.
- **Consistency** — common property names let clients read and process entities from
  different publishers without custom parsing.
- **Flexibility** — games and clients adopt, ignore, or fork any convention. Variation
  across implementations is expected, not exceptional.

Conventions standardize *where the data sits*, never what a client does with it. How a
weapon looks on screen is the client's business; how it behaves in play is RUNS's. The
conventions only make the underlying events legible to whoever chooses to read them.

## The Three Tiers

| Tier | Lives in | Binding? |
|------|----------|----------|
| **Protocol** — the four event kinds and the provenance chain | [aems-schema](https://github.com/enduring-game-standard/aems-schema) | Mandatory for compliance |
| **Conventions** — tag and property vocabularies, naming, namespacing | this repo | Optional, recommended |
| **Ecosystem** — published Entities, Manifestations, decks, content | the commons (Nostr events) | Permissionless |

## Universal Conventions

These patterns apply broadly across AEMS events.

### 1. Reference your Entity legibly

The schema already requires every Manifestation to reference at least one parent Entity —
this is a kernel rule, not a convention, and it holds whether or not you adopt anything
here. The *convention* is to make that reference legible: alongside the Entity's
machine reference, include its human-readable `d` value so the event can be read and
relay-queried without resolving the parent first.

```json
["entity", "<entity reference>", "<entity d value>"]
```

The loose coupling this reference provides — a Manifestation extends an Entity it does not
own — is what lets fan extensions and reskins exist without touching the original Entity.

### 2. Namespace with umbrella prefixes for readability

Prefix a `d`-tag with the game or creator it is scoped to, so the name reads clearly:

- Manifestations: `minecraft:iron-sword`, `botw:master-sword`, `{creator}:...`
- Style or theme variants: `classic:`, `modern:`, `text:`

Prefixes are **readability convenience, never identity**. Identity is cryptographic: a
publisher's key plus the `d`-tag. Two publishers can use the same prefix and their events
stay distinct, because references resolve by the signed event, not by the string. There is
no blessed or "standard" namespace — a naming becomes broadly shared only by being widely
referenced (convergence by use; see [aems-schema ADR-0007](https://github.com/enduring-game-standard/aems-schema/blob/main/docs/adr/0007-an-entity-event-is-a-naming-not-the-concept.md)),
never by wearing a prefix. A bare, unprefixed `d` (`sword`) is the convention for a naming
intended as broadly shared.

### 3. Put classification on Manifestations

Grouping tags exist for cross-event discovery — they are how a client queries relays for
"all weapons" or "everything from this game." Carry them on **Manifestations**:

- `["category", "..."]` — broad domain (`weapon`, `enemy`, `playing-card`)
- `["type", "..."]` / `["subtype", "..."]` — hierarchical classification (`melee` / `bladed`)
- `["origin", "..."]` — lore or franchise source (`zelda`, `doom`)
- `["game", "..."]` — specific game (`minecraft`, `doom-classic`)

An Entity carries only its naming — `d`, `name`, and a `content` description. An Entity is
a dictionary entry: it carries the word and its definition, not the shelving codes a
library files it under. Classification is what the *users* of a naming do with it, so it
lives on the Manifestation. Discovery works by querying Manifestations by tag, then reading
up the chain to their parent Entities. See
[entity-abstraction.md](./entity-abstraction.md) for the reasoning.

### 4. Reuse common property names

Where the data lives in a Manifestation's `content` — read once a client already has the
event — reuse common property names across domains so a client can render or process
without a per-publisher adapter:

| Domain | Common properties |
|--------|-------------------|
| Combat | `damage`, `durability`, `health`, `speed` |
| Visual | `model`, `image_url`, `texture`, `sprite_front` |
| Audio | `sound_attack`, `sound_death`, `sound_hit` |

A client that knows how to read `image_url` can pull the art for entities from any
publisher that follows the convention, with no game-specific code.

### 5. Keep Entities IP-agnostic

Name Entities for what something *is* in universal terms — `sword`, `monster`,
`healing-item` — never for a trademarked character or franchise-specific item. Anything
owned by someone becomes a **Manifestation** that references a generic Entity and carries
the franchise attribution itself: the Master Sword is a Manifestation of `sword`, the
Pinky Demon a Manifestation of `monster`.

An Entity published to the commons is permanent, signed, and built against by others. An
IP-agnostic Entity is infrastructure anyone can reference without inheriting someone's
trademark; naming the trademark as an Entity would stamp it permanently into the commons
layer. See [entity-abstraction.md](./entity-abstraction.md) for the full guidance.

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

- [entity-abstraction.md](./entity-abstraction.md) — guidance for deciding when a concept
  is worth its own Entity naming versus a Manifestation of a broader one. *(Work in
  progress.)*
- [playing-cards.md](./playing-cards.md) — standard decks, rendering properties,
  composition.

---

*Part of the [Enduring Game Standard](https://github.com/enduring-game-standard). See the
[AEMS schema](https://github.com/enduring-game-standard/aems-schema) for the core protocol,
[RUNS](https://github.com/enduring-game-standard/runs-spec) for game execution,
[MAPS](https://github.com/enduring-game-standard/maps-notation) for design notation, and
[WOCS](https://github.com/enduring-game-standard/wocs-protocol) for coordination. Canonical
acronym expansions live in the [EGS Glossary](https://github.com/enduring-game-standard/.github/blob/main/profile/README.md#glossary).*

**MIT License** — Open for use, adaptation, and critique.
