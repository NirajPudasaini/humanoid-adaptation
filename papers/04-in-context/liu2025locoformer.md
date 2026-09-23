# LocoFormer: Generalist Locomotion via Long-context Adaptation ★

**Citekey:** `liu2025locoformer`
**Authors:** Min Liu, Deepak Pathak, Ananye Agarwal
**Venue:** CoRL 2025 (Award Finalist)
**Links:** [arXiv](https://arxiv.org/abs/2509.23745)
**Read on:** _not yet_
**Bucket:** `04-in-context`

## The three axes

| Axis | Answer |
|---|---|
| What is unknown | the entire embodiment (link lengths, mass distribution, actuator strength, missing/failed motors) plus terrain and payload |
| How the context is obtained | implicitly, as attention over ~18 s of raw proprioceptive history with memory persisting across episode boundaries, never as a named quantity |
| Timescale of change | quasi-static per robot and slowly drifting within a trial; the cross-trial memory explicitly targets change on the order of whole episodes (learn from a fall, do better next rollout), not sub-second transients |
| Is the unknown recoverable from measurement? | _fill while reading_ |

## Mechanism

A Transformer-XL policy over proprioception and past actions in a unified superset-joint space (each robot masks out the joints it lacks), with 6 layers and segment length 128 giving ~896 timesteps / ~18 s of effective memory at 50 Hz and segment recurrence that carries hidden state across trial boundaries, trained with massive-scale RL on ~100k procedurally generated bipeds, quadrupeds and their wheeled variants under aggressive domain randomization so that morphology and dynamics are inferred implicitly from the context window rather than estimated as an explicit variable.

_Verify this against the paper. Rewrite it in your own words once you can state the input tensor, the horizon, and the loss._

## Claims and evidence

| Claim | Evidence | Do I believe it? |
|---|---|---|
| | | |

## What it buys me

This is the polar opposite design point to FAME on all three axes and should be argued against explicitly in the thesis intro. FAME's unknown is a physically recoverable, instantaneous quantity (hand wrench via RNEA + arm Jacobian) with a 3-step history; LocoFormer's is an unidentifiable morphology prior needing 896 steps. The concrete argument to extract: context length is set by the identifiability timescale of the unknown, and an exogenous hand force that can step in 50 ms is simply not learnable from an 18 s window, because by the time enough evidence accumulates the quantity has already changed. Also gives him the strongest available citation for 'why not just use a long-history Transformer' reviewer question.

## Where it fails / what it does not cover

## Relation to FAME

Does this method assume the unknown is unobservable? Would it still make sense if the
quantity were measurable? What would it do on a fixed-stance task where stepping is a failure?

## Reviewer question it answers or raises

## Open threads

- [ ]
