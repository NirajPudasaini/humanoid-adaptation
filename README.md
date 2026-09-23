# humanoid-adaptation

Reading notes and research on **online adaptation for humanoid control**: context-conditioned
policies, latent adaptation, and in-context learning, with ideas drawn from locomotion,
manipulation, and meta-RL.

This is a working repo, not a survey. Notes are opinionated and written to be argued with.

## The organizing question

Papers in this area get lumped together as "adaptation," which hides the only distinction
that matters. Every method in this list is answering three questions, and it is the
*combination* of answers that determines the architecture:

1. **What is unknown?** Terrain friction, body mass, a locked joint, an exogenous hand force,
   the whole morphology.
2. **How is the context obtained?** Handed to the policy directly, regressed from a privileged
   teacher, inferred implicitly from state-action history, or estimated from a dynamics model.
3. **How fast does it change?** Fixed per episode, drifting over seconds, or switching at
   control rate.

There is a fourth question that the field mostly leaves implicit, and it is the one this repo
cares most about:

4. **Is the unknown actually unobservable, or is it recoverable from measurement?**

Most of the adaptation literature assumes unobservable and builds machinery to infer. When the
quantity *is* recoverable, that machinery is not obviously the right answer. Every note in this
repo fills out those four axes before anything else.

## Two anchor papers

The list is framed around the gap between two papers that both "do online adaptation" and share
almost nothing else.

| | LocoFormer | FAME |
|---|---|---|
| What varies | Embodiment and dynamics parameters, mostly persistent within an episode | Exogenous hand loads, time-varying, can change at control rate |
| How context is obtained | Implicit, from long state-action history | Explicit, model-based force estimate plus measured arm pose |
| Architecture | Transformer-XL, long memory, cross-trial | MLP encoder, 3-step latent history |
| Timescale | Seconds to multiple episodes | Essentially instantaneous |
| Scope | Generalist, procedurally generated robots, zero-shot to unseen morphologies | Specialist, fixed-stance humanoid standing |
| Objective | Velocity and goal tracking; stepping is a legitimate recovery | Keep feet and hands in place; stepping is a failure |
| Is the unknown recoverable? | No. That is the premise. | Yes, from joint torques via RNEA and the arm Jacobian. |

Strictly, FAME is **context-conditioned control with an estimated context**, not online adaptation
in the RMA or LocoFormer sense. Keeping that distinction sharp is the point of the taxonomy below.

## Start here

Already read: [RMA](papers/02-teacher-student/kumar2021rma.md) (Kumar et al., RSS 2021). That is
the anchor everything below is positioned against.

Five papers, in this order. Together they cover every branch of the taxonomy and both anchor papers.

