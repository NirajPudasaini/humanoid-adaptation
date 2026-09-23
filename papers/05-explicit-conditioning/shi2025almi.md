# Adversarial Locomotion and Motion Imitation for Humanoid Policy Learning

**Citekey:** `shi2025almi`
**Authors:** Jiyuan Shi, Xinzhe Liu, Dewei Wang, et al. (Chenjia Bai, Xuelong Li)
**Venue:** NeurIPS 2025
**Links:** [arXiv](https://arxiv.org/abs/2504.14305)
**Read on:** _not yet_
**Bucket:** `05-explicit-conditioning`

## The three axes

| Axis | Answer |
|---|---|
| What is unknown | the internal reaction wrench the upper body imposes on the lower body, which is an endogenous disturbance, not an exogenous load |
| How the context is obtained | never estimated; absorbed into the policy by adversarial co-training, so the disturbance distribution is learned offline rather than identified online |
| Timescale of change | fast, within-episode, but bounded by the motion distribution seen in training |
| Is the unknown recoverable from measurement? | _fill while reading_ |

## Mechanism

Decouples the humanoid into a lower-body policy tracking velocity commands and an upper-body policy tracking reference motions, then trains them adversarially and iteratively: each is optimized against the disturbance the other creates, so the lower body learns to reject the reaction wrench of arbitrary arm motion and the upper body learns to track while the base moves; released with ALMI-X, a large-scale (80K+ trajectory) language-annotated whole-body trajectory dataset generated in MuJoCo by the trained policy, and deployed on the full-size Unitree H1.

_Verify this against the paper. Rewrite it in your own words once you can state the input tensor, the horizon, and the loss._

## Claims and evidence

| Claim | Evidence | Do I believe it? |
|---|---|---|
| | | |

## What it buys me

A named FAME baseline, so the citation has to be exactly right, but the more useful thing to extract is the framing gap. ALMI's whole notion of 'disturbance' is self-generated arm motion, which is knowable from the commanded arm trajectory; FAME's is an exogenous hand load that no command reveals. Stating that distinction sharply is the cleanest way to justify why FAME needs an estimator at all, and why upper/lower decoupling alone does not cover the FAME task. ALMI-X is also a candidate motion source if the thesis ever needs arm-pose diversity beyond the current curriculum.

## Where it fails / what it does not cover

## Relation to FAME

Does this method assume the unknown is unobservable? Would it still make sense if the
quantity were measurable? What would it do on a fixed-stance task where stepping is a failure?

## Reviewer question it answers or raises

## Open threads

- [ ]
