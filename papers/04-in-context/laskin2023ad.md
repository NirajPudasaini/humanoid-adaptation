# In-context Reinforcement Learning with Algorithm Distillation

**Citekey:** `laskin2023ad`
**Authors:** Michael Laskin, Luyu Wang, Junhyuk Oh, Emilio Parisotto, Stephen Spencer, et al. (DeepMind)
**Venue:** ICLR 2023 (oral; arXiv 2210.14215, Oct 2022)
**Links:** [arXiv](https://arxiv.org/abs/2210.14215)
**Read on:** _not yet_
**Bucket:** `04-in-context`

## The three axes

| Axis | Answer |
|---|---|
| What is unknown | the task's optimal policy |
| How the context is obtained | implicitly, from a long context containing an entire learning trajectory including its mistakes, so the evidence is reward-labelled behaviour rather than sensor readings |
| Timescale of change | across episodes, deliberately spanning the full learning curve |
| Is the unknown recoverable from measurement? | _fill while reading_ |

## Mechanism

Records the full across-episode learning history of a source RL algorithm on many tasks and trains a causal transformer to autoregressively predict the next action given that history, so the distilled model reproduces the improvement curve itself in context, getting better within a deployment without any parameter update, and can end up more sample-efficient than the algorithm that generated the data.

_Verify this against the paper. Rewrite it in your own words once you can state the input tensor, the horizon, and the loss._

## Claims and evidence

| Claim | Evidence | Do I believe it? |
|---|---|---|
| | | |

## What it buys me

This is the mechanistic explanation for LocoFormer's most striking result, the emergent few-shot cross-trial improvement where early falls make later rollouts better. Reading it tells him that behaviour is a known, named phenomenon with a training-data prerequisite, not magic, and that the prerequisite is a context containing suboptimal-then-better behaviour. That gives him a sharp negative result to state: FAME's task forbids it structurally, because stepping away is disallowed and a hand-load spike gives one attempt, so there is no across-trial improvement channel and the information must come from the current torque reading.

## Where it fails / what it does not cover

## Relation to FAME

Does this method assume the unknown is unobservable? Would it still make sense if the
quantity were measurable? What would it do on a fixed-stance task where stepping is a failure?

## Reviewer question it answers or raises

## Open threads

- [ ]
