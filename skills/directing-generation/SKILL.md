---
name: directing-generation
description: Creative direction for AI image generation — distill a codebase's FEEL canon (taste tokens, TDRs, persona files) into prompt-ready material vocabulary, map product architecture to scene systems, and enforce visual discipline across banner sets and scene stacks.
user-invocable: true
allowed-tools: Read, Write, Glob, Grep, Edit
---

# Directing Generation

Creative direction discipline for AI image output. When a codebase has an established visual canon — taste tokens, TDRs, persona documents, reference art — this skill distills that canon into the material vocabulary that an image model can execute against.

This skill is the **eye**. For the **hand** — the prompt engineering technique itself (Images 2.0 format, size limits, reference roles, sanitization) — pair with The Mint's `prompting-images`.

The distinction matters. A prompt engineer without direction produces plausible-but-uncanny output. A creative director without prompting technique produces beautiful briefs that never land correctly. Run this skill first. Run `prompting-images` second.

## Trigger

```
/direct-generation
"direct a generation session from our FEEL canon"
"build a scene stack for the product stack"
"extract prompt vocabulary from our taste tokens"
```

## When to Use

- Producing a hero banner for a product that already has a visual canon
- Building a scene stack / banner system across multiple repos or layers (one image per product layer)
- Translating an existing design system (taste.md, TDRs) into AI-generation vocabulary
- Reviewing AI-generated output against the canon for taste compliance
- Running the shelf test on a set of banners before shipping

## The Distill → Direct → Validate Loop

### Phase 1 — Distill from canon

Before writing a prompt, read the codebase's canon. Don't invent.

```
[project-root]/
├── src/lib/design/taste.md        ← chromatic doctrine, motion language
├── grimoires/[project]/tdr/       ← Taste Decision Records, named rationales
├── grimoires/[project]/identity/  ← persona files (visual directors, design voices)
├── grimoires/[project]/context/   ← material direction, composition briefs
└── [refs folder]                  ← existing concept art, moodboards
```

What to extract:

**From `taste.md`:**
- Palette tokens in OKLCH (L/C/H values, not color names)
- Per-hue semantic assignment (which hue carries what meaning)
- Motion language (spring configs, duration vocabulary, easing)
- Typography stack and register (display/data/authority voices)
- Anti-patterns explicitly called out ("Crimson hero numbers — REJECTED")

**From TDRs:**
- Named treatments ("phosphor doubling," "CRT burn-in," "moiré sigil")
- Discipline rules ("single-accent-per-scene," "no opacity," "currentColor only")
- Material metaphors ("wet neon = density, not softness")

**From persona / identity docs:**
- The visual voice (clinical vs. atmospheric, sharp vs. soft)
- Lineage references *internal to the project* (these stay in the brief, NOT in the shipped prompt)
- Rejected directions (what the canon has explicitly refused)

**From reference art:**
- Composition patterns (where the subject sits, how scale works)
- Atmospheric treatment (rain, haze, lighting temperature)
- Register coherence across multiple pieces (same hand? same world?)

### Phase 2 — Direct by architectural function

Scene stacks are not aesthetic variations. Each scene inhabits a **location that fits the function of the product layer it represents.** Map product architecture to spatial architecture.

Example (from a real product stack):

| Layer | Function | Spatial metaphor | Scene register |
|---|---|---|---|
| Protocol / schemas | Broadcast definition, no runtime | Transmission spire, skyline | No figures, monument-quiet |
| Runtime / router | Session dispatch between destinations | Underground transit, tunnel + routing display | Passage, transit, small figures |
| Platform / gateway | Public intake, billing | Street-level tower with entrance, queued figures | Arrival, transaction |
| Oracle / read-only | Conviction-gated voice | Interior chamber with altar | Contemplation, receiving |
| Framework / possession | The deepest layer, install = mount | Industrial courtyard, single figure + descending entity | Confrontation, surrender |
| Distribution plane | Peer bazaar, cross-cutting | Rooftop camp of operators | Camaraderie, working |

Name-metaphor mining: if a product uses naming from a specific tradition (Vodou Loa/Hounfour, Gibson Sprawl/Freeside, Japanese ma/kaironic), that naming is conceptually load-bearing — honor it visually. The horse as possession-vehicle, the temple as protocol-room, the caravan as platform-gateway. The metaphor is the direction.

### Phase 3 — Enforce visual discipline

Three rules transfer across every scene in the stack:

**1. Single-stamp discipline.** Exactly ONE accent-colored object per scene. Rarity = weight. If you violate it once, you have to violate it in every scene, and the stamp no longer carries meaning. An accent that appears on one object carries more signal than the same accent spread across five. This is Alexander's Strong Centers principle applied to palette.

**2. Per-scene treatment + shared DNA.** Treatments like phosphor doubling, moiré sigil, cathode scanline overlay, structured block-noise — apply these *consistently across the whole set*. Per-scene variance lives in composition and subject, not in treatment vocabulary. This is what makes a set feel like rooms in a building vs. unrelated illustrations.

