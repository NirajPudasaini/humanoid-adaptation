# FALCON: Learning Force-Adaptive Humanoid Loco-Manipulation ★

**Citekey:** `zhang2025falcon`
**Authors:** Yuanhang Zhang, Yifu Yuan, Prajwal Gurunath, et al. (Tairan He, Guanya Shi)
**Venue:** L4DC 2026 (Oral)
**Links:** [arXiv](https://arxiv.org/abs/2505.06776)
**Read on:** _not yet_
**Bucket:** `05-explicit-conditioning`

**Also relevant to:** `05-explicit-conditioning`

## The three axes

| Axis | Answer |
|---|---|
| What is unknown | magnitude/direction of the external end-effector wrench (0-20N payload transport, 0-40N door opening, 0-100N cart pulling in the real-world tasks) |
| How the context is obtained | never estimated at deployment; inferred implicitly from a short (5-step) proprioceptive history, with the true force used only as a privileged critic input at train time |
| Timescale of change | fast, within-episode, force changes step to step under the curriculum; no cross-episode memory |
| Is the unknown recoverable from measurement? | _fill while reading_ |

## Mechanism

Two decoupled PPO agents with separate rewards (lower body for locomotion under disturbance, upper body for end-effector position tracking), trained jointly in the same simulation, whose actors observe only a five-step history of joint positions, joint velocities, root angular velocity, projected gravity and previous actions; the external end-effector force is given only to the critics as privileged information under asymmetric actor-critic training, and robustness comes from a torque-limit-aware 3D force curriculum that computes the admissible force along each direction from the EE Jacobian and joint torque limits, then escalates it over training.

_Verify this against the paper. Rewrite it in your own words once you can state the input tensor, the horizon, and the loss._

## Claims and evidence

| Claim | Evidence | Do I believe it? |
|---|---|---|
| | | |

## What it buys me

This is the direct antagonist of FAME's thesis and the single most important baseline to argue against. FALCON's upper-body agent is described as doing 'implicit adaptive force compensation' and its actor sees only a five-step proprioceptive history, which is almost exactly FAME's 3-step encoder window but with the force left implicit and confined to the critic. Reading it gives Niraj (a) the precise claim he must falsify or bound, i.e. that implicit adaptation over a very short window is enough, (b) the torque-limit-aware force curriculum as a training-side trick he can adopt independently of the estimator, and (c) a clean three-way axis for the thesis: FALCON implicit + short window, LocoFormer implicit + long window, FAME explicit + short window. Note the released code supports Unitree G1 (29 DoF) and Booster T1 (29 DoF), not H1-2, and the robot is allowed to step, so it is not a drop-in comparison for the feet-and-hands-planted task.

## Where it fails / what it does not cover

## Relation to FAME

Does this method assume the unknown is unobservable? Would it still make sense if the
quantity were measurable? What would it do on a fixed-stance task where stepping is a failure?

## Reviewer question it answers or raises

## Open threads

- [ ]
