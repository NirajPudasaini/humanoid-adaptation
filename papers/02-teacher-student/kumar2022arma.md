# Adapting Rapid Motor Adaptation for Bipedal Robots

**Citekey:** `kumar2022arma`
**Authors:** Ashish Kumar, Zhongyu Li, Jun Zeng, Deepak Pathak, Koushil Sreenath, Jitendra Malik
**Venue:** IROS 2022
**Links:** [arXiv](https://arxiv.org/abs/2205.15299)
**Read on:** _not yet_
**Bucket:** `02-teacher-student`

## The three axes

| Axis | Answer |
|---|---|
| What is unknown | same extrinsics family as RMA (mass, friction, motor and terrain properties) but on an underactuated, low-inertia-margin platform |
| How the context is obtained | two-stage regression plus a third RL phase that closes the loop on estimator error |
| Timescale of change | sub-second, with the crucial point that bipedal failure modes are faster than the estimator's convergence |
| Is the unknown recoverable from measurement? | _fill while reading_ |

## Mechanism

Adds a third phase to RMA on bipedal Cassie: after the adaptation module is regressed onto the privileged extrinsics encoder, the base policy is fine-tuned with model-free RL while consuming the imperfect estimated extrinsics, so the controller is optimized against its own estimator error rather than against the ground-truth latent it was trained on.

_Verify this against the paper. Rewrite it in your own words once you can state the input tensor, the horizon, and the loss._

## Claims and evidence

| Claim | Evidence | Do I believe it? |
|---|---|---|
| | | |

## What it buys me

The direct evidence that naive RMA degrades when the platform cannot tolerate a transiently wrong latent, which is exactly FAME's regime: a humanoid with planted hands and feet under a fast-changing load has no recovery step available (stepping away is disallowed). The train-on-your-own-estimator-error trick is a cheap, concrete ablation he can run on FAME's force channel, and it gives him a principled answer to the reviewer question of what happens when the RNEA force estimate is biased or lagging.

## Where it fails / what it does not cover

## Relation to FAME

Does this method assume the unknown is unobservable? Would it still make sense if the
quantity were measurable? What would it do on a fixed-stance task where stepping is a failure?

## Reviewer question it answers or raises

## Open threads

- [ ]
