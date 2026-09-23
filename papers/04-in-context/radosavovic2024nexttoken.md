# Humanoid Locomotion as Next Token Prediction

**Citekey:** `radosavovic2024nexttoken`
**Authors:** Ilija Radosavovic, Bike Zhang, Baifeng Shi, Jathushan Rajasegaran, Sarthak Kamat, Trevor Darrell, Koushil Sreenath, Jitendra Malik
**Venue:** NeurIPS 2024 (arXiv 2402.19469)
**Links:** [arXiv](https://arxiv.org/abs/2402.19469)
**Read on:** _not yet_
**Bucket:** `04-in-context`

## The three axes

| Axis | Answer |
|---|---|
| What is unknown | the dynamics and the correct action, with no separated latent for either |
| How the context is obtained | implicitly, through next-token prediction over a sensorimotor history, and notably through supervised sequence modelling rather than RL |
| Timescale of change | within-episode context, adaptation to command and terrain changes over seconds |
| Is the unknown recoverable from measurement? | _fill while reading_ |

## Mechanism

Casts humanoid control as autoregressive modelling of interleaved sensor and action tokens, trains a causal transformer on a heterogeneous mixture of RL-policy rollouts (10k trajectories), an Agility Robotics model-based controller (20k trajectories), ~1k KIT/AMASS motion-capture sequences and human YouTube video, and handles action-free data by inserting mask tokens and ignoring the loss on the masked positions so each trajectory supervises only the modalities it actually contains, then deploys on hardware across San Francisco over a week from about 27 hours of walking data.

_Verify this against the paper. Rewrite it in your own words once you can state the input tensor, the horizon, and the loss._

## Claims and evidence

| Claim | Evidence | Do I believe it? |
|---|---|---|
| | | |

## What it buys me

The follow-up that shows the humanoid in-context line does not need RL at all, only sequence prediction over enough heterogeneous data. Worth reading for two thesis moves: it is the strongest argument that a generative sensorimotor prior can substitute for an explicit estimator, which he must answer; and its mask-token trick (loss ignored on masked modalities) is directly reusable if he ever wants to train FAME's force encoder on logs where the true hand wrench is unlabelled, treating the RNEA estimate as a label on the subset where it is trustworthy.

## Where it fails / what it does not cover

## Relation to FAME

Does this method assume the unknown is unobservable? Would it still make sense if the
quantity were measurable? What would it do on a fixed-stance task where stepping is a failure?

## Reviewer question it answers or raises

## Open threads

- [ ]
