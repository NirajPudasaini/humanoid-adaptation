# Proprioceptive External Torque Learning for Floating Base Robot and its Applications to Humanoid Locomotion

**Citekey:** `lim2023proprioceptive`
**Authors:** Daegyu Lim, Myeong-Ju Kim, Junhyeok Cha, Donghyeon Kim, Jaeheung Park
**Venue:** IROS 2023
**Links:** [arXiv](https://arxiv.org/abs/2309.04138)
**Read on:** _not yet_
**Bucket:** `05-explicit-conditioning`

## The three axes

| Axis | Answer |
|---|---|
| What is unknown | external joint torques and the resulting contact wrench on a floating base |
| How the context is obtained | explicitly estimated from a proprioceptive time series, learned (GRU) and benchmarked head-to-head against the analytic momentum-observer baseline |
| Timescale of change | continuous and fast, tracked every control step during walking |
| Is the unknown recoverable from measurement? | _fill while reading_ |

## Mechanism

Trains a GRU on random-walking data to regress external joint torque for a floating-base humanoid from encoders and IMU only, reports significantly smaller error than a model-based momentum observer (MOB) with friction modeling, converts the estimated joint torques into a contact wrench, and closes the loop by feeding that wrench into ZMP feedback control for stable walking, with the estimator holding up under changes to robot configuration.

_Verify this against the paper. Rewrite it in your own words once you can state the input tensor, the horizon, and the loss._

## Claims and evidence

| Claim | Evidence | Do I believe it? |
|---|---|---|
| | | |

## What it buys me

This is the reference that makes FAME's central premise defensible rather than assumed, and it does so on a real humanoid with a floating base, which is where the naive fixed-base RNEA argument gets challenged. It supplies the momentum-observer baseline framing Niraj should acknowledge next to his Pinocchio RNEA + Jacobian recovery, quantifies how much a learned estimator beats the analytic one, and shows a worked example of an estimated wrench actually driving a balance controller. It is also the cleanest citation for 'external wrench estimation on legged robots is a mature line, not a side assumption'.

## Where it fails / what it does not cover

## Relation to FAME

Does this method assume the unknown is unobservable? Would it still make sense if the
quantity were measurable? What would it do on a fixed-stance task where stepping is a failure?

## Reviewer question it answers or raises

## Open threads

- [ ]
