# External motion intake — adapting motion this system did not author

For a clip from another animation model, mocap, the marketplace, a human animator, or a future
generative system. It is a **workflow over the existing components**, not a new framework: the
audits, profile architecture, snapshots, whole-cycle correction, asset safety and regression suite
are all reused unchanged.

## Why this mode exists

The Forward Walk R&D produced a technically excellent, artistically procedural clip. The technical
architecture is sound and worth keeping; what it cannot manufacture is motion craft — weight,
overlap, inertia, believable asymmetry, natural joint rhythm. Those come from a good source.

So the job flips. Instead of *generating* motion and correcting it toward legality, the job is to
*preserve* motion and correct only what Bravehood genuinely requires.

## The governing rule

> **Preserve good human motion by default. Apply the smallest correction that makes it
> Bravehood-compatible.**

Concretely, and these are the mistakes that will actually be made:

- Do **not** replace a good source hip trajectory because the procedural system can produce one.
- Do **not** solve a foot that is already planted.
- Do **not** stabilise a head that already looks excellent.
- Do **not** reconstruct a natural arm to match an internal mathematical target.

The current canonical walk profile is a **technical reference authority, not a motion template.**
Nothing incoming has to become it. `CombatWalkCanonicalBaseline.asset` answers "is the machinery
behaving", never "is this the shape the motion should be".

## Stage 1 — raw source audit, READ-ONLY

Run no solver. Measure first, and classify every finding before touching anything:

| | measure |
|---|---|
| timing | clip length, cadence, implied speed from foot travel |
| root | root motion present? in-place? drift? |
| contact | contact frames, support schedule, double-support fraction, skate |
| body | posture, anatomy (`LocomotionAnatomicalRotationAudit`), pelvis excursion |
| quality | jitter/temporal continuity, loop seam (position AND velocity) |
| combat | arm and weapon behaviour, blade discipline |
| read | gameplay-camera silhouette |

Classify each aspect **KEEP / ADAPT / FIX / REJECT**. A single REJECT on something central (a walk
that is a different character) is a reason to reject the source, not to rebuild it.

Use `WeaponSolveExecutionTrace.BeginIsolation()` around any measurement described as raw — see the
isolation rule in SKILL.md.

## Stage 2 — modular correction authorities

Each is independent and each **defaults to OFF / preserve source.** Turn one on only when Stage 1
produced evidence naming it, and record which finding justified it:

```
timing adaptation          root / in-place conversion     speed matching
support correction         foot lock                      loop / seam repair
combat posture adaptation  weapon adaptation              head adaptation
anatomical correction
```

"The system has a pass for it" is not evidence.

## Stage 3 — motion preservation budget

After adapting, report SOURCE → ADAPTED deviation for each of:

```
pelvis trajectory    knee trajectories    feet
chest                head                 clavicles         hands
```

Not one scalar score — a scalar hides exactly the case that matters, a large deviation in one
channel averaged away by six small ones. **Any large deviation needs a named justification**
traceable to a Stage 1 finding. A large deviation with no such justification means the adaptation
overreached, and the fix is to turn an authority back off, not to explain the number.

## Bravehood art direction still governs

External motion gets no automatic artistic approval. The Knight 1H combat walk must read:

> calm · dangerous · prepared · grounded · experienced · restrained · controlled forward pressure ·
> tall composed torso · low controlled sword

A source can be beautifully human and still wrong — a casual civilian walk, a heroic parade march,
a sneaking assassin, an exaggerated Souls crouch. Adapt it or reject it. Preserving human quality
is not a licence to ship the wrong character.

## Challenger A/B

Always three clips, never two:

```
CURRENT TECHNICAL REFERENCE     __seqRepair_c40
NEW SOURCE, RAW                 unmodified, so its own quality is visible
NEW SOURCE, MINIMALLY ADAPTED   after Stage 2
```

Raw is in the comparison on purpose: it is the only way to see what adaptation cost.

A challenger wins only on **all three** of:

- **A.** it keeps Bravehood combat identity
- **B.** human review clearly prefers its motion quality
- **C.** technical gates remain acceptable

"Technically better" alone does not win. Neither does "looks nicer" while reading as the wrong
character.

## Intake checklist

1. Import the clip safely
2. Confirm Humanoid compatibility (rig, avatar, `humanScale`)
3. Canonicalise the measurement context (identity frame — see SKILL.md)
4. **Runtime gameplay IK OFF**
5. Raw visual + technical audit → KEEP / ADAPT / FIX / REJECT per aspect
6. **Write down explicitly what must NOT change**
7. State the Bravehood timing and art requirements this clip must meet
8. Apply minimum intervention — only authorities Stage 1 justified
9. Repeat the technical audit; report the motion preservation budget
10. Gameplay-camera review
11. **Runtime IK ON** compatibility review (`references/runtime-ik.md`)
12. Human artistic approval — the only source of it
13. Version it
14. Serialized controller integration + `AnimationAssetSafety` verification
15. Full regression suite

Step 6 is the one that gets skipped, and skipping it is how a good source becomes another
procedural clip.
