# Learning Quadrupedal Locomotion over Challenging Terrain ★

**Citekey:** `lee2020challenging`
**Authors:** Joonho Lee, Jemin Hwangbo, Lorenz Wellhausen, Vladlen Koltun, Marco Hutter
**Venue:** Science Robotics 5(47), eabc5986
**Links:** [arXiv](https://arxiv.org/abs/2010.11251)
**Read on:** _not yet_
**Bucket:** `02-teacher-student`

## The three axes

| Axis | Answer |
|---|---|
| What is unknown | terrain geometry, foot contact state/force, friction, and external disturbance force applied to the base, none of which the real robot senses |
| How the context is obtained | two-stage privileged distillation, with the student's history encoder regressed onto the teacher's privileged latent (the paper ablates this latent term and shows naive imitation is worse) |
| Timescale of change | the history window is 2.0 s and the ablation shows performance rising monotonically from TCN-1 (0.02 s) to TCN-100; no cross-episode memory |
| Is the unknown recoverable from measurement? | _fill while reading_ |

## Mechanism

Trains a privileged teacher whose MLP encoder compresses ground-truth terrain profile (9 height scans per foot), foot contact states and forces, friction coefficients and the external disturbance force applied to the base into a latent l_t, then distills it into a TCN student that sees only a window of proprioceptive history (TCN-100 = the last 100 steps, 2.0 s) and is trained by DAgger on a two-term supervised loss: match the teacher's action AND regress the teacher's latent.

_Verify this against the paper. Rewrite it in your own words once you can state the input tensor, the horizon, and the loss._

## Claims and evidence

| Claim | Evidence | Do I believe it? |
|---|---|---|
| | | |

## What it buys me

The ancestor of the whole bucket and the cleanest statement of the load-bearing claim FAME implicitly relies on: a short proprioceptive window carries enough signal to identify unobserved contact physics. It matters doubly because the teacher here is privileged on external disturbance force, the exact quantity FAME recovers analytically via RNEA instead of regressing, and because their supplementary decoder experiments literally reconstruct the estimated external force on the torso under a 10 kg payload, i.e. they already show the latent encodes force. That is the sharpest version of the counterargument (why regress a latent at all if the force is recoverable?), and the TCN history encoder is the design his 3-step history encoder is a compressed instance of.

## Where it fails / what it does not cover

## Relation to FAME

Does this method assume the unknown is unobservable? Would it still make sense if the
quantity were measurable? What would it do on a fixed-stance task where stepping is a failure?

## Reviewer question it answers or raises

## Open threads

- [ ]
