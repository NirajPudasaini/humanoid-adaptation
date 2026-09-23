# Learning a Unified Policy for Position and Force Control in Legged Loco-Manipulation

**Citekey:** `zhi2025unifiedforce`
**Authors:** Peiyuan Zhi, Peiyang Li, Jianqin Yin, Baoxiong Jia, Siyuan Huang
**Venue:** CoRL 2025 (Proceedings of the 9th Conference on Robot Learning, PMLR 305:652-669)
**Links:** [arXiv](https://arxiv.org/abs/2505.20829)
**Read on:** _not yet_
**Bucket:** `05-explicit-conditioning`

**Also relevant to:** `03-implicit-estimation`

## The three axes

| Axis | Answer |
|---|---|
| What is unknown | the external contact wrench at the end effector plus unmeasurable base velocity and end-effector position |
| How the context is obtained | explicit supervised regression from a 32-step proprioceptive window, single-stage joint training, no distillation and no wrist sensor |
| Timescale of change | 32 steps of history, roughly 0.6 s at a typical 50 Hz control rate (the paper does not state the rate explicitly), deliberately longer than the HIM/DreamWaQ 5-step window because contact wrench must be disentangled from the arm's own inertial torques |
| Is the unknown recoverable from measurement? | _fill while reading_ |

## Mechanism

An encoder over H=32 timesteps of proprioceptive history o_[t-H..t] (base orientation and angular velocity, joint positions and velocities, previous actions, commands) feeds a state estimator head that predicts the external force F = F_ext + F_react, the end-effector position and the base velocity, all trained jointly with the actor in one PPO run with an MSE loss on the estimator, giving force tracking with average errors within 10 N for commanded forces up to 60 N in simulation (real-world evaluation capped at 40 N on some axes by hardware limits) on a Unitree B2 with a 6-DoF Z1 arm and on the 29-DoF Unitree G1, with no force/torque sensor.

_Verify this against the paper. Rewrite it in your own words once you can state the input tensor, the horizon, and the loss._

## Claims and evidence

| Claim | Evidence | Do I believe it? |
|---|---|---|
| | | |

## What it buys me

This is the nearest neighbour to FAME in the whole bucket and therefore the paper most likely to be raised as prior work, so it belongs in the repo with a clear delta written next to it. Both estimate an external hand force from joint-level signals with no F/T sensor and both feed it back into a single-stage policy; the differences are that this work learns the force estimate as a regression head, whereas FAME computes it analytically via RNEA plus the arm Jacobian, and that it targets force tracking during manipulation on a quadruped-plus-arm and G1 rather than holding feet and hands planted under a fast-changing exogenous load on H1-2. Its 32-step window is direct empirical evidence on the question the thesis has to answer with FAME's 3-step history: how much proprioceptive context a wrench estimate actually needs. Cite the CoRL 2025 proceedings version (PMLR v305), not the arXiv preprint.

## Where it fails / what it does not cover

## Relation to FAME

Does this method assume the unknown is unobservable? Would it still make sense if the
quantity were measurable? What would it do on a fixed-stance task where stepping is a failure?

## Reviewer question it answers or raises

## Open threads

- [ ]
