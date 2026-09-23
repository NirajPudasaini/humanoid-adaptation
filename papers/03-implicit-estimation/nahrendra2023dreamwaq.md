# DreamWaQ: Learning Robust Quadrupedal Locomotion With Implicit Terrain Imagination via Deep Reinforcement Learning ★

**Citekey:** `nahrendra2023dreamwaq`
**Authors:** I Made Aswin Nahrendra, Byeongho Yu, Hyun Myung
**Venue:** ICRA 2023
**Links:** [arXiv](https://arxiv.org/abs/2301.10602)
**Read on:** _not yet_
**Bucket:** `03-implicit-estimation`

**Also relevant to:** `02-teacher-student`

## The three axes

| Axis | Answer |
|---|---|
| What is unknown | terrain geometry and contact properties, plus unmeasurable base linear velocity |
| How the context is obtained | hybrid, one explicit supervised head (velocity) and one implicit unsupervised head (beta-VAE latent trained only by next-observation reconstruction + KL to a standard normal) |
| Timescale of change | 5 steps of history at the stated 50 Hz inference rate, i.e. 0.1 s adaptation, with the latent re-inferred every control tick (PD controller runs at 200 Hz) |
| Is the unknown recoverable from measurement? | _fill while reading_ |

## Mechanism

CENet is a single shared encoder over H=5 frames of proprioceptive history with two heads: one regresses base linear velocity v_t against simulator ground truth (MSE), the other is a beta-VAE decoder that reconstructs the next observation o_{t+1}, with the VAE posterior q(z_t | o^H_t) serving as the terrain context handed to the actor; CENet, actor and an asymmetric privileged critic are all optimized synchronously in one training phase, trained for 1,000 iterations with 4,096 domain-randomized agents in parallel.

_Verify this against the paper. Rewrite it in your own words once you can state the input tensor, the horizon, and the loss._

## Claims and evidence

| Claim | Evidence | Do I believe it? |
|---|---|---|
| | | |

## What it buys me

DreamWaQ is the architectural archetype of FAME's own split: one explicitly recoverable physical quantity estimated by regression sitting next to an implicit latent that absorbs everything else. Reading it gives Niraj a ready-made framing sentence for the thesis ('the explicit head is whatever is identifiable from proprioception; in DreamWaQ that is base velocity, in FAME it is the hand wrench') and an ablation template, since the paper already ablates explicit-only vs latent-only vs both. It is also the canonical citation for 'one-stage asymmetric actor-critic with a context estimator', which is the exact bucket label, and it is the paper to cite when justifying that the estimator does not need a privileged teacher.

## Where it fails / what it does not cover

## Relation to FAME

Does this method assume the unknown is unobservable? Would it still make sense if the
quantity were measurable? What would it do on a fixed-stance task where stepping is a failure?

## Reviewer question it answers or raises

## Open threads

- [ ]
