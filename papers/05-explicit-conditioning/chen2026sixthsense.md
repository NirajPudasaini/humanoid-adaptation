# SixthSense: Task-Agnostic Proprioception-Only Whole-Body Wrench Estimation for Humanoids ★

**Citekey:** `chen2026sixthsense`
**Authors:** Xingzhou Chen, Xiayan Xu, Yan Ning, et al. (Haodong Zhang, Ling Shi)
**Venue:** arXiv 2026
**Links:** [arXiv](https://arxiv.org/abs/2605.01427)
**Read on:** _not yet_
**Bucket:** `05-explicit-conditioning`

## The three axes

| Axis | Answer |
|---|---|
| What is unknown | where on the body contact is happening and the six-axis wrench there, not just a hand force |
| How the context is obtained | explicitly estimated from a proprioceptive + IMU history by a generative (flow matching) model, rather than an analytic RNEA residual |
| Timescale of change | event-driven and fast, sparse in time, must resolve contact onset within a control-relevant window |
| Is the unknown recoverable from measurement? | _fill while reading_ |

## Mechanism

Infers whole-body contact timing, location and wrench from proprioception plus IMU alone by training a conditional flow matching model on proprioceptive histories, treating contact events as spatiotemporally sparse, and validates it on collision detection, physical human-robot interaction and force-feedback teleoperation across standing, walking and motion-tracking behaviors.

_Verify this against the paper. Rewrite it in your own words once you can state the input tensor, the horizon, and the loss._

## Claims and evidence

| Claim | Evidence | Do I believe it? |
|---|---|---|
| | | |

## What it buys me

This is the strongest available evidence for FAME's core premise that the external force is recoverable rather than hidden, and it is on humanoids and task-agnostic across standing and walking. Two concrete uses: it is the accuracy baseline FAME's Pinocchio RNEA + arm-Jacobian estimator should be compared against or cited beside, and it generalizes FAME's single-hand-force assumption to arbitrary contact location, which is the obvious reviewer question ('what if the load is on the forearm or torso?'). It also shows the learned-estimator alternative to an analytic one, which is a real design fork for the next version of FAME. Caveat: it is a very recent preprint (submitted May 2026) with no venue yet, so cite it as arXiv.

## Where it fails / what it does not cover

## Relation to FAME

Does this method assume the unknown is unobservable? Would it still make sense if the
quantity were measurable? What would it do on a fixed-stance task where stepping is a failure?

## Reviewer question it answers or raises

## Open threads

- [ ]
