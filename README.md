# humanoid-adaptation

Reading notes on online adaptation for legged and humanoid control.

Work in this area gets lumped together as "adaptation," which hides the distinction that
actually drives the architecture: what is unknown, how the policy gets hold of it, and how
fast it changes. A method that regresses a hidden terrain latent from proprioceptive history
and a method that reads an external force off an inverse-dynamics estimate are solving
different problems, even when both are called online adaptation.

So this repo sorts papers by how a policy obtains its context, not by application domain.
A quadruped terrain paper and a humanoid force paper end up next to each other when the
mechanism is the same. Each entry is one file under `papers/` with the citation and a short
description of what the method does.

## Sections

Five sections, ordered roughly by how directly the policy is given what it needs.

### `01-meta-rl` Meta-RL and online fine-tuning

Where adaptation-as-a-learning-problem was first posed. Covers both branches: gradient-based, where a test-time update changes the weights, and memory-based, where a recurrent or attentional policy adapts inside its hidden state with no gradient at all. Everything downstream is a specialization of one of these two.

- **[RL²: Fast Reinforcement Learning via Slow Reinforcement Learning](https://arxiv.org/abs/1611.02779)** · Yan Duan, John Schulman, Xi Chen, Peter L. Bartlett, Ilya Sutskever, Pieter Abbeel, arXiv 2016 · [notes](papers/01-meta-rl/duan2016rl2.md)
  An RNN policy that receives observation, action, reward and termination at every step and keeps its hidden state across episodes, so a slow outer RL loop shapes the recurrent weights into a fast inner learning algorithm. Adaptation is the hidden state itself, with no gradient step at deployment.
- **[Model-Agnostic Meta-Learning for Fast Adaptation of Deep Networks](https://arxiv.org/abs/1703.03400)** · Chelsea Finn, Pieter Abbeel, Sergey Levine, ICML 2017 · [notes](papers/01-meta-rl/finn2017maml.md)
  Optimizes an initial parameter vector so that one or a few gradient steps on a new task maximize post-update return, differentiating the outer objective through the inner update. The foundational gradient-based meta-learning method.
- **[Human-Timescale Adaptation in an Open-Ended Task Space](https://arxiv.org/abs/2301.07608)** · Adaptive Agent Team (DeepMind), ICML 2023 · [notes](papers/01-meta-rl/ada2023humantimescale.md)
  Scales black-box meta-RL with a vast procedurally generated task space, a Transformer-XL memory over multi-trial context, and an automatic curriculum that tracks the agent's competence. Produces adaptation within a handful of trials, plus clean scaling laws in network size, memory length and task diversity.

### `02-teacher-student` Two-stage privileged teacher, history-based student

Train a teacher with privileged access to the unknown, then train a student to recover the same information from what the robot can actually sense. It dominates the field because it makes the unknown explicit and supervised. The cost is a two-stage pipeline, and a student that can never be better than its regression target.

- **[Learning Quadrupedal Locomotion over Challenging Terrain](https://arxiv.org/abs/2010.11251)** · Joonho Lee, Jemin Hwangbo, Lorenz Wellhausen, Vladlen Koltun, Marco Hutter, Science Robotics 5(47), 2020 · [notes](papers/02-teacher-student/lee2020challenging.md)
  A privileged teacher encodes ground-truth terrain, foot contacts, friction and base disturbance into a latent, which is distilled into a student that sees only 2 seconds of proprioceptive history. The origin of teacher-student privileged learning in legged locomotion.
- **[RMA: Rapid Motor Adaptation for Legged Robots](https://arxiv.org/abs/2107.04034)** · Ashish Kumar, Zipeng Fu, Deepak Pathak, Jitendra Malik, RSS 2021 · [notes](papers/02-teacher-student/kumar2021rma.md)
  A base policy conditioned on an 8-D extrinsics latent encoded from privileged simulator variables, plus an adaptation module that regresses the same latent from 0.5 s of proprioceptive history at runtime. The reference two-stage formulation.
- **[Adapting Rapid Motor Adaptation for Bipedal Robots](https://arxiv.org/abs/2205.15299)** · Ashish Kumar, Zhongyu Li, Jun Zeng, Deepak Pathak, Koushil Sreenath, Jitendra Malik, IROS 2022 · [notes](papers/02-teacher-student/kumar2022arma.md)
  Carries RMA onto a biped, where estimation error destabilizes rather than merely degrades. Adds a third phase that fine-tunes the base policy against its own imperfect estimator instead of the ground-truth latent.

### `03-implicit-estimation` Single-stage implicit estimation

Same goal, one stage. The latent is learned jointly with the policy through the critic, a VAE, a contrastive objective, or a self-supervised prediction loss. Nothing is named, so nothing has to be nameable. These have become common actor backbones in recent legged and humanoid work, which makes the bucket worth knowing in detail.

- **[DreamWaQ: Learning Robust Quadrupedal Locomotion With Implicit Terrain Imagination via Deep Reinforcement Learning](https://arxiv.org/abs/2301.10602)** · I Made Aswin Nahrendra, Byeongho Yu, Hyun Myung, ICRA 2023 · [notes](papers/03-implicit-estimation/nahrendra2023dreamwaq.md)
  Single stage, no teacher: one encoder over a short proprioceptive history with two heads, an explicitly supervised base velocity estimate and a beta-VAE latent trained by reconstructing the next observation. Privileged state reaches only the critic.
- **[Hybrid Internal Model: Learning Agile Legged Locomotion with Simulated Robot Response](https://arxiv.org/abs/2312.11460)** · Junfeng Long, Zirui Wang, Quanyi Li, Jiawei Gao, Liu Cao, Jiangmiao Pang, ICLR 2024 · [notes](papers/03-implicit-estimation/long2024him.md)
  Infers disturbance from the mismatch between commanded and realized dynamics, using a 5-step proprioceptive window and a contrastive loss against the robot's own successor observation. No teacher and no latent regression.

### `04-in-context` In-context and memory-based adaptation

The RL-squared idea carried onto real robots: no estimation module, no test-time gradient, just a transformer over a long history of what the robot did and what happened next. This is where the scaling argument meets hardware.

- **[Real-World Humanoid Locomotion with Reinforcement Learning](https://arxiv.org/abs/2303.03381)** · Ilija Radosavovic, Tete Xiao, Bike Zhang, Trevor Darrell, Jitendra Malik, Koushil Sreenath, Science Robotics 9(89), 2024 · [notes](papers/04-in-context/radosavovic2024realworld.md)
  A causal transformer over a 16-step observation-action history, predicting actions autoregressively at 50 Hz and deployed zero-shot on a full-size humanoid. Tests whether history alone carries enough about unmodeled dynamics to adapt in context.
- **[LocoFormer: Generalist Locomotion via Long-context Adaptation](https://arxiv.org/abs/2509.23745)** · Min Liu, Deepak Pathak, Ananye Agarwal, CoRL 2025 (Award Finalist) · [notes](papers/04-in-context/liu2025locoformer.md)
  A Transformer-XL policy with roughly 18 seconds of effective memory, carried across trial boundaries, trained on about 100k procedurally generated robots. Morphology and dynamics are inferred implicitly from context rather than estimated as an explicit variable.

### `05-explicit-conditioning` Explicit conditioning on a known or estimated quantity

Hand the policy the varying quantity. There is no inference problem, so there is no history requirement. The open questions are whether the quantity can be obtained at all, and where in the network it should enter. Most of this bucket is the force thread: work that estimates or controls external contact forces rather than inferring hidden dynamics.

- **[One Policy to Run Them All: an End-to-end Learning Approach to Multi-Embodiment Locomotion](https://arxiv.org/abs/2409.06366)** · Nico Bohlinger, Grzegorz Czechmanowski, Maciej Krupka et al., CoRL 2024 · [notes](papers/05-explicit-conditioning/bohlinger2024urma.md)
  Conditions one policy on an explicit per-joint description vector read from the URDF, fused with live proprioception by a morphology-agnostic encoder. The same weights run quadrupeds, bipeds, humanoids and a hexapod.
- **[FAME: Force-Adaptive RL for Expanding the Manipulation Envelope of a Full-Scale Humanoid](https://arxiv.org/abs/2603.08961)** · Niraj Pudasaini, Yutong Zhang, Jensen Lavering, Alessandro Roncone, Nikolaus Correll, arXiv 2026 · [notes](papers/05-explicit-conditioning/pudasaini2026fame.md)
  Conditions a full-scale humanoid standing policy on a latent encoding upper-body joint configuration and bimanual interaction forces, with the forces estimated from robot dynamics at deployment rather than measured. Targets holding a stance under hand loads without wrist force/torque sensors.
- **[Learning Force Control for Legged Manipulation](https://arxiv.org/abs/2405.01402)** · Tifanny Portela, Gabriel B. Margolis, Yandong Ji, Pulkit Agrawal, ICRA 2024 · [notes](papers/05-explicit-conditioning/portela2024forcecontrol.md)
  A legged manipulator conditioned on a commanded end-effector force, with a separate head regressing gripper state and actual contact force from 30 steps of proprioception. No force-torque sensor.
- **[FALCON: Learning Force-Adaptive Humanoid Loco-Manipulation](https://arxiv.org/abs/2505.06776)** · Yuanhang Zhang, Yifu Yuan, Prajwal Gurunath, et al. (Tairan He, Guanya Shi), L4DC 2026 (Oral) · [notes](papers/05-explicit-conditioning/zhang2025falcon.md)
  Decoupled upper- and lower-body humanoid agents where neither actor observes force, so force adaptation is learned implicitly through privileged critics. The force curriculum uses the end-effector Jacobian and joint torque limits to ramp only feasible loads.
- **[Learning a Unified Policy for Position and Force Control in Legged Loco-Manipulation](https://arxiv.org/abs/2505.20829)** · Peiyuan Zhi, Peiyang Li, Jianqin Yin, Baoxiong Jia, Siyuan Huang, CoRL 2025 (Best Paper) · [notes](papers/05-explicit-conditioning/zhi2025unifiedforce.md)
  Jointly trains an estimator head predicting external force, end-effector position and base velocity from 32 steps of proprioception, alongside the actor in a single PPO run. Force tracking within about 10 N for commands up to 60 N, with no force-torque sensor.
- **[SixthSense: Task-Agnostic Proprioception-Only Whole-Body Wrench Estimation for Humanoids](https://arxiv.org/abs/2605.01427)** · Xingzhou Chen, Xiayan Xu, Yan Ning, et al. (Haodong Zhang, Ling Shi), arXiv 2026 · [notes](papers/05-explicit-conditioning/chen2026sixthsense.md)
  Infers whole-body contact timing, location and wrench from proprioception and IMU alone, with a conditional flow matching model that treats contact as sparse in space and time. Not restricted to a single known contact point.

## Also here

- [`backlog.md`](backlog.md): papers found while building this list but left out to keep it
  startable. Same verified citations, ready to promote into a section.
- [`notes/`](notes/): cross-cutting writing: taxonomy arguments, comparisons, longer drafts.
  Anything about more than one paper goes here.
- [`templates/paper-note.md`](templates/paper-note.md): the file format. Title, citation,
  short description. Add structure below it as you read.
