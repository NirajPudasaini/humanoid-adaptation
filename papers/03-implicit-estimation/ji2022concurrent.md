# Concurrent Training of a Control Policy and a State Estimator for Dynamic and Robust Legged Locomotion

**Citekey:** `ji2022concurrent`
**Authors:** Gwanghyeon Ji, Juhyeok Mun, Hyeongjun Kim, Jemin Hwangbo
**Venue:** IEEE RA-L 7(2), April 2022 (also presented at ICRA 2022)
**Links:** [arXiv](https://arxiv.org/abs/2202.05481)
**Read on:** _not yet_
**Bucket:** `03-implicit-estimation`

**Also relevant to:** `02-teacher-student`

## The three axes

| Axis | Answer |
|---|---|
| What is unknown | named, physically meaningful robot states that are unmeasurable on hardware (base linear velocity, foot height, contact probability) rather than environment parameters |
| How the context is obtained | fully explicit supervised regression, concurrent with policy optimization, no latent and no distillation |
| Timescale of change | per-control-step; the estimate is a filtered instantaneous quantity, not an episode-level property |
| Is the unknown recoverable from measurement? | _fill while reading_ |

## Mechanism

A state estimation network is trained by supervised regression onto simulator ground truth for base linear velocity, foot height and foot contact probability from the same proprioceptive history the policy sees, and its outputs are fed straight into the policy observation while both networks are updated in the same PPO loop, so the policy learns against the estimator's actual error distribution rather than perfect state; the resulting policy runs at up to 3.75 m/s on flat ground and 3.54 m/s on a slippery plate at friction 0.22.

_Verify this against the paper. Rewrite it in your own words once you can state the input tensor, the horizon, and the loss._

## Claims and evidence

| Claim | Evidence | Do I believe it? |
|---|---|---|
| | | |

## What it buys me

This is the ancestor of the whole bucket and the purest 'explicit estimate, single stage' point on the taxonomy, which is exactly the corner FAME occupies. Its key argument is the one Niraj needs to make defensively: training the policy concurrently with an imperfect estimator makes the policy robust to that estimator's bias, which is the direct answer to a reviewer asking why FAME conditions on an RNEA-derived force estimate with known error rather than on a ground-truth force. Cite it as the precedent that estimator noise is a feature of joint training, and as the contrast case that has no implicit latent at all, bracketing LocoFormer at the opposite end.

## Where it fails / what it does not cover

## Relation to FAME

Does this method assume the unknown is unobservable? Would it still make sense if the
quantity were measurable? What would it do on a fixed-stance task where stepping is a failure?

## Reviewer question it answers or raises

## Open threads

- [ ]
