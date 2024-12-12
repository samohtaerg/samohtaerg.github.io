# Preliminaries on Score-Based Diffusion Models

## Forward Process
The core idea of diffusion models is to gradually add noise to data through a forward process, which can be formalized as a stochastic differential equation (SDE):

$$
dx = f(x, t) dt + g(t) dw
$$

where $$f(x, t)$$ is the drift coefficient (deterministic component), $$g(t)$$ is the diffusion coefficient (stochastic component), $$w$$ is the standard Wiener process (Brownian motion), $$x(0) \sim p_0$$ represents the initial data distribution, and $$x(T) \sim p_T$$ represents the final Gaussian noise distribution.

The transition kernel from $$x_0$$ to $$x_t$$ follows a Gaussian distribution:

$$
p_t(x_t | x_0) = \mathcal{N}(x_t; s_t x_0, s_t^2 \sigma_t^2 I_n)
$$

where $$s_t = \exp \left( \int_0^t f(\xi) d\xi \right)$$ and $$\sigma_t = \sqrt{\int_0^t \frac{g^2(\xi)}{s^2(\xi)} d\xi}$$.

## Reverse Process
A key result from Anderson (1982) shows that the reverse process is also a diffusion process, described by the reverse-time SDE:

$$
dx = \left[ f(x, t) - g(t)^2 \nabla_x \log p_t(x) \right] dt + g(t) d\bar{w}
$$

where $$\bar{w}$$ is a standard Wiener process running backwards in time from $$T$$ to $$0$$, $$dt$$ is an infinitesimal negative timestep, and $$\nabla_x \log p_t(x)$$ is the score function.

---
title: 'Geometric structure in Score-Based Diffusion Models'
date: 2024-11-1
permalink: /posts/2024/11/Geometric structure in Score-Based Diffusion Models/
tags:
  - Diffusion Models
  - Score Estimation
  - Machine Learning
  - High-Dimensional Data
math: true
---


## Score Estimation
To estimate the score function $$\nabla_x \log p_t(x)$$, we train a time-dependent score-based model $$s_\theta(x, t)$$ by minimizing:

$$
\theta^* = \arg \min_\theta \mathbb{E}_t \left[ \lambda(t) \mathbb{E}_{x(0)} \mathbb{E}_{x(t)|x(0)} \left[ \| s_\theta(x(t), t) - \nabla_{x(t)} \log p_{0t}(x(t) | x(0)) \|_2^2 \right] \right]
$$

where $$\lambda(t)$$ is a positive weighting function, $$t$$ is uniformly sampled over $$[0, T]$$, $$x(0) \sim p_0(x)$$, and $$x(t) \sim p_t(x(t) | x(0))$$.

## Posterior Mean Estimation
A key relationship established by Robbins (1992) and Miyasawa (1961) shows that the score function can be derived from the posterior mean:

$$
\nabla_x \log p_t(x) = \frac{\mathbb{E}[x_0 | x_t] - x_t}{\sigma_t^2}
$$

where $$\mathbb{E}[x_0 | x_t]$$ is the posterior mean representing the expected clean data given a noisy observation.

To estimate the posterior mean $$\mathbb{E}[x_0 | x_t]$$, we train a time-dependent denoising model $$f_\theta(x, t)$$ by minimizing:

$$
\theta^* = \arg \min_\theta \mathbb{E}_t \left[ \lambda(t) \mathbb{E}_{x(0)} \mathbb{E}_{x(t) | x(0)} \left[ \| f_\theta(x(t), t) - x(0) \|^2 \right] \right]
$$

where $$\lambda(t)$$ is a positive weighting function, $$t$$ is uniformly sampled over $$[0, T]$$, $$x(0) \sim p_0(x)$$, and $$x(t) \sim p_t(x(t) | x(0))$$. Once $$f_\theta$$ is well-trained, the score function can be recovered through the equation above.

