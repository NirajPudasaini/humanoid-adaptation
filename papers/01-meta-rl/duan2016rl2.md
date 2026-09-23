# RL²: Fast Reinforcement Learning via Slow Reinforcement Learning

**Citekey:** `duan2016rl2`
**Authors:** Yan Duan, John Schulman, Xi Chen, Peter L. Bartlett, Ilya Sutskever, Pieter Abbeel
**Venue:** arXiv 2016 (arXiv comment: 'Under review as a conference paper at ICLR 2017'; no proceedings publication found)
**Links:** [arXiv](https://arxiv.org/abs/1611.02779)
**Read on:** _not yet_
**Bucket:** `01-meta-rl`

**Also relevant to:** `04-in-context`

## The three axes

| Axis | Answer |
|---|---|
| What is unknown | the identity of the MDP itself (reward and transition structure) drawn from a training distribution |
| How the context is obtained | implicitly, in RNN hidden state accumulated over the full multi-episode trial, with reward fed in as an observation |
| Timescale of change | fixed per trial and changing only between trials, i.e. the slowest regime in the whole taxonomy |
| Is the unknown recoverable from measurement? | _fill while reading_ |

## Mechanism

Trains an RNN policy that receives observation, action, reward and termination flag at every step and keeps its hidden state across episodes within a sampled MDP, so a slow outer RL loop shapes the recurrent weights into a fast inner learning algorithm whose entire adaptation state is the RNN activation vector, with no inner-loop gradient step at deployment.

_Verify this against the paper. Rewrite it in your own words once you can state the input tensor, the horizon, and the loss._

## Claims and evidence

| Claim | Evidence | Do I believe it? |
|---|---|---|
| | | |

## What it buys me

This is the origin point of the whole bucket and the cleanest one-paragraph statement of the mechanism FAME is choosing not to use: adaptation state as an opaque hidden vector shaped only by task return. Reading it buys him the precise framing sentence for the related-work section, namely that FAME's force latent is the same architectural slot as an RL^2 hidden state but with a supervised, physically-grounded target instead of an emergent one, which is what makes it interpretable, debuggable and sim-transferable. It is also the paper that establishes the cross-episode memory convention LocoFormer inherits. Cite it as an arXiv preprint, not as an ICLR paper.

## Where it fails / what it does not cover

## Relation to FAME

Does this method assume the unknown is unobservable? Would it still make sense if the
quantity were measurable? What would it do on a fixed-stance task where stepping is a failure?

## Reviewer question it answers or raises

## Open threads

- [ ]
