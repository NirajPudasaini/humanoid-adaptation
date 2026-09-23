# SLR: Learning Quadruped Locomotion without Privileged Information

**Citekey:** `chen2024slr`
**Authors:** Shiyi Chen, Zeyu Wan, Shiyang Yan, Chun Zhang, Weiyi Zhang, Qiang Li, Debing Zhang, Fasih Ud Din Farrukh
**Venue:** CoRL 2024 (Proceedings of the 8th Conference on Robot Learning, PMLR v270)
**Links:** [arXiv](https://arxiv.org/abs/2406.04835)
**Read on:** _not yet_
**Bucket:** `03-implicit-estimation`

## The three axes

| Axis | Answer |
|---|---|
| What is unknown | nothing is named at all, the environment representation is entirely self-discovered from the MDP's own transition structure |
| How the context is obtained | fully implicit and fully self-supervised (latent-space forward model + triplet contrast), plus critic backpropagation, in a single PPO stage |
| Timescale of change | 10 proprioceptive steps in, one step ahead predicted, so again sub-0.2 s |
| Is the unknown recoverable from measurement? | _fill while reading_ |

## Mechanism

An encoder compresses 10 steps of proprioception into a 20-dim latent z_t; a learned state transition model predicts z_tilde_{t+1} from (z_t, a_t) and a triplet loss pulls z_tilde_{t+1} toward the true next-step encoding z_{t+1} while pushing it away from encodings z_{t+n} at other times, and the latent enters the actor stop-gradiented (a_t = pi(o_t, sg[z_t])) but stays gradient-connected through the critic, so the encoder is shaped jointly by return maximization and self-prediction with zero privileged information anywhere in training.

_Verify this against the paper. Rewrite it in your own words once you can state the input tensor, the horizon, and the loss._

## Claims and evidence

| Claim | Evidence | Do I believe it? |
|---|---|---|
| | | |

## What it buys me

SLR is the sharp counterexample Niraj needs to argue against: it shows that a policy given no privileged information and no named estimate can beat explicit-estimation baselines on rough terrain, and it explicitly frames the field as 'explicit vs implicit estimation', which is precisely the thesis's own organizing axis stated by someone else. Read it to pre-empt the reviewer question 'why compute the force at all if a self-supervised latent gets there for free?' The honest answer FAME can give is task-specific: SLR's terrain properties are quasi-static over an episode, whereas an exogenous hand load can step discontinuously and is analytically recoverable from torques, so paying for the explicit estimate buys reaction latency and interpretability rather than raw terrain robustness. Its related-work section is also the best short taxonomy of this bucket in print.

## Where it fails / what it does not cover

## Relation to FAME

Does this method assume the unknown is unobservable? Would it still make sense if the
quantity were measurable? What would it do on a fixed-stance task where stepping is a failure?

## Reviewer question it answers or raises

## Open threads

- [ ]
