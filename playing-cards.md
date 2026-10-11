# Playing Cards Conventions

> **Status**: Conceptual example. This file shows the *shape* of a playing-card convention
> under the [universal conventions](./README.md#universal-conventions). It is not a
> reference artifact, and nothing here is canonical. It may be wrong.

These optional conventions let clients and games (poker, bridge, solitaire) read card events
from different publishers the same way. They say where card data sits. What a client draws,
and when, is the client's business.

## Entities: one per card

Name one Entity per rank and suit, with a bare, unprefixed `d` value (universal convention
#2):

- Ranks: `ace`, `2` to `10`, `jack`, `queen`, `king`
- Suits: `spades`, `hearts`, `diamonds`, `clubs`
- `d` values: `ace-spades`, `10-hearts`, `queen-diamonds`

Each card is its own Entity because rank-and-suit *is* its identity. Playing cards are the
medium exception in [entity-abstraction.md](./entity-abstraction.md). Strip the card from the
Ace of Spades and nothing recognizable remains. The reason is not behavior. An ace is high in
one game and low in another, and those rules belong to the game.

A card Entity carries only its naming. That is a `d` tag, a `name` tag, and a `content`
description (universal convention #3).

```json
{
  "kind": "<AEMS Entity kind>",
  "tags": [
    ["d", "ace-spades"],
    ["name", "Ace of Spades"]
  ],
  "content": {
    "description": "The ace of the spades suit in a French-suited deck."
  }
}
```

## Decks are composition, not an Entity's tags

Which cards make up a deck is composition. The AEMS schema excludes composites and leaves
them to the application layer
([AEMS schema, What AEMS Deliberately Excludes](https://github.com/enduring-game-standard/aems-schema#what-aems-deliberately-excludes)).
No convention here lists a deck's cards on any event. A game or client that needs the
52-card deck assembles it from the 52 card Entities.

## Manifestations: one per card per style

A style publishes one Manifestation per card. Each one references its card Entity and
carries the style's data for that card.

Tags:

- `d` with the style prefix, such as `text:ace-spades` or `classic:ace-spades` (universal
  convention #2)
- the Entity reference with its readable `d` value (universal convention #1)
- `["category", "playing-card"]`, `["suit", "spades"]`, and `["rank", "ace"]`

These tag names have more than one letter, so a client filters on them after it fetches the
events. Relays do not index them (universal convention #3).

Properties in `content`:

| Property | Holds |
|----------|-------|
| `image_url` | the card face, the same property universal convention #4 names for art |
| `back_image_url` | the card back. The common vocabulary has no name for a second image, so this convention adds one. |
| `text` | a plain-text face, such as `A♠` |
| `symbol` | the suit symbol, such as `♠` |
| `color` | the suit color, `black` or `red` |

A style with one shared back repeats the same `back_image_url` on every card.

```json
{
  "kind": "<AEMS Manifestation kind>",
  "tags": [
    ["d", "text:ace-spades"],
    ["entity", "<ace-spades entity reference>", "ace-spades"],
    ["category", "playing-card"],
    ["suit", "spades"],
    ["rank", "ace"]
  ],
  "content": {
    "text": "A♠",
    "symbol": "♠",
    "color": "black"
  }
}
```

## An ecosystem artifact

The [aems-french-52-deck](https://github.com/wScottSh/aems-french-52-deck) repo publishes a
signed French-suited deck. It is an ecosystem artifact, not part of these conventions. Its
fidelity is unverified, and it does not follow these conventions yet:

- Its events use kinds 30001 and 30002. NIP-51 already assigns those kinds to lists (30001
  is a deprecated list kind, and 30002 is relay sets). AEMS kinds are not pinned yet.
- Its Manifestations reference card Entities by a bare `d` string, not by an event
  reference.
- It prefixes its card and deck namings as if they belonged to a standard namespace. There
  is no standard namespace (universal convention #2).
- It lists the deck's cards on its deck event, which these conventions leave to the
  application layer.

This repo will record a deck by its event references once one is published that follows
the conventions, and once use shows that it is the one people reference
([ADR-0001](./docs/adr/0001-conventions-index-not-vault.md)).

---

*Part of [AEMS Conventions](./README.md).*

**MIT License** — Open for use, adaptation, and critique.
