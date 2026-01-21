# AEMS Conventions

**Community Guidelines for Interoperable AEMS Implementations**

This repository collects optional, community-driven conventions for using AEMS. These are **not part of the core protocol**—the standard remains deliberately minimal to avoid over-specification.

## Why Conventions?

AEMS defines only the four event kinds and basic structure, leaving much unspecified (rendering, namespacing, composites, etc.). This minimalism ensures neutrality and flexibility.

However, for real-world interoperability—e.g., different clients/games rendering the same deck consistently—shared conventions are essential. These guidelines:
- Enhance discoverability and consistency.
- Remain fully optional (games/clients can ignore or fork them).
- Focus on patterns that have emerged from early implementations (e.g., playing cards, weapons, enemies).
- Demonstrate how variations per use case are a feature, not a bug.

Conventions are split into:
- **Universal** (apply broadly).
- **Domain-specific** (per file, with examples).

## Universal Conventions

These are recommended for all AEMS events to maximize interoperability:

1. **Mandatory `entity` Tag in Manifestations**  
   Always include `["entity", "<entity_event_id>", "<entity_d_value>"]`.  
   - The event ID ensures provenance.  
   - The `d` value aids readability/querying.  
   - Rationale: Loose coupling for fan extensions/reskins.

2. **Namespacing via `d`-Tags**  
   - Prefix Entities with domain/indication: e.g., `std:` for community-ratified universals.  
   - Prefix Manifestations with style/theme: e.g., `classic:`, `modern:`, `{creator}:`.  
   - Avoid collisions through reputation/pubkey.

3. **Grouping Tags**  
   Use lightweight tags for filtering:  
   - `["category", "..."]` (e.g., "weapon", "enemy").  
   - `["type", "..."]` / `["subtype", "..."]` for hierarchies.  
   - `["origin", "..."]` for lore/source.

4. **Property Consistency**  
   Where possible, use common property names in Manifestation `content` (e.g., `health`, `damage`, `model`, `image_url`).

5. **Iconicity/Distinct Class Threshold for Entities**  
   - Broad/generic concepts: One Entity (e.g., "sword").  
   - Iconic/distinct: Separate Entity (e.g., "master-sword", "energy-sword") if it has persistent lore/mechanics not reducible to variants.

Domain-specific files below provide concrete examples and variations.

## Domain-Specific Conventions (so far)

- [playing-cards.md](./playing-cards.md) — Standard decks, rendering, composition.
- [weapons.md](./weapons.md) — Swords vs. legendary/distinct weapons.
- [enemies.md](./enemies.md) — Monsters like Pinky Demon across game eras.

Contributions welcome—open issues/PRs for new domains or refinements!

**MIT License** — These conventions are free to use/adapt.
