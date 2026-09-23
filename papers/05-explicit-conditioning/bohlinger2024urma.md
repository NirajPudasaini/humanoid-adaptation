# One Policy to Run Them All: an End-to-end Learning Approach to Multi-Embodiment Locomotion ★

**Citekey:** `bohlinger2024urma`
**Authors:** Nico Bohlinger, Grzegorz Czechmanowski, Maciej Krupka et al.
**Venue:** CoRL 2024 (PMLR v270)
**Links:** [arXiv](https://arxiv.org/abs/2409.06366)
**Read on:** _not yet_
**Bucket:** `05-explicit-conditioning`

## The three axes

| Axis | Answer |
|---|---|
| What is unknown | the robot's kinematics/actuator parameters, i.e. which body you are in |
| How the context is obtained | handed in directly, read off the URDF at episode start, never inferred from history |
| Timescale of change | constant within an episode (embodiment changes only between deployments), so this is zero-timescale adaptation: pure conditioning with no online estimation loop |
| Is the unknown recoverable from measurement? | _fill while reading_ |

## Mechanism

URMA hands the policy an explicit per-joint description vector (relative 3D position in the nominal joint configuration, rotation axis, number of direct child joints, nominal joint position, torque and velocity limits, damping, rotor inertia, stiffness, friction, min/max control range) plus global robot attributes (PD gains, action scale, mass, length, width, height); a morphology-agnostic encoder MLP fuses each joint's description with its live proprioception and collapses the variable-length set into one fixed latent through an attention head with learnable temperature, and a decoder re-pairs that latent with each joint description to emit per-joint action mean and std, so the same weights run 16 training robots spanning quadrupeds, humanoids, a biped and a hexapod.

_Verify this against the paper. Rewrite it in your own words once you can state the input tensor, the horizon, and the loss._

## Claims and evidence

| Claim | Evidence | Do I believe it? |
|---|---|---|
| | | |

## What it buys me

This is the cleanest statement of the bucket's thesis and it comes with the comparison FAME needs: URMA's own baselines are the two things reviewers will suggest instead of conditioning (a multi-head-per-morphology network with a shared core, and zero-padded observations plus a task-ID one-hot). Those are exactly the framings you should cite when you argue that FAME's force latent is a description vector rather than a task ID. It also gives the strongest counterpoint to LocoFormer: when the varying quantity is genuinely readable, you read it, and the paper shows the read-it design transferring zero-shot to unseen real robots (Unitree A1, MAB Honey Badger, MAB Silver Badger) without any in-context memory at all.

## Where it fails / what it does not cover

## Relation to FAME

Does this method assume the unknown is unobservable? Would it still make sense if the
quantity were measurable? What would it do on a fixed-stance task where stepping is a failure?

## Reviewer question it answers or raises

## Open threads

- [ ]
