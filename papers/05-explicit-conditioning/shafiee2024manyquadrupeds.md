# ManyQuadrupeds: Learning a Single Locomotion Policy for Diverse Quadruped Robots

**Citekey:** `shafiee2024manyquadrupeds`
**Authors:** Milad Shafiee, Guillaume Bellegarda, Auke Ijspeert
**Venue:** ICRA 2024
**Links:** [arXiv](https://arxiv.org/abs/2310.10486)
**Read on:** _not yet_
**Bucket:** `05-explicit-conditioning`

## The three axes

| Axis | Answer |
|---|---|
| What is unknown | robot scale and leg kinematics |
| How the context is obtained | handed in, but as analytic scaling constants in a structured output layer rather than as a feature vector into the network |
| Timescale of change | static per robot; the adaptation is done at design time, not online |
| Is the unknown recoverable from measurement? | _fill while reading_ |

## Mechanism

Keeps the learned part embodiment-agnostic by making the policy modulate the frequencies and amplitudes of a CPG rhythm generator, and pushes all robot-specific knowledge into a Pattern Formation layer whose only per-robot content is stride-height and stride-length scaling, letting one policy cover 12- and 16-DoF quadrupeds, three distinct morphologies, 2-200 kg and 18-100 cm nominal standing heights, with sim-to-real on Unitree Go1 and A1 including a 15 kg added load (125% of A1's nominal mass).

_Verify this against the paper. Rewrite it in your own words once you can state the input tensor, the horizon, and the loss._

## Claims and evidence

| Claim | Evidence | Do I believe it? |
|---|---|---|
| | | |

## What it buys me

The cheapest version of this bucket, and worth one read as a structural argument rather than a baseline. It shows that when you know the varying quantity analytically you can sometimes remove it from the learning problem entirely instead of conditioning on it. The FAME analogue is the question of how much of the hand-force response should be an analytic feedforward term computed from the estimated wrench and the arm Jacobian, versus something the policy has to learn from the force latent. If a fraction of your gain over ALMI and HOMIE is reproducible by an analytic compensation term, you want to have found that out yourself.

## Where it fails / what it does not cover

## Relation to FAME

Does this method assume the unknown is unobservable? Would it still make sense if the
quantity were measurable? What would it do on a fixed-stance task where stepping is a failure?

## Reviewer question it answers or raises

## Open threads

- [ ]
