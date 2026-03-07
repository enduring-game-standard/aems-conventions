# AEMS Conventions

**Shared patterns for interoperable AEMS events.**

These conventions are optional, community-maintained guidelines for structuring [AEMS](https://github.com/enduring-game-standard/aems-schema) events. They are not part of the core protocol. The standard defines the four event kinds and their architecture; these conventions define the shared vocabulary that helps independent implementations render and process the same entities consistently.

Because AEMS events are plain-text Nostr events, conventions are discoverable via relay queries and publishable by anyone without permission. Adopting them is voluntary. Forking them is expected.

## Why Conventions?

AEMS deliberately leaves much unspecified: rendering, namespacing, composites, property names. This minimalism preserves neutrality. But when two clients need to render the same deck of cards or the same weapon, they need shared assumptions about how those events are structured.

Conventions provide those shared assumptions:

- **Discoverability**: Consistent tags and namespacing make entities queryable across clients.
- **Consistency**: Common property names let clients render entities from different publishers without custom parsing.
- **Flexibility**: Games and clients can adopt, ignore, or fork any convention. Variation across implementations is expected, not exceptional.

Conventions are organized into:
- **Universal** — apply broadly to all AEMS events.
- **Domain-specific** — per-domain files with worked examples (playing cards, weapons, enemies).

## Universal Conventions

These patterns are recommended for all AEMS events to maximize interoperability:

### 1. Mandatory `entity` Tag in Manifestations

Always include `["entity", "<entity_event_id>", "<entity_d_value>"]` in every Manifestation.

- The event ID ensures provenance: any client can verify which Entity a Manifestation extends.
- The `d` value aids readability and relay querying.
- Loose coupling via this tag enables fan extensions and reskins without modifying the original Entity.

### 2. Namespacing via `d`-Tags

Prefix Entities with domain indicators:
- `std:` for community-ratified universals (e.g., `std:sword`, `std:playing-card`)
- Game or creator prefixes for scoped implementations (e.g., `minecraft:`, `botw:`)

Prefix Manifestations with style or theme indicators:
- `classic:`, `modern:`, `text:`, or `{creator}:` prefixes

Collisions are avoided through Nostr pubkey identity: two publishers can use the same `d`-tag prefix, but their events are distinguishable by pubkey.

### 3. Grouping Tags (Manifestations Only)

Use lightweight tags on **Manifestations** for cross-client filtering:
- `["category", "..."]` — broad domain (e.g., "weapon", "enemy", "playing-card")
- `["type", "..."]` / `["subtype", "..."]` — hierarchical classification (e.g., "melee" / "bladed")
- `["origin", "..."]` — lore or franchise source (e.g., "zelda", "doom")
- `["game", "..."]` — specific game (e.g., "minecraft", "doom-classic")

Entities have **no tags** beyond `d` and `name`. Discovery works by searching Manifestations by tags, then reducing to their parent Entities. See [Entity Abstraction Conventions](./entity-abstraction.md) for the rationale.

### 4. Property Consistency in Manifestation Content

Where possible, use common property names in Manifestation `content` across domains:

| Domain | Common Properties |
|--------|-------------------|
| Combat | `damage`, `durability`, `health`, `speed` |
| Visual | `model`, `image_url`, `texture`, `sprite_front` |
| Audio | `sound_attack`, `sound_death`, `sound_hit` |

Consistent property names matter because they enable cross-client rendering and shared processing. A client that knows how to display `image_url` can render entities from any publisher that follows this convention, without game-specific adapters.

### 5. IP Boundary Rule

Not every concept deserves its own Entity:

- **Broad, generic concepts** → one Entity. A `sword` Entity covers iron swords, steel swords, diamond swords. Variations in material, stats, and visuals go in Manifestations.
- **Mechanically distinct concepts** → separate Entity via the four-test rubric. An `energy-sword` has a property cluster ({plasma blades, battery, lunge, energy depletion}) distinct from `sword`. Test 4 (Mechanical Signature) catches these without requiring cultural recognition.
- **IP-specific concepts** → **always Manifestations**, never Entities. A `master-sword` is mechanically just a strong sword and is trademarked by Nintendo. It becomes a Manifestation of `sword`. The Pinky Demon becomes a Manifestation of `monster`. The BFG 9000 becomes a Manifestation of `super-weapon`.

Entities are IP-agnostic commons infrastructure. If the name is owned by someone, it cannot be an Entity. See [Entity Abstraction Conventions](./entity-abstraction.md) for the full IP Boundary Principle and the four-test rubric.

## Example: Convention-Compliant Events

A generic sword Entity and a game-specific Manifestation, following all five conventions:

**Entity (Universal Archetype)**
```json
{
  "kind": 30050,
  "tags": [
    ["d", "sword"],
    ["name", "Sword"]
  ],
  "content": {
    "description": "A handheld bladed weapon consisting of a long blade attached to a hilt, used for slashing or thrusting."
  }
}
```

**Manifestation (Game-Specific Implementation)**
```json
{
  "kind": 30051,
  "tags": [
    ["d", "minecraft:iron-sword"],
    ["entity", "<sword_entity_id>", "sword"],
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
    "sound_swing": "https://.../swing.wav"
  }
}
```

The Entity is universal, immutable, and tag-free. The Manifestation is scoped to Minecraft, references the Entity via provenance tag, uses namespaced `d`-tag, follows common property names, and carries all grouping tags. A different game can publish its own Manifestation of the same Entity with entirely different stats, visuals, and tags.

## Domain-Specific Conventions

Each domain file provides worked examples and recommended patterns:

- [entity-abstraction.md](./entity-abstraction.md) — Four-test rubric for Entity vs. Manifestation decisions, IP Boundary Principle, Entities-as-Roles composition model, and open questions.
- [playing-cards.md](./playing-cards.md) — Standard decks, rendering properties, composition.

---

*Part of the [Enduring Game Standard](https://github.com/enduring-game-standard). See [AEMS Standard](https://github.com/enduring-game-standard/aems-schema) for the core protocol, [RUNS](https://github.com/enduring-game-standard/runs-spec) for game execution, [MAPS Notation](https://github.com/enduring-game-standard/maps-notation) for design notation, and [WOCS](https://github.com/enduring-game-standard/wocs-protocol) for coordination.*

**MIT License** — Open for use, adaptation, and critique.
