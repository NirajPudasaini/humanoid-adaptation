# Robot Trains Robot: Automatic Real-World Policy Adaptation and Learning for Humanoids

**Citekey:** `hu2025rtr`
**Authors:** Kaizhe Hu, Haochen Shi, Yao He, Weizhuo Wang, C. Karen Liu, Shuran Song
**Venue:** CoRL 2025 (PMLR v305)
**Links:** [arXiv](https://arxiv.org/abs/2508.12252)
**Read on:** _not yet_
**Bucket:** `01-meta-rl`

## The three axes

| Axis | Answer |
|---|---|
| What is unknown | the residual sim-to-real dynamics gap of a specific humanoid, captured in a latent that was trained in simulation over randomized dynamics |
| How the context is obtained | real-world reward-driven search in that latent space, so weights are frozen and only a handful of dimensions move |
| Timescale of change | tens of minutes per robot, and the result is persistent calibration, not within-episode tracking |
| Is the unknown recoverable from measurement? | _fill while reading_ |

## Mechanism

Uses a robot-arm teacher to physically support the humanoid, supply reward from its own tracked state, apply perturbations, detect failures and reset automatically, and then adapts on hardware by optimizing a single low-dimensional dynamics-encoded latent variable rather than the full policy, doubling zero-shot walking speed after about 20 minutes of real interaction and learning a periodic swing-up from scratch in about 15 minutes.

_Verify this against the paper. Rewrite it in your own words once you can state the input tensor, the horizon, and the loss._

## Claims and evidence

| Claim | Evidence | Do I believe it? |
|---|---|---|
| | | |

## What it buys me

The closest thing in this bucket to Niraj's own platform and setup, including the detail that the teacher arm deliberately perturbs the humanoid, which is his disturbance setting with a robot instead of a human applying the load. The latent-optimization trick is the natural bridge sentence for his thesis: the same low-dimensional conditioning input can be fitted slowly offline for persistent dynamics or estimated fast online for exogenous force, and FAME is the second case. It is also the strongest candidate for a 'real-world adaptation on humanoid hardware' citation that a reviewer will expect to see and that predates his submission.

## Where it fails / what it does not cover

## Relation to FAME

Does this method assume the unknown is unobservable? Would it still make sense if the
quantity were measurable? What would it do on a fixed-stance task where stepping is a failure?

## Reviewer question it answers or raises

## Open threads

- [ ]
