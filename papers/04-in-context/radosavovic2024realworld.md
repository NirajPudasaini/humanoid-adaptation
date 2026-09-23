# Real-World Humanoid Locomotion with Reinforcement Learning ★

**Citekey:** `radosavovic2024realworld`
**Authors:** Ilija Radosavovic, Tete Xiao, Bike Zhang, Trevor Darrell, Jitendra Malik, Koushil Sreenath
**Venue:** Science Robotics 9(89):eadi9579, 2024 (arXiv 2303.03381; journal version titled in sentence case, 'Real-world humanoid locomotion with reinforcement learning')
**Links:** [arXiv](https://arxiv.org/abs/2303.03381)
**Read on:** _not yet_
**Bucket:** `04-in-context`

## The three axes

| Axis | Answer |
|---|---|
| What is unknown | unmodeled body and environment dynamics (terrain compliance, actuator lag, payload, sim-to-real gap) |
| How the context is obtained | implicitly from a causal Transformer over a context window of exactly 16 observation-action steps, i.e. ~0.32 s at 50 Hz, with no privileged encoder and no regression target |
| Timescale of change | the short window supports fast reactive compensation, while adaptation to episode-level environment change (pavement to grass) comes from randomized training rather than from long memory; there is no mechanism that resolves a fast-switching exogenous input as a named quantity |
| Is the unknown recoverable from measurement? | _fill while reading_ |

## Mechanism

A causal Transformer takes a 16-step history of proprioceptive observations and past actions as a token sequence and autoregressively predicts the next action at 50 Hz (with a 1 kHz joint PD layer underneath), trained model-free in randomized simulation and deployed zero-shot on a full-size humanoid, with the explicit hypothesis that the observation-action history carries enough information about unmodeled dynamics for the model to adapt in context without any weight update or explicit system-identification head.

_Verify this against the paper. Rewrite it in your own words once you can state the input tensor, the horizon, and the loss._

## Claims and evidence

| Claim | Evidence | Do I believe it? |
|---|---|---|
| | | |

## What it buys me

This is the in-context-adaptation baseline in FAME's own domain: a real humanoid, proprioception only, no explicit estimator. The 16-step context is the number to quote, because it sits in the same order of magnitude as FAME's 3-step encoder and therefore kills the lazy reading that implicit methods always mean long context. It is the right citation when FAME argues that a raw history encoder alone underperforms on fast load changes, and it gives a clean ablation template: same actor, same observations, swap the FAME force-latent for a longer raw history, and show the failure is not capacity but identifiability. Note the axis difference: they take disturbances as something to be robust to, FAME takes the disturbance as the control input to be measured.

## Where it fails / what it does not cover

## Relation to FAME

Does this method assume the unknown is unobservable? Would it still make sense if the
quantity were measurable? What would it do on a fixed-stance task where stepping is a failure?

## Reviewer question it answers or raises

## Open threads

- [ ]
