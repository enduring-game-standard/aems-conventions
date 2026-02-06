# Playing Cards Conventions

These optional conventions enable consistent rendering and use of playing cards across clients/games (poker, solitaire, etc.).

## Reference Canonical Publication
Base on the canonical French 52 deck:  
https://github.com/wScottSh/aems-french-52-deck  
(Pinned Entity event IDs listed there.)

## Conforming to Universal Conventions
Examples below:
- Use mandatory `entity` tags.
- Namespacing: `stdcard:{rank}-{suit}`, `stddeck:french-52`.
- Grouping: `["category", "playing-card"]`, `["suit", "spades"]`, etc.

## Recommended Conventions

1. **Per-Card Entities**  
   One Entity per unique rank/suit (semantic distinction—ace-spades behaves differently from king-hearts in most games).

2. **Rendering Properties** (in Manifestation `content`)  
   - `front_image_url`  
   - `back_image_url` (optional per-card override)  
   - `front_text` (fallback, e.g., "A♠")  
   - `symbol` and `color` (for text rendering)  

3. **Face-Up/Down Rules**  
   - Face-up: Use card Manifestation's front.  
   - Face-down: Use deck Manifestation's back (with per-card override).  

4. **Composition**  
   Optional: Repeated `["contains", "stdcard:{rank}-{suit}", "1"]` tags on deck Entity.  
   Alternative: Emergent from State events (preferred for instances).

## Example: Conforming Manifestation (Text Style)

```json
{
  "kind": 30051,
  "tags": [
    ["d", "text:ace-spades"],
    ["entity", "<stdcard_ace-spades_entity_id>", "stdcard:ace-spades"],
    ["category", "playing-card"]
  ],
  "content": {
    "front_text": "A♠",
    "symbol": "♠",
    "color": "black"
  }
}
