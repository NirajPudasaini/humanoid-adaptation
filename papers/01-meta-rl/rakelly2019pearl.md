# Efficient Off-Policy Meta-Reinforcement Learning via Probabilistic Context Variables

**Citekey:** `rakelly2019pearl`
**Authors:** Kate Rakelly, Aurick Zhou, Deirdre Quillen, Chelsea Finn, Sergey Levine
**Venue:** ICML 2019 (PMLR v97, pp. 5331-5340)
**Links:** [arXiv](https://arxiv.org/abs/1903.08254)
**Read on:** _not yet_
**Bucket:** `01-meta-rl`

## The three axes

| Axis | Answer |
|---|---|
| What is unknown | a task identity, entangling reward and dynamics, compressed into a low-dimensional z |
| How the context is obtained | amortized probabilistic inference from a buffer of recent transitions, decoupled from the control policy's own training |
| Timescale of change | a few exploratory episodes to concentrate the posterior, and because the encoder is a set function it discards ordering and cannot track a task that changes mid-episode |
| Is the unknown recoverable from measurement? | _fill while reading_ |

## Mechanism

Encodes a set of recently collected (s, a, r, s') tuples with a permutation-invariant inference network into a Gaussian posterior over a latent task variable z, conditions an off-policy SAC actor and critic on a sample of z, and explores by posterior sampling, so at test time nothing is back-propagated and only the belief over z moves.

_Verify this against the paper. Rewrite it in your own words once you can state the input tensor, the horizon, and the loss._

## Claims and evidence

| Claim | Evidence | Do I believe it? |
|---|---|---|
| | | |

## What it buys me

The bridge inside this bucket to FAME's actual architecture, because PEARL's latent-conditioned actor is structurally what FAME is, with a learned encoder in place of RNEA plus the Jacobian. Reading it buys him a precise statement of what he gave up and what he bought: PEARL must infer z from reward-bearing transitions and can only represent what the training task distribution contained, whereas FAME's latent is grounded in a physically identifiable quantity with known units. The set-encoder's order invariance is also a good foil for his 3-step history and for LocoFormer's 18-second ordered context.

## Where it fails / what it does not cover

## Relation to FAME

Does this method assume the unknown is unobservable? Would it still make sense if the
quantity were measurable? What would it do on a fixed-stance task where stepping is a failure?

## Reviewer question it answers or raises

## Open threads

- [ ]
