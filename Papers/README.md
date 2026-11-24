# Papers Collection Overview

## Vision Language Action Foundations

| Title | TLDR | Year | Link |
|---|---|---|---|
| RT-1: Robotics Transformer for Real-World Control at Scale | Google's Transformer policy trained on ~130k real-robot episodes that maps images and language instructions to discretized actions. Shows robot policies scale with data size and diversity, like models in NLP/vision. | 2022 | [PDF](./RT-1-%20ROBOTICS%20TRANSFORMER%20FOR%20REAL-WORLD%20CONTROL%20AT%20SCALE.pdf) |
| RT-2: Vision-Language-Action Models Transfer Web Knowledge to Robotic Control | Co-fine-tunes large VLMs on web data and robot trajectories, with actions written as text tokens, which coined the term "VLA". Web pretraining gives much better generalization and emergent semantic reasoning. | 2023 | [PDF](./RT-2%20VLA.pdf) |
| GR00T N1: An Open Foundation Model for Generalist Humanoid Robots | NVIDIA's open dual-system VLA: a VLM (System 2) for reasoning and a diffusion transformer (System 1) for real-time actions. Trained on real-robot, human-video, and synthetic data, and deployed on the Fourier GR-1 humanoid. | 2025 | [PDF](./Gr00t.pdf) |
| EgoVLA: Learning Vision-Language-Action Models from Egocentric Human Videos | Trains a VLA on large-scale egocentric human videos to predict wrist and hand actions, then converts them to robot actions via IK and retargeting. A few robot demos are enough to fine-tune it for a bimanual humanoid; also introduces a humanoid manipulation simulation benchmark. | 2025 | [PDF](./EgoVLA-%20Learning%20Vision-Language-Action%20Models%20from%20Egocentric%20Human%20Videos.pdf) |
| π0.7: a Steerable Generalist Robotic Foundation Model with Emergent Capabilities | Physical Intelligence's generalist model, prompted with rich context (detailed language, generated subgoal images, episode metadata) so it can be steered on *how* to do a task, not just *what* to do. Composes skills from robot and non-robot data to solve new tasks across environments and robot types. | 2026 | [PDF](./%CF%800.7-%20a%20Steerable%20Generalist%20Robotic%20Foundation%20Model%20with%20Emergent%20Capabilities.pdf) |


## Reinforcement Learning

| Title | TLDR | Year | Link |
|---|---|---|---|
| I-CTRL: Imitation to Control Humanoid Robots Through Bounded Residual RL | Bounded residual RL refines kinematically retargeted human motions into physically feasible motions for legged humanoids. One policy generalizes across five robots, with an automatic priority scheduler for large motion datasets. | 2025 | [PDF](./I-CTRL_Imitation_to_Control_Humanoid_Robots_Through_Bounded_Residual_Reinforcement_Learning.pdf) |
| Improving Vision-Language-Action Model with Online Reinforcement Learning (iRe-VLA) | Applying online RL directly to large VLAs is unstable and compute-heavy. iRe-VLA alternates between RL and supervised fine-tuning to get the exploration benefits of RL while keeping training stable. | 2025 | [PDF](./Improving%20VLA%20Models%20with%20Online%20RL.pdf) |
| A Hierarchical Vision-Language and RL Framework for Robotic Task and Motion Planning in Collaborative Manipulation (VL-RL) | Pairs a vision-language high-level task planner with an RL low-level motion planner that adapts to environment changes in real time without re-querying the VLM. Demonstrated on dual-robot collaborative, dynamic grasping, and long-horizon tasks. | 2026 | [PDF](./A_Hierarchical_Vision-Language_and_Reinforcement_Learning_Framework_for_Robotic_Task_and_Motion_Planning_in_Collaborative_Manipulation.pdf) |
| Z-1: Efficient Reinforcement Learning for Vision-Language-Action Models | GRPO-based RL post-training for flow-based VLAs (built on π0.5), using shared-prefix rollouts, tree-structured branching, and reward calibration. Reaches 80.6% average success on 24 RoboCasa tasks, +13.2 points over its SFT starting point. | 2026 | [PDF](./Efficient%20RL%20for%20VLA%20Models.pdf) |
| Vision-Language-Action Jump-Starting for Reinforcement Learning Robotic Agents (VLAJS) | Uses sparse action suggestions from pretrained VLAs (e.g., OpenVLA, Octo) as a directional guidance loss to jump-start on-policy RL (PPO). Improves exploration on long-horizon tasks and tasks with imperfect rewards, and narrows the sim-to-real gap. | 2026 | [PDF](./VLA%20Jump-Starting%20for%20RL%20Robotic%20Agents.pdf) |


## Unsupervised Approaches

| Title | TLDR | Year | Link |
|---|---|---|---|
| ImitationNet: Unsupervised Human-to-Robot Motion Retargeting via Shared Latent Space | Uses contrastive learning to build a shared human–robot latent pose space without paired data, so a robot can mimic human poses. Supports control from text, RGB video, or key poses. | 2023 | [PDF](./ImitationNet-%20Unsupervised%20Human-to-Robot%20Motion%20Retargeting%20via%20Shared%20Latent%20Space.pdf) |

## Other

| Title | TLDR | Year | Link |
|---|---|---|---|
| Robot Interaction Behavior Generation based on Social Motion Forecasting for Human-Robot Interaction (ECHO) | A transformer that forecasts social motion in a shared human–robot space, refining each agent's predicted motion based on the agents around it. Generates text-controllable robot interaction behaviors without any robot data in training. | 2024 | [PDF](./Robot%20Interaction%20Behavior%20Generation%20based%20on%20Social%20Motion%20Forecasting%20for%20Human-Robot%20Interaction.pdf) |