# Tools and Frameworks

## Simulation Environments

In humanoid research, simulation primarily serves two distinct purposes:
- **Locomotion** - Low level RL Training: Massive parallel simulation to learn balance, ...
- **Manipulation** - High Level VLAs e.g.: Renderings and rich physics for visual reasoning and object interaction

The most important simulators used in this context comprise:
- **Nvidia Isaac Sim/Lab**: Ideal for VLA Research, but require heavy compute (work on Nvidia RTX only). The simulation offers a great photorealistic environment and the lab is used e.g. for training and fine-tuning reinforcement learning models for motor policies in parallel (to be used though SSH|SLURM on cluster).
- Google Deepmind's **MuJoCo**: An ultra-fast physics simulator, optimized for control flow and dynamics. It lacks photorealistic rendering, but is the standard for rapid algorithm prototyping (and works on mac :)).
- Gazebo: Outdated, legacy, open-source and ROS-native simulator.
- Unity: Flexible game engine, featuring rich environment creation tools for high customization but more overhead work.

Bridging the **sim-to-real gap** is a super important concept in robotics that is achieved through a combination of the following:
- Visual and dynamic *domain randomization*
- *Abstract action spaces* (e.g. not having the VLA output raw torques, but encode them, as in VLA-JEPA)
- *Latency invariance* through historic snapshots and accounting for different frequencies

## Unitree Environment

For the Unitree Ecosystem please check out the documentation at [Unitree Ecosystem](./Unitree%20Ecosystem.md)

## Hugging Face Environment

For the Hugging Face Tooling information relevant to this project, please check out the documentation at [Hugging Face LeRobot](./LeRobot.md)

## TU Wien Resources

> TL;DR: **Mac for writing code + MuJoCo, SLURM cluster for everything that needs a real NVIDIA GPU** (Isaac, training, VLA benchmarks).

### Overview: what is what

Check out the [**dataLAB portal**](https://login.datalab.tuwien.ac.at/if/user/#/library) for links to all services!

| Resource | Description | Use Case | To Avoid |
|---|---|---|---|
| [**SLURM GPU Cluster**](https://datalab.tuwien.ac.at/science/slurm/) | Shared GPU servers (A100, H100, A40, L40S). Submit jobs which run when GPUs free up | Main workhorse: VLA fine-tuning, RL training in Isaac Lab, headless benchmarks | Interactive GUI work, anything that has to talk to the real robot |
| [**JaaS** – Jupyter as a Service](https://hub.jaas.datalab.tuwien.ac.at/) | JupyterLab in the browser with limited GPU availabilities | Quick experiments, looking at datasets, plotting results | Very long training runs, Isaac Sim |
| [**OpenStack**](https://datalab.tuwien.ac.at/infrastructure/openstack/) | A personal VM in the TU cloud | Small always-on stuff (e.g. a CPU box for a dashboard / data server) | GPUs: right now all GPUs are taken |
| **Autonomous Systems Lab** | The lab's own machines + the G1 | Real-robot tests, local RTX workstation for Isaac Sim with GUI | – |

### Which machine for which job

- **MacBook**
    - Write code, read papers, and run small scripts.
    - Run MuJoCo (`unitree_mujoco`, `unitree_rl_mjlab`) with a viewer using `mjpython` (slow on bigger sim benchmarks).
    - Run small VLA inference via PyTorch MPS only; bigger models like GR00T need CUDA.
    - Cannot run Isaac Sim / Isaac Lab, heavy RL training, VLA fine-tuning, or real robot control (only SSH access).

- **SLURM Cluster**
    - Run heavy RL locomotion training with many parallel environments.
    - Run VLA fine-tuning (GR00T N1.x, UnifoLM-VLA/WLA, π0.5) on A100 80GB or H100 GPUs.
    - Run MuJoCo headless and large VLA sim benchmarks (LIBERO, RoboCasa, Unitree/Isaac tasks) as job arrays.
    - Run Isaac Sim / Isaac Lab (`unitree_rl_lab`) headless (--headless) on RTX GPUs (like A40 or L40S).
    - Cannot connect to the real robot.

- **Lab PC / G1**
    - Run MuJoCo with a viewer.
    - Run Isaac Sim / Isaac Lab with a GUI if the PC has an RTX GPU.
    - Run small RL training runs.
    - Run VLA inference on a GPU PC and send actions to the G1 over the network (DDS).
    - Deploy on the real robot.

**Important GPU gotcha:** Isaac Sim needs RT cores (raytracing) for rendering. **A100 and H100 don't have them**, so Isaac Sim with cameras won't work there, use them for pure training of big models. **A40 or L40S**, on the other hand, can be used for anything with Isaac (rendering).


## SLURM

- Official docs: [SLURM](https://slurm.schedmd.com/documentation.html)
- TU docs: [dataLAB Slurm Cluster](https://datalab.tuwien.ac.at/science/slurm/)
- Script helper: [code generator](https://code-gen.datalab.tuwien.ac.at/)

### What is SLURM?

- A **job scheduler / queue** for a shared cluster. On a cluster, the workflow involves asking for resources (GPUs, CPUs, RAM, time) and SLURM then is responsible for starting jobs according to availabilities.
- Log in via SSH onto a **login (head) node**: only for editing, installing, submitting. **Never to be train on.**
- The actual work runs on **compute nodes** with the GPUs.

### Command Crash Course

| Command | Description |
|---|---|
| `sinfo` | Show partitions and which nodes are free |
| `sbatch job.sh` | Submit a batch job (the normal way) |
| `squeue -u $USER` | See your jobs (PD = pending, R = running) |
| `scancel <jobid>` | Kill a job |
| `srun --partition=GPU-a40 --gres=gpu:1 --time=01:00:00 --pty bash` | Interactive shell on a GPU node (for debugging) |
| `sacct -j <jobid>` | Info about finished jobs (time, memory, exit code) |
| `nvidia-smi` | (inside job) check GPU usage |

### Example job script structure (use the script helper linked above)

```bash
#!/bin/bash
#SBATCH --job-name=pi-finetune
#SBATCH --partition=GPU-a100        # A100 80GB for VLA training; GPU-a40 for Isaac
#SBATCH --gres=gpu:1                # number of GPUs
#SBATCH --cpus-per-task=16
#SBATCH --mem=64G
#SBATCH --time=24:00:00             # max 168h
#SBATCH --output=logs/%x_%j.out     # %x = job name, %j = job id

# 1. source and activate environment
# 2. copy data to fast local disk
# 3. run finetuning script
# 4. save results before the job ends (scratch is local to the node!)
```