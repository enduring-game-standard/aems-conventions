# AEMS Conventions

**Shared patterns for interoperable AEMS events.**

These conventions are optional, community-maintained guidelines for structuring [AEMS](https://github.com/enduring-game-standard/aems-standard) events. They are not part of the core protocol. The standard defines the four event kinds and their architecture; these conventions define the shared vocabulary that helps independent implementations render and process the same entities consistently.

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

### 3. Grouping Tags

Use lightweight tags for cross-client filtering:
- `["category", "..."]` — broad domain (e.g., "weapon", "enemy", "playing-card")
- `["type", "..."]` / `["subtype", "..."]` — hierarchical classification (e.g., "melee" / "bladed")
- `["origin", "..."]` — lore or franchise source (e.g., "zelda", "doom")

These tags enable clients to query for "all weapons" or "all entities from Zelda" without parsing event content.

### 4. Property Consistency in Manifestation Content

Where possible, use common property names in Manifestation `content` across domains:

| Domain | Common Properties |
|--------|-------------------|
| Combat | `damage`, `durability`, `health`, `speed` |
| Visual | `model`, `image_url`, `texture`, `sprite_front` |
| Audio | `sound_attack`, `sound_death`, `sound_hit` |

Consistent property names matter because they enable cross-client rendering and shared processing. A client that knows how to display `image_url` can render entities from any publisher that follows this convention, without game-specific adapters.

### 5. Iconicity Threshold for Entities

Not every variant deserves its own Entity:

- **Broad, generic concepts** → one Entity. A `sword` Entity covers iron swords, steel swords, diamond swords. Variations in material, stats, and visuals go in Manifestations.
- **Iconic or distinct-class concepts** → separate Entity. A `master-sword` or `energy-sword` gets its own Entity when it has persistent lore, identity, or core mechanics that are not reducible to stat variants.

The test: if removing the specific identity would lose something meaningful (lore, unique mechanics, cross-media recognition), it warrants a separate Entity.

## Example: Convention-Compliant Events

A generic sword Entity and a game-specific Manifestation, following all five conventions:

**Entity (Universal Archetype)**
```json
{
  "kind": 30050,
  "tags": [
    ["d", "sword"],
    ["name", "Sword"],
    ["category", "weapon"],
    ["type", "melee"],
    ["subtype", "bladed"]
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

The Entity is universal and immutable. The Manifestation is scoped to Minecraft, references the Entity via provenance tag, uses namespaced `d`-tag, and follows common property names. A different game can publish its own Manifestation of the same Entity with entirely different stats and visuals.

## Domain-Specific Conventions

Each domain file provides worked examples and recommended patterns:

- [playing-cards.md](./playing-cards.md) — Standard decks, rendering properties, composition.
- [weapons.md](./weapons.md) — Generic vs. legendary/distinct weapons, material variants, cross-genre adaptation.
- [enemies.md](./enemies.md) — Archetypal monsters across game eras, behavioral properties.

---

*Part of the [Enduring Game Standard](https://github.com/enduring-game-standard). See [AEMS Standard](https://github.com/enduring-game-standard/aems-standard) for the core protocol, [RUNS](https://github.com/enduring-game-standard/runs-standard) for game execution, [MAPS Notation](https://github.com/enduring-game-standard/ludic-notation-standard) for design notation, and [WOCS](https://github.com/enduring-game-standard/wocs-standard) for coordination.*

**MIT License** — Open for use, adaptation, and critique.
