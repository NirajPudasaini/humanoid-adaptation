# Learning by Cheating

**Citekey:** `chen2019lbc`
**Authors:** Dian Chen, Brady Zhou, Vladlen Koltun, Philipp Krähenbühl
**Venue:** CoRL 2019, PMLR 100
**Links:** [arXiv](https://arxiv.org/abs/1912.12294)
**Read on:** _not yet_
**Bucket:** `02-teacher-student`

## The three axes

| Axis | Answer |
|---|---|
| What is unknown | the full scene state that pixels only partially reveal |
| How the context is obtained | offline imitation of a privileged teacher, with the teacher queryable off-distribution |
| Timescale of change | not an online adaptation method at all; the unknown is static per frame and there is no notion of context accumulating over time |
| Is the unknown recoverable from measurement? | _fill while reading_ |

## Mechanism

Decomposes vision-based driving into a privileged agent trained on ground-truth scene layout and actor positions, then a sensorimotor student imitated from that teacher with on-policy DAgger-style supervision and dense white-box targets (the teacher can be queried at any state and for all conditional branches, not just the one it executed).

_Verify this against the paper. Rewrite it in your own words once you can state the input tensor, the horizon, and the loss._

## Claims and evidence

| Claim | Evidence | Do I believe it? |
|---|---|---|
| | | |

## What it buys me

The correct ancestor to cite when he writes the bucket's origin sentence, and worth reading precisely because it isolates what privileged learning gives you (a dense, always-available supervision signal) from what RMA added on top (a history encoder that makes the student's input a time series). That separation is the cleanest way to explain why LocoFormer's in-context memory is a different mechanism rather than a longer version of the same one, and it keeps him from over-crediting privileged training for the adaptation behavior. Skim-depth reading; one section is enough.

## Where it fails / what it does not cover

## Relation to FAME

Does this method assume the unknown is unobservable? Would it still make sense if the
quantity were measurable? What would it do on a fixed-stance task where stepping is a failure?

## Reviewer question it answers or raises

## Open threads

- [ ]
