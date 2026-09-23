# Human-Timescale Adaptation in an Open-Ended Task Space

**Citekey:** `ada2023humantimescale`
**Authors:** Adaptive Agent Team (DeepMind): Jakob Bauer, Kate Baumli, Satinder Baveja, Feryal Behbahani, Avishkar Bhoopchand, et al.
**Venue:** ICML 2023 (PMLR 202:1887-1935, oral; arXiv 2301.07608)
**Links:** [arXiv](https://arxiv.org/abs/2301.07608)
**Read on:** _not yet_
**Bucket:** `04-in-context`

## The three axes

| Axis | Answer |
|---|---|
| What is unknown | the goal and rules of a freshly sampled task |
| How the context is obtained | implicitly, in a long attention memory spanning several trials, with reward and outcome as the only evidence |
| Timescale of change | minutes, over repeated trials; adaptation is deliberately slow and exploratory rather than reactive |
| Is the unknown recoverable from measurement? | _fill while reading_ |

## Mechanism

Scales black-box meta-RL by combining a vast procedurally generated task space, a large attention-based memory (Transformer-XL) over multi-trial context, and an automatic curriculum that keeps sampling tasks near the agent's current competence, producing hypothesis-driven trial-and-error within a handful of trials and clean scaling laws in network size, memory length and task diversity.

_Verify this against the paper. Rewrite it in your own words once you can state the input tensor, the horizon, and the loss._

## Claims and evidence

| Claim | Evidence | Do I believe it? |
|---|---|---|
| | | |

## What it buys me

This is the paper that establishes the three ingredients implicit adaptation needs to work at all, task diversity, memory capacity and curriculum, and gives measured scaling laws for each. That makes it the honest steelman of the LocoFormer position and the right citation when he needs to concede what implicit methods buy. It also supplies the cost argument for FAME: those scaling laws are why LocoFormer pays ~500x a specialist policy's compute and trains on 100k procedural robots, a budget a single-embodiment H1-2 contact-rich task cannot and need not pay. State the counter-argument fairly: LocoFormer's own framing is that the per-robot amortized cost falls (1 day vs 0.005 day per robot), which is precisely the trade that does not help a thesis targeting one platform.

## Where it fails / what it does not cover

## Relation to FAME

Does this method assume the unknown is unobservable? Would it still make sense if the
quantity were measurable? What would it do on a fixed-stance task where stepping is a failure?

## Reviewer question it answers or raises

## Open threads

- [ ]
