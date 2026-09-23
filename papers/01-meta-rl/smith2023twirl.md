# Learning and Adapting Agile Locomotion Skills by Transferring Experience

**Citekey:** `smith2023twirl`
**Authors:** Laura Smith, J. Chase Kew, Tianyu Li, Linda Luu, Xue Bin Peng, Sehoon Ha, Jie Tan, Sergey Levine
**Venue:** RSS 2023 (DOI 10.15607/RSS.2023.XIX.051)
**Links:** [arXiv](https://arxiv.org/abs/2304.09834)
**Read on:** _not yet_
**Bucket:** `01-meta-rl`

## The three axes

| Axis | Answer |
|---|---|
| What is unknown | how to solve a harder task, or the same task under changed dynamics or objective, given a controller that is suboptimal for it |
| How the context is obtained | offline data from the old controller mixed into a new optimization, which is adaptation at training time rather than deployment time |
| Timescale of change | a full retraining run, mostly in simulation with direct hardware transfer, so it does not adapt at all once deployed |
| Is the unknown recoverable from measurement? | _fill while reading_ |

## Mechanism

Rolls out an existing source controller in the target MDP to build a source dataset, then trains the new task with DroQ (SAC plus dropout and layer normalization) while sampling each minibatch from the source and target replay buffers at a fixed ratio, so what transfers is off-policy experience rather than weights, a reward term or a KL penalty.

_Verify this against the paper. Rewrite it in your own words once you can state the input tensor, the horizon, and the loss._

## Claims and evidence

| Claim | Evidence | Do I believe it? |
|---|---|---|
| | | |

## What it buys me

The practical answer to a question Niraj will hit directly: he already has trained HOMIE and ALMI baselines, and this shows how to reuse their rollouts to bootstrap a force-conditioned policy instead of starting from scratch. It also marks the boundary of this bucket, since the weights change but never during deployment, which makes it a useful contrast case when he argues that 'adaptation' in the literature covers at least three different clocks. The project page names the method TWiRL, which justifies the citekey.

## Where it fails / what it does not cover

## Relation to FAME

Does this method assume the unknown is unobservable? Would it still make sense if the
quantity were measurable? What would it do on a fixed-stance task where stepping is a failure?

## Reviewer question it answers or raises

## Open threads

- [ ]
