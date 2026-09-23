# Hybrid Internal Model: Learning Agile Legged Locomotion with Simulated Robot Response ★

**Citekey:** `long2024him`
**Authors:** Junfeng Long, Zirui Wang, Quanyi Li, Jiawei Gao, Liu Cao, Jiangmiao Pang
**Venue:** ICLR 2024 (poster)
**Links:** [arXiv](https://arxiv.org/abs/2312.11460)
**Read on:** _not yet_
**Bucket:** `03-implicit-estimation`

**Also relevant to:** `02-teacher-student`

## The three axes

| Axis | Answer |
|---|---|
| What is unknown | all external states (friction, restitution, elevation, payload, applied external force) treated as lumped IMC-style disturbance, never named or regressed |
| How the context is obtained | implicitly, from a 5-step proprioceptive window, supervised only by self-prediction of the robot's own next state (contrastive, batch-level) plus one explicit velocity regression |
| Timescale of change | very short, 5 steps at the stated 50 Hz policy rate is 0.1 s, so the embedding tracks per-step disturbance response rather than an episode-constant parameter |
| Is the unknown recoverable from measurement? | _fill while reading_ |

## Mechanism

A 3-layer MLP (hidden dims 512/256/128) encodes a 5-step proprioceptive history window o_{t-H:t} with H=5 (joint encoders + IMU only) into a 'hybrid internal embedding' split into an explicit 3-dim base linear velocity head regressed on simulator ground truth and a 16-dim implicit latent (Z subset of R^16) trained by a swapped-assignment contrastive loss explicitly modeled on SwAV (the paper writes the objective as J_SwAV) that pulls the history embedding toward an encoding of the successor observation o_{t+1}, with negatives drawn batch-level across the parallel environments, and this Hybrid Internal Optimization step alternating with PPO in every iteration so no privileged encoder or teacher policy ever exists.

_Verify this against the paper. Rewrite it in your own words once you can state the input tensor, the horizon, and the loss._

## Claims and evidence

| Claim | Evidence | Do I believe it? |
|---|---|---|
| | | |

## What it buys me

This is FAME's actor, so the thesis must state its mechanism exactly: HIM's latent is NOT an environment-parameter estimate, it is a contrastive one-step response predictor, and the only explicitly regressed quantity is base velocity. That matters for the thesis claim, because FAME adds a second explicit channel (hand force from RNEA + arm Jacobian) alongside HIM's implicit latent, i.e. FAME is best described as extending HIM's explicit head from velocity to force. It also gives the cleanest published argument for why single-stage beats RMA-style two-stage: no information loss in the mimicking phase, no regression onto noisy randomized parameters. And the 5-step window (0.1 s at 50 Hz, with the PD controller at 500 Hz) is the natural empirical anchor for the 'fast-changing load' end of the taxonomy against LocoFormer's ~18 s context.

## Where it fails / what it does not cover

## Relation to FAME

Does this method assume the unknown is unobservable? Would it still make sense if the
quantity were measurable? What would it do on a fixed-stance task where stepping is a failure?

## Reviewer question it answers or raises

## Open threads

- [ ]
