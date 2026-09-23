# Force-Aware Reinforcement Learning with Hybrid Sensorless Force Estimation for Wheeled-Legged Loco-Manipulation

**Citekey:** `zeng2026forceaware`
**Authors:** Xuanqi Zeng, Jiaming Wang, Tianlin Zhang, et al. (Zhongyu Li, Yun-Hui Liu)
**Venue:** arXiv 2026
**Links:** [arXiv](https://arxiv.org/abs/2609.13779)
**Read on:** _not yet_
**Bucket:** `05-explicit-conditioning`

**Also relevant to:** `05-explicit-conditioning`

## The three axes

| Axis | Answer |
|---|---|
| What is unknown | end-effector wrench, entangled with the support-contact reaction forces |
| How the context is obtained | explicitly estimated by a hybrid pipeline, analytic momentum observer plus contact-constrained projection plus a learned temporal residual, then fed to the policy as an explicit observation |
| Timescale of change | fast, continuous, tracked online while contact conditions change mid-task |
| Is the unknown recoverable from measurement? | _fill while reading_ |

## Mechanism

Estimates end-effector wrench without a force sensor by running a second-order generalized momentum observer on proprioception to get a whole-body disturbance residual, then a contact-constrained wrench projection that uses kinematics and support-contact feasibility to jointly estimate support reactions and the end-effector wrench, then a temporal residual network (TCN) that compensates the remaining estimation bias; the estimate feeds a force-aware RL controller with free-motion, force-regulation and hybrid force/position modes, demonstrated on a real wheeled-legged platform with a 6-DoF arm doing valve rotation, wiping, door opening and zero-force human-guided motion.

_Verify this against the paper. Rewrite it in your own words once you can state the input tensor, the horizon, and the loss._

## Claims and evidence

| Claim | Evidence | Do I believe it? |
|---|---|---|
| | | |

## What it buys me

The closest published pipeline to FAME's estimator, and it makes two points Niraj's thesis needs. First, the contact-constrained projection step is the part FAME does not currently do: on a wheeled-legged or planted humanoid the momentum residual mixes hand load with foot reactions, so this paper is the template for defending the separation. Second, the learned-residual-on-top-of-model-based structure is a concrete upgrade path for the Pinocchio tool if RNEA error becomes the accuracy bottleneck. It is also the strongest existing demonstration that an explicitly estimated wrench fed to the policy works on hardware, which is the empirical claim FAME shares. Note the platform is wheeled-legged, not a biped, so the balance-coupling argument does not transfer directly.

## Where it fails / what it does not cover

## Relation to FAME

Does this method assume the unknown is unobservable? Would it still make sense if the
quantity were measurable? What would it do on a fixed-stance task where stepping is a failure?

## Reviewer question it answers or raises

## Open threads

- [ ]
