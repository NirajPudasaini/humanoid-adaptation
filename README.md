# humanoid-adaptation

Reading notes on **online adaptation for legged and humanoid control**: context-conditioned
policies, latent adaptation, and in-context learning, with ideas drawn from locomotion,
manipulation, and meta-RL.

This is a working repo, not a survey. Notes are opinionated and written to be argued with.

## The organizing question

Papers in this area get lumped together as "adaptation," which hides the only distinction
that matters. Every method in this list is answering three questions, and it is the
*combination* of answers that determines the architecture:

1. **What is unknown?** Terrain friction, body mass, a locked joint, an external contact
   force, the whole morphology.
2. **How is the context obtained?** Handed to the policy directly, regressed from a privileged
   teacher, inferred implicitly from state-action history, or estimated from a dynamics model.
3. **How fast does it change?** Fixed per episode, drifting over seconds, or switching at
   control rate.

There is a fourth question the field mostly leaves implicit, and it is the one this repo
cares most about:

4. **Is the unknown actually unobservable, or is it recoverable from measurement?**

Most of the adaptation literature assumes unobservable and builds machinery to infer. When the
quantity *is* recoverable, that machinery is not obviously the right answer. Every note here
fills out those four axes before anything else.

## Two ways of doing it

The clearest statement of the fork is two recent papers that both adapt online and share almost
nothing else.

| | LocoFormer | Zhi et al. |
|---|---|---|
| What varies | Embodiment and dynamics parameters, mostly persistent within an episode | External contact wrench at the end effector, changing at contact-event rate |
| How context is obtained | Implicitly, as attention over roughly 18 s of proprioceptive history | Explicitly, supervised regression from a 32-step proprioceptive window |
| Architecture | Transformer-XL, long memory, carried across trials | Single-stage estimator trained alongside the policy |
| Timescale | Seconds to multiple episodes | Essentially instantaneous |
| Scope | Generalist, procedurally generated robots, zero-shot to unseen morphologies | Specialist, position and force control in loco-manipulation |
| Is the unknown recoverable? | No, and that is the premise | Yes, from proprioception, and that is the premise |

Both get called adaptation. Only one of them is solving an inference problem. A method that
conditions on a recoverable quantity is better described as context-conditioned control with an
estimated context, and keeping that distinction sharp is the point of the taxonomy below.

## Start here

Five papers, in this order. Together they cover every branch of the taxonomy and both sides of
the fork above.

