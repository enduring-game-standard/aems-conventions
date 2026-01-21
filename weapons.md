# Weapons Conventions

These optional conventions enable consistent representation and rendering of weapons across clients and games, particularly in RPGs, FPS, action, or strategy titles. This allows shared archetypes (e.g., a recognizable sword or gun) to be imported while supporting material, era-specific, or game-specific variations.

## Reference Canonical Publications
Conventions are illustrated with examples for generic swords, legendary swords (Master Sword from Zelda), and distinct-class weapons (Energy Sword from Halo). Implementers are encouraged to publish their own canonical Entity events in dedicated repos (e.g., `aems-rpg-weapons`, `aems-fps-weapons`) and pin event IDs for discoverability.

## Conforming to Universal Conventions
Examples below:
- Use the mandatory `entity` tag in all Manifestations.
- Namespacing: Broad generics like `sword`; iconic/distinct like `master-sword`, `energy-sword`.
- Grouping tags: `["category", "weapon"]`, `["type", "melee"]` or `["type", "ranged"]`, `["subtype", "bladed"]` or `["subtype", "energy"]`, `["origin", "zelda"]` or `["origin", "halo"]`.

## Recommended Conventions

1. **Iconicity/Distinct Class Threshold for Entities**  
   - **Broad/generic concepts**: One universal Entity (e.g., `sword`, `pistol`). Variations in material, stats, or visuals go in Manifestations.  
   - **Separate Entity** for iconic or distinct-class weapons:  
     - Persistent lore/identity across media (e.g., Master Sword).  
     - Unique core mechanics not reducible to stats/materials (e.g., Energy Sword's lunge, energy depletion, plasma blades).  
   This preserves essence without over-proliferation.

2. **Manifestations for Variations**  
   - Materials/stats (iron vs. steel, damage values).  
   - Visuals/audio assets.  
   - Game-specific mechanics (durability, special abilities).

3. **Common Properties in Manifestation `content`**  
   - Stats: `damage`, `damage_slash`, `damage_thrust`, `durability`, `battery` (for energy).  
   - Visuals: `model`, `image_url`, `texture`, `animation_*`.  
   - Audio: `sound_swing`, `sound_activate`, `sound_hit`.  
   - Optional: `material`, `special_ability` (array), `glows: true`.

4. **State for Instances**  
   Mutable data (condition, ammo/battery, enchantments) references a Manifestation.

## Example: Universal Entity (Generic Sword)

```json
{
  "kind": 30001,
  "tags": [
    ["d", "sword"],
    ["name", "Sword"],
    ["category", "weapon"],
    ["type", "melee"],
    ["subtype", "bladed"]
  ],
  "content": {
    "description": "A handheld bladed weapon consisting of a long blade attached to a hilt, primarily used for slashing or thrusting in combat."
  }
}
```

## Example: Conforming Manifestation (Iron Sword)

```json
{
  "kind": 30002,
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

## Example: Universal Entity (Master Sword – Legendary/Iconic)

```json
{
  "kind": 30001,
  "tags": [
    ["d", "master-sword"],
    ["name", "Master Sword"],
    ["alt", "Blade of Evil's Bane"],
    ["category", "weapon"],
    ["type", "melee"],
    ["subtype", "bladed"],
    ["origin", "zelda"]
  ],
  "content": {
    "description": "The legendary blade chosen by the goddess, capable of repelling evil. It rests in sacred places and can only be wielded by true heroes."
  }
}
```

## Example: Conforming Manifestation (Breath of the Wild)

```json
{
  "kind": 30002,
  "tags": [
    ["d", "botw:master-sword"],
    ["entity", "<master-sword_entity_id>", "master-sword"],
    ["game", "zelda-botw"]
  ],
  "content": {
    "base_damage": 30,
    "glows_near_evil": true,
    "durability_trial": true,
    "powered_up_damage": 60,
    "model": "https://.../master_sword_botw.gltf"
  }
}
```

## Example: Universal Entity (Energy Sword – Distinct Class)

```json
{
  "kind": 30001,
  "tags": [
    ["d", "energy-sword"],
    ["name", "Energy Sword"],
    ["alt", "Plasma Sword"],
    ["category", "weapon"],
    ["type", "melee"],
    ["subtype", "energy"],
    ["origin", "halo"]
  ],
  "content": {
    "description": "A Covenant melee weapon consisting of dual superheated plasma blades shaped by magnetic fields. Known for its lunge capability and devastating close-range strikes."
  }
}
```

## Example: Conforming Manifestation (Halo Infinite)

```json
{
  "kind": 30002,
  "tags": [
    ["d", "infinite:energy-sword"],
    ["entity", "<energy-sword_entity_id>", "energy-sword"],
    ["game", "halo-infinite"]
  ],
  "content": {
    "battery_swings": 12,
    "lunge_range": 10,
    "model": "https://.../energy_sword_infinite.gltf",
    "sound_activate": "https://.../activate.ogg"
  }
}
```

## Variations (AEMS Flexibility Feature)

- **Material Variants**: Multiple Manifestations of generic `sword` (steel, diamond, wooden) share the same Entity.
- **Era Reskins**: Master Sword Manifestations differ per Zelda game (e.g., glowing intensity, durability mechanics).
- **Fan/Custom Weapons**: New Manifestations referencing canonical Entities (e.g., `mod:red-energy-sword` with color override).
- **Cross-Genre**: An RPG client adds enchantment properties; an FPS client emphasizes lunge mechanics.

This enables canonical weapons to feel consistent across titles while adapting perfectly to each game's balance and style.
