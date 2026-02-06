# Enemies Conventions

These optional conventions enable consistent representation and rendering of enemy entities (monsters, NPCs, bosses) across clients and games, particularly in FPS, RPG, or action titles. This allows shared archetypes (e.g., a recognizable monster) to be imported while supporting era-specific or game-specific variations.

## Reference Canonical Publications
Conventions are illustrated with the Pinky Demon from Doom as a reference example. Implementers are encouraged to publish their own canonical Entity events in dedicated repos (e.g., `aems-doom-enemies`) and pin event IDs for discoverability.

## Conforming to Universal Conventions
Examples below:
- Use the mandatory `entity` tag in all Manifestations.
- Namespacing: `pinky-demon` for the universal Entity; prefixes like `classic:`, `doom3:`, `modern:` for Manifestations.
- Grouping tags: `["category", "enemy"]`, `["family", "demon"]`, `["origin", "doom"]`.

## Recommended Conventions

1. **Single Universal Entity per Distinct Archetype**  
   Create one immutable Entity for the timeless, lore-based concept. Avoid over-generalizing (e.g., do **not** merge Pinky Demon into "generic-melee-demon"). Use the Iconicity/Distinct Class Threshold:  
   - Separate Entity if the enemy has persistent identity/lore across media (e.g., Pinky Demon, Cacodemon, Zombie).  
   - Broader Entity only for truly interchangeable types (rare for enemies).

2. **Manifestations for Era/Game-Specific Variations**  
   - Visuals/audio assets.  
   - Stats (health, speed, damage).  
   - Behavioral properties (AI quirks, attack patterns).  
   Classic-era assets can share one Manifestation if identical.

3. **Common Properties in Manifestation `content`**  
   - Health/stats: `health`, `speed`, `damage_*`.  
   - Visuals: `model`, `sprite_*`, `texture`, `animation_*`.  
   - Audio: `sound_pain`, `sound_death`, `sound_attack`.  
   - Optional: `weakness` (array), `behavior` hints for clients.

4. **State for Instances**  
   Mutable data (position, current_health, state) is tracked via State events referencing the player's Asset.

## Example: Universal Entity (Pinky Demon)

```json
{
  "kind": 30050,
  "tags": [
    ["d", "pinky-demon"],
    ["name", "Pinky Demon"],
    ["alt", "Pinkie", "Bull Demon"],
    ["category", "enemy"],
    ["family", "demon"],
    ["origin", "doom"]
  ],
  "content": {
    "description": "A fast-moving, aggressive demon that charges directly at targets and attacks with powerful bites. Known for its distinctive pink skin and bulldog-like charge."
  }
}
```

## Example: Conforming Manifestation (Classic Doom)

```json
{
  "kind": 30051,
  "tags": [
    ["d", "classic:pinky-demon"],
    ["entity", "<pinky-demon_entity_id>", "pinky-demon"],
    ["game", "doom-classic"],
    ["style", "sprite-2d"]
  ],
  "content": {
    "health": 400,
    "speed": 10,
    "damage_bite": 20,
    "sprite_front": "https://.../PINKA1.png",
    "sprite_side": "https://.../PINKB0B8.png",
    "sprite_attack": "https://.../PINKC0.png",
    "sound_pain": "https://.../dspains.wav",
    "sound_death": "https://.../dspdeth.wav"
  }
}
```

## Example: Conforming Manifestation (Doom 2016/Eternal)

```json
{
  "kind": 30051,
  "tags": [
    ["d", "modern:pinky-demon"],
    ["entity", "<pinky-demon_entity_id>", "pinky-demon"],
    ["game", "doom-2016"],
    ["style", "3d-pbr"]
  ],
  "content": {
    "health": 1200,
    "speed": 15,
    "damage_bite": 45,
    "model": "https://.../pinky.gltf",
    "material_albedo": "https://.../pinky_albedo.png",
    "animation_charge": "https://.../charge.anim",
    "animation_glorykill": "https://.../glory_punch.anim"
  }
}
```

## Variations (AEMS Flexibility Feature)

- **Shared Classic Manifestation**: Doom 1, Doom 2, and mods can all reference the same `classic:pinky-demon` Manifestation (identical assets/stats).
- **Fan Reskin**: A community member publishes a new Manifestation (e.g., `fan:green-pinky`) referencing the same Entity—clients can optionally load it.
- **Different Genres**: An RPG client interprets the same Entity with turn-based stats; an FPS client uses real-time behaviors.
- **Boss Variants**: A "Cyber-Pinky" could be a separate Entity if sufficiently distinct, or a Manifestation with special properties (e.g., `cybernetic: true`).

This enables one canonical archetype to appear consistently recognizable yet adapt perfectly to each game's engine and style—true interoperability.