1. **[Learning Quadrupedal Locomotion over Challenging Terrain](https://arxiv.org/abs/2010.11251)**
   · Lee et al., Science Robotics 5(47), 2020 · [notes](papers/02-teacher-student/lee2020challenging.md)
   The origin of the teacher-student privileged-learning paradigm that RMA later refined. Read it
   alongside RMA to see where the two-stage idea came from and why the field adopted it wholesale.

2. **[Hybrid Internal Model](https://arxiv.org/abs/2312.11460)**
   · Long et al., ICLR 2024 · [notes](papers/03-implicit-estimation/long2024him.md)
   The single-stage alternative to teacher-student: an implicit estimate of the environment learned
   by contrastive prediction of the robot's own response, with no distillation phase. It has become
   a common actor backbone in recent legged and humanoid work, so it is worth knowing in detail.

3. **[RL²: Fast Reinforcement Learning via Slow Reinforcement Learning](https://arxiv.org/abs/1611.02779)**
   · Duan et al., arXiv 2016 · [notes](papers/01-meta-rl/duan2016rl2.md)
   The foundation of memory-based in-context adaptation. A recurrent policy whose hidden state persists
   across episodes, adapting with no gradient updates at test time. LocoFormer's cross-trial formulation
   is this, scaled up with a transformer. Short and conceptually load-bearing.

4. **[Real-World Humanoid Locomotion with Reinforcement Learning](https://arxiv.org/abs/2303.03381)**
   · Radosavovic et al., Science Robotics 9(89), 2024 · [notes](papers/04-in-context/radosavovic2024realworld.md)
   In-context adaptation on real humanoid hardware: a causal transformer over observation-action history,
   deployed zero-shot on Digit. The bridge between RL² and LocoFormer.

5. **[Human-Timescale Adaptation in an Open-Ended Task Space](https://arxiv.org/abs/2301.07608)**
   · Adaptive Agent Team (DeepMind), ICML 2023 · [notes](papers/01-meta-rl/ada2023humantimescale.md)
   The scaling argument for in-context adaptation: long-context memory plus a very broad task distribution
   produces emergent adaptation. Not a robotics paper, but it is the conceptual justification LocoFormer
   borrows, down to the figure design.

### If the interest is contact forces specifically

Four papers in `05-explicit-conditioning` form their own thread, on estimating and controlling external
contact forces rather than inferring hidden dynamics. They are recent, and three of the four postdate
most of the list above. Worth reading in parallel rather than after.

- **[Learning Force Control for Legged Manipulation](https://arxiv.org/abs/2405.01402)** · Portela et al., ICRA 2024.
  Force conditioning on a legged platform with no force-torque sensor, with the force estimate regressed
  from proprioception.
- **[Learning a Unified Policy for Position and Force Control in Legged Loco-Manipulation](https://arxiv.org/abs/2505.20829)**
  · Zhi et al., CoRL 2025 (Best Paper). Explicit supervised wrench regression from a 32-step proprioceptive
  window, single stage. The strongest current statement of the explicit side of the fork.
- **[SixthSense](https://arxiv.org/abs/2605.01427)** · Chen et al., arXiv 2026. Whole-body wrench estimation
  from proprioception and IMU alone, task-agnostic across standing and walking, and not restricted to a
  single known contact location. The best available evidence that external force is recoverable rather
  than hidden. Very recent preprint, no venue yet.
- **[FALCON](https://arxiv.org/abs/2505.06776)** · Zhang et al., L4DC 2026 (Oral). Force-adaptive humanoid
  loco-manipulation.

## Repo map

```
papers/
  01-meta-rl/                  adaptation that changes weights, and where memory-based adaptation began
  02-teacher-student/          privileged teacher, then history-based student
  03-implicit-estimation/      single-stage, latent learned with the policy
  04-in-context/               long history, no test-time gradients
  05-explicit-conditioning/    the quantity is handed to the policy, incl. the force thread
backlog.md                     everything found but left out, ready to promote
notes/                         cross-cutting writing, comparisons, drafts
templates/paper-note.md        the note format
```

★ marks the papers that are load-bearing for understanding a bucket, as opposed to
useful once you are already in it.

## The list

### `01-meta-rl` Meta-RL and online fine-tuning

Where adaptation-as-a-learning-problem was first posed. Covers both branches: gradient-based, where a test-time update changes the weights, and memory-based, where a recurrent or attentional policy adapts inside its hidden state with no gradient at all. Everything downstream is a specialization of one of these two.

- **[RL$^2$: Fast Reinforcement Learning via Slow Reinforcement Learning](https://arxiv.org/abs/1611.02779)** · Yan Duan, John Schulman, Xi Chen, Peter L. Bartlett, Ilya Sutskever, Pieter Abbeel, arXiv 2016 (arXiv comment: 'Under review as a conference paper at ICLR 2017'; no proceedings publication found) · [notes](papers/01-meta-rl/duan2016rl2.md)
  - *Unknown:* the identity of the MDP itself (reward and transition structure) drawn from a training distribution.
  - *Obtained:* implicitly, in RNN hidden state accumulated over the full multi-episode trial, with reward fed in as an observation.
  - *Timescale:* fixed per trial and changing only between trials, i.e. the slowest regime in the whole taxonomy
- **[Model-Agnostic Meta-Learning for Fast Adaptation of Deep Networks](https://arxiv.org/abs/1703.03400)** · Chelsea Finn, Pieter Abbeel, Sergey Levine, ICML 2017 · [notes](papers/01-meta-rl/finn2017maml.md)
  - *Unknown:* the task, meaning reward function or dynamics, drawn from a training distribution.
  - *Obtained:* an explicit gradient step on freshly collected on-task experience, so the context lives in the weight delta and nowhere else.
  - *Timescale:* one episode batch per adaptation, with no mechanism for change within an episode
- **[Human-Timescale Adaptation in an Open-Ended Task Space](https://arxiv.org/abs/2301.07608)** · Adaptive Agent Team (DeepMind): Jakob Bauer, Kate Baumli, Satinder Baveja, Feryal Behbahani, Avishkar Bhoopchand, et al., ICML 2023 (PMLR 202:1887-1935, oral; arXiv 2301.07608) · [notes](papers/01-meta-rl/ada2023humantimescale.md)
  - *Unknown:* the goal and rules of a freshly sampled task.
  - *Obtained:* implicitly, in a long attention memory spanning several trials, with reward and outcome as the only evidence.
  - *Timescale:* minutes, over repeated trials; adaptation is deliberately slow and exploratory rather than reactive

### `02-teacher-student` Two-stage privileged teacher, history-based student

Train a teacher with privileged access to the unknown, then train a student to recover the same information from what the robot can actually sense. It dominates the field because it makes the unknown explicit and supervised. The cost is a two-stage pipeline, and a student that can never be better than its regression target.

- ★ **[Learning Quadrupedal Locomotion over Challenging Terrain](https://arxiv.org/abs/2010.11251)** · Joonho Lee, Jemin Hwangbo, Lorenz Wellhausen, Vladlen Koltun, Marco Hutter, Science Robotics 5(47), eabc5986 · [notes](papers/02-teacher-student/lee2020challenging.md)
  - *Unknown:* terrain geometry, foot contact state/force, friction, and external disturbance force applied to the base, none of which the real robot senses.
  - *Obtained:* two-stage privileged distillation, with the student's history encoder regressed onto the teacher's privileged latent (the paper ablates this latent term and shows naive imitation is worse).
  - *Timescale:* the history window is 2.0 s and the ablation shows performance rising monotonically from TCN-1 (0.02 s) to TCN-100; no cross-episode memory
- **[RMA: Rapid Motor Adaptation for Legged Robots](https://arxiv.org/abs/2107.04034)** · Ashish Kumar, Zipeng Fu, Deepak Pathak, Jitendra Malik, RSS 2021 · [notes](papers/02-teacher-student/kumar2021rma.md)
  - *Unknown:* payload mass/placement, friction, motor strength, terrain height, i.e. a fixed 17-D vector of physical parameters squeezed to 8-D.
  - *Obtained:* supervised latent regression from a 0.5 s proprioceptive window (two-stage).
  - *Timescale:* fractions of a second; parameters are quasi-static within an episode but the estimate is refreshed at 10 Hz
- **[Adapting Rapid Motor Adaptation for Bipedal Robots](https://arxiv.org/abs/2205.15299)** · Ashish Kumar, Zhongyu Li, Jun Zeng, Deepak Pathak, Koushil Sreenath, Jitendra Malik, IROS 2022 · [notes](papers/02-teacher-student/kumar2022arma.md)
  - *Unknown:* same extrinsics family as RMA (mass, friction, motor and terrain properties) but on an underactuated, low-inertia-margin platform.
  - *Obtained:* two-stage regression plus a third RL phase that closes the loop on estimator error.
  - *Timescale:* sub-second, with the crucial point that bipedal failure modes are faster than the estimator's convergence

### `03-implicit-estimation` Single-stage implicit estimation

Same goal, one stage. The latent is learned jointly with the policy through the critic, a VAE, a contrastive objective, or a self-supervised prediction loss. Nothing is named, so nothing has to be nameable. These have become common actor backbones in recent legged and humanoid work, which makes the bucket worth knowing in detail.

- ★ **[DreamWaQ: Learning Robust Quadrupedal Locomotion With Implicit Terrain Imagination via Deep Reinforcement Learning](https://arxiv.org/abs/2301.10602)** · I Made Aswin Nahrendra, Byeongho Yu, Hyun Myung, ICRA 2023 · [notes](papers/03-implicit-estimation/nahrendra2023dreamwaq.md)
  - *Unknown:* terrain geometry and contact properties, plus unmeasurable base linear velocity.
  - *Obtained:* hybrid, one explicit supervised head (velocity) and one implicit unsupervised head (beta-VAE latent trained only by next-observation reconstruction + KL to a standard normal).
  - *Timescale:* 5 steps of history at the stated 50 Hz inference rate, i.e. 0.1 s adaptation, with the latent re-inferred every control tick (PD controller runs at 200 Hz)
- ★ **[Hybrid Internal Model: Learning Agile Legged Locomotion with Simulated Robot Response](https://arxiv.org/abs/2312.11460)** · Junfeng Long, Zirui Wang, Quanyi Li, Jiawei Gao, Liu Cao, Jiangmiao Pang, ICLR 2024 (poster) · [notes](papers/03-implicit-estimation/long2024him.md)
  - *Unknown:* all external states (friction, restitution, elevation, payload, applied external force) treated as lumped IMC-style disturbance, never named or regressed.
  - *Obtained:* implicitly, from a 5-step proprioceptive window, supervised only by self-prediction of the robot's own next state (contrastive, batch-level) plus one explicit velocity regression.
  - *Timescale:* very short, 5 steps at the stated 50 Hz policy rate is 0.1 s, so the embedding tracks per-step disturbance response rather than an episode-constant parameter

### `04-in-context` In-context and memory-based adaptation

The RL-squared idea carried onto real robots: no estimation module, no test-time gradient, just a transformer over a long history of what the robot did and what happened next. This is where the scaling argument meets hardware.

- ★ **[Real-World Humanoid Locomotion with Reinforcement Learning](https://arxiv.org/abs/2303.03381)** · Ilija Radosavovic, Tete Xiao, Bike Zhang, Trevor Darrell, Jitendra Malik, Koushil Sreenath, Science Robotics 9(89):eadi9579, 2024 (arXiv 2303.03381; journal version titled in sentence case, 'Real-world humanoid locomotion with reinforcement learning') · [notes](papers/04-in-context/radosavovic2024realworld.md)
  - *Unknown:* unmodeled body and environment dynamics (terrain compliance, actuator lag, payload, sim-to-real gap).
  - *Obtained:* implicitly from a causal Transformer over a context window of exactly 16 observation-action steps, i.e. ~0.32 s at 50 Hz, with no privileged encoder and no regression target.
  - *Timescale:* the short window supports fast reactive compensation, while adaptation to episode-level environment change (pavement to grass) comes from randomized training rather than from long memory; there is no mechanism that resolves a fast-switching exogenous input as a named quantity
- ★ **[LocoFormer: Generalist Locomotion via Long-context Adaptation](https://arxiv.org/abs/2509.23745)** · Min Liu, Deepak Pathak, Ananye Agarwal, CoRL 2025 (Award Finalist) · [notes](papers/04-in-context/liu2025locoformer.md)
  - *Unknown:* the entire embodiment (link lengths, mass distribution, actuator strength, missing/failed motors) plus terrain and payload.
  - *Obtained:* implicitly, as attention over ~18 s of raw proprioceptive history with memory persisting across episode boundaries, never as a named quantity.
  - *Timescale:* quasi-static per robot and slowly drifting within a trial; the cross-trial memory explicitly targets change on the order of whole episodes (learn from a fall, do better next rollout), not sub-second transients

### `05-explicit-conditioning` Explicit conditioning on a known or estimated quantity

Hand the policy the varying quantity. There is no inference problem, so there is no history requirement. The open questions are whether the quantity can be obtained at all, and where in the network it should enter. Most of this bucket is the force thread: work that estimates or controls external contact forces rather than inferring hidden dynamics.

- ★ **[One Policy to Run Them All: an End-to-end Learning Approach to Multi-Embodiment Locomotion](https://arxiv.org/abs/2409.06366)** · Nico Bohlinger, Grzegorz Czechmanowski, Maciej Krupka et al., CoRL 2024 (PMLR v270) · [notes](papers/05-explicit-conditioning/bohlinger2024urma.md)
  - *Unknown:* the robot's kinematics/actuator parameters, i.e. which body the policy is running on.
  - *Obtained:* handed in directly, read off the URDF at episode start, never inferred from history.
  - *Timescale:* constant within an episode (embodiment changes only between deployments), so this is zero-timescale adaptation: pure conditioning with no online estimation loop
- **[FAME: Force-Adaptive RL for Expanding the Manipulation Envelope of a Full-Scale Humanoid](https://arxiv.org/abs/2603.08961)** · Niraj Pudasaini, Yutong Zhang, Jensen Lavering, Alessandro Roncone, Nikolaus Correll, arXiv 2026 · [notes](papers/05-explicit-conditioning/pudasaini2026fame.md)
  - *Unknown:* the bimanual interaction forces acting through the arms.
  - *Obtained:* explicitly, estimated from the robot dynamics at deployment and encoded together with upper-body joint configuration into a learned latent, with no wrist force/torque sensor and no privileged teacher.
  - *Timescale:* fast, the load can change within a few control steps
- ★ **[Learning Force Control for Legged Manipulation](https://arxiv.org/abs/2405.01402)** · Tifanny Portela, Gabriel B. Margolis, Yandong Ji, Pulkit Agrawal, ICRA 2024 · [notes](papers/05-explicit-conditioning/portela2024forcecontrol.md)
  - *Unknown:* the contact force at the end effector and the compliance of whatever is being pushed.
  - *Obtained:* two ways at once, a force *command* fed explicitly as conditioning and a force *estimate* regressed from 30 steps of proprioception, so the same paper sits on both sides of the explicit/implicit line.
  - *Timescale:* fast, contact-event rate (tens of ms), which is the regime contact-rich loco-manipulation lives in
- ★ **[FALCON: Learning Force-Adaptive Humanoid Loco-Manipulation](https://arxiv.org/abs/2505.06776)** · Yuanhang Zhang, Yifu Yuan, Prajwal Gurunath, et al. (Tairan He, Guanya Shi), L4DC 2026 (Oral) · [notes](papers/05-explicit-conditioning/zhang2025falcon.md)
  - *Unknown:* magnitude/direction of the external end-effector wrench (0-20N payload transport, 0-40N door opening, 0-100N cart pulling in the real-world tasks).
  - *Obtained:* never estimated at deployment; inferred implicitly from a short (5-step) proprioceptive history, with the true force used only as a privileged critic input at train time.
  - *Timescale:* fast, within-episode, force changes step to step under the curriculum; no cross-episode memory
- ★ **[SixthSense: Task-Agnostic Proprioception-Only Whole-Body Wrench Estimation for Humanoids](https://arxiv.org/abs/2605.01427)** · Xingzhou Chen, Xiayan Xu, Yan Ning, et al. (Haodong Zhang, Ling Shi), arXiv 2026 · [notes](papers/05-explicit-conditioning/chen2026sixthsense.md)
  - *Unknown:* where on the body contact is happening and the six-axis wrench there, not just a hand force.
  - *Obtained:* explicitly estimated from a proprioceptive + IMU history by a generative (flow matching) model, rather than an analytic RNEA residual.
  - *Timescale:* event-driven and fast, sparse in time, must resolve contact onset within a control-relevant window
- **[Learning a Unified Policy for Position and Force Control in Legged Loco-Manipulation](https://arxiv.org/abs/2505.20829)** · Peiyuan Zhi, Peiyang Li, Jianqin Yin, Baoxiong Jia, Siyuan Huang, CoRL 2025 (Proceedings of the 9th Conference on Robot Learning, PMLR 305:652-669) · [notes](papers/05-explicit-conditioning/zhi2025unifiedforce.md)
  - *Unknown:* the external contact wrench at the end effector plus unmeasurable base velocity and end-effector position.
  - *Obtained:* explicit supervised regression from a 32-step proprioceptive window, single-stage joint training, no distillation and no wrist sensor.
  - *Timescale:* 32 steps of history, roughly 0.6 s at a typical 50 Hz control rate (the paper does not state the rate explicitly), deliberately longer than the HIM/DreamWaQ 5-step window because contact wrench must be disentangled from the arm's own inertial torques

## Open questions

The questions this reading is meant to answer, roughly in order of how unsettled they are.

- **The missing head-to-head.** Nobody has run a long-context implicit policy against an explicit
  estimator on a task where the unknown genuinely *is* measurable. Until someone does, the choice
  between them is argued from priors rather than evidence.
- **Residual inference.** Analytic wrench estimators inherit every error in the dynamics model they
  are built on. A long-context module that learns the residual on hardware is the obvious way to
  combine the model-based and in-context lines, and it is largely unexplored.
- **Pre-contact anticipation.** Force estimation is reactive by construction. It says nothing about
  an object's mass before it is picked up. Cross-trial adaptation over repeated interactions is where
  in-context methods have something estimation cannot get.
- **Load-carrying locomotion.** Where the implicit line and the explicit force line meet most directly,
  and where stepping stops being something a controller can be forbidden from doing.
- **When is estimation better than inference?** The general version of the question. Probably a function
  of measurement SNR, model error, and how fast the quantity changes relative to the information rate
  of the history. No one has written this down properly.

## Conventions

- One file per paper, in the bucket folder that matches how it obtains context. If a paper spans
  buckets, file it under its *mechanism*, not its application domain, and cross-link.
- Filenames are citekeys: `kumar2021rma.md`, `long2024him.md`.
- Start from `templates/paper-note.md`. Fill the three-axis table before writing prose. If the
  axes are hard to fill in, that difficulty is the interesting part of the paper.
- `notes/` is for cross-cutting writing: taxonomy arguments, comparisons, longer drafts. Anything
  that is about more than one paper goes there.
