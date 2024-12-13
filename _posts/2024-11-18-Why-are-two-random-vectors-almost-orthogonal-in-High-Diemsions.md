---
title: 'Why Are Two Random Vectors Almost Always Orthogonal in High Dimensions'
date: 2024-11-18
permalink: /posts/2024/11/Why Are Two Random Vectors Almost Orthogonal in High Dimensions/
tags:
  - High-Dimensional Geometry
  - Random Vectors
  - Orthogonality
math: true
---


This phenomenon seems counterintuitive because in our everyday experience in 2D and 3D spaces, angles between random vectors feel distributed across the full range. However, in higher dimensions, things change drastically. Let’s dig into the math and theory to understand this.

---

### 1. **Understanding the Angle Distribution**

#### The Setup
Consider two random vectors in $$n$$-dimensional space:
1. A fixed vector $$\mathbf{y} = (1, 0, \dots, 0)$$.
2. A random vector $$\mathbf{x} = (x_1, x_2, \dots, x_n)$$, uniformly distributed on the $$n$$-dimensional unit sphere.

The angle $$\theta$$ between these vectors is determined by:

$$
\cos\theta = \langle \mathbf{x}, \mathbf{y} \rangle = x_1.
$$

Hence, the angle $$\theta$$ directly corresponds to the spherical coordinate $$\phi_1$$ in the hyperspherical system:

$$
\theta = \arccos(x_1) = \phi_1.
$$

#### The Probability of an Angle $$\theta$$
The probability that $$\phi_1 \leq \theta$$ is given by integrating over the $$n$$-dimensional hyperspherical surface:

$$
P_n(\phi_1 \leq \theta) = \frac{\int_0^{\theta} \sin^{n-2}(\phi_1) \, d\phi_1}{\int_0^{\pi} \sin^{n-2}(\phi_1) \, d\phi_1}.
$$

The normalization constant in the denominator is the total surface area of the $$(n-1)$$-dimensional unit sphere:

$$
\text{Surface Area} = \frac{2\pi^{n/2}}{\Gamma(n/2)}.
$$

Thus, the probability density function for $$\phi_1 = \theta$$ is:

$$
p_n(\theta) = \frac{\Gamma(n/2)}{\Gamma((n-1)/2)\sqrt{\pi}} \sin^{n-2}(\theta).
$$

---

### 2. **Key Insights from the Distribution**

#### Uniformity in Low Dimensions
- **In 2D ($$n = 2$$):** The probability density $$p_2(\theta)$$ is constant, implying that the angle $$\theta$$ is uniformly distributed.
- **In 3D ($$n = 3$$):** The distribution of $$\cos\theta$$ (not $$\theta$$) is uniform.

#### Concentration in High Dimensions
When $$n$$ becomes large, the factor $$\sin^{n-2}(\theta)$$ becomes sharply peaked around $$\theta = \pi/2$$ (90°). This is because:

$$
\sin^{n-2}(\theta) \to e^{-(n-2)(\theta - \pi/2)^2/2}, \quad \text{for } \theta \approx \pi/2.
$$

This behavior can be approximated using a Gaussian:

$$
p_n(\theta) \approx \frac{1}{\sqrt{2\pi / (n-2)}} e^{-(n-2)(\theta - \pi/2)^2 / 2}.
$$

---

### 3. **Variance of the Angle**

The variance of $$\theta$$ tells us how tightly the angles are clustered around $$\pi/2$$. For large $$n$$, the variance can be shown to approach:
$$\text{Var}_n(\theta) \approx \frac{1}{n-2}.$$

Let’s tabulate some numerical results for $$\text{Var}_n(\theta)$$:

| $$n$$   | 3      | 10    | 20     | 50     | 100    | 200    | 1000   |
|---------|--------|--------|--------|--------|--------|--------|--------|
| $$\text{Var}_n(\theta)$$ | 0.4674 | 0.1107 | 0.0526 | 0.0204 | 0.0101 | 0.0050 | 0.0010 |

Clearly, as $$n \to \infty$$, $$\text{Var}_n(\theta) \to 0$$. This confirms that as the dimensionality increases, the angles between random vectors concentrate ever more tightly around $$\pi/2$$.

---

### 4. **Probability Density of $$\cos\theta$$**

If we are more interested in the distribution of $$\eta = \cos\theta$$, the probability density function becomes:

$$
p_n(\eta) = \frac{\Gamma(n/2)}{\Gamma((n-1)/2)\sqrt{\pi}} (1 - \eta^2)^{(n-3)/2}.
$$

For high $$n$$, most of the density is concentrated near $$\eta = 0$$, meaning $$\cos\theta \approx 0$$ and $$\theta \approx \pi/2$$.

---

### 5. **Conclusion: Almost Orthogonal Vectors**

In high dimensions:
1. Angles between random vectors are concentrated around $$\pi/2$$ (90°).
2. This concentration grows sharper as $$n$$ increases, with the variance shrinking as $$1/(n-2)$$.
3. The geometry of high-dimensional spaces explains why random vectors are almost always orthogonal: most of the volume of the unit sphere lies near the equator relative to any fixed direction.

This is one of the many counterintuitive phenomena that arise in high-dimensional spaces, fundamentally challenging our low-dimensional intuitions.

--- 
