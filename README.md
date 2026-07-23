# The Hellcaller — Foundry VTT module

A playable **Hellcaller** class for the D&D 5e (2024) system in Foundry VTT: a Charisma-based
half-caster who spends **Malice** to summon and command demonic **Thralls**.

- **Foundry:** v13 (verified build 351) — manifest allows up to v14
- **System:** `dnd5e` 5.x (built and validated against 5.2.0)

## What's included

| Compendium | Contents |
|---|---|
| **Hellcaller Content** (Items) | The class, 3 subclasses, 31 features, 6 original spells — 41 documents |
| **Hellcaller Summons** (Actors) | Thrall, Warden, and Sovereign Demon statblocks, 4 scaling tiers each — 12 actors |
| **Hellcaller Reference** (Journals) | The Hellcaller spell list + a "How to Use" page |

## Installation

### Option A: install via manifest URL (recommended)

1. In Foundry: **Add-on Modules → Install Module**
2. Paste into **Manifest URL**:
   `https://github.com/DungeonNexusEntertainment/hellcaller-class/releases/latest/download/module.json`
3. Click **Install**, then enable **The Hellcaller** in *Manage Modules*.

**Option B: import the raw documents**

`packs/_source/` contains every document as plain JSON. Create a compendium in your world and use
*Import Data* on individual files, or drag them in. Useful if you'd rather not run a module, but you
lose the pre-wired compendium links between the class, its features, and the summon statblocks.

## How it works

**Scale values.** Prepared spells, cantrips known, Malice, and Max Thralls are all `ScaleValue`
advancements on the class, so they update automatically as you level. They're referenceable in
formulas as `@scale.hellcaller.malice`, `@scale.hellcaller.max-thralls`, etc.

**Malice.** Tracked as the limited uses on the **Malice** feature. Features that cost Malice consume
from that pool automatically (the same mechanism the system uses for Monk's Focus). Short Rest
recovers half your maximum rounded up; Long Rest recovers all.

**Summoning.** Thrall Summoning, Warden's Anointment, and Sovereign Bond use native dnd5e `summon`
activities. The correct statblock tier for your level is chosen automatically, and the summoner's
values are applied at summon time:

| Creature | HP bonus | AC bonus | Damage bonus |
|---|---|---|---|
| Thrall | + your CHA mod | + your PB | + your PB |
| Warden | + your CHA mod | + your PB | + your PB |
| Sovereign Demon | — | — | + your CHA mod | 
{See sheet for sovereign details}

## Known deviations and manual bits

These are all also flagged in a **Foundry Note** section on the relevant feature in-app.

- **Thrall attack bonus.** Summoned Thralls match your spell attack bonus (CHA + PB). The Thrall
  table uses *half* your CHA mod + PB at Hellcaller levels 1–10, so subtract half your CHA modifier
  from those attack rolls for strict RAW. dnd5e's summon system can't express a half-ability bonus.
- **Immolate** is set to 1d6 (one adjacent Thrall). Total damage depends on how many Thralls are
  within 5 feet of each individual target — add dice in the damage dialog.
- **Abyssal Rift** consumes *all* remaining Malice, which a fixed cost can't express. Zero out your
  Malice uses after using it.
- **Demonic Endurance** (AC by nearby Thralls), **Infernal Crescendo** (escalating damage),
  **Tyrant's Decree**, **Living Fortress**, and **Unbreakable Line** depend on live battlefield
  positioning. The dnd5e system has no rules engine for that; apply them at the table or automate
  with an active-effect module.
- **Apocalypse Swarm's** +3 Thralls are not in the class scale value (it's subclass-specific) — add
  it yourself. Abyssal Champion's +2 at level 20 *is* included.

## Spell list caveat

Seven spells on the Hellcaller list have **no compendium entry in the dnd5e system**, because they
are Player's Handbook content rather than SRD 5.2 content:

> Mind Sliver, Armor of Agathys, Arms of Hadar, Witch Bolt, Cloud of Daggers, Crown of Madness,
> Hunger of Hadar

The source document states the non-original spells are "drawn from the SRD" — that isn't quite true
for these seven. They're listed by name on the spell list journal page but aren't linked. If you own
a module that provides them (e.g. the official D&D Beyond Player's Handbook), add them to the spell
list page and they'll work normally. Everything else on the list resolves to the system's SRD packs.

## Rebuilding

Content lives in `build/data/*.mjs` and is compiled into LevelDB packs.

```bash
npm install
npm run build            # validate + compile packs
npm run install-module   # also copy into the local Foundry data dir
```

The build validates document IDs, internal compendium links, icon paths, and that every feature is
actually granted by the class or a subclass. It fails loudly rather than shipping a broken pack.

## Licensing

Class text is from *The Hellcaller* by Dungeon Nexus Entertainment. This work includes material from
the SRD 5.2 by Wizards of the Coast LLC, licensed under CC-BY-4.0.
