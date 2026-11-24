# Vision Language Action Models - VLAs

#### Vision Language Models (VLMs) as Backbones

Vision Language Models (VLMs) are models that can learn simultaneously from **images and text** to tackle many tasks, from visual question answering to image captioning. Large VLMs have good zero-shot capabilities, generalize well and work with lots of image types including web pages and docs. Some even capture spatial image properties such as locatipons and bounding boxes. 

VLMs can be **pretrained** in different ways, e.g. with (expensive) end-to-end training plus language-only instruction fine-tuning. Usually, however, the process involves an **image encoder**, **multimodal projector** and **text decoder**. Initially, only the projector is trained to align image and text and in a a second step the text decoder is fine-tuned on top.

![VLM Training](assets/vlm-training.png)

In short, VLMs are the models we use everyday, so the best known ones include GPT-5, Gemini-3, Claude Opus etc., all of which are multimodal by now (i.e. you can upload and interact with media other than just plain text). Interesting open source alternatives include **Qwen** by Alibaba, **Mistral** and **PaliGemma** by Google.

In **Robot Learning**, VLAs are a class of multimodal foundation models that **integrate vision, language and actions**. Given an input image/video and a text instruction, a VLA directly outputs low-level robot actions that can be used to accomplish the task.

VLAs are usually constructed by **co-fine-tuning** a VLM on a large-scale dataset that pairs visual observations and language instructions with robot trajectories. These models essentially replace a VLMs text decoder with an **action decoder** that transforms elements from a latent space (encoded by the vision transformer) into continous output actions.

Usually, VLAs rely on off the shelf VLMs (prior understanding) serving as the perception and reasoning core. In a second stage, they are extended with an action decoder mapping tokens to discrete symbols which are then de-tokenized into continuous robot commands. Output actions are represented in the same way as language tokens, but refer to the degrees of freedom (DoF) of the robot's end effector.

The concept of Vision Langage Action Models was first pioneered by [Google Deepmind's RT-2](../Papers/RT-2%20VLA.pdf), then made open-source through OpenVLA and Octo, and extended into generalized cross-embodiment by [$\Pi_0$](../Papers/π0.7-%20a%20Steerable%20Generalist%20Robotic%20Foundation%20Model%20with%20Emergent%20Capabilities.pdf) and humanoids by Helix and [Groot](../Papers/Gr00t.pdf).

A special design choice of a VLA is the robot action format:

#### Action Tokenization

The most common approach involves **discrete output tokens** (RT-2, OpenVLA). Each motion is generated as an action string, keeping the same output layer as the VLM and making training straightforward. However, this approach slightly limits spatial accuracy and temporal resolution. 

[$\Pi_0$](../Papers/π0.7-%20a%20Steerable%20Generalist%20Robotic%20Foundation%20Model%20with%20Emergent%20Capabilities.pdf) for example, makes use of an alternative approach, outputting **continuous actions** in order to achieve accurate dexterity and high frequency control. **Diffusion Models** start with random noise and gradually clean it into a smooth result, whereas **flow models** draw a direct highway that smoothly slides random noise into the result values. For continuous values, the VLAs head passes its raw numbers straight into one of these smoothers which return continuous mathematical vectors.

> NOTE: VLA models can be organized either as a single end-to-end network (RT-2, OpenVLA, $\Pi_0$) or as a dual-system - one component handling input and another producing output (Groot, Helix).