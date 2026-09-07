# v012 NOT GENERATED — the assumed sword/shoulder tradeoff does not exist in this architecture

The sweep was run as specified and came back FLAT. There is no knee in the curve because there is
no curve. Torso foundation, lower body, timing and support all untouched; nothing installed.

## SWEEP TABLE — weaponFrameTranslationFollow

Every other parameter identical to `UpperBodyWholeCycleFoundation.asset`.

| follow | clavicle mean/max | clav step | shoulder min/max | saturated | upper-arm dev | forearm dev | sword | vert | lat | fwd | blade | hand step |
|---|---|---|---|---|---|---|---|---|---|---|---|---|
| foundation (no weapon frame) | 18.9 / 21.0 | 1.3 | -1.00 / -1.00 | **100%** | 12.5 | 11.0 | 181 | 42 | 10 | 10 | 8.9 | 7.9 |
| 45% | 18.5 / 19.6 | 1.2 | -1.00 / -1.00 | **100%** | 12.8 | 10.0 | 209 | 39 | 20 | 18 | 9.4 | 9.8 |
| 60% | 18.4 / 19.5 | 1.1 | -1.00 / -1.00 | **100%** | 12.7 | 9.9 | 200 | 39 | 20 | 17 | 9.1 | 10.1 |
| 75% | 18.4 / 19.5 | 1.1 | -1.00 / -1.00 | **100%** | 12.6 | 9.9 | 198 | 39 | 19 | 17 | 9.2 | 10.5 |
| 90% | 18.4 / 19.5 | 1.2 | -1.00 / -1.00 | **100%** | 12.6 | 9.9 | 198 | 39 | 19 | 17 | 9.0 | 10.6 |
| 100% | 18.5 / 19.5 | 1.2 | -1.00 / -1.00 | **100%** | 12.6 | 9.9 | 198 | 40 | 19 | 16 | 8.8 | 10.7 |

Clavicle deviation moves 0.5 deg across the entire range and the shoulder is pinned at -1.000 for
100% of the cycle at every sample. Sword path rises 181 -> ~200 mm and then stops. Nothing is
being traded, because nothing is being bought.

## WHY THE OLD 100% RESULT WAS DIFFERENT

The earlier architecture at full chest-space gave clavicle 0.2 deg and sword 648 mm. That number is
no longer reachable, and the reason is the qualified foundation itself: the chest is now stabilised
(roll 20.0 -> 15.4, head roll 22.9 -> 3.6). Following a stabilised chest completely costs only
~200 mm instead of 648. The foundation removed the cost - and with it, the mechanism that was
delivering the benefit. The tradeoff was real in the OLD torso; it is absent in the new one.

## SECOND EXPERIMENT — CLAVICLE REMOVED FROM THE IK CHAIN

Since the frame's reference space changed nothing, I tested the other obvious cause: the solve
reaching with the clavicle because minimum-norm regularisation has no anatomical preference.

| | clavicle mean | saturated | sword |
|---|---|---|---|
| clavicle in chain, 75% | 18.4 | 100% | 198 |
| **clavicle EXCLUDED from chain, 75%** | **18.5** | **100%** | **202** |

Also flat. Removing the clavicle from the weapon chain entirely does not change the clavicle.

## WHERE THE SATURATION ACTUALLY COMES FROM

| configuration | Right Shoulder Down-Up |
|---|---|
| approved pose v005 | +0.099 |
| mocap source | -0.085 .. 0.038 |
| **both weapon paths inert** (frame architecture on, stability 0) | **+0.068 .. +0.118** |
| weapon frame active, clavicle excluded from chain | -1.000 .. -1.000 |
| foundation / v011 / canonical | -1.000 .. -1.000 |

With the weapon solve doing nothing the shoulder sits exactly at the approved value. So the weapon
solve is definitively responsible - but neither of the two levers this milestone was scoped around
touches it: not the target's reference frame, and not the chain's membership. The mechanism is
inside `SolveWeaponHand` itself, below the level of the dials, and it is the same at every follow
value because it does not depend on where the target is.

That is the blocker. It is not an artistic compromise to be chosen; there is currently no setting
of `weaponFrameTranslationFollow` that produces a shoulder worth choosing between.

## WHAT THIS MILESTONE DID ESTABLISH

- The requested region is mapped and the result is unambiguous: no knee, no usable tradeoff.
- The prior "clavicle relief tracks translation follow" conclusion is **superseded**. It was an
  artefact of the pre-foundation torso, not a property of the weapon target.
- The weapon frame architecture works as designed and is cheap: at 75% follow the sword gains
  ~17 mm of path over the foundation, stays smooth (blade range 9.2 vs 8.9, hand step 10.5 mm),
  and the torso and lower body are unaffected. It is simply not solving the shoulder.

## TORSO / LOWER BODY / TECHNICAL

The qualified foundation held throughout - chest and head results, planting, support L1/R1 and
lower-body isolation were unchanged by every weapon variant, which is itself confirmation that
`WeaponFramePass` re-solves only the 6-DOF body frame and never the legs.

Live profile restored to `CombatWalkCanonicalBaseline` and the zero-authority invariant re-verified:
**worst curve difference 0.000000** against the canonical probe.

Regression suite: **26 passed / 0 failed / 0 skipped** (O1, O2, O3, O4 all PASS).

v008 `7a7ca07437d46f9ade...`, v009 `5546958e...`, v010 `7c44dd1d...`, v011 `e3705c2f...` unchanged.
Controller from disk: `Light_Walk8` = v011 @ ts 1.00, `Travel_Walk8` = v008 @ ts 0.50,
`CombatWalkSpeedScale` 0.65. No temporary clip referenced.

Evidence kept: `__wc_cp12` (qualified foundation), `__wf75` (best weapon-frame sample),
`__nc75` (clavicle excluded), `__noweap` (weapon solve inert - the healthy-shoulder control).

## SMALLEST NEXT PROBLEM

Find why `SolveWeaponHand` drives the clavicle to its limit even when the clavicle is not in its
chain and the target is only ~20 mm from where the hand already is. The `__noweap` control isolates
it cleanly: everything upstream produces a healthy +0.099 shoulder, and one pass destroys it. That
is a solver-internals question, not an art-direction one, and it should be answered before any
further weapon art direction is attempted.

`v012 NOT GENERATED — sword/shoulder tradeoff is absent: clavicle stays 18.4-18.5 deg and 100% saturated across the entire 45-100% follow range, and also when excluded from the IK chain; cause is inside SolveWeaponHand, below the swept parameters`
