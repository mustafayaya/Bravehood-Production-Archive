# Free-travel walk set — regeneration brief (Unity AI)

Target: the **non-locked-on** walk set. In `Knight_Controller` this is the
`Travel_Walk8` blend tree (Base Layer ▸ `Locomotion` ▸ `Stance_Blend` @ Stance = 0).
Its eight children today are the MoCapCentral clips
`Assets/Plugins/MoCapCentral/MC_DungeonLife/Animations/SwordShield/MCU_am_SwordShield_Loco_Walk_*.FBX`.

## Why they are being replaced (put this in front of yourself before writing prompts)

1. **Wrong prop.** It is a sword *and shield* capture. The player carries a one-handed
   sword and nothing in the left hand, so the left arm braces around thin air.
2. **The 8-way grid is not 8-way.** After the square-up reimport the clips' real travel
   directions are `0°, 45°, 48.9°, 79.4°, 148.6°, -155.9°, -125.7°, -84.5°`. Three clips
   crowd the front-right quadrant, and there are **69° and 84° holes** where a pure right
   strafe and a forward-left step should be. Pushing "right" blends a back-right clip with
   a backward clip.
3. **Speeds are all over the place** — 0.91 to 1.33 m/s across the set, so the stride
   matcher scrubs playback by up to ±40% depending which way you walk.
4. This set is on screen **most of the time**: the Player prefab ships
   `FaceMovementWhenFree = 0`, so unlocked the body faces the camera and WASD strafes
   around it. All eight directions are in constant use, not just forward.

---

## Shared preamble — paste at the top of every prompt

> A lean human warrior in light leather-and-mail armour walks at a steady, unhurried
> travel pace. He carries a one-handed arming sword in his right hand, blade angled down
> and slightly out from the leg, wrist relaxed — the weapon is being carried, not held on
> guard. His left hand is empty and swings freely at his side. Shoulders square and level,
> spine tall, chin up and eyes forward. Weight settles cleanly over each foot; heel strikes
> first, toe pushes off. The motion is grounded and confident, not stiff, not swaggering,
> no crouch, no combat stance. Seamless looping cycle, exactly two full steps, feet return
> to the starting pose.

## The eight prompts

Naming to generate: `Bravehood_TravelWalk_<Dir>` for
`Fwd, FwdRight, Right, BwdRight, Bwd, BwdLeft, Left, FwdLeft`.

**Fwd (0°)** — highest priority, it is the clip you will look at most.
> …walks straight forward at a constant pace. Natural contralateral arm swing: the empty
> left arm swings forward as the right leg steps forward. The sword hand swings with a
> heavier, shorter arc — the weight of the blade damps it. Very slight vertical bob at
> each footfall, no side-to-side sway.

**FwdRight (45°)**
> …walks forward and to his right along a diagonal while keeping his chest and face
> pointing straight ahead, past his left shoulder. Hips lead the diagonal, torso resists
> and stays square to camera. Steps cross slightly, the trailing left foot closing in
> behind rather than crossing over.

**Right (90°)**
> …side-steps to his right, chest and face staying square to the front the whole time.
> The right foot leads out, the left foot closes to it without ever crossing in front.
> Knees stay soft and slightly bent, feet stay low and skim the ground, weight stays
> centred. A controlled combat side-step, not a shuffle and not a dance step.

**BwdRight (135°)**
> …steps backward and to his right on a diagonal while his chest and face stay pointing
> forward. He glances his weight back over the trailing foot, toe touching down before
> heel. Slightly cautious, as if giving ground while keeping his eyes on what is in front.

**Bwd (180°)**
> …walks straight backward, chest and face still pointing forward. Toe touches down first
> on each step, then the heel lowers. Torso leans a fraction forward to counterbalance,
> stride shorter than his forward walk. Steady and deliberate, not a stumble or a retreat.

**BwdLeft (-135°)**
> …steps backward and to his left on a diagonal while his chest and face stay pointing
> forward. Weight rolls back onto the trailing foot, toe first. Sword hand drifts a little
> across the body as he gives ground.

**Left (-90°)**
> …side-steps to his left, chest and face staying square to the front the whole time. The
> left foot leads out, the right foot closes to it without crossing in front. Knees soft,
> feet low, weight centred. Mirror-clean against the right side-step.

**FwdLeft (-45°)**
> …walks forward and to his left along a diagonal while keeping his chest and face pointing
> straight ahead, past his right shoulder. Hips lead the diagonal, torso stays square to
> camera. The sword arm swings a touch wider to clear the leading left hip.

---

## Generation settings

| | |
|---|---|
| Package | `com.unity.ai.generators` — **not installed yet**; add it in Package Manager (the project only has `com.unity.ai.assistant`) |
| Where | `Assets ▸ Create ▸ Animation ▸ Generate`, or the Generate button on an AnimationClip |
| Rig | Humanoid. The player rig imports as Humanoid (`animationType: 3`), so generated humanoid clips retarget straight onto it |
| Length | ~1.1 s per clip = **2 full steps at 30 fps (33 frames)**, matching the current `Loco_Walk_Fwd` |
| Character scale | Player is **~1.59 m** tall — world scale is locked, do not resize anything to fit a clip |
| Ground speed | Aim for the **same speed in all eight clips**, ≈ **1.35 m/s**. Uniform speed is the single biggest win over the current set; it keeps the stride matcher near 1.0× in every direction |
| Reference video | If the generator accepts a video reference, feed it a locked-off side and front view of a real one-handed-sword travel walk — it beats text for foot contact quality |

## Import settings after generation

Match the existing travel clips exactly:

- `Loop Time` on, `Loop Pose` on
- Root Transform Rotation: **Bake Into Pose OFF**, Based Upon **Original** ← mandatory on this rig
- Root Transform Position (Y): Bake Into Pose on, Based Upon Original
- Root Transform Position (XZ): **Bake Into Pose OFF** (the travel set is root-motion; the controller consumes it in `OnAnimatorMove`)
- Keep the RM version. Do **not** wire a `_NoRM` variant into `Travel_Walk8` — the double-rotation trap.

## Integration checklist

1. Drop the eight clips in `Assets/Animations/Travel_Walk8_AI/`.
2. Repoint the eight children of `Travel_Walk8` in
   [Knight_Controller.controller](Assets/Core/Animations/Knight_Controller.controller:2780).
3. Set the children's `m_Position` back to the **exact 45° grid**
   — `(0,1) (0.707,0.707) (1,0) (0.707,-0.707) (0,-1) (-0.707,-0.707) (-1,0) (-0.707,0.707)` —
   this is what fixes the 69°/84° holes.
4. Re-measure each clip's real ground speed and update the tables in
   [CharacterAnimator.cs:78](Assets/Core/Scripts/Gameplay/CharacterAnimator.cs:78):
   `TravelWalkAng` becomes the clean `{-135,-90,-45,0,45,90,135,180}` grid and
   `TravelWalkRef` becomes the eight measured speeds.
5. Update `TravelWalkRefSpeed` (currently 1.33) on the Player's `CharacterAnimator` to the
   new forward speed.
6. `Travel_Jog8` and the guard set (`Gait_Light`, used when locked on) are untouched — but
   the jog set has the same crowding problem and is the obvious next batch.
