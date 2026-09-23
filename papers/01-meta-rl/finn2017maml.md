# Model-Agnostic Meta-Learning for Fast Adaptation of Deep Networks

**Citekey:** `finn2017maml`
**Authors:** Chelsea Finn, Pieter Abbeel, Sergey Levine
**Venue:** ICML 2017
**Links:** [arXiv](https://arxiv.org/abs/1703.03400)
**Read on:** _not yet_
**Bucket:** `01-meta-rl`

## The three axes

| Axis | Answer |
|---|---|
| What is unknown | the task, meaning reward function or dynamics, drawn from a training distribution |
| How the context is obtained | an explicit gradient step on freshly collected on-task experience, so the context lives in the weight delta and nowhere else |
| Timescale of change | one episode batch per adaptation, with no mechanism for change within an episode |
| Is the unknown recoverable from measurement? | _fill while reading_ |

## Mechanism

Optimizes an initial parameter vector so that one or a few policy-gradient steps computed on a small batch of trajectories from a new task maximize post-update return, differentiating the outer objective through the inner update.

_Verify this against the paper. Rewrite it in your own words once you can state the input tensor, the horizon, and the loss._

## Claims and evidence

| Claim | Evidence | Do I believe it? |
|---|---|---|
| | | |

## What it buys me

The reference point the whole bucket is defined against, worth reading once so the thesis can state precisely what 'adaptation' means in the gradient sense: a weight delta paid for with on-task reward. That definition is what lets Niraj draw the three-way contrast cleanly, since FAME's adaptation is a forward pass on an estimated force, LocoFormer's is an activation state in a long context, and MAML's is an optimizer step. The RL experiments also make plain that the inner loop needs reward, which FAME has no access to at deployment under an unknown hand load.

## Where it fails / what it does not cover

## Relation to FAME

Does this method assume the unknown is unobservable? Would it still make sense if the
quantity were measurable? What would it do on a fixed-stance task where stepping is a failure?

## Reviewer question it answers or raises

## Open threads

- [ ]
