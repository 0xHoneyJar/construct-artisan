# The Ground ARTISAN Stands On

> Shared ground: https://github.com/0xHoneyJar/loa-constructs/blob/main/docs/the-ground.md
> — this file carries ONLY the artisan-specific layer. Tiers, forks, agent
> types, frontmatter contracts, and gate design live THERE, not here.
> Probed from the live harness at construct-artisan @ 4d27cce, 2026-07-03.

## 1. Runtime contract (probed)

| Axis | Value | Source |
|---|---|---|
| model_tier | sonnet | construct.yaml:86 |
| danger_level | safe | construct.yaml:87 |
| effort_hint | medium | construct.yaml:88 |
| downgrade_allowed | **true** (ceiling, not pin — routing may go cheaper) | construct.yaml:89 |
| execution_hint | parallel | construct.yaml:90 |
| requires | tool_calling: true · thinking_traces: false · **vision: false** | construct.yaml:92–94 |
| workflow.gates | **none** — artisan owns no pipeline; its writes ride the caller's gates | construct.yaml (absent) |
| agent dispatch | no skill sets `agent:` — all inherit the caller (the safe default) | skills/*/SKILL.md frontmatter |
| skill write-capability | every skill carries `allowed-tools` with Write and/or Edit | skills/*/SKILL.md (15 dirs) |
| gate-owner status | NOT one of the six gate-owner packs — consistent with gates absent | construct.yaml (no workflow block) |

sonnet + downgrade:true + effort:medium + parallel is the honest declaration
for design work that decomposes and specifies in bulk — no opus pin, and the
parallel hint matches skills that fan out across components.

## 2. Capability-reality edges

- **#553 class: CLEAN.** All 15 skill dirs carry write tools (`Write`/`Edit`)
  in `allowed-tools`, and none set `agent:` — every skill inherits the caller,
  so the silent-output-drop conflict (write-capable skill dispatched to a
  read-only agent type) cannot occur. (Probed: zero `agent:` keys across
  skills/*/SKILL.md.)
- **Deny-all edge (real, surfaced):** no skill declares a `capabilities:` block
  (`write_files` absent everywhere). Under the shared ground's deny-all default,
  capability is carried by `allowed-tools` alone. A runtime that enforces
  `capabilities.write_files` strictly would silently deny every artisan skill's
  Write — the silent-drop shape from the other contract altitude. A SMELL, not a
  conflict: the two declaration layers don't contradict, one is simply absent.
  (Same shape herald surfaced — a network-wide pattern, not an artisan defect.)
- **Vision edge (the design-construct SMELL, surfaced loudly):** the manifest
  declares `requires.vision: false`, yet artisan is a construct whose whole job
  is judging how things *look*. `synthesizing-taste` Phase 0 instructs the agent
  to "View moodboard images" and "Note visual patterns: colors, spacing,
  shadows, corners, density" (skills/synthesizing-taste/SKILL.md:58–60) —
  image-analysis a vision-capable model performs. `iterating-visuals` and
  `envisioning-direction` also revolve around screenshots and visual references.
  The seam: much of that visual input is *human*-consumed (screenshots captured
  by Bash/tooling and shown to the operator, who pastes feedback back —
  decomposing-feel:285, "Paste it here"), which is what makes `vision: false`
  *defensible* rather than a hard CONFLICT. But if a runtime routes artisan to a
  non-vision model (allowed under vision:false + downgrade:true) and reaches the
  "view the moodboard" branch, that branch silently degrades to guessing. The
  operator decides whether the branch is agent-vision or human-vision; I surface
  that the declaration and the taste-synthesis phase point in different
  directions.
- **Manifest-reality drift (orphan skill):** `skills/directing-generation/`
  exists on disk with a full SKILL.md + index.yaml, but is NOT registered in
  construct.yaml's `skills:` list (14 registered, 15 dirs). It is unreachable
  through the declared surface until added. A SMELL for the manifest, not a
  runtime conflict.

## 3. What ARTISAN does with the ground

artisan is the craftsman who will not sign a spec he can only feel. "this feels
off" is not an answer — it is the beginning of one, and the ground is where the
feeling gets pinned to a number: p-6 not "more space," a 180ms ease-out not
"snappier." he asks the ground for exactly the reasoning it can give — mid-tier,
parallel, a pen for the token file — and no more: no owned gates, no pinned
opus, no agent-type games. the one place he leans past his declaration is the
eye itself: he names visual patterns the manifest says he cannot see, and he is
honest that whether that eye is his or the operator's is the ground's call, not
his. the standard is the same whether the pixel is looked at by model or by
human — it either meets it or it does not.