**3. What belongs to the generator vs. what belongs to the composite.** The scene, the atmosphere, the subject, the palette — all generated. The wordmark, the logo, the partnership mark, the tier badges, the ambient telemetry text — all composited locally after generation. The model will mangle brand assets every time; don't let it try.

### Phase 4 — Validate with the shelf test

Before shipping any scene, verify the whole set:

```
[ ] Arrange all generated scenes as a grid. Step back.
[ ] FAMILY RESEMBLANCE: do they feel like rooms in one building?
    Same palette discipline, same atmospheric treatment, same register.
[ ] INSTANT DISTINGUISHABILITY: can you tell them apart in one
    glance? Different architecture, different composition, different
    subject — but same world.
[ ] STAMP EQUIVALENCE: is the single-accent object roughly
    equivalent in weight across scenes? (Except intentional outliers
    like a "hero" scene with highest red intensity.)
[ ] LOCKUP LEGIBILITY: does the brand lockup (same composite pattern)
    read clearly against all scenes?
[ ] NO SCENE DOMINATES: no single banner makes the others look
    unfinished. If one does, either the dominant scene is too loud or
    the others are underspecified.
```

Adapted from Loa's TDR-008 logo system shelf test (construct-the-easel).

## The Prompt Hand-Off

Once direction is set, hand off to The Mint's `prompting-images` with:

1. **The shared constraints block** — palette discipline in OKLCH, treatment vocabulary, hard negatives. Paste into every scene's prompt.
2. **Per-scene IMPORTANT DETAILS** — composition, subject, atmospheric specifics, scene-specific treatment variants.
3. **Reference image assignments per scene** — which image is composition anchor, which is palette anchor, which (if any) is edit target.
4. **The sanitization pass** — internal brief retains IP context (named lineage, franchise references); the shipped prompt is scrubbed. `prompting-images` has the strike-replace table.

## What NOT to Do

- **Do not invent a visual language.** If the codebase has a canon, inherit it. Inventing in parallel produces drift.
- **Do not describe the lineage in the shipped prompt.** "In the style of [named artist]" fails guardrails and outsources your direction to the model's interpretation of a name. Describe the material qualities instead. Keep the lineage in the brief for your team's context.
- **Do not treat variations as a stack.** Six images of the same scene with different compositions is not a stack — it's a mood board. A stack is six scenes that inhabit six distinct locations in one world.
- **Do not iterate without the selection test.** Every generation is billed. Define the pass/fail conditions (single-stamp holds, composition matches anchor, treatments visible, negatives satisfied) *before* generating. Kill near-misses. Accept only clean passes.
- **Do not try to generate brand marks.** The moment you ask the model for "the logo" you have already compromised the brand. Composite locally.

## Case Study Reference

The worked example of the full loop lives at:
`loa-constructs/.../six-scene-stack.md` (cycle 2026-04-21)

A vertical-city banner set for the Loa ecosystem:
- Source canon: sprawl-world `src/lib/design/taste.md` (OKLCH doctrine, three-color vocabulary, phosphor register) + 27 TDRs (bloodstain presence, sigil particle treatment, brand visual provenance, construct logo system) + MASAMUNE persona (visual director) + summon-materiality direction doc (six-materials vocabulary) + seven 0xhanijaru concept art pieces.
- Output: six scenes (Constructs / L5 Dixie / L4 Freeside / L3 Finn / L2 Hounfour / L1 Loa), each mapped to a distinct architectural space, all sharing eight named treatments, all composited with identical brand lockup (horse-mark + wordmark upper-left, partnership mark upper-right).
- Validation: shelf test passed on first pass after sanitization; prompts survive IP guardrails because lineage references were stripped but material vocabulary preserved.

Read that brief as the canonical example of this skill in execution.

## Related Skills

- The Mint `prompting-images` — technique-side partner (the hand)
- Artisan `synthesizing-taste` — extracting taste tokens from reference materials into taste.md
- Artisan `recording-taste` — formalizing new taste decisions as TDRs
- Artisan `inscribing-taste` — applying taste tokens to components (UI side)
- Artisan `envisioning-direction` — capturing visual direction interactively
- The Easel (construct-the-easel) — project-specific taste and TDR authorship

## Principles

1. **Taste is inherited, not invented.** If the canon exists, read it first. The best creative direction is archaeology, not invention.
2. **Architecture is direction.** The product's layer structure, naming mythology, and functional decomposition already contain the scene metaphors. Mine them before reaching for external references.
3. **Discipline is the aesthetic.** Palette discipline, stamp rarity, treatment consistency — these produce coherence. Freedom in every direction produces noise.
4. **The brief is for the team. The prompt is for the model.** Don't conflate them. Keep the full IP context and lineage in the brief (useful for the team reading this in six months). Sanitize only what ships to the API.
5. **The model generates; you compose.** Scene, atmosphere, subject: model's job. Brand marks, readable text, pixel-perfect SVG shapes: your job, locally, deterministically.
