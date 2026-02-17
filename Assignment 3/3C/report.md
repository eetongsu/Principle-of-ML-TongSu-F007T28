# Denoising Diffusion Probabilistic Models (DDPM)

## 1. Model Description

### 1.1 Overview

In this lab, I implemented a **Denoising Diffusion Probabilistic Model (DDPM)** trained on the MNIST dataset.

Diffusion models are generative models that work in two stages:

1. **Forward diffusion process** – gradually add Gaussian noise to an image.
2. **Reverse diffusion process** – train a neural network to remove noise step-by-step and reconstruct images.

The model learns to predict the noise added at each timestep and uses this to iteratively generate images from pure Gaussian noise.

------

### 1.2 Forward Diffusion Process

The forward process is defined as:
$$
x_t = \sqrt{\bar{\alpha}_t} x_0 + \sqrt{1 - \bar{\alpha}_t} \epsilon
$$
where:

- $x_0$ is the original image
- $\epsilon \sim \mathcal{N}(0, I)$ is Gaussian noise
- $\bar{\alpha}*t = \prod*{i=1}^{t} (1 - \beta_i)$

A **linear noise schedule** was implemented:
$$
\beta_t \in [\beta_{start}, \beta_{end}]
$$
This gradually increases the amount of noise added across timesteps.

------

### 1.3 Reverse Diffusion Process

The reverse process estimates:
$$
x_{t-1} = \frac{1}{\sqrt{\alpha_t}}\left(x_t - \frac{\beta_t}{\sqrt{1 - \bar{\alpha}*t}}\epsilon*\theta(x_t, t)\right) + \sigma_t z
$$
where:

- $\epsilon_\theta(x_t, t)$ is the predicted noise from the neural network
- $z \sim \mathcal{N}(0, I)$

The model progressively denoises an image from pure noise to a structured digit.

------

### 1.4 Model Architecture

The denoising network is a simplified **U-Net** consisting of:

- Sinusoidal time embedding
- Encoder (downsampling path)
- Bottleneck (middle block)
- Decoder (upsampling path)
- Skip connections

#### Key Components

- **Sinusoidal Position Embedding**
  Encodes timestep $t$ similarly to Transformer positional encoding.
- **Conv Blocks**
  Two convolution layers with time embedding injection.
- **Skip Connections**
  Preserve high-resolution features during upsampling.

------

### 1.5 Training Objective

The model is trained using Mean Squared Error (MSE):
$$
\mathcal{L} = \mathbb{E}*{t, x_0, \epsilon}
\left[
|\epsilon - \epsilon*\theta(x_t, t)|^2
\right]
$$
At each iteration:

1. Random timestep $t$ is sampled
2. Noise is added to clean image
3. Model predicts noise
4. MSE loss is computed

------

## 2. Model Evaluation

### 2.1 Training Loss Curve

The model was trained for **10 epochs** on MNIST.

Below is the training loss curve:

![image-1](Figures\F1.png)

The loss steadily decreased across epochs, indicating:

- Stable training
- Successful noise prediction learning
- Proper diffusion schedule implementation

There were no divergence issues or instability during training.

------

### 2.2 Generated Samples

After training, the model was used to generate images starting from pure Gaussian noise.

![image-2](Figures\F2.png)

Observations:

- The model successfully generated recognizable MNIST digits.
- Generated samples show variation across digit classes.
- Longer training may improve sharpness and clarity.

------

### 2.3 Reverse Diffusion Visualization

The reverse diffusion process was visualized to observe progressive denoising.

![image-3](Figures\F3.png)

From left to right, the image transitions from noise to structured digit patterns, demonstrating:

- Gradual feature emergence
- Coarse-to-fine reconstruction
- Learned denoising capability

------

## 3. Discussion

### 3.1 Model Strengths

- Stable training
- Clear denoising progression
- Successful digit generation
- Conceptually simple implementation

### 3.2 Limitations

- Slight blurriness due to:
  - Limited epochs
  - Small U-Net architecture

------

## 4. Reflection and Takeaways

This lab provided hands-on understanding of diffusion models and how they differ from GANs and VAEs.

Key takeaways:

- Diffusion models transform a complex generative task into repeated denoising steps.
- Predicting noise is more stable than directly predicting images.
- Time embeddings are essential for conditioning the model on diffusion steps.
- U-Net architecture is well-suited for generative tasks due to skip connections.
- Even a relatively small diffusion model can generate coherent digits.

Overall, this exercise deepened my understanding of modern generative modeling techniques and the mathematical principles behind diffusion-based image generation.