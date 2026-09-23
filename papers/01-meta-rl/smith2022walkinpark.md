# A Walk in the Park: Learning to Walk in 20 Minutes With Model-Free Reinforcement Learning

**Citekey:** `smith2022walkinpark`
**Authors:** Laura Smith, Ilya Kostrikov, Sergey Levine
**Venue:** RSS 2023 (demo track, published as "Demonstrating A Walk in the Park..."); arXiv 2022
**Links:** [arXiv](https://arxiv.org/abs/2208.07860)
**Read on:** _not yet_
**Bucket:** `01-meta-rl`

## The three axes

| Axis | Answer |
|---|---|
| What is unknown | everything, since there is no prior and no context variable at all, only the true dynamics reached through data |
| How the context is obtained | reward-driven gradient descent on hardware |
| Timescale of change | 20 minutes to competence, which sets the empirical floor for how fast pure weight updates can absorb a new situation |
| Is the unknown recoverable from measurement? | _fill while reading_ |

## Mechanism

Trains a regularized off-policy SAC agent (DroQ-style: dropout and layer-norm Q-ensemble, high update-to-data ratio) directly on an A1 with no reference motion and no simulated training, reaching a walking gait on indoor and outdoor terrain in about 20 minutes of wall-clock on-robot experience; simulation is used only to ablate design decisions.

_Verify this against the paper. Rewrite it in your own words once you can state the input tensor, the horizon, and the loss._

## Claims and evidence

| Claim | Evidence | Do I believe it? |
|---|---|---|
| | | |

## What it buys me

This is the number that makes the timescale axis of his taxonomy quantitative: the fastest known model-free on-hardware learning is about 20 minutes, while FAME's hand load can change in a fraction of a second, so the two live three or four orders of magnitude apart. It also supplies the honest caveat that this speed comes from a quadruped that may fall freely, which an H1-2 holding a load with both hands and forbidden from stepping cannot do. Useful in the related-work paragraph that closes off the on-robot-RL option for his task.

## Where it fails / what it does not cover

## Relation to FAME

Does this method assume the unknown is unobservable? Would it still make sense if the
quantity were measurable? What would it do on a fixed-stance task where stepping is a failure?

## Reviewer question it answers or raises

## Open threads

- [ ]
