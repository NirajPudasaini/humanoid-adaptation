# Learning robust perceptive locomotion for quadrupedal robots in the wild

**Citekey:** `miki2022perceptive`
**Authors:** Takahiro Miki, Joonho Lee, Jemin Hwangbo, Lorenz Wellhausen, Vladlen Koltun, Marco Hutter
**Venue:** Science Robotics 7(62), eabk2822
**Links:** [arXiv](https://arxiv.org/abs/2201.08117)
**Read on:** _not yet_
**Bucket:** `02-teacher-student`

## The three axes

| Axis | Answer |
|---|---|
| What is unknown | true terrain geometry plus, critically, the reliability of the sensed estimate of it |
| How the context is obtained | teacher-student distillation into a recurrent belief state that fuses a noisy measured channel with history, with an auxiliary decoder reconstructing the clean signal |
| Timescale of change | per-step gating, though the recurrent structure lets a corrected belief persist after the misleading measurement is gone |
| Is the unknown recoverable from measurement? | _fill while reading_ |

## Mechanism

Extends the Lee-2020 teacher-student recipe with an attention-based recurrent belief state encoder that learns how far to trust noisy exteroceptive height samples versus proprioceptive history, trained by behavior cloning from a privileged teacher plus a reconstruction loss on the noiseless height samples and privileged state, so the belief degrades gracefully to proprioception-only when the map lies.

_Verify this against the paper. Rewrite it in your own words once you can state the input tensor, the horizon, and the loss._

## Claims and evidence

| Claim | Evidence | Do I believe it? |
|---|---|---|
| | | |

## What it buys me

The methodological template for the problem FAME actually has: a measured-but-untrustworthy channel (the RNEA force estimate, corrupted by model error, friction and unmodeled payload) fused with proprioceptive history. Miki's attention gate is the principled alternative to feeding the estimated force in raw, and the deliberately-corrupted-map experiments map one-to-one onto an experiment he can run by corrupting the force estimate. It also supports the argument that explicit and implicit are not exclusive, the synthesis position between FAME and LocoFormer.

## Where it fails / what it does not cover

## Relation to FAME

Does this method assume the unknown is unobservable? Would it still make sense if the
quantity were measurable? What would it do on a fixed-stance task where stepping is a failure?

## Reviewer question it answers or raises

## Open threads

- [ ]
