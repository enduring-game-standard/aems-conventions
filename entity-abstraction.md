# Entity Abstraction Conventions

These optional conventions provide a structured rubric for deciding when a game concept warrants its own AEMS Entity versus when it should be a Manifestation of a broader archetype. They extend the [Iconicity Threshold](./README.md#5-iconicity-threshold-for-entities) with cross-domain reasoning and a repeatable four-test framework.

The rubric applies to all AEMS domains — weapons, enemies, items, decorations, projectiles, and any future category. It is domain-neutral by design: the same tests that determine whether a sword warrants its own Entity also determine whether a health pickup or a decorative pillar does.

## The Problem

AEMS Entities are universal archetypes. They should be broad enough to be discovered and referenced across games, but specific enough to carry meaningful identity. Too broad (a single `item` Entity) and queries return noise. Too narrow (a separate Entity for every stat variant) and the namespace proliferates without adding discoverability.

The right abstraction level is not obvious. A health-restoring item in DOOM (stimpack, picked up instantly) and in Final Fantasy VII (Potion, stored in inventory, used from menu) do the same thing through different interaction patterns. Are they the same Entity? A DOOM pillar and a Quake pillar look different but serve the same mechanical role. Are they the same Entity? The rubric answers these questions from first principles.

## Theoretical Foundations

### Homeostatic Property Cluster Theory

Aristotelian essentialism holds that kind membership requires an *essence* — a necessary and sufficient set of properties. Under this view, a sword is a sword because it has a blade, a handle, and cuts things. Applied strictly to AEMS, essentialism leads to excessive splitting: every difference in "essential" properties demands a new Entity.

The Homeostatic Property Cluster (HPC) theory (Boyd, 1991) offers a more flexible alternative. Kind membership is determined by a *cluster* of co-occurring properties, not any single essential property. Gold is gold because it has atomic number 79, is yellow, is malleable, conducts electricity, and melts at 1064°C. No single property is strictly necessary — some gold alloys aren't yellow — but the cluster is reliable. HPC is the recommended theoretical basis for AEMS Entity boundaries.

### Faceted Classification

S.R. Ranganathan's faceted classification (1933) decomposes subjects into five independent facets:

| Facet | Definition | AEMS Layer |
|-------|-----------|------------|
| **Personality** | What it fundamentally is | Entity |
| **Matter** | What it's made of, its material | Manifestation |
| **Energy** | What process it involves, how it works | Manifestation |
| **Space** | Where it exists | Manifestation / Asset |
| **Time** | When, duration, temporal state | State |

The key insight: only the Personality facet belongs on the Entity. All other facets are contextual and belong on Manifestations or lower layers. This means properties like acquisition behavior (pickup vs. inventory), damage values, appearance, and sound effects are Manifestation-level — they describe *how a particular game interprets the concept*, not *what the concept is*.

### The Lumper/Splitter Spectrum

Biological taxonomists have argued for centuries about granularity. "Lumpers" combine similar species into broader groups; "Splitters" create fine-grained categories for every detectable difference. Neither is objectively correct — the right granularity depends on purpose. AEMS optimizes for **discoverability across games**: a developer searching for "shotgun" should find one universal Entity they can build a Manifestation against, not fifty game-specific variants.

## The Four-Test Rubric

Score each candidate Entity on all four tests. **Pass 3 of 4 → create an Entity. Pass 2 → create an Entity only with strong justification. Pass 0–1 → this is a Manifestation of an existing Entity.**

### Test 1: The Verb Test

> "What does the player *do* with this thing?"

The player's primary interaction verb determines the mechanical pattern. Things that share the same verb cluster belong to the same Entity family:

| Verb Cluster | Pattern | Examples |
|-------------|---------|---------|
| `pick up → instant effect` | Pickup | DOOM stimpack, Zelda heart |
| `pick up → store → use later` | Consumable | FF7 Potion, Skyrim healing potion |
| `pick up → equip → active use` | Weapon | DOOM shotgun, Halo pistol |
| `shoot / throw → impact effect` | Projectile | DOOM rocket, Quake nail |
| `none (blocked by / navigate around)` | Obstacle | Pillar, barrel, crate |
| `approach → unlocks passage` | Key | DOOM keycard, Zelda key |

The Verb Test reveals the *type* of entity. Two things with the same verb but different effects (e.g., pickup-instant-heal vs. pickup-instant-armor) are different Entities. Two things with the same effect but different verbs (e.g., instant-heal-pickup vs. stored-heal-consumable) are the **same Entity** — the verb difference is an Energy facet and belongs on the Manifestation.

### Test 2: The Cross-Game Test

> "Does this concept appear in 3+ unrelated games with the same fundamental mechanical role?"

If independent designers in independent contexts arrive at the same concept, it is a natural kind — not an accident. Convergent evolution across games is strong evidence for a universal Entity.

| Concept | Cross-Game Evidence | Verdict |
|---------|-------------------|---------|
| Healing item | DOOM, Quake, Zelda, Final Fantasy, Minecraft, … | ✅ Universal Entity |
| Shotgun | DOOM, Quake, Halo, Half-Life, RE4, … | ✅ Universal Entity |
| Night vision | DOOM, Splinter Cell, Arma, Tarkov, CoD, … | ✅ Universal Entity |
| Explosive barrel | DOOM, Half-Life, RE4, Far Cry, Halo, … | ✅ Universal Entity |
| BFG 9000 | DOOM exclusively | ❌ Game-specific Entity |
| "Tall green DOOM pillar" | DOOM exclusively | ❌ Manifestation of `obstacle-pillar` |

### Test 3: The Substitution Test

> "Could you meaningfully swap this Entity's Manifestation from one game into another?"

The AEMS standard states: *"A 'Health Potion' Entity might become a 'Flask' in Dark Souls, a 'Potion' in Final Fantasy VII, or a 'Splash Potion' in Minecraft."* If this substitution makes sense — if a developer could use the Entity as a scaffold for their game's version — the Entity is at the right abstraction level.

If substitution breaks ("swapping DOOM's BFG 9000 into Minecraft" doesn't produce a meaningful analog), the thing may need its own Entity.

### Test 4: The Mechanical Signature Test

> "Does this thing have a unique cluster of co-occurring mechanical properties?"

Per HPC theory, things with the same property cluster belong to the same kind. A shotgun has {spread pattern, close range, high per-shot damage, slow fire rate}. A pistol has {single projectile, medium range, moderate fire rate}. Different property clusters = different kinds.

If a candidate's property cluster is indistinguishable from an existing Entity's cluster across multiple games, it is a Manifestation of that Entity, not a new one.

## Applying the Rubric

### Acquisition Behavior: Entity or Manifestation?

**Verdict: Manifestation property.**

A DOOM stimpack (instant on pickup) and an FF7 Potion (stored, used from menu) both restore health. The Verb Test shows they differ in the Energy facet (how the process works), not the Personality facet (what the thing is). Per Ranganathan, Energy is contextual.

A single Entity (e.g., `healing-item`) can have Manifestations that specify `acquisition_behavior` as a property:

```json
{
  "kind": 30051,
  "tags": [
    ["d", "doom-classic:stimpack"],
    ["entity", "<entity_id>", "healing-item"],
    ["property", "acquisition_behavior", "instant", "string"],
    ["property", "heal_amount", "10", "integer"]
  ]
}
```

A RUNS Processor reads `acquisition_behavior` and handles accordingly. The Entity remains a clean, universal archetype.

### Ammo Taxonomy: Disambiguation at Creation

Ambiguous d-tags (e.g., `ammo-shell`) should be avoided. "Shell" means both shotgun shell (small arms cartridge containing pellets) and artillery shell (explosive projectile). Disambiguation at Entity creation time prevents confusion at discovery time:

| ✅ Recommended | ❌ Avoid | Why |
|---------------|---------|-----|
| `ammo-shotgun-shell` | `ammo-shell` | Ambiguous — shotgun vs. artillery |
| `healing-item` | `health-potion` | Too specific — excludes stimpacks, hearts, medkits |
| `night-vision` | `light-amplification-visor` | Too specific — DOOM's name for a universal concept |
| `projectile-rocket` | `rocket` | Ambiguous — the weapon or the projectile? |

### Decorations and Non-Interactive Objects

Non-interactive objects (pillars, torches, trees, gore decorations) almost never pass the Cross-Game Test as specific visuals. But the *functional concept* often does: pillars appear in every game with architecture, torches in every game with medieval settings.

Decoration Entities should be defined at the **functional concept level** (e.g., `obstacle-pillar`, `torch`, `gore-prop`). Game-specific variants are Manifestations:

```json
{
  "kind": 30050,
  "tags": [
    ["d", "obstacle-pillar"],
    ["name", "Pillar"],
    ["category", "decoration"],
    ["type", "obstacle"],
    ["subtype", "structural"]
  ],
  "content": {
    "description": "A solid vertical structural element that blocks movement and projectiles."
  }
}
```

### Projectiles: Mechanical Signature Grouping

Projectiles rarely warrant individual Entities. Instead, group them by their mechanical signature — the HPC property cluster that determines how they behave in gameplay:

| Entity | Property Cluster | Examples |
|--------|-----------------|---------|
| `projectile-hitscan` | {instant hit, no visible projectile, accuracy falloff} | Bullets, lasers |
| `projectile-fireball` | {visible, dodgeable, travels in line/arc, no splash} | Imp fireball, plasma ball |
| `projectile-rocket` | {visible, splash damage, self-damage possible} | Rocket, grenade |
| `projectile-seeking` | {tracks target, visible, dodgeable with effort} | Homing missile, Needler |

Each game's specific projectiles become Manifestations of the appropriate cluster Entity.

## Cross-Domain Validation

The rubric's theoretical foundations converge independently across multiple domains:

| Domain | Principle | AEMS Application |
|--------|----------|-----------------|
| **Philosophy** (HPC theory) | Kind = property cluster, not essence | Entity = stable property cluster across games |
| **Library science** (Ranganathan) | Personality facet = Entity; Energy/Matter = contextual | Only "what it is" goes on Entity; "how it works" is Manifestation |
| **Biology** (lumpers vs. splitters) | Right granularity depends on purpose | AEMS optimizes for cross-game discoverability → lump |
| **Game design** (Björk/Holopainen) | Pickup, Consumable, Equipment are distinct patterns | These are verb clusters, belonging to the Energy facet → Manifestation |
| **DOOM modding** (ZDoom class hierarchy) | Actor > Inventory > Weapon/Ammo/Powerup | 25 years of community practice validates category-level splitting |
| **Theater** (prop classification) | Set dressing, hand prop, personal prop | Interaction pattern determines classification |
| **D&D** (item taxonomy) | "Wondrous Items" = honest catch-all | Any taxonomy needs a catch-all; minimize what falls into it |

---

*Part of [AEMS Conventions](./README.md). See [AEMS Standard](https://github.com/enduring-game-standard/aems-standard) for the core protocol.*

**MIT License** — Open for use, adaptation, and critique.
