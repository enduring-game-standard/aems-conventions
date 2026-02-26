# Entity Abstraction Conventions

These optional conventions provide a structured rubric for deciding when a game concept warrants its own AEMS Entity versus when it should be a Manifestation of a broader archetype. They extend the [Iconicity Threshold](./README.md#5-iconicity-threshold-for-entities) with cross-domain reasoning and a repeatable four-test framework.

The rubric applies to all AEMS domains — weapons, enemies, items, environment objects, projectiles, NPCs, and any future category. It is domain-neutral by design: the same tests that determine whether a sword warrants its own Entity also determine whether a health pickup, a locked door, or a decorative pillar does.

## The Problem

AEMS Entities are universal archetypes. They should be broad enough to be discovered and referenced across games, but specific enough to carry meaningful identity. Too broad (a single `item` Entity) and queries return noise. Too narrow (a separate Entity for every stat variant) and the namespace proliferates without adding discoverability.

The right abstraction level is not obvious. A health-restoring item in DOOM (stimpack, picked up instantly) and in Final Fantasy VII (Potion, stored in inventory, used from menu) do the same thing through different interaction patterns. Are they the same Entity? A DOOM pillar and a Quake pillar look different but serve the same mechanical role. Are they the same Entity? The rubric answers these questions from first principles.

## Scope: Things, Not Mechanics

The rubric applies to **things** — objects a player possesses, encounters, or interacts with in a game world. Swords, health potions, locked doors, enemies, puzzle panels, audio logs, vehicles. These are AEMS Entities.

The rubric does **not** apply to **mechanics** — rules governing how those things behave. Constraint-satisfaction logic, stamina-attack tradeoffs, time-rewind rules, portal-traversal physics. These are [MAPS Patterns](../ludic-notation-standard/README.md), not AEMS Entities. Mechanics describe *conversation structure* — the grammar that makes gameplay meaningful. They belong in the notation layer (cumulative craft), not the entity layer (durable substrate).

The rubric also does not apply to **conditions** — states applied to things. Poison, blindness, confusion, paralysis, and fear are qualities or states in the Aristotelian sense: they inhere in substances rather than existing independently. In MAPS terms, they are States modified by Verbs. What *are* AEMS Entities are the **things that deliver conditions**: a Potion of Poison (consumable Entity), a Scroll of Confusion (scroll Entity), a monster with a paralysis attack (enemy Entity whose Manifestation specifies `special_attack: paralysis`). The delivery vehicle is the thing; the condition is the state change it causes.

The distinction follows from the AEMS standard's own founding analogy: *a chess piece belongs to you regardless of which board you play on*. The chess piece is the Entity. The L-shaped movement rule governing the knight is not an Entity — it is a Pattern. The piece persists as a thing. The rule transmits as knowledge.

Seven independent lines of evidence converge on this boundary:

| Domain | Principle | Implication |
|--------|-----------|-------------|
| **AEMS standard** | "Chess piece belongs to you" | Entities are things you possess/encounter, not rules |
| **Ludii** (Browne, 2020) | G = ⟨Mode, Equipment, Rules⟩ | Equipment (AEMS) and Rules (MAPS) are formally distinct categories |
| **Ranganathan** | Personality vs. Energy | Things have Personality (identity); mechanics are Energy (process) |
| **Wittgenstein** | "Meaning is use" | The piece is the object; the rule is the grammar that gives it meaning |
| **Music ontology** | Instrument vs. Form | A violin is an object; a fugue is a structure — ontologically distinct |
| **HPC theory** | Property clusters of *kinds* | Kinds are categories of things, not processes |
| **Three-pillar architecture** | Substrate ≠ Craft | Durable substrate (things) and cumulative craft (knowledge) are different pillars |

> [!IMPORTANT]
> **Why this matters for the rubric.** The four tests below can produce false positives when applied to mechanics. A maze has a verb ("solve"), cross-game presence (mazes appear everywhere), substitutability (any game could have a maze), and a property cluster ({grid, entrance, exit, find path}). It passes 4/4. But a maze is not a thing — it is a rule structure. The scope boundary catches this *before* the tests are applied: if the candidate is a process rather than an object, it is a MAPS Pattern, and the four tests do not apply.

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
| `pick up → equip → use (limited charges)` | Charged Device | D&D wand, Zelda magic items, Megaman weapons |
| `shoot / throw → impact effect` | Projectile | DOOM rocket, Quake nail |
| `none (blocked by / navigate around)` | Obstacle | Pillar, barrel, crate |
| `approach → unlocks passage` | Key | DOOM keycard, Zelda key |
| `find → possess → triggers game completion` | Quest Objective | Amulet of Yendor, Triforce (assembled) |
| `find → collect N → unlocks zone/ability` | Progression Collectible | Power Stars, Shine Sprites, Korok Seeds |

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

The Cross-Game Test identifies natural kinds via *convergent evolution* — independent designers arriving at the same concept independently. A different kind of evidence exists: *franchise persistence*. An Octorok appears in 20+ Zelda titles across 40 years but zero non-Zelda games. It fails Cross-Game strictly, yet clearly warrants an Entity. Franchise persistence proves *cultural durability*, not convergent evolution — a different mechanism establishing the same conclusion. The 3/4 scoring rule already handles this: franchise icons typically pass the Verb, Substitution, and Mechanical Signature tests while failing Cross-Game, yielding 3/4 and Entity status. The Iconicity Threshold (see [README §5](./README.md#5-iconicity-threshold-for-entities)) provides the formal justification.

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

### Environment Objects

Environment objects fall into two categories with very different Entity implications:

**Interactive gating objects** — locked doors, destructible walls, pushable blocks — are among the most universal archetypes in gaming. They pass all four tests overwhelmingly. A locked door appears in DOOM, Zelda, Resident Evil, Metroid, Dark Souls, Binding of Isaac, and hundreds of other unrelated games with the same fundamental mechanical role: {blocks passage, requires key/condition, opens permanently}. These warrant Entities at the **mechanical function level**:

| Entity | Property Cluster | Examples |
|--------|-----------------|---------|
| `locked-door` | {blocks passage, requires key, opens permanently} | DOOM keyed door, Zelda locked door, RE door |
| `destructible-wall` | {appears solid, destroyed by weapon/tool, reveals passage} | DOOM secret wall, Zelda bombable wall, Metroid breakable block |
| `pushable-block` | {movable by player, reveals passage or triggers event} | Zelda dungeon block, Tomb Raider block, Sokoban crate |

The Verb Test is the discriminator. Interactive environment objects have verb clusters (`approach + key → opens`, `attack → destroys`, `push → reveals`) that distinguish them from passive obstacles.

**Non-interactive obstacles** (pillars, barrels, trees, gore decorations) almost never pass the Cross-Game Test as specific visuals. But the *functional concept* often does: pillars appear in every game with architecture, torches in every game with medieval settings. Non-interactive Entities should be defined at the **functional concept level** (e.g., `obstacle-pillar`, `torch`, `gore-prop`). Game-specific variants are Manifestations:

```json
{
  "kind": 30050,
  "tags": [
    ["d", "obstacle-pillar"],
    ["name", "Pillar"],
    ["category", "environment"],
    ["type", "obstacle"],
    ["subtype", "structural"]
  ],
  "content": {
    "description": "A solid vertical structural element that blocks movement and projectiles."
  }
}
```

### Iconicity vs. Mechanical Archetype

Some things are simultaneously a universal mechanical pattern and an iconic specific thing. The Triforce in Zelda functions mechanically as a `quest-macguffin` (collect N pieces to access the final area — a pattern shared by Mario 64 Stars, Sonic's Chaos Emeralds, and Banjo-Kazooie's Jiggies). But the Triforce also has 40 years of persistent lore, cross-media recognition, and identity that would be *lost* if filed under `quest-macguffin`.

Ranganathan resolves this. The Personality facet is *what something fundamentally is*. The Triforce's Personality is "the Triforce" — a sacred golden relic of Hyrule. Its mechanical function as a collectible progression gate is the Energy facet (how the game uses it), which belongs on the Manifestation.

**The precedence rule:** When a thing has both a universal mechanical archetype (`quest-macguffin`) and an iconic identity that passes the Iconicity Threshold (`triforce`), the iconic identity takes precedence as the Entity. The mechanical pattern becomes a grouping tag (e.g., `["type", "quest-macguffin"]`) on the Entity, preserving discoverability without sacrificing identity.

```json
{
  "kind": 30050,
  "tags": [
    ["d", "triforce"],
    ["name", "Triforce"],
    ["category", "artifact"],
    ["type", "quest-macguffin"],
    ["origin", "zelda"]
  ],
  "content": {
    "description": "A sacred golden relic composed of three triangles, each embodying Power, Wisdom, or Courage. Collecting its pieces is the central quest of the Zelda series."
  }
}
```

This generalizes: the Master Sword is Entity `master-sword` with `["type", "melee"]`, not a Manifestation of `sword`. Ganon is Entity `ganon` with `["type", "boss"]`, not a Manifestation of `final-boss`. Whenever removing the specific identity would lose something meaningful, the identity is the Personality and the mechanical role is Energy.

### Quest and Progression Items

Quest objectives and progression collectibles are universal archetypes that pass all four tests — they appear in hundreds of games with the same fundamental mechanical role. Their verb clusters are in the table above.

| Entity | Property Cluster | Examples |
|--------|-----------------|---------|
| `quest-objective` | {unique, triggers completion, carried, game-ending} | Amulet of Yendor, assembled Triforce |
| `progression-collectible` | {multiple instances, countable, gate-threshold, zone-unlocking} | Power Stars, Shine Sprites, Korok Seeds, Chaos Emeralds |

These archetypes follow the same Iconicity rules as every other domain. A Power Star is a Manifestation of `progression-collectible`. But when a progression collectible accumulates persistent lore, cross-media recognition, and cultural identity — the Chaos Emeralds, the Triforce pieces — the Iconicity Threshold applies and the iconic identity becomes the Entity:

- **Chaos Emeralds** → Entity: `chaos-emerald` with `["type", "progression-collectible"]`, `["origin", "sonic"]`
- **Korok Seeds** → Manifestation of `progression-collectible` (iconic within BotW/TotK but not at Iconicity Threshold)
- **Amulet of Yendor** → Manifestation of `quest-objective` (iconic within roguelikes but not at Iconicity Threshold)

A useful heuristic for this domain: **could you swap the object's identity for something else without changing the game's mechanical structure?** If swapping the Amulet of Yendor for a "Golden Crown" changes nothing about Rogue's gameplay, the identity is interchangeable — it's a Manifestation. If removing the Triforce from Zelda would alter the game's meaning and not just its theme, the identity is load-bearing — it's an Entity.

### Charged Devices

Charged devices have a verb cluster distinct from both consumables (single-use, destroyed on use) and equipment (unlimited use). Their pattern — equipped, limited uses, may be recharged or depleted — is the Verb Test discriminator. A wand with 5 charges and a wand with 50 charges are Manifestations of the same Entity (`wand`); the charge count is a Manifestation property (Energy facet).

### Projectiles: Mechanical Signature Grouping

Projectiles rarely warrant individual Entities. Instead, group them by their mechanical signature — the HPC property cluster that determines how they behave in gameplay:

| Entity | Property Cluster | Examples |
|--------|-----------------|---------|
| `projectile-hitscan` | {instant hit, no visible projectile, accuracy falloff} | Bullets, lasers |
| `projectile-fireball` | {visible, dodgeable, travels in line/arc, no splash} | Imp fireball, plasma ball |
| `projectile-rocket` | {visible, splash damage, self-damage possible} | Rocket, grenade |
| `projectile-seeking` | {tracks target, visible, dodgeable with effort} | Homing missile, Needler |

Each game's specific projectiles become Manifestations of the appropriate cluster Entity.

### Games Without Traditional Entities

Many acclaimed games have few or no combat entities — no weapons, no enemies, no inventory. The rubric still applies: test each *thing* in the game world. But expect a thin Entity layer. The richness of these games lives in their mechanical grammar (MAPS Patterns), not their object taxonomy (AEMS Entities).

| Game | AEMS Entities (Things) | MAPS Patterns (Mechanics) |
|------|----------------------|-------------------------|
| The Witness | puzzle-panel, locked-door, audio-log, vehicle | maze, constraint-separation, symmetry, shape-fitting, environmental-perception |
| Tetris | playfield | shape-fitting, line-clearing |
| Braid | player-character, key, door, puzzle-piece | time-rewind, time-immunity, shadow-clone |
| Portal | player-character, portal-gun, companion-cube, turret | portal-traversal, momentum-conservation |

A thin Entity layer is the correct answer for these games, not a sign that the rubric failed. A puzzle panel is a thing (Entity). The constraint-separation rule governing what the player draws on it is a mechanic (MAPS Pattern). A time-rewind power is a mechanic (MAPS Pattern), not a thing the player picks up and stores.

The general principle: **the more a game's identity lives in its mechanics rather than its objects, the thinner its AEMS layer and the richer its MAPS layer.** Both layers are necessary. They are different pillars.

## Cross-Domain Validation

The rubric's theoretical foundations converge independently across multiple domains:

| Domain | Principle | AEMS Application |
|--------|----------|-----------------|
| **Philosophy** (HPC theory) | Kind = property cluster, not essence | Entity = stable property cluster across games |
| **Library science** (Ranganathan) | Personality facet = Entity; Energy/Matter = contextual | Only "what it is" goes on Entity; "how it works" is Manifestation |
| **Biology** (lumpers vs. splitters) | Right granularity depends on purpose | AEMS optimizes for cross-game discoverability → lump |
| **Game design** (Björk/Holopainen) | Pickup, Consumable, Equipment are distinct patterns | These are verb clusters, belonging to the Energy facet → Manifestation |
| **Formal game theory** (Ludii) | G = ⟨Mode, Equipment, Rules⟩ | Equipment (AEMS) and Rules (MAPS) are formally distinct categories |
| **DOOM modding** (ZDoom class hierarchy) | Actor > Inventory > Weapon/Ammo/Powerup | 25 years of community practice validates category-level splitting |
| **Theater** (prop classification) | Set dressing, hand prop, personal prop | Interaction pattern determines classification |
| **D&D** (item taxonomy) | "Wondrous Items" = honest catch-all | Any taxonomy needs a catch-all; minimize what falls into it |


---

*Part of [AEMS Conventions](./README.md). See [AEMS Standard](https://github.com/enduring-game-standard/aems-standard) for the core protocol.*

**MIT License** — Open for use, adaptation, and critique.
