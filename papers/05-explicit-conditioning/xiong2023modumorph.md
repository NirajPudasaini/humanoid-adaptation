# Universal Morphology Control via Contextual Modulation

**Citekey:** `xiong2023modumorph`
**Authors:** Zheng Xiong, Jacob Beck, Shimon Whiteson
**Venue:** ICML 2023
**Links:** [arXiv](https://arxiv.org/abs/2302.11070)
**Read on:** _not yet_
**Bucket:** `05-explicit-conditioning`

## The three axes

| Axis | Answer |
|---|---|
| What is unknown | morphology context, same as MetaMorph |
| How the context is obtained | handed in, but consumed as network parameters and as a fixed attention prior rather than as an input feature |
| Timescale of change | static per morphology; the point is about *where* in the architecture a known constant should enter, not about tracking a changing quantity |
| Is the unknown recoverable from measurement? | _fill while reading_ |

## Mechanism

Argues that hard parameter sharing with a concatenated morphology context (the MetaMorph-style baseline) under-uses that context, and instead makes the context generate the network: hypernetworks conditioned on the morphology context emit morphology-dependent control parameters for the per-limb modules, and a fixed attention mechanism computed purely from the morphology (not from the running state) modulates limb-to-limb interaction; the released code names the method ModuMorph.

_Verify this against the paper. Rewrite it in your own words once you can state the input tensor, the horizon, and the loss._

## Claims and evidence

| Claim | Evidence | Do I believe it? |
|---|---|---|
| | | |

## What it buys me

Read this specifically for the design question you will be asked in your defense: why does FAME's estimated force go into a latent encoder concatenated with the actor's observation, rather than modulating the policy (FiLM, hypernetwork, gating)? ModuMorph is the strongest published evidence that hard parameter sharing plus concatenated context leaves performance on the table. If you have never tried a FiLM-style force conditioning ablation against your 3-step-history latent encoder, this paper is why a reviewer will ask for it.

## Where it fails / what it does not cover

## Relation to FAME

Does this method assume the unknown is unobservable? Would it still make sense if the
quantity were measurable? What would it do on a fixed-stance task where stepping is a failure?

## Reviewer question it answers or raises

## Open threads

- [ ]
