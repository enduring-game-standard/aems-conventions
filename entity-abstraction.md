# Entity Abstraction Conventions

These optional conventions provide a structured rubric for deciding when a game concept warrants its own AEMS Entity versus when it should be a Manifestation of a broader archetype. They provide cross-domain reasoning and a repeatable four-test framework, grounded in the principle that Entities are IP-agnostic named roles in the commons.

The rubric applies to all AEMS domains — weapons, enemies, items, environment objects, projectiles, NPCs, game pieces, playing cards, and any future category across any medium. It is domain-neutral and medium-neutral by design: the same tests that determine whether a sword warrants its own Entity also determine whether a health pickup, a locked door, a chess piece, or a playing card does.

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

## Medium Transparency

The medium through which a game concept is delivered — card, die, token, tile, miniature, sprite, digital widget — is **never** the Personality of that concept. It is always the Matter facet (Ranganathan), an accident rather than the substance (Aristotle). Entity d-tags must never include a medium suffix (`-card`, `-die`, `-token`, `-tile`) unless the object has no identity apart from the medium.

This principle is implicit in Ranganathan's faceted classification — Matter is Manifestation-level — but it requires explicit statement because medium-encoding is the most common source of false Entities when analyzing board games, card games, or any domain where the physical delivery mechanism is prominent.

### The Diagnostic Test

> *"Would this thing's identity persist unchanged if the medium changed?"*

If the answer is yes, the medium is Matter and belongs on the Manifestation. If the answer is no — if the medium IS the identity — then the medium is fused with Personality and the object is genuinely identity-bearing.

| Thing | Change Medium | Identity Persists? | Verdict |
|-------|--------------|-------------------|---------|
| Ace of Spades | Physical card → digital → clay tile | ✅ Still "Ace of Spades" | **Identity-bearing** — rank + suit ARE the Personality |
| HEAT Speed-3 Card | Card → die face → digital counter | ✅ Still "3 units of movement" | **Resource-bearing** — card is Matter |
| MTG Lightning Bolt | Card → digital cast → spoken spell | ✅ Still "Lightning Bolt" | **Identity-bearing** — named, iconic entity |
| Dominion Curse | Card → debuff icon → penalty cube | ✅ Still "accumulated hindrance" | **Resource-bearing** — card is Matter |
| Yomi Punch Card | Card → button press → mouse click | ✅ Still "attack" | **Resource-bearing** — card delivers an action |

### Why This Matters

Without medium transparency, the rubric breaks across media. Yomi (a card game) and Street Fighter (a video game) descend from the same fighting game lineage — David Sirlin explicitly designed Yomi to translate Street Fighter mechanics to cards. A punch in Street Fighter (button input → animation → damage) and a Punch Card in Yomi (play face-down → reveal → damage) are the same Entity: `attack`. Creating `punch-card` as distinct from `punch` doubles the namespace for no ontological gain.

The same trap appears in any cross-medium analysis. A board game's "Speed Card" and a video game's "speed powerup" and a die roll's "movement points" are all Manifestations of the same concept (movement resource) in different Matter. A board game's "penalty card" and a video game's "debuff stack" and a tabletop RPG's "fatigue token" are all the same `hindrance` Entity in different Matter.

Six convergent lines of evidence establish the principle:

| Domain | Principle | Implication |
|--------|-----------|-------------|
| **Ranganathan** (library science) | Matter facet ≠ Personality facet | Card/die/token/sprite = Material, not identity |
| **Aristotle** (metaphysics) | Substance vs. accident | A chair is a chair whether wood or metal; a movement resource is a movement resource whether cardboard or pixels |
| **Ludii** (formal game theory) | Equipment = {pieces, cards, dice} as functionally equivalent component types | Medium is a container property, not component identity |
| **Substrate independence** (philosophy) | Informational patterns persist across substrates | "3 movement points" is substrate-independent |
| **Music** (notation) | A note is substrate-independent | Middle C on piano = Middle C on violin; the instrument is Matter |
| **Economics** | Currency is medium-independent | A dollar is a dollar whether paper, coin, or digital |

### The Exception: Identity-Bearing Media

Standard playing cards are the canonical exception. The Ace of Spades has no identity *beyond* its medium — rank and suit constitute the complete Personality. There is nothing "behind" the card that the card is delivering. The same applies to Chess pieces: a bishop's identity (diagonal-moving piece) is inseparable from its role in the game of Chess. These objects are identity-bearing: the medium and the Personality are fused.

