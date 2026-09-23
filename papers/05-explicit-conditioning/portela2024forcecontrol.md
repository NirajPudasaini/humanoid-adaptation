# Learning Force Control for Legged Manipulation ★

**Citekey:** `portela2024forcecontrol`
**Authors:** Tifanny Portela, Gabriel B. Margolis, Yandong Ji, Pulkit Agrawal
**Venue:** ICRA 2024
**Links:** [arXiv](https://arxiv.org/abs/2405.01402)
**Read on:** _not yet_
**Bucket:** `05-explicit-conditioning`

**Also relevant to:** `05-explicit-conditioning`

## The three axes

| Axis | Answer |
|---|---|
| What is unknown | the contact force at the end effector and the compliance of whatever is being pushed |
| How the context is obtained | two ways at once, a force *command* fed explicitly as conditioning and a force *estimate* regressed from 30 steps of proprioception, so the same paper sits on both sides of the explicit/implicit line |
| Timescale of change | fast, contact-event rate (tens of ms), which is the regime FAME cares about |
| Is the unknown recoverable from measurement? | _fill while reading_ |

## Mechanism

An RL policy on a Unitree B1 + Z1 arm takes a commanded 3D end-effector force (sampled roughly -70 to 70 N per axis) as part of its task command, concatenated with an H=30-step history of projected gravity, foot clock phases, joint positions, joint velocities and previous actions at 50 Hz; a separately trained estimator head regresses gripper position, gripper velocity and external contact force from the same proprioceptive stream (no F/T sensor), and training applies a randomized spring-damper soft-contact load at the gripper while the episode resamples between force-tracking and position-tracking objectives.

_Verify this against the paper. Rewrite it in your own words once you can state the input tensor, the horizon, and the loss._

## Claims and evidence

| Claim | Evidence | Do I believe it? |
|---|---|---|
| | | |

## What it buys me

The closest prior art to FAME's core claim on a legged platform, and the best template for how to write the claim up. It establishes that force can be conditioned on without an F/T sensor, but it gets the force from a learned proprioceptive regressor rather than RNEA plus the arm Jacobian. That is your differentiator paragraph: FAME's Pinocchio inverse-dynamics estimate is model-based and physically grounded, so it degrades predictably and is auditable, whereas a learned regressor inherits the training distribution. Also gives you a ready-made ablation axis (commanded force vs estimated force as the conditioning input) and the force-tracking-vs-position-tracking alternation as a curriculum idea for the planted-hands task.

## Where it fails / what it does not cover

## Relation to FAME

Does this method assume the unknown is unobservable? Would it still make sense if the
quantity were measurable? What would it do on a fixed-stance task where stepping is a failure?

## Reviewer question it answers or raises

## Open threads

- [ ]