1. **[Learning Quadrupedal Locomotion over Challenging Terrain](https://arxiv.org/abs/2010.11251)**
   · Lee et al., Science Robotics 5(47), 2020 · [notes](papers/02-teacher-student/lee2020challenging.md)
   The origin of the teacher-student privileged-learning paradigm that RMA refined. Read it after
   RMA to see where the two-stage idea came from and why the field adopted it wholesale.

2. **[Hybrid Internal Model](https://arxiv.org/abs/2312.11460)**
   · Long et al., ICLR 2024 · [notes](papers/03-implicit-estimation/long2024him.md)
   The single-stage alternative to teacher-student: an implicit estimate of the environment learned
   by contrastive prediction of the robot's own response, with no distillation phase. FAME's actor is
   HIM-style, so this needs to be understood well enough to defend in a viva.

3. **[RL²: Fast Reinforcement Learning via Slow Reinforcement Learning](https://arxiv.org/abs/1611.02779)**
   · Duan et al., arXiv 2016 · [notes](papers/04-in-context/duan2016rl2.md)
   The foundation of memory-based in-context adaptation. A recurrent policy whose hidden state persists
   across episodes, adapting with no gradient updates at test time. LocoFormer's cross-trial formulation
   is this, scaled up with a transformer. Short and conceptually load-bearing.

4. **[Real-World Humanoid Locomotion with Reinforcement Learning](https://arxiv.org/abs/2303.03381)**
   · Radosavovic et al., Science Robotics 9(89), 2024 · [notes](papers/04-in-context/radosavovic2024realworld.md)
   In-context adaptation on real humanoid hardware: a causal transformer over observation-action history,
   deployed zero-shot on Digit. The bridge between RL² and LocoFormer.

5. **[Human-Timescale Adaptation in an Open-Ended Task Space](https://arxiv.org/abs/2301.07608)**
   · Adaptive Agent Team (DeepMind), ICML 2023 · [notes](papers/04-in-context/ada2023humantimescale.md)
   The scaling argument for in-context adaptation: long-context memory plus a very broad task distribution
   produces emergent adaptation. Not a robotics paper, but it is the conceptual justification LocoFormer
   borrows, down to the figure design.

### If the near-term goal is the force thread, not the framing

These four are closer to the actual research neighborhood than items 3 to 5 above, and two of them
postdate the ICRA submission. Read them in parallel with the list above, not after it.

- **[Learning Force Control for Legged Manipulation](https://arxiv.org/abs/2405.01402)** · Portela et al., ICRA 2024.
  The closest prior art to the core FAME claim: force conditioning on a legged platform with no F/T sensor.
  It gets the force from a learned proprioceptive regressor rather than RNEA plus the arm Jacobian, which
  is exactly the differentiator paragraph.
- **[Learning a Unified Policy for Position and Force Control in Legged Loco-Manipulation](https://arxiv.org/abs/2505.20829)**
  · Zhi et al., CoRL 2025 (Best Paper). Explicit supervised wrench regression from a 32-step proprioceptive
  window, single stage. This is the paper a reviewer is most likely to hold FAME up against.
- **[SixthSense: Task-Agnostic Proprioception-Only Whole-Body Wrench Estimation for Humanoids](https://arxiv.org/abs/2605.01427)**
  · Chen et al., arXiv 2026. The strongest available evidence for FAME's premise that the external force
  is recoverable rather than hidden, on humanoids, and it generalizes past the single-hand-force assumption
  to arbitrary contact location. Answers "what if the load is on the forearm?" before it is asked.
- **[LocoFormer: Generalist Locomotion via Long-context Adaptation](https://arxiv.org/abs/2509.23745)**
  · Liu et al., CoRL 2025 (Award Finalist). The other anchor.

## Repo map

```
papers/
  01-meta-rl/                  adaptation that changes weights
  02-teacher-student/          privileged teacher, then history-based student
  03-implicit-estimation/      single-stage, latent learned with the policy
  04-in-context/               long history, no test-time gradients
  05-explicit-conditioning/    the quantity is handed to the policy, incl. the force thread
notes/                         cross-cutting writing, comparisons, chapter drafts
templates/paper-note.md        the note format
```

★ marks the papers that are load-bearing for understanding a bucket, as opposed to
useful once you are already in it.

## The list

### `01-meta-rl` Meta-RL and online fine-tuning

Adaptation that actually changes weights. Slower than everything above, and the only family that can adapt to something entirely outside the training distribution. Worth reading mostly to know what you are giving up by staying zero-shot.

- ★ **[Learning to Adapt in Dynamic, Real-World Environments Through Meta-Reinforcement Learning](https://arxiv.org/abs/1803.11347)** · Anusha Nagabandi, Ignasi Clavera, Simin Liu, Ronald S. Fearing, Pieter Abbeel, Sergey Levine, Chelsea Finn, ICLR 2019 · [notes](papers/01-meta-rl/nagabandi2019adapt.md)
  - *Unknown:* the local dynamics function itself (missing leg, slope, miscalibration, towed payload), not a task index.
  - *Obtained:* supervised regression on a short sliding window of recent proprioceptive transitions, with the meta-objective making one SGD step sufficient.
  - *Timescale:* the fastest gradient-based option in the literature, adapting within tens of timesteps and re-adapting continuously, versus minutes-to-hours for policy fine-tuning
- ★ **[Legged Robots that Keep on Learning: Fine-Tuning Locomotion Policies in the Real World](https://arxiv.org/abs/2110.05457)** · Laura Smith, J. Chase Kew, Xue Bin Peng, Sehoon Ha, Jie Tan, Sergey Levine, ICRA 2022 · [notes](papers/01-meta-rl/smith2022keeplearning.md)
  - *Unknown:* the deployment environment's dynamics and terrain, which were not in the training distribution and are not represented by any latent input.
  - *Obtained:* reward-driven gradient updates to the policy weights from on-robot rollouts, so the context is absorbed into the parameters rather than inferred.
  - *Timescale:* hours, and once adapted the policy is specialized to that terrain, so it assumes the disturbance is stationary
- **[Model-Agnostic Meta-Learning for Fast Adaptation of Deep Networks](https://arxiv.org/abs/1703.03400)** · Chelsea Finn, Pieter Abbeel, Sergey Levine, ICML 2017 · [notes](papers/01-meta-rl/finn2017maml.md)
  - *Unknown:* the task, meaning reward function or dynamics, drawn from a training distribution.
  - *Obtained:* an explicit gradient step on freshly collected on-task experience, so the context lives in the weight delta and nowhere else.
  - *Timescale:* one episode batch per adaptation, with no mechanism for change within an episode
- **[Efficient Off-Policy Meta-Reinforcement Learning via Probabilistic Context Variables](https://arxiv.org/abs/1903.08254)** · Kate Rakelly, Aurick Zhou, Deirdre Quillen, Chelsea Finn, Sergey Levine, ICML 2019 (PMLR v97, pp. 5331-5340) · [notes](papers/01-meta-rl/rakelly2019pearl.md)
  - *Unknown:* a task identity, entangling reward and dynamics, compressed into a low-dimensional z.
  - *Obtained:* amortized probabilistic inference from a buffer of recent transitions, decoupled from the control policy's own training.
  - *Timescale:* a few exploratory episodes to concentrate the posterior, and because the encoder is a set function it discards ordering and cannot track a task that changes mid-episode
- **[Rapidly Adaptable Legged Robots via Evolutionary Meta-Learning](https://arxiv.org/abs/2003.01239)** · Xingyou Song, Yuxiang Yang, Krzysztof Choromanski, Ken Caluwaerts, Wenbo Gao, Chelsea Finn, Jie Tan, IROS 2020 · [notes](papers/01-meta-rl/song2020evomaml.md)
  - *Unknown:* a shift in the robot's own dynamics such as a weakened or loaded actuator.
  - *Obtained:* zeroth-order search over policy weights scored by real rollout return, deliberately avoiding gradients because on-robot reward estimates are too noisy for them.
  - *Timescale:* minutes, and the result is a new fixed parameter vector, so it assumes the dynamics shift then holds
- **[A Walk in the Park: Learning to Walk in 20 Minutes With Model-Free Reinforcement Learning](https://arxiv.org/abs/2208.07860)** · Laura Smith, Ilya Kostrikov, Sergey Levine, RSS 2023 (demo track, published as "Demonstrating A Walk in the Park..."); arXiv 2022 · [notes](papers/01-meta-rl/smith2022walkinpark.md)
  - *Unknown:* everything, since there is no prior and no context variable at all, only the true dynamics reached through data.
  - *Obtained:* reward-driven gradient descent on hardware.
  - *Timescale:* 20 minutes to competence, which sets the empirical floor for how fast pure weight updates can absorb a new situation
- **[Learning and Adapting Agile Locomotion Skills by Transferring Experience](https://arxiv.org/abs/2304.09834)** · Laura Smith, J. Chase Kew, Tianyu Li, Linda Luu, Xue Bin Peng, Sehoon Ha, Jie Tan, Sergey Levine, RSS 2023 (DOI 10.15607/RSS.2023.XIX.051) · [notes](papers/01-meta-rl/smith2023twirl.md)
  - *Unknown:* how to solve a harder task, or the same task under changed dynamics or objective, given a controller that is suboptimal for it.
  - *Obtained:* offline data from the old controller mixed into a new optimization, which is adaptation at training time rather than deployment time.
  - *Timescale:* a full retraining run, mostly in simulation with direct hardware transfer, so it does not adapt at all once deployed
- **[Robot Trains Robot: Automatic Real-World Policy Adaptation and Learning for Humanoids](https://arxiv.org/abs/2508.12252)** · Kaizhe Hu, Haochen Shi, Yao He, Weizhuo Wang, C. Karen Liu, Shuran Song, CoRL 2025 (PMLR v305) · [notes](papers/01-meta-rl/hu2025rtr.md)
  - *Unknown:* the residual sim-to-real dynamics gap of a specific humanoid, captured in a latent that was trained in simulation over randomized dynamics.
  - *Obtained:* real-world reward-driven search in that latent space, so weights are frozen and only a handful of dimensions move.
  - *Timescale:* tens of minutes per robot, and the result is persistent calibration, not within-episode tracking

### `02-teacher-student` Two-stage privileged teacher, history-based student

Train a teacher with privileged access to the unknown, then train a student to recover the same information from what the robot can actually sense. It dominates the field because it makes the unknown explicit and supervised. The cost is a two-stage pipeline, and a student that can never be better than its regression target.

- ★ **[Learning Quadrupedal Locomotion over Challenging Terrain](https://arxiv.org/abs/2010.11251)** · Joonho Lee, Jemin Hwangbo, Lorenz Wellhausen, Vladlen Koltun, Marco Hutter, Science Robotics 5(47), eabc5986 · [notes](papers/02-teacher-student/lee2020challenging.md)
  - *Unknown:* terrain geometry, foot contact state/force, friction, and external disturbance force applied to the base, none of which the real robot senses.
  - *Obtained:* two-stage privileged distillation, with the student's history encoder regressed onto the teacher's privileged latent (the paper ablates this latent term and shows naive imitation is worse).
  - *Timescale:* the history window is 2.0 s and the ablation shows performance rising monotonically from TCN-1 (0.02 s) to TCN-100; no cross-episode memory
- **[Learning by Cheating](https://arxiv.org/abs/1912.12294)** · Dian Chen, Brady Zhou, Vladlen Koltun, Philipp Krähenbühl, CoRL 2019, PMLR 100 · [notes](papers/02-teacher-student/chen2019lbc.md)
  - *Unknown:* the full scene state that pixels only partially reveal.
  - *Obtained:* offline imitation of a privileged teacher, with the teacher queryable off-distribution.
  - *Timescale:* not an online adaptation method at all; the unknown is static per frame and there is no notion of context accumulating over time
- **[RMA: Rapid Motor Adaptation for Legged Robots](https://arxiv.org/abs/2107.04034)** · Ashish Kumar, Zipeng Fu, Deepak Pathak, Jitendra Malik, RSS 2021 · [notes](papers/02-teacher-student/kumar2021rma.md)
  - *Unknown:* payload mass/placement, friction, motor strength, terrain height, i.e. a fixed 17-D vector of physical parameters squeezed to 8-D.
  - *Obtained:* supervised latent regression from a 0.5 s proprioceptive window (two-stage).
  - *Timescale:* fractions of a second; parameters are quasi-static within an episode but the estimate is refreshed at 10 Hz
- **[Adapting Rapid Motor Adaptation for Bipedal Robots](https://arxiv.org/abs/2205.15299)** · Ashish Kumar, Zhongyu Li, Jun Zeng, Deepak Pathak, Koushil Sreenath, Jitendra Malik, IROS 2022 · [notes](papers/02-teacher-student/kumar2022arma.md)
  - *Unknown:* same extrinsics family as RMA (mass, friction, motor and terrain properties) but on an underactuated, low-inertia-margin platform.
  - *Obtained:* two-stage regression plus a third RL phase that closes the loop on estimator error.
  - *Timescale:* sub-second, with the crucial point that bipedal failure modes are faster than the estimator's convergence
- **[Learning robust perceptive locomotion for quadrupedal robots in the wild](https://arxiv.org/abs/2201.08117)** · Takahiro Miki, Joonho Lee, Jemin Hwangbo, Lorenz Wellhausen, Vladlen Koltun, Marco Hutter, Science Robotics 7(62), eabk2822 · [notes](papers/02-teacher-student/miki2022perceptive.md)
  - *Unknown:* true terrain geometry plus, critically, the reliability of the sensed estimate of it.
  - *Obtained:* teacher-student distillation into a recurrent belief state that fuses a noisy measured channel with history, with an auxiliary decoder reconstructing the clean signal.
  - *Timescale:* per-step gating, though the recurrent structure lets a corrected belief persist after the misleading measurement is gone

### `03-implicit-estimation` Single-stage implicit estimation

Same goal, one stage. The latent is learned jointly with the policy through the critic, a VAE, a contrastive objective, or a self-supervised prediction loss. Nothing is named, so nothing has to be nameable. FAME's actor is HIM-style, so this bucket is the one to be able to defend line by line.

- ★ **[DreamWaQ: Learning Robust Quadrupedal Locomotion With Implicit Terrain Imagination via Deep Reinforcement Learning](https://arxiv.org/abs/2301.10602)** · I Made Aswin Nahrendra, Byeongho Yu, Hyun Myung, ICRA 2023 · [notes](papers/03-implicit-estimation/nahrendra2023dreamwaq.md)
  - *Unknown:* terrain geometry and contact properties, plus unmeasurable base linear velocity.
  - *Obtained:* hybrid, one explicit supervised head (velocity) and one implicit unsupervised head (beta-VAE latent trained only by next-observation reconstruction + KL to a standard normal).
  - *Timescale:* 5 steps of history at the stated 50 Hz inference rate, i.e. 0.1 s adaptation, with the latent re-inferred every control tick (PD controller runs at 200 Hz)
- ★ **[Hybrid Internal Model: Learning Agile Legged Locomotion with Simulated Robot Response](https://arxiv.org/abs/2312.11460)** · Junfeng Long, Zirui Wang, Quanyi Li, Jiawei Gao, Liu Cao, Jiangmiao Pang, ICLR 2024 (poster) · [notes](papers/03-implicit-estimation/long2024him.md)
  - *Unknown:* all external states (friction, restitution, elevation, payload, applied external force) treated as lumped IMC-style disturbance, never named or regressed.
  - *Obtained:* implicitly, from a 5-step proprioceptive window, supervised only by self-prediction of the robot's own next state (contrastive, batch-level) plus one explicit velocity regression.
  - *Timescale:* very short, 5 steps at the stated 50 Hz policy rate is 0.1 s, so the embedding tracks per-step disturbance response rather than an episode-constant parameter
- **[Concurrent Training of a Control Policy and a State Estimator for Dynamic and Robust Legged Locomotion](https://arxiv.org/abs/2202.05481)** · Gwanghyeon Ji, Juhyeok Mun, Hyeongjun Kim, Jemin Hwangbo, IEEE RA-L 7(2), April 2022 (also presented at ICRA 2022) · [notes](papers/03-implicit-estimation/ji2022concurrent.md)
  - *Unknown:* named, physically meaningful robot states that are unmeasurable on hardware (base linear velocity, foot height, contact probability) rather than environment parameters.
  - *Obtained:* fully explicit supervised regression, concurrent with policy optimization, no latent and no distillation.
  - *Timescale:* per-control-step; the estimate is a filtered instantaneous quantity, not an episode-level property
- **[SLR: Learning Quadruped Locomotion without Privileged Information](https://arxiv.org/abs/2406.04835)** · Shiyi Chen, Zeyu Wan, Shiyang Yan, Chun Zhang, Weiyi Zhang, Qiang Li, Debing Zhang, Fasih Ud Din Farrukh, CoRL 2024 (Proceedings of the 8th Conference on Robot Learning, PMLR v270) · [notes](papers/03-implicit-estimation/chen2024slr.md)
  - *Unknown:* nothing is named at all, the environment representation is entirely self-discovered from the MDP's own transition structure.
  - *Obtained:* fully implicit and fully self-supervised (latent-space forward model + triplet contrast), plus critic backpropagation, in a single PPO stage.
  - *Timescale:* 10 proprioceptive steps in, one step ahead predicted, so again sub-0.2 s
- **[Advancing Humanoid Locomotion: Mastering Challenging Terrains with Denoising World Model Learning](https://arxiv.org/abs/2408.14472)** · Xinyang Gu, Yen-Jen Wang, Xiang Zhu, Chengming Shi, Yanjiang Guo, Yichen Liu, Jianyu Chen, RSS 2024 (Best Paper Award Finalist) · [notes](papers/03-implicit-estimation/gu2024dwl.md)
  - *Unknown:* the true underlying state behind four distinct corruption sources, including quantities that are structurally missing rather than merely noisy.
  - *Obtained:* implicit latent, but supervised against privileged simulator state inside the single RL loop (privileged information is used as a regression target, never as a separate teacher policy).
  - *Timescale:* per-step denoising of the current state; adaptation is instantaneous filtering, not slow system identification

### `04-in-context` In-context and memory-based adaptation

No estimation module and no gradient at test time. Adaptation is whatever the hidden state or the attention context does with a long history of what the robot did and what happened next. This is where the scaling argument lives, and where LocoFormer sits.

- ★ **[Real-World Humanoid Locomotion with Reinforcement Learning](https://arxiv.org/abs/2303.03381)** · Ilija Radosavovic, Tete Xiao, Bike Zhang, Trevor Darrell, Jitendra Malik, Koushil Sreenath, Science Robotics 9(89):eadi9579, 2024 (arXiv 2303.03381; journal version titled in sentence case, 'Real-world humanoid locomotion with reinforcement learning') · [notes](papers/04-in-context/radosavovic2024realworld.md)
  - *Unknown:* unmodeled body and environment dynamics (terrain compliance, actuator lag, payload, sim-to-real gap).
  - *Obtained:* implicitly from a causal Transformer over a context window of exactly 16 observation-action steps, i.e. ~0.32 s at 50 Hz, with no privileged encoder and no regression target.
  - *Timescale:* the short window supports fast reactive compensation, while adaptation to episode-level environment change (pavement to grass) comes from randomized training rather than from long memory; there is no mechanism that resolves a fast-switching exogenous input as a named quantity
- ★ **[LocoFormer: Generalist Locomotion via Long-context Adaptation](https://arxiv.org/abs/2509.23745)** · Min Liu, Deepak Pathak, Ananye Agarwal, CoRL 2025 (Award Finalist) · [notes](papers/04-in-context/liu2025locoformer.md)
  - *Unknown:* the entire embodiment (link lengths, mass distribution, actuator strength, missing/failed motors) plus terrain and payload.
  - *Obtained:* implicitly, as attention over ~18 s of raw proprioceptive history with memory persisting across episode boundaries, never as a named quantity.
  - *Timescale:* quasi-static per robot and slowly drifting within a trial; the cross-trial memory explicitly targets change on the order of whole episodes (learn from a fall, do better next rollout), not sub-second transients
- **[RL$^2$: Fast Reinforcement Learning via Slow Reinforcement Learning](https://arxiv.org/abs/1611.02779)** · Yan Duan, John Schulman, Xi Chen, Peter L. Bartlett, Ilya Sutskever, Pieter Abbeel, arXiv 2016 (arXiv comment: 'Under review as a conference paper at ICLR 2017'; no proceedings publication found) · [notes](papers/04-in-context/duan2016rl2.md)
  - *Unknown:* the identity of the MDP itself (reward and transition structure) drawn from a training distribution.
  - *Obtained:* implicitly, in RNN hidden state accumulated over the full multi-episode trial, with reward fed in as an observation.
  - *Timescale:* fixed per trial and changing only between trials, i.e. the slowest regime in the whole taxonomy
- **[Transformer-XL: Attentive Language Models Beyond a Fixed-Length Context](https://arxiv.org/abs/1901.02860)** · Zihang Dai, Zhilin Yang, Yiming Yang, Jaime Carbonell, Quoc V. Le, Ruslan Salakhutdinov, ACL 2019 (long paper) · [notes](papers/04-in-context/dai2019transformerxl.md)
  - Not an adaptation paper: it is the architectural primitive that sets how far back context can reach. Relevant axis contribution: it determines the maximum timescale of change an implicit method can track, and its cost scales with that horizon.
- **[Human-Timescale Adaptation in an Open-Ended Task Space](https://arxiv.org/abs/2301.07608)** · Adaptive Agent Team (DeepMind): Jakob Bauer, Kate Baumli, Satinder Baveja, Feryal Behbahani, Avishkar Bhoopchand, et al., ICML 2023 (PMLR 202:1887-1935, oral; arXiv 2301.07608) · [notes](papers/04-in-context/ada2023humantimescale.md)
  - *Unknown:* the goal and rules of a freshly sampled task.
  - *Obtained:* implicitly, in a long attention memory spanning several trials, with reward and outcome as the only evidence.
  - *Timescale:* minutes, over repeated trials; adaptation is deliberately slow and exploratory rather than reactive
- **[In-context Reinforcement Learning with Algorithm Distillation](https://arxiv.org/abs/2210.14215)** · Michael Laskin, Luyu Wang, Junhyuk Oh, Emilio Parisotto, Stephen Spencer, et al. (DeepMind), ICLR 2023 (oral; arXiv 2210.14215, Oct 2022) · [notes](papers/04-in-context/laskin2023ad.md)
  - *Unknown:* the task's optimal policy.
  - *Obtained:* implicitly, from a long context containing an entire learning trajectory including its mistakes, so the evidence is reward-labelled behaviour rather than sensor readings.
  - *Timescale:* across episodes, deliberately spanning the full learning curve
- **[Humanoid Locomotion as Next Token Prediction](https://arxiv.org/abs/2402.19469)** · Ilija Radosavovic, Bike Zhang, Baifeng Shi, Jathushan Rajasegaran, Sarthak Kamat, Trevor Darrell, Koushil Sreenath, Jitendra Malik, NeurIPS 2024 (arXiv 2402.19469) · [notes](papers/04-in-context/radosavovic2024nexttoken.md)
  - *Unknown:* the dynamics and the correct action, with no separated latent for either.
  - *Obtained:* implicitly, through next-token prediction over a sensorimotor history, and notably through supervised sequence modelling rather than RL.
  - *Timescale:* within-episode context, adaptation to command and terrain changes over seconds

### `05-explicit-conditioning` Explicit conditioning on a known or estimated quantity

Hand the policy the varying quantity. There is no inference problem, so there is no history requirement. The open questions are whether you can get the quantity at all, and where in the network a known constant should enter. This is where FAME sits, and where the force thread is filed.

- ★ **[One Policy to Run Them All: an End-to-end Learning Approach to Multi-Embodiment Locomotion](https://arxiv.org/abs/2409.06366)** · Nico Bohlinger, Grzegorz Czechmanowski, Maciej Krupka et al., CoRL 2024 (PMLR v270) · [notes](papers/05-explicit-conditioning/bohlinger2024urma.md)
  - *Unknown:* the robot's kinematics/actuator parameters, i.e. which body you are in.
  - *Obtained:* handed in directly, read off the URDF at episode start, never inferred from history.
  - *Timescale:* constant within an episode (embodiment changes only between deployments), so this is zero-timescale adaptation: pure conditioning with no online estimation loop
- **[MetaMorph: Learning Universal Controllers with Transformers](https://arxiv.org/abs/2203.11931)** · Agrim Gupta, Linxi Fan, Surya Ganguli, Li Fei-Fei, ICLR 2022 · [notes](papers/05-explicit-conditioning/gupta2022metamorph.md)
  - *Unknown:* limb-level morphology and the dynamics parameters attached to it.
  - *Obtained:* tokenized and handed in per limb, from the design spec, not inferred.
  - *Timescale:* static per body, though the paper reports zero-shot robustness to dynamics parameters it was never told about, which is where implicit generalization quietly re-enters
- **[Universal Morphology Control via Contextual Modulation](https://arxiv.org/abs/2302.11070)** · Zheng Xiong, Jacob Beck, Shimon Whiteson, ICML 2023 · [notes](papers/05-explicit-conditioning/xiong2023modumorph.md)
  - *Unknown:* morphology context, same as MetaMorph.
  - *Obtained:* handed in, but consumed as network parameters and as a fixed attention prior rather than as an input feature.
  - *Timescale:* static per morphology; the point is about *where* in the architecture a known constant should enter, not about tracking a changing quantity
- **[ManyQuadrupeds: Learning a Single Locomotion Policy for Diverse Quadruped Robots](https://arxiv.org/abs/2310.10486)** · Milad Shafiee, Guillaume Bellegarda, Auke Ijspeert, ICRA 2024 · [notes](papers/05-explicit-conditioning/shafiee2024manyquadrupeds.md)
  - *Unknown:* robot scale and leg kinematics.
  - *Obtained:* handed in, but as analytic scaling constants in a structured output layer rather than as a feature vector into the network.
  - *Timescale:* static per robot; the adaptation is done at design time, not online

#### The force thread

The thesis neighborhood: work that estimates, conditions on, or directly controls external
contact forces on legged and humanoid platforms. Filed here rather than under a mechanism bucket
because the force thread is the reason to read them.

- ★ **[Learning Force Control for Legged Manipulation](https://arxiv.org/abs/2405.01402)** · Tifanny Portela, Gabriel B. Margolis, Yandong Ji, Pulkit Agrawal, ICRA 2024 · [notes](papers/05-explicit-conditioning/portela2024forcecontrol.md)
  - *Unknown:* the contact force at the end effector and the compliance of whatever is being pushed.
  - *Obtained:* two ways at once, a force *command* fed explicitly as conditioning and a force *estimate* regressed from 30 steps of proprioception, so the same paper sits on both sides of the explicit/implicit line.
  - *Timescale:* fast, contact-event rate (tens of ms), which is the regime FAME cares about
- ★ **[FALCON: Learning Force-Adaptive Humanoid Loco-Manipulation](https://arxiv.org/abs/2505.06776)** · Yuanhang Zhang, Yifu Yuan, Prajwal Gurunath, et al. (Tairan He, Guanya Shi), L4DC 2026 (Oral) · [notes](papers/05-explicit-conditioning/zhang2025falcon.md)
  - *Unknown:* magnitude/direction of the external end-effector wrench (0-20N payload transport, 0-40N door opening, 0-100N cart pulling in the real-world tasks).
  - *Obtained:* never estimated at deployment; inferred implicitly from a short (5-step) proprioceptive history, with the true force used only as a privileged critic input at train time.
  - *Timescale:* fast, within-episode, force changes step to step under the curriculum; no cross-episode memory
- ★ **[SixthSense: Task-Agnostic Proprioception-Only Whole-Body Wrench Estimation for Humanoids](https://arxiv.org/abs/2605.01427)** · Xingzhou Chen, Xiayan Xu, Yan Ning, et al. (Haodong Zhang, Ling Shi), arXiv 2026 · [notes](papers/05-explicit-conditioning/chen2026sixthsense.md)
  - *Unknown:* where on the body contact is happening and the six-axis wrench there, not just a hand force.
  - *Obtained:* explicitly estimated from a proprioceptive + IMU history by a generative (flow matching) model, rather than an analytic RNEA residual.
  - *Timescale:* event-driven and fast, sparse in time, must resolve contact onset within a control-relevant window
- **[Proprioceptive External Torque Learning for Floating Base Robot and its Applications to Humanoid Locomotion](https://arxiv.org/abs/2309.04138)** · Daegyu Lim, Myeong-Ju Kim, Junhyeok Cha, Donghyeon Kim, Jaeheung Park, IROS 2023 · [notes](papers/05-explicit-conditioning/lim2023proprioceptive.md)
  - *Unknown:* external joint torques and the resulting contact wrench on a floating base.
  - *Obtained:* explicitly estimated from a proprioceptive time series, learned (GRU) and benchmarked head-to-head against the analytic momentum-observer baseline.
  - *Timescale:* continuous and fast, tracked every control step during walking
- **[HOMIE: Humanoid Loco-Manipulation with Isomorphic Exoskeleton Cockpit](https://arxiv.org/abs/2502.13013)** · Qingwei Ben, Feiyu Jia, Jia Zeng, Junting Dong, Dahua Lin, Jiangmiao Pang, RSS 2025 (open-sourced as OpenHomie) · [notes](papers/05-explicit-conditioning/ben2025homie.md)
  - *Unknown:* the quasi-static load and CoM shift induced by an arbitrary, operator-chosen arm pose.
  - *Obtained:* measured, not estimated; the arm joint pose is fed to the policy as a direct observation.
  - *Timescale:* quasi-static to medium, changing at teleoperation speed, with no model of what the hands are holding
- **[Adversarial Locomotion and Motion Imitation for Humanoid Policy Learning](https://arxiv.org/abs/2504.14305)** · Jiyuan Shi, Xinzhe Liu, Dewei Wang, et al. (Chenjia Bai, Xuelong Li), NeurIPS 2025 · [notes](papers/05-explicit-conditioning/shi2025almi.md)
  - *Unknown:* the internal reaction wrench the upper body imposes on the lower body, which is an endogenous disturbance, not an exogenous load.
  - *Obtained:* never estimated; absorbed into the policy by adversarial co-training, so the disturbance distribution is learned offline rather than identified online.
  - *Timescale:* fast, within-episode, but bounded by the motion distribution seen in training
- **[Learning a Unified Policy for Position and Force Control in Legged Loco-Manipulation](https://arxiv.org/abs/2505.20829)** · Peiyuan Zhi, Peiyang Li, Jianqin Yin, Baoxiong Jia, Siyuan Huang, CoRL 2025 (Proceedings of the 9th Conference on Robot Learning, PMLR 305:652-669) · [notes](papers/05-explicit-conditioning/zhi2025unifiedforce.md)
  - *Unknown:* the external contact wrench at the end effector plus unmeasurable base velocity and end-effector position.
  - *Obtained:* explicit supervised regression from a 32-step proprioceptive window, single-stage joint training, no distillation and no wrist sensor.
  - *Timescale:* 32 steps of history, roughly 0.6 s at a typical 50 Hz control rate (the paper does not state the rate explicitly), deliberately longer than the HIM/DreamWaQ 5-step window because contact wrench must be disentangled from the arm's own inertial torques
- **[Force-Aware Reinforcement Learning with Hybrid Sensorless Force Estimation for Wheeled-Legged Loco-Manipulation](https://arxiv.org/abs/2609.13779)** · Xuanqi Zeng, Jiaming Wang, Tianlin Zhang, et al. (Zhongyu Li, Yun-Hui Liu), arXiv 2026 · [notes](papers/05-explicit-conditioning/zeng2026forceaware.md)
  - *Unknown:* end-effector wrench, entangled with the support-contact reaction forces.
  - *Obtained:* explicitly estimated by a hybrid pipeline, analytic momentum observer plus contact-constrained projection plus a learned temporal residual, then fed to the policy as an explicit observation.
  - *Timescale:* fast, continuous, tracked online while contact conditions change mid-task

## Open threads

Things I want to be able to answer by the end of this, roughly in order of how much they
would change the thesis.

- **The missing baseline.** "Why not just give the policy a long history and let it infer the
  load implicitly, LocoFormer-style?" There is currently no long-context implicit baseline on a
  fixed-stance force task. Running one would either validate explicit estimation or kill it.
- **Residual inference.** The RNEA estimate recovers instantaneous force but inherits every
  error in the arm dynamics model. A long-context module that corrects the residual on hardware
  is the clean way to combine the two lines.
- **Pre-contact anticipation.** The force estimate is by construction reactive. It says nothing
  about an object's mass before it is picked up. Cross-trial adaptation over repeated picks is
  where in-context methods have something explicit estimation cannot get.
- **Load-carrying locomotion.** Where the implicit line and the explicit force line meet most
  directly, and where "stepping is a failure" stops being true.
- **When is estimation better than inference?** The general version of the question. Probably
  a function of measurement SNR, model error, and how fast the quantity changes relative to the
  information rate of the history.

## Conventions

- One file per paper, in the bucket folder that matches how it obtains context. If a paper
  spans buckets, file it under its *mechanism*, not its application domain, and cross-link.
- Filenames are citekeys: `kumar2021rma.md`, `long2024him.md`.
- Start from `templates/paper-note.md`. Fill the three-axis table before writing prose. If the
  axes are hard to fill in, that difficulty is the interesting part of the paper.
- `notes/` is for cross-cutting writing: taxonomy arguments, comparisons, thesis-chapter drafts.
  Anything that is about more than one paper goes there.