The test is simple: **does stripping the medium leave a residual identity?** A HEAT Speed-3 Card stripped of card-ness leaves "3 movement points" — residual identity exists, so the card is Matter. An Ace of Spades stripped of card-ness leaves... nothing recognizable. The rank+suit system IS the identity.

When in doubt, apply the Yomi/Street Fighter test: if the same concept crosses from a card game to a video game (or vice versa), the medium is not Personality.

> [!CAUTION]
> **Common trap.** When analyzing board games or card games, the physical medium is salient — you hold the cards, you move the tokens. This salience tempts analysts into naming Entities after the medium: `speed-card`, `penalty-card`, `upgrade-card`. Strip the medium first. Name the Entity for its Personality: `movement-resource`, `hindrance`, `power-up`. If the resulting name matches an Entity that already exists in video game analysis, that's not a coincidence — it's convergent evolution proving the Entity is real.

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
| `receive → inspect → validate/reject` | Inspection Target | Papers Please passport, detective case evidence |
| `draw → hold → play/discard` | Drawn Action | Poker card, MTG creature, Dominion action, Slay the Spire card |
| `select → move to position → (capture/interact)` | Piece | Chess knight, checkers king, Shogi tile, XCOM soldier, Fire Emblem unit |
| `select from supply → place → (control/capture)` | Placed Token | Go stone, Othello disc, Connect Four chip, Catan settlement |

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
| Fighter character | Street Fighter, Tekken, Yomi (card game), BattleCON (board game), D&D (tabletop) | ✅ Universal Entity |
| BFG 9000 | DOOM exclusively | ❌ Manifestation of a broader archetype (e.g., `super-weapon`) |
| "Tall green DOOM pillar" | DOOM exclusively | ❌ Manifestation of `obstacle-pillar` |

The Cross-Game Test identifies natural kinds via *convergent evolution* — independent designers arriving at the same concept independently. Franchise persistence (e.g., an Octorok appearing in 20+ Zelda titles) does not establish Entity status; it establishes cultural significance, which is a Manifestation-level property. A concept that appears in only one franchise is a Manifestation of a broader structural Entity, regardless of how many installments it spans.

### Test 3: The Substitution Test

> "Could you meaningfully swap this Entity's Manifestation from one game into another?"

The AEMS standard states: *"A 'Health Potion' Entity might become a 'Flask' in Dark Souls, a 'Potion' in Final Fantasy VII, or a 'Splash Potion' in Minecraft."* If this substitution makes sense — if a developer could use the Entity as a scaffold for their game's version — the Entity is at the right abstraction level.

