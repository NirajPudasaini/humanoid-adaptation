# HOMIE: Humanoid Loco-Manipulation with Isomorphic Exoskeleton Cockpit

**Citekey:** `ben2025homie`
**Authors:** Qingwei Ben, Feiyu Jia, Jia Zeng, Junting Dong, Dahua Lin, Jiangmiao Pang
**Venue:** RSS 2025 (open-sourced as OpenHomie)
**Links:** [arXiv](https://arxiv.org/abs/2502.13013)
**Read on:** _not yet_
**Bucket:** `05-explicit-conditioning`

## The three axes

| Axis | Answer |
|---|---|
| What is unknown | the quasi-static load and CoM shift induced by an arbitrary, operator-chosen arm pose |
| How the context is obtained | measured, not estimated; the arm joint pose is fed to the policy as a direct observation |
| Timescale of change | quasi-static to medium, changing at teleoperation speed, with no model of what the hands are holding |
| Is the unknown recoverable from measurement? | _fill while reading_ |

## Mechanism

An RL lower-body policy conditioned on the measured upper-body joint pose, trained with an upper-body pose curriculum, a height-tracking reward and symmetry augmentation so the base stays stable and can squat to a commanded height under arbitrary arm configurations, driven at runtime by an isomorphic exoskeleton whose joint readings are copied directly to the robot arms with no IK.

_Verify this against the paper. Rewrite it in your own words once you can state the input tensor, the horizon, and the loss._

## Claims and evidence

| Claim | Evidence | Do I believe it? |
|---|---|---|
| | | |

## What it buys me

The other named FAME baseline, and the one that isolates exactly what FAME adds. HOMIE already conditions on measured arm pose, which is half of FAME's conditioning vector, so any FAME-over-HOMIE gain is attributable specifically to the estimated force channel and not to pose awareness. That makes HOMIE the right anchor for the force_zero ablation story. Reading it also pins down the pose-curriculum and height-tracking reward details needed to keep the trained-baseline comparison honest, since it is a different repo and protocol from the inference-time ablation.

## Where it fails / what it does not cover

## Relation to FAME

Does this method assume the unknown is unobservable? Would it still make sense if the
quantity were measurable? What would it do on a fixed-stance task where stepping is a failure?

## Reviewer question it answers or raises

## Open threads

- [ ]
