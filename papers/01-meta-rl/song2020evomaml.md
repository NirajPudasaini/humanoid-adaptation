# Rapidly Adaptable Legged Robots via Evolutionary Meta-Learning

**Citekey:** `song2020evomaml`
**Authors:** Xingyou Song, Yuxiang Yang, Krzysztof Choromanski, Ken Caluwaerts, Wenbo Gao, Chelsea Finn, Jie Tan
**Venue:** IROS 2020
**Links:** [arXiv](https://arxiv.org/abs/2003.01239)
**Read on:** _not yet_
**Bucket:** `01-meta-rl`

## The three axes

| Axis | Answer |
|---|---|
| What is unknown | a shift in the robot's own dynamics such as a weakened or loaded actuator |
| How the context is obtained | zeroth-order search over policy weights scored by real rollout return, deliberately avoiding gradients because on-robot reward estimates are too noisy for them |
| Timescale of change | minutes, and the result is a new fixed parameter vector, so it assumes the dynamics shift then holds |
| Is the unknown recoverable from measurement? | _fill while reading_ |

## Mechanism

Replaces MAML's inner gradient step with a Batch Hill-Climbing operator inside an ES meta-training loop, so adaptation perturbs policy parameters and keeps perturbations that score well on real noisy rollouts, converging on a quadruped from under three minutes of real data without estimating second-order gradients.

_Verify this against the paper. Rewrite it in your own words once you can state the input tensor, the horizon, and the loss._

## Claims and evidence

| Claim | Evidence | Do I believe it? |
|---|---|---|
| | | |

## What it buys me

The specific reason gradient-based meta-RL underperforms on hardware is reward noise, and this paper is the cleanest statement of it. That matters for FAME because it shows the alternative to a physics-grounded signal is a noisy return estimate that takes minutes to average down, which is the sharpest possible contrast with a torque-derived force available every control tick. If Niraj is ever asked why he did not simply meta-learn the H1-2 policy, this is the citation for the answer.

## Where it fails / what it does not cover

## Relation to FAME

Does this method assume the unknown is unobservable? Would it still make sense if the
quantity were measurable? What would it do on a fixed-stance task where stepping is a failure?

## Reviewer question it answers or raises

## Open threads

- [ ]
