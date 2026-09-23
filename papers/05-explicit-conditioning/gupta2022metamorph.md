# MetaMorph: Learning Universal Controllers with Transformers

**Citekey:** `gupta2022metamorph`
**Authors:** Agrim Gupta, Linxi Fan, Surya Ganguli, Li Fei-Fei
**Venue:** ICLR 2022
**Links:** [arXiv](https://arxiv.org/abs/2203.11931)
**Read on:** _not yet_
**Bucket:** `05-explicit-conditioning`

## The three axes

| Axis | Answer |
|---|---|
| What is unknown | limb-level morphology and the dynamics parameters attached to it |
| How the context is obtained | tokenized and handed in per limb, from the design spec, not inferred |
| Timescale of change | static per body, though the paper reports zero-shot robustness to dynamics parameters it was never told about, which is where implicit generalization quietly re-enters |
| Is the unknown recoverable from measurement? | _fill while reading_ |

## Mechanism

Treats morphology as just another input modality: each limb of a modular agent becomes a Transformer token built by concatenating that limb's proprioception (position, orientation quaternion, linear and angular velocity, joint position and velocity) with its static morphology descriptors (link shape parameters, density, relative geometry, joint type/range/axis, motor gear ratio), so a single Transformer pretrained over a large design space transfers zero-shot to unseen dynamics variations (armature, density, damping, gear) without dynamics randomization and fine-tunes 2-3x more sample-efficiently to new bodies.

_Verify this against the paper. Rewrite it in your own words once you can state the input tensor, the horizon, and the loss._

## Claims and evidence

| Claim | Evidence | Do I believe it? |
|---|---|---|
| | | |

## What it buys me

This is the citation for the sentence 'conditioning on the varying quantity is a modality problem, not a memory problem' that your intro probably wants. For FAME the useful transfer is the tokenization idea: your force estimate plus measured arm pose is a small structured context vector encoded into a latent, which is MetaMorph's move at a different granularity. It also lets you frame the contrast with LocoFormer historically rather than as a hot take: MetaMorph conditions on the description, LocoFormer refuses the description and reads 18s of history, and your thesis argues the choice should be decided by whether the quantity is recoverable.

## Where it fails / what it does not cover

## Relation to FAME

Does this method assume the unknown is unobservable? Would it still make sense if the
quantity were measurable? What would it do on a fixed-stance task where stepping is a failure?

## Reviewer question it answers or raises

## Open threads

- [ ]
