---
title: 'Geometric Structure in Score-Based Diffusion Models'
date: 2024-7-1
permalink: /posts/2024/11/Understanding Score-Based Diffusion Models/
tags:
  - Diffusion Models
  - Score Estimation
  - Machine Learning
  - High-Dimensional Data
math: true
---


## Preliminaries on Score-Based Diffusion Models

### Forward Process
The core idea of diffusion models is to gradually add noise to data through a forward process, which can be formalized as a stochastic differential equation (SDE):
$$dx = f(x, t) \, dt + g(t) \, dw$$
where $$f(x, t)$$ is the drift coefficient (deterministic component), $$g(t)$$ is the diffusion coefficient (stochastic component), $$w$$ is the standard Wiener process (Brownian motion), $$x(0) \sim p_0$$ represents the initial data distribution, and $$x(T) \sim p_T$$ represents the final Gaussian noise distribution.

The transition kernel from $$x_0$$ to $$x_t$$ follows a Gaussian distribution:
$$p_t(x_t | x_0) = \mathcal{N}(x_t; s_t x_0, s_t^2 \sigma_t^2 I_n)$$
where $$s_t = \exp \left( \int_0^t f(\xi) \, d\xi \right)$$ and $$\sigma_t = \sqrt{\int_0^t \frac{g^2(\xi)}{s^2(\xi)} \, d\xi}$$.

### Reverse Process
A key result from \citet{anderson1982reverse} shows that the reverse process is also a diffusion process, described by the reverse-time SDE:
$$dx = \left[ f(x, t) - g(t)^2 \nabla_x \log p_t(x) \right] dt + g(t) \, d\bar{w}$$
where $$\bar{w}$$ is a standard Wiener process running backwards in time from $$T$$ to $$0$$, $$dt$$ is an infinitesimal negative timestep, and $$\nabla_x \log p_t(x)$$ is the score function.

### Score Estimation
To estimate the score function $$\nabla_x \log p_t(x)$$, we train a time-dependent score-based model $$s_\theta(x, t)$$ by minimizing:
$$\theta^* = \arg \min_\theta \mathbb{E}_t \left[ \lambda(t) \mathbb{E}_{x(0)} \mathbb{E}_{x(t)|x(0)} \left[ \| s_\theta(x(t), t) - \nabla_{x(t)} \log p_{0t}(x(t) | x(0)) \|_2^2 \right] \right]$$

### Posterior Mean Estimation
A key relationship established by \citet{robbins1992empirical} and \citet{miyasawa1961empirical} shows that the score function can be derived from the posterior mean:
$$\nabla_x \log p_t(x) = \frac{\mathbb{E}[x_0 | x_t] - x_t}{\sigma_t^2}$$
where $$\mathbb{E}[x_0 | x_t]$$ is the posterior mean representing the expected clean data given a noisy observation.

To estimate the posterior mean $$\mathbb{E}[x_0 | x_t]$$, we train a time-dependent denoising model $$f_\theta(x, t)$$ by minimizing:
$$\theta^* = \arg \min_\theta \mathbb{E}_t \left[ \lambda(t) \mathbb{E}_{x(0)} \mathbb{E}_{x(t) | x(0)} \left[ \| f_\theta(x(t), t) - x(0) \|^2 \right] \right]$$

Once $$f_\theta$$ is well-trained, the score function can be recovered through the posterior mean equation.

### Adaptive Basis Representation
\citet{kadkhodaie2023generalization} used Jacobian eigendecomposition to try to reveal the properties of trained DNNs on diffusion models. For a denoising estimator $$f(x_t, t)$$, we can express it in terms of its local linearization:
$$f(x_t, t) = \nabla f(x_t, t) x_t = \sum_{k=1}^d \lambda_k(x_t) \langle x_t, e_k(x_t) \rangle e_k(x_t)$$
where $$\{ e_k(x_t) \}_{1 \leq k \leq d}$$ forms an adaptive orthonormal basis and $$\lambda_k(x_t)$$ are the corresponding eigenvalues of the Jacobian $$\nabla f(x_t, t)$$.

### Properties of Optimal Denoiser
For the optimal denoiser $$f^*(x_t, t)$$, we have:
$$f^*(x_t, t) = \mathbb{E}[x_0 | x_t] = x_t + \sigma_t^2 \nabla_{x_t} \log p_t(x_t)$$
$$\nabla f^*(x_t, t) = \text{Id} + \sigma_t^2 \nabla^2_{x_t} \log p_t(x_t) = \sigma_t^{-2} \text{Cov}[x_0 | x_t]$$

The Jacobian of the optimal denoiser is proportional to the posterior covariance matrix.

### Low-Rank Structure
The effectiveness of the denoiser is characterized by its mean squared error (MSE):
$$\text{MSE}(f, \sigma_t^2) = \mathbb{E} \left[ 2 \sigma_t^2 \, \text{tr} \, \nabla f(x_t, t) + \| x_t - f(x_t, t) \|^2 - \sigma_t^2 d \right]$$

### MoLRG
Similarly shown in \citet{wang2024diffusion}, the optimal denoiser takes the form:
$$f_\theta(x_t, t) = \frac{s_t^2}{s_t^2 + \gamma_t^2} \sum_{k=1}^K w_k(\theta; x_t) U_k U_k^T x_t$$
where $$\{ U_k \}_{k=1}^K \subset O^{n \times d}$$ denotes orthonormal bases and $$\pi_k$$ represents mixing proportions.
