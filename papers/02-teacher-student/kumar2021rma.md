# RMA: Rapid Motor Adaptation for Legged Robots

**Citekey:** `kumar2021rma`
**Authors:** Ashish Kumar, Zipeng Fu, Deepak Pathak, Jitendra Malik
**Venue:** RSS 2021
**Links:** [arXiv](https://arxiv.org/abs/2107.04034)
**Read on:** _not yet_
**Bucket:** `02-teacher-student`

## The three axes

| Axis | Answer |
|---|---|
| What is unknown | payload mass/placement, friction, motor strength, terrain height, i.e. a fixed 17-D vector of physical parameters squeezed to 8-D |
| How the context is obtained | supervised latent regression from a 0.5 s proprioceptive window (two-stage) |
| Timescale of change | fractions of a second; parameters are quasi-static within an episode but the estimate is refreshed at 10 Hz |
| Is the unknown recoverable from measurement? | _fill while reading_ |

## Mechanism

ALREADY READ (anchor for this bucket): phase 1 trains a base policy conditioned on an 8-D extrinsics latent z_t encoded from a 17-D vector of privileged simulator variables (payload mass and its location, friction, motor strength, terrain height); phase 2 regresses an adaptation module to predict that same z_t from the last k=50 steps (0.5 s) of the 30-D state (joint positions, joint velocities, torso roll/pitch, binary foot contacts) and actions, deployed asynchronously at 10 Hz against the 100 Hz base policy.

_Verify this against the paper. Rewrite it in your own words once you can state the input tensor, the horizon, and the loss._

## Claims and evidence

| Claim | Evidence | Do I believe it? |
|---|---|---|
| | | |

## What it buys me

The reference point he already holds. Its value going forward is as the explicit foil in his framing section: RMA's extrinsics are a low-dimensional, quasi-static, unidentifiable-by-construction latent, whereas FAME's hand force is high-bandwidth, exogenous, adversarially time-varying and analytically recoverable from torques. Re-skim only the asynchronous two-rate deployment detail (10 Hz estimator under a 100 Hz policy, a real precedent for a slow force channel) and the payload experiments, since a changing payload is the closest RMA gets to FAME's changing hand load.

## Where it fails / what it does not cover

## Relation to FAME

Does this method assume the unknown is unobservable? Would it still make sense if the
quantity were measurable? What would it do on a fixed-stance task where stepping is a failure?

## Reviewer question it answers or raises

## Open threads

- [ ]