If substitution breaks ("swapping DOOM's BFG 9000 into Minecraft" doesn't produce a meaningful analog), the concept is too game-specific for Entity status. Abstract upward until substitution works: if a `super-weapon` substitutes meaningfully but `bfg-9000` does not, then `super-weapon` is the Entity and BFG 9000 is its Manifestation.

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
    ["name", "Pillar"]
  ],
  "content": {
    "description": "A solid vertical structural element that blocks movement and projectiles."
  }
}
```

### IP Boundary Principle

**Entities are IP-agnostic infrastructure.** No Entity d-tag should name a trademarked character, franchise-specific item, or copyrighted concept. Entity d-tags describe *what something is* in universal terms: `sword`, `monster`, `healing-item`, `projectile-fireball`. Franchise-specific identities — the Master Sword, the Pinky Demon, the BFG 9000 — are **always Manifestations**. They reference the generic Entity and carry the franchise attribution on the Manifestation.

This boundary follows the id Software precedent: the engine (RUNS) is open infrastructure; the WAD (AEMS content) separates generic data structures (Entities) from authored content (Manifestations). An Entity published to Nostr is commons infrastructure that any developer can build against without IP risk. A Manifestation carries the creative expression — and the responsibility.

The rule is simple: **if the name is owned by someone, it cannot be an Entity.** Chess pieces and playing cards are fine (public domain). The Master Sword is not (Nintendo). The Triforce is not (Nintendo). The Pinky Demon is not (id Software / Bethesda). These become Manifestations of their structural Entities: `sword`, `quest-objective`, `monster`.

> [!IMPORTANT]
> **Why this matters on Nostr.** Every Entity is a cryptographically signed, permanently attributable Nostr event. Publishing an Entity named `master-sword` creates a permanent commons-layer reference to Nintendo's IP. Any developer who then publishes a Manifestation against that Entity creates a provable, unforgeable link to trademarked content. The [Authorial Provenance Standard](../.github/profile/APS.md) makes this link stronger, not weaker. IP-agnostic Entities eliminate this risk entirely.

### Entities Have No Tags

An Entity event contains only:
- `d` — the Entity name (e.g., `sword`)
- `name` — a human-readable label (e.g., `Sword`)
- `content.description` — a plain-language description

All grouping tags (`category`, `type`, `subtype`, `origin`, `game`) belong on **Manifestations**, not Entities. Discovery works by searching Manifestations by tags, then reducing to their parent Entities.

This prevents Entities from accumulating an informal taxonomy that pretends to be formal. Tags on Entities create the same over-specificity problem as excessive Entity splitting — they narrow a classification that is supposed to be broad. If tags can arbitrarily narrow an Entity, you recreate the `charging-melee-demon` problem in a different form.

### Quest and Progression Items

Quest objectives and progression collectibles are universal archetypes that pass all four tests — they appear in hundreds of games with the same fundamental mechanical role. Their verb clusters are in the table above.

| Entity | Property Cluster | Examples |
|--------|-----------------|---------|
| `quest-objective` | {unique, triggers completion, carried, game-ending} | Amulet of Yendor, assembled Triforce |
| `progression-collectible` | {multiple instances, countable, gate-threshold, zone-unlocking} | Power Stars, Shine Sprites, Korok Seeds, Chaos Emeralds |

All franchise-specific quest items and progression collectibles are Manifestations of these structural Entities:

- **Triforce** → Manifestation of `quest-objective`
- **Chaos Emeralds** → Manifestation of `progression-collectible`
- **Korok Seeds** → Manifestation of `progression-collectible`
- **Amulet of Yendor** → Manifestation of `quest-objective`
- **Power Stars** → Manifestation of `progression-collectible`

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

### Games with Thin Entity Layers

Some games have few Entity types — their depth lives in mechanical grammar (MAPS Patterns), not object taxonomy (AEMS Entities). The rubric still applies: test each *thing* in the game world. But expect a thin Entity layer. This is the correct answer, not a sign that the rubric failed.

| Game | AEMS Entities (Things) | MAPS Patterns (Mechanics) |
|------|----------------------|-------------------------|
| Chess | chess-king, chess-queen, chess-rook, chess-bishop, chess-knight, chess-pawn | turn-alternation, piece-movement, capture, check, checkmate, castling, en-passant, promotion |
| Go | go-stone, go-board | stone-placement, capture-by-surrounding, ko-rule, territory-scoring |
| The Witness | puzzle-panel, locked-door, audio-log, vehicle | maze, constraint-separation, symmetry, shape-fitting, environmental-perception |
| Tetris | playfield | shape-fitting, line-clearing |
| Braid | player-character, key, door, puzzle-piece | time-rewind, time-immunity, shadow-clone |
| Portal | player-character, portal-gun, companion-cube, turret | portal-traversal, momentum-conservation |
| HEAT: Pedal to the Metal | vehicle, speed-limit-zone, finish-line, hindrance, condition-modifier | gear-shifting, heat-management, cornering, slipstream, boost, cooldown, deck-cycling |

A thin Entity layer is the correct answer for these games, not a sign that the rubric failed. A puzzle panel is a thing (Entity). The constraint-separation rule governing what the player draws on it is a mechanic (MAPS Pattern). A time-rewind power is a mechanic (MAPS Pattern), not a thing the player picks up and stores.

The general principle: **the more a game's identity lives in its mechanics rather than its objects, the thinner its AEMS layer and the richer its MAPS layer.** Both layers are necessary. They are different pillars.

## Cross-Domain Validation

The rubric's theoretical foundations converge independently across multiple domains:

| Domain | Principle | AEMS Application |
|--------|----------|-----------------|
| **Philosophy** (HPC theory) | Kind = property cluster, not essence | Entity = stable property cluster across games |
| **Library science** (Ranganathan) | Personality facet = Entity; Energy/Matter = contextual | Only "what it is" goes on Entity; "how it works" is Manifestation |
| **Aristotle** (substance/accident) | Substance persists through accidental change | A chair is a chair whether wood or metal; medium is accidental, not substantial |
| **Biology** (lumpers vs. splitters) | Right granularity depends on purpose | AEMS optimizes for cross-game discoverability → lump |
| **Game design** (Björk/Holopainen) | Pickup, Consumable, Equipment are distinct patterns | These are verb clusters, belonging to the Energy facet → Manifestation |
| **Formal game theory** (Ludii) | G = ⟨Mode, Equipment, Rules⟩; Equipment = {pieces, cards, dice} | Equipment and Rules are distinct; pieces/cards/dice are functionally equivalent |
| **Substrate independence** (philosophy) | Informational patterns persist across substrates | A game concept retains identity regardless of physical medium |
| **DOOM modding** (ZDoom class hierarchy) | Actor > Inventory > Weapon/Ammo/Powerup | 25 years of community practice validates category-level splitting |
| **Theater** (prop classification) | Set dressing, hand prop, personal prop | Interaction pattern determines classification |
| **D&D** (item taxonomy) | "Wondrous Items" = honest catch-all | Any taxonomy needs a catch-all; minimize what falls into it |


## Entities as Named Roles

An Entity is not a type in a hierarchy. It is a **named role** — a concept that a game object can participate in. This model is inspired by the Entity Component System (ECS) architecture dominant in modern game engines, where entities are composed of components rather than classified into types.

A Manifestation can reference **multiple Entities** via multiple `entity` tags, declaring all the roles the game object participates in:

```json
{
  "kind": 30051,
  "tags": [
    ["d", "minecraft:oak-plank"],
    ["entity", "<building-block_id>", "building-block"],
    ["entity", "<resource_id>", "resource"],
    ["category", "material"],
    ["game", "minecraft"]
  ],
  "content": {
    "description": "A basic wooden plank used for building, crafting, and fuel."
  }
}
```

A receiving game reads the Entity references and picks the one(s) it understands. A building game sees `building-block` and imports a wall material. A crafting game sees `resource` and imports a crafting ingredient. The Manifestation says "this thing participates in all of these roles." The receiving game selects which roles it can serve.

The Entity's purpose is **recognition**: a receiving game reads the Entity reference and says "I know that role. I can build a local version of this thing that fills that role." This is the core portability function — not taxonomy, not classification, but **making your digital stuff recognizable to any game that speaks the same language.**

### Object-Entities vs. Agent-Entities

Entity names fall into two natural categories:

**Object-Entities** have identity independent of behavior. Nouns. A sword is a sword whether you swing it or display it on a wall. These are named for what they ARE: `sword`, `door`, `potion`, `tennis-racket`, `chess-rook`.

**Agent-Entities** have identity defined by their role in gameplay. Verbs-as-nouns. An enemy IS what it does — strip the behavior and nothing remains. These are named for what role they FILL: `monster`, `boss`, `companion`, `merchant`.

Agent-Entity names inevitably contain behavioral language. This is not AEMS/MAPS overlap — it is the recognition that for agents, identity and behavior are inseparable. The Entity names the ROLE ("monster"). MAPS describes the RULES that govern the role ("monsters chase when detected and attack when in range").

## Open Questions

The following questions are acknowledged as unresolved. They require further analysis across multiple game inventories before principled answers can emerge.

1. **Multi-Entity cardinality.** How many Entity references should a Manifestation support? Unlimited risks making Entities functionally equivalent to tags. A principled cap has not been established.

2. **Agent-Entity granularity.** Is `monster` the right Entity for all non-boss hostile creatures? Or should the Entity layer distinguish `melee-enemy`, `ranged-enemy`, `flying-enemy` as separate structural Entities? The correct granularity should emerge from applying the four-test rubric to multiple game inventories, not from armchair reasoning.

3. **Constitutive vs. substitutable things.** Chess pieces are constitutive — remove a piece type and you have a different game. DOOM enemies are substitutable — replace any enemy and you still have DOOM. Does this distinction determine Entity structure, or merely explain naming conventions?

4. **The AEMS/MAPS boundary for agents.** Entity names the ROLE. MAPS describes the RULES. But agent-Entity names like `boss` imply rules (phases, arenas, climactic encounter). How much behavioral implication is acceptable in an Entity name?

5. **Referential vs. constructive Entities.** Object-Entities that correspond to real-world things (sword, tennis racket) have obvious, uncontroversial identity. Constructive Entities invented for gameworlds (super-weapon, collectible-creature) require abstraction from specific implementations. Should the rubric handle these differently, or is the four-test framework sufficient for both?

---

*Part of [AEMS Conventions](./README.md). See [AEMS Standard](https://github.com/enduring-game-standard/aems-standard) for the core protocol.*

**MIT License** — Open for use, adaptation, and critique.
