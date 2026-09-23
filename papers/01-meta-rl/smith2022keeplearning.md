# Legged Robots that Keep on Learning: Fine-Tuning Locomotion Policies in the Real World ★

**Citekey:** `smith2022keeplearning`
**Authors:** Laura Smith, J. Chase Kew, Xue Bin Peng, Sehoon Ha, Jie Tan, Sergey Levine
**Venue:** ICRA 2022
**Links:** [arXiv](https://arxiv.org/abs/2110.05457)
**Read on:** _not yet_
**Bucket:** `01-meta-rl`

## The three axes

| Axis | Answer |
|---|---|
| What is unknown | the deployment environment's dynamics and terrain, which were not in the training distribution and are not represented by any latent input |
| How the context is obtained | reward-driven gradient updates to the policy weights from on-robot rollouts, so the context is absorbed into the parameters rather than inferred |
| Timescale of change | hours, and once adapted the policy is specialized to that terrain, so it assumes the disturbance is stationary |
| Is the unknown recoverable from measurement? | _fill while reading_ |

## Mechanism

Takes a motion-imitation policy pretrained in simulation and continues training its weights on hardware with REDQ (randomized ensembled double Q-learning, high gradient-steps-per-environment-step), using a simulation-trained fall recovery / reset policy for autonomous resets, reaching reliable skills after under 2.5 hours of real interaction per terrain (under 2 hours on the outdoor lawn), including battery swaps and malfunctions.

_Verify this against the paper. Rewrite it in your own words once you can state the input tensor, the horizon, and the loss._

## Claims and evidence

| Claim | Evidence | Do I believe it? |
|---|---|---|
| | | |

## What it buys me

This is the honest upper bound on what weight-changing adaptation costs on real legged hardware, and it is the number Niraj should cite to bound the whole bucket out of FAME's regime. A hand load that changes in under a second cannot be tracked by a process that needs two hours and a fall recovery controller, which motivates both FAME's estimator and LocoFormer's in-context memory as the only viable families. It also gives him the vocabulary (autonomous resets, sample-efficient off-policy fine-tuning) for a future-work section on adapting FAME on the H1-2 between sessions rather than within an episode.

## Where it fails / what it does not cover

## Relation to FAME

Does this method assume the unknown is unobservable? Would it still make sense if the
quantity were measurable? What would it do on a fixed-stance task where stepping is a failure?

## Reviewer question it answers or raises

## Open threads

- [ ]
