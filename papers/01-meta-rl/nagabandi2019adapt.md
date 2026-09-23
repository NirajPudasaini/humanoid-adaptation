# Learning to Adapt in Dynamic, Real-World Environments Through Meta-Reinforcement Learning ★

**Citekey:** `nagabandi2019adapt`
**Authors:** Anusha Nagabandi, Ignasi Clavera, Simin Liu, Ronald S. Fearing, Pieter Abbeel, Sergey Levine, Chelsea Finn
**Venue:** ICLR 2019
**Links:** [arXiv](https://arxiv.org/abs/1803.11347)
**Read on:** _not yet_
**Bucket:** `01-meta-rl`

## The three axes

| Axis | Answer |
|---|---|
| What is unknown | the local dynamics function itself (missing leg, slope, miscalibration, towed payload), not a task index |
| How the context is obtained | supervised regression on a short sliding window of recent proprioceptive transitions, with the meta-objective making one SGD step sufficient |
| Timescale of change | the fastest gradient-based option in the literature, adapting within tens of timesteps and re-adapting continuously, versus minutes-to-hours for policy fine-tuning |
| Is the unknown recoverable from measurement? | _fill while reading_ |

## Mechanism

Meta-trains a dynamics-model prior with a MAML-style inner loop (GrBAL) or a recurrent hidden state (ReBAL) so that one gradient step on the last M real transitions (s, a, s') produces a locally correct model, which is then re-planned through with MPC at every control step, giving weight-level adaptation on a roughly sub-second horizon.

_Verify this against the paper. Rewrite it in your own words once you can state the input tensor, the horizon, and the loss._

## Claims and evidence

| Claim | Evidence | Do I believe it? |
|---|---|---|
| | | |

## What it buys me

This is the strongest existing argument that gradient-based adaptation can be fast enough to matter online, and therefore the paper Niraj must answer when he claims FAME needs an explicit estimated force instead. The pulled-payload experiment is almost exactly FAME's disturbance, but handled by silently refitting a dynamics model rather than by naming the force. It gives him a clean framing sentence: gradient adaptation identifies dynamics implicitly and needs a window of transitions to do it, whereas RNEA plus the arm Jacobian gives the force in closed form at one timestep, which is what a load that changes in 100 ms demands.

## Where it fails / what it does not cover

## Relation to FAME

Does this method assume the unknown is unobservable? Would it still make sense if the
quantity were measurable? What would it do on a fixed-stance task where stepping is a failure?

## Reviewer question it answers or raises

## Open threads

- [ ]
