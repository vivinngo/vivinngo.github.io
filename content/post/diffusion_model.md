---
title: "What are Diffusion Models? How do they work? — Are they the Future of Gen AI?"
date: 2026-05-31T11:06:13+01:00
description: "Today, I will introduce Diffusion models, which refines entire output all at once each step, rather than one piece at a time like autoregressive models."
tags: ["diffusion_model", "AI_model", "explaination"]
categories: ["AI topic"]
---
![Cover image](/img/post2_diffusion_model/background.jpeg)

Have you ever noticed how Chat GPT or Gemini generate text? They generate word by word. That is how autoregressive models work. They predict the next word based on all words before it. It is widely used in text AI models such as GPT, Gemini, Llama and Claude; or some image/video models such as PixelCNN and Dall-E. Today, I will introduce Diffusion models, which refines entire output all at once each step, rather than one piece at a time like autoregressive models.

## TL; DR
Diffusion model is trained by adding and removing noise.
Diffusion model can outperform autoregressive in the speed of generating outputs and avoid error snowballing.
Currently it is adopted in generating images and videos, while generating text is still a challenging area.

## Example overall process from prompt to output

Before diving deeper to model architecture, let’s look at the overall process to understand how diffusion models generate output from prompt.
![Alt text](/img/post2_diffusion_model/example1.jpeg)
Prompt is tokenized and encoded into vectors. Starting from very noisy image (almost black and gray dots), Unets predict noise with the guidance from text representation and timestep. Based on the predicted noise from Unets, a Scheduler will remove noise gradually and produce a sharp image. After all denoising steps, stable diffusion model will use a Decoder to upscale the image.

## What is Diffusion model?

From the example above, you can see that the way it generates images are totally different from Autoregressive model. Now, let’s dive deeper into the training phase.
Diffusion models are a class of Gen AI models that learns by noising and denoising images/videos/text.
Training process includes two main phases:
1. Forward process: gradually adds Gaussian noise to real data over many small steps (e.g. T=1000) until it becomes pure random noise. This is pure math, no learning involved.
2. Reverse process: a neural network learns to undo each noising step, predicting and removing a small amount of noise at a time. Once trained, you can start from pure noise and iteratively denoise it to generate entirely new data.

Think of it like a photo restorer trained on millions of damaged photos. When you show them enough examples of “here’s the damage, here’s the original”, they can develop such deep intuition for what photos should look like. Then they can restore, or even invent, convincing details.

Diffusion does the same. The network sees real images at every noise level and learns to predict exactly what noise was added. After millions of iterations, its weights encode a deep understanding of what real data looks like, which it uses at inference time to sculpt new outputs from pure noise.

![Alt text](/img/post2_diffusion_model/example2.jpeg)

Testing with a simple prompt “Create a small mouse with a smiling face, wearing a suit and holding a rose, standing among the rice field”, we get two different results from two models. First result from diffusion model looks nice but a bit unreal, like a scene in cartoon. Meanwhile, the image from autoregressive model looks more real with detail like small grass, blending color of the sky and how the sun shines.

![Alt text](/img/post2_diffusion_model/example3.jpeg)![Alt text](/img/post2_diffusion_model/example4.jpeg)

This is a very simple example. You can test two models on a more complex prompt.

## Why learning from noise can help?
The diffusion method is inspired by thermodynamic diffusion; particles spread until equilibrium. If you can learn to reverse entropy, you can generate structure from chaos.
Earlier methods struggle because learning a realistic data distribution in one shot is intractable:
![Alt text](/img/post2_diffusion_model/table.jpeg)
Diffusion sidesteps this by instead of generating from nothing, it only asks “remove a tiny bit of noise from this almost clean image.” Chain hundreds of these easy steps, and you get from pure noise to a coherent output.

## Conclusion
Diffusion models have become the dominant generative architecture for continuous media, with image generation leading the way through mature platforms like Stable Diffusion and DALL-E, while video and audio applications are rapidly closing the gap. However, generating text is still the hardest domain for diffusion because language is discrete (tokens) rather than continuous (pixel values). The natural language follows sequences, which is closer to autoregressive models than text diffusion models.

Overall, autoregressive models are still dominant now and in the near future, where tokens are discrete and sequential. The potential of diffusion methods is still being explored, which could open up a major shift for Gen AI.

## References
[1] Diffusion Explainer, https://poloclub.github.io/diffusion-explainer/ 

[2] Generating images using Stable Diffusion model: https://stablediffusionweb.com/app/image-generator

---
