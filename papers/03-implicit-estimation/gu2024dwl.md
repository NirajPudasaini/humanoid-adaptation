# Advancing Humanoid Locomotion: Mastering Challenging Terrains with Denoising World Model Learning

**Citekey:** `gu2024dwl`
**Authors:** Xinyang Gu, Yen-Jen Wang, Xiang Zhu, Chengming Shi, Yanjiang Guo, Yichen Liu, Jianyu Chen
**Venue:** RSS 2024 (Best Paper Award Finalist)
**Links:** [arXiv](https://arxiv.org/abs/2408.14472)
**Read on:** _not yet_
**Bucket:** `03-implicit-estimation`

## The three axes

| Axis | Answer |
|---|---|
| What is unknown | the true underlying state behind four distinct corruption sources, including quantities that are structurally missing rather than merely noisy |
| How the context is obtained | implicit latent, but supervised against privileged simulator state inside the single RL loop (privileged information is used as a regression target, never as a separate teacher policy) |
| Timescale of change | per-step denoising of the current state; adaptation is instantaneous filtering, not slow system identification |
| Is the unknown recoverable from measurement? | _fill while reading_ |

## Mechanism

An encoder maps a history of noisy proprioceptive observations to a latent z_t and a decoder reconstructs the true simulator state s_t, with a denoising loss L_denoise = ||s_tilde_t - s_t||_2 + lambda_r ||z_t||_1 added directly to the PPO actor and critic losses so the actor pi(a_t | Encoder(z_t | o_<=t)) consumes the denoised latent while an asymmetric critic sees privileged state (friction, push forces, height scans, body mass), all in one optimization; the taxonomy of what is denoised is explicit, naming environmental, dynamics, sensory and masking noise, the last covering quantities like base linear velocity that hardware simply cannot measure.

_Verify this against the paper. Rewrite it in your own words once you can state the input tensor, the horizon, and the loss._

## Claims and evidence

| Claim | Evidence | Do I believe it? |
|---|---|---|
| | | |

## What it buys me

The humanoid-scale existence proof for this bucket on a 1.65 m, 57 kg robot (XBot-L; a smaller 1.2 m, 38 kg XBot-S is also used), which is the closest platform class to H1-2, and the strongest sim-to-real evidence that single-stage latent estimation survives humanoid actuator and contact noise. Its four-way noise taxonomy is a useful piece of vocabulary for the thesis: FAME's hand force is a 'masking noise' quantity in DWL's sense (unmeasurable, reconstructed), except FAME reconstructs it analytically via Pinocchio rather than learning the reconstruction. One correction to carry into the repo: this is RSS 2024, not Science Robotics, so the citation must not be written from memory.

## Where it fails / what it does not cover

## Relation to FAME

Does this method assume the unknown is unobservable? Would it still make sense if the
quantity were measurable? What would it do on a fixed-stance task where stepping is a failure?

## Reviewer question it answers or raises

## Open threads

- [ ]
