# Transformer-XL: Attentive Language Models Beyond a Fixed-Length Context

**Citekey:** `dai2019transformerxl`
**Authors:** Zihang Dai, Zhilin Yang, Yiming Yang, Jaime Carbonell, Quoc V. Le, Ruslan Salakhutdinov
**Venue:** ACL 2019 (long paper)
**Links:** [arXiv](https://arxiv.org/abs/1901.02860)
**Read on:** _not yet_
**Bucket:** `04-in-context`

## The three axes

| Axis | Answer |
|---|---|
| What is unknown | |
| How the context is obtained | |
| Timescale of change | |
| Is the unknown recoverable from measurement? | _fill while reading_ |

> Axis breakdown not yet split out. Raw note:
> Not an adaptation paper: it is the architectural primitive that sets how far back context can reach. Relevant axis contribution: it determines the maximum timescale of change an implicit method can track, and its cost scales with that horizon.

## Mechanism

Adds segment-level recurrence, caching and reusing the previous segment's hidden states as extra keys and values, plus a relative positional encoding that stays valid when states are reused, so effective context grows roughly with depth times segment length instead of being capped at one attention window.

_Verify this against the paper. Rewrite it in your own words once you can state the input tensor, the horizon, and the loss._

## Claims and evidence

| Claim | Evidence | Do I believe it? |
|---|---|---|
| | | |

## What it buys me

Read this only for the mechanism section, because it is exactly why LocoFormer's 6 layers x 128-step segments yield ~896 steps / ~18 s of memory rather than 128 (the paper itself states the 896-timestep O(NL) figure). Knowing the depth-times-segment arithmetic lets him state quantitatively in the thesis what history budget an implicit method needs, and contrast it with FAME's 3-step encoder plus a Pinocchio inverse-dynamics call that recovers the same class of information in one timestep. Useful for the compute/latency argument on an H1-2 at control rate.

## Where it fails / what it does not cover

## Relation to FAME

Does this method assume the unknown is unobservable? Would it still make sense if the
quantity were measurable? What would it do on a fixed-stance task where stepping is a failure?

## Reviewer question it answers or raises

## Open threads

- [ ]
