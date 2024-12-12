---
title: 'Scaling law in ADAM: Batch Size vs Learning Rate'
date: 2024-06-14
permalink: /posts/2013/08/blog-post-2/
tags:
  - ADAM
  - Optimization
  - Scaling Law
---

### How Does Batch Size Impact Training Dynamics?

Intuitively, when the batch size increases, the gradient of each batch becomes more accurate, allowing for larger steps—i.e., increasing the learning rate—to reach the goal faster and shorten training time. This is generally intuitive. The question is, how much should it be increased to be optimal?

## Square Root Scaling

The earliest answer to this question might be square root scaling. Specifically, if the batch size is increased by a factor of $n$, then the learning rate should be increased by $\sqrt{n}$. This concept originates from the 2014 paper "One Weird Trick for Parallelizing Convolutional Neural Networks." The derivation principle is to keep the variance of the SGD increments unchanged. Specifically, let the gradient of a randomly sampled single example be denoted as $\tilde{g}$, with its mean and covariance represented as $g$ and $\Sigma$, respectively. Here, $g$ is the gradient of all samples. When the sample size increases to $B$, we have:

$$
\tilde{g}_B \coloneqq \frac{1}{B} \sum_{i=1}^B \tilde{g}^{(i)}, \quad \mathbb{E}[\tilde{g}_B] = g, \quad \mathbb{E}\left[(\tilde{g}_B - g)(\tilde{g}_B - g)^\top\right] = \frac{\Sigma}{B}
$$

This means that increasing the sample size does not change the mean but reduces the covariance to $\frac{\Sigma}{B}$. For the SGD optimizer, the increment is $-\eta \tilde{g}_B$, whose covariance is proportional to $$\frac{\eta^2}{B}$$. Since a moderate amount of noise is necessary for the optimization process, when the batch size $B$ changes, we adjust the learning rate $\eta$ to keep the noise intensity (i.e., the covariance matrix) constant. This leads to:

$$
\eta^2 B = \text{constant} \quad \Rightarrow \quad \eta \propto \sqrt{B}
$$

This establishes the square root scaling law between the learning rate and batch size. The later paper "Train Longer, Generalize Better: Closing the Generalization Gap in Large Batch Training of Neural Networks" also supports this choice.

## Linear Scaling

Interestingly, linear scaling—where $\eta \propto B$—often performs better in practice. Even the authors of the earliest paper proposing square root scaling, "One Weird Trick for Parallelizing Convolutional Neural Networks," acknowledged this and stated they could not provide a reasonable explanation.

To some extent, linear scaling aligns more closely with our intuitive understanding. For example, in "Accurate, Large Minibatch SGD: Training ImageNet in 1 Hour," assuming that the gradient direction changes little over $n$ consecutive batches, linear scaling becomes almost evidently valid. However, this assumption is clearly too strong. Relaxing it requires linking SGD with SDEs (Stochastic Differential Equations), as done in "Stochastic Modified Equations and Dynamics of Stochastic Gradient Algorithms I: Mathematical Foundations." The first paper to point out the scaling relationship between learning rate and batch size was "On the Generalization Benefit of Noise in Stochastic Gradient Descent."

In hindsight, establishing this connection is not difficult to understand. Let the model parameters be $\theta$. The SGD update rule can be rewritten as:

$$
\theta_{t+1} = \theta_t - \eta \tilde{g}_t^B = \theta_t - \eta g_t - \eta (\tilde{g}_t^B - g_t)
$$

Here, $\tilde{g}_t^B - g_t$ represents the gradient noise. So far, we have made no assumptions about the distribution of this noise, only that its mean is 0 and its covariance is $\frac{\Sigma_t}{B}$. Next, we assume that this noise follows a normal distribution $\mathcal{N}(0, \frac{\Sigma_t}{B})$, allowing us to further rewrite the iteration as:

$$
\theta_{t+1} = \theta_t - \eta g_t - \eta \Sigma_t^{1/2} B^{-1/2} z, \quad z \sim \mathcal{N}(0, I)
$$

This implies that the SGD iteration $\theta_{t+1} = \theta_t - \eta \tilde{g}_t^B$ is approximately solving the SDE:

$$
d\theta = -g_t dt - \eta \Sigma_t^{1/2} B^{-1/2} dw
$$

Therefore, to ensure that the runtime results remain consistent when $B$ changes, the form of the above SDE should remain unchanged. This leads to linear scaling $\eta \propto B$. The critical step in this process is that the noise term's step size is the square root of the non-noise term, allowing us to separate out a factor of $\sqrt{\eta}$.

## Facing the Loss Function

It is certain that whether it's square root scaling or linear scaling, these methods can only hold approximately within a local range because they both imply that "as long as the batch size is sufficiently large, the learning rate can be increased indefinitely," which is clearly impossible. Moreover, the previous sections focused on variance, but our fundamental task is to minimize the loss function. Therefore, a loss function-oriented approach might be more essential.

## Monotonic Bounding

A classic work from OpenAI on this perspective is "An Empirical Model of Large-Batch Training," which analyzes the optimal learning rate for SGD using a second-order approximation of the loss function. It concludes that "the learning rate increases monotonically with batch size but has an upper bound." A similar approach appears earlier in "Dissecting Adam: The Sign, Magnitude, and Variance of Stochastic Gradients," though that paper was not intended to analyze the role of batch size.

The key idea in the entire derivation process is to treat the learning rate as an optimization parameter. Let the loss function be $L(\theta)$, and the current batch's gradient be $\tilde{g}^B$. Then, after an SGD step, the loss function becomes $L(\theta - \eta \tilde{g}^B)$. We treat the optimization of the optimal learning rate as the following problem:

$$
\eta^* = \arg\min_\eta \mathbb{E}[L(\theta - \eta \tilde{g}^B)]
$$

This objective is intuitive: choose the learning rate that maximizes training efficiency on average (i.e., minimizes the loss function as quickly as possible). To solve this problem, we approximate the loss function to the second order:

$$
L(\theta - \eta \tilde{g}^B) \approx L(\theta) - \eta \tilde{g}^B^\top \frac{\partial L(\theta)}{\partial \theta} + \frac{1}{2} \eta^2 \tilde{g}^B^\top \frac{\partial^2 L(\theta)}{\partial \theta^2} \tilde{g}^B \coloneqq H\tilde{g}^B = L(\theta) - \eta \tilde{g}^B^\top g + \frac{1}{2} \eta^2 \tilde{g}^B^\top H \tilde{g}^B
$$

Here, $H$ is the Hessian matrix, and $\frac{\partial L(\theta)}{\partial \theta}$ is the gradient of the loss function. Ideally, the gradient is based on all samples, which is why its mean is $g$. Taking the expectation, we get:

$$
\mathbb{E}[L(\theta - \eta \tilde{g}^B)] \approx \mathbb{E}\left[L(\theta) - \eta \tilde{g}^B^\top g + \frac{1}{2} \eta^2 \tilde{g}^B^\top H \tilde{g}^B\right] = L(\theta) - \eta g^\top g + \frac{1}{2} \eta^2 \mathbb{E}[\tilde{g}^B^\top H \tilde{g}^B]
$$

The last term requires some manipulation:

$$
\mathbb{E}[\tilde{g}^B^\top H \tilde{g}^B] = \mathbb{E}[\text{Tr}(\tilde{g}^B^\top H \tilde{g}^B)] = \mathbb{E}[\text{Tr}(\tilde{g}^B \tilde{g}^B^\top H)] = \text{Tr}\left(\mathbb{E}[\tilde{g}^B \tilde{g}^B^\top] H\right) = \text{Tr}\left((gg^\top + \frac{\Sigma}{B}) H\right)
$$

This transformation mainly uses the property $\text{Tr}(AB) = \text{Tr}(BA)$. Assuming that $H$ is positive definite, the problem becomes a quadratic function, and the optimal solution is easily obtained:

$$
\eta^* \approx \frac{g^\top g}{g^\top H g + \frac{\text{Tr}(\Sigma H)}{B}} = \eta_{\text{max}} \frac{1}{1 + \frac{B_{\text{noise}}}{B}}
$$

This yields the result that "the learning rate increases monotonically with $B$ but has an upper bound," where:

$$
\eta_{\text{max}} = \frac{g^\top g}{g^\top H g}, \quad B_{\text{noise}} = \frac{\text{Tr}(\Sigma H)}{g^\top H g}
$$

## Practical Analysis

In practice, the most critical issue is how to estimate $\eta_{\text{max}}$ and $B_{\text{noise}}$, especially since $B_{\text{noise}}$ directly relates to the learning rate scaling law and the saturation of training efficiency. Direct computation involves the Hessian matrix $H$, whose computational cost scales with the square of the number of parameters. With models having hundreds of millions of parameters considered small today, computing the Hessian matrix is clearly impractical, necessitating more efficient computation methods.

First, consider $B_{\text{noise}}$, defined as:

$$
B_{\text{noise}} = \frac{\text{Tr}(\Sigma H)}{g^\top H g}
$$

The presence of $H$ in both the numerator and denominator suggests an urge to "cancel them out." Indeed, a simplified approach is to assume $H$ is approximately a multiple of the identity matrix, yielding:

$$
B_{\text{noise}} \approx \frac{\text{Tr}(\Sigma)}{g^\top g} \coloneqq B_{\text{simple}}
$$

$B_{\text{simple}}$ is more computationally feasible and is often a good approximation of $B_{\text{noise}}$ in experiments. Therefore, we choose to estimate $B_{\text{simple}}$ instead of $B_{\text{noise}}$. Note that $\text{Tr}(\Sigma)$ only requires computing the diagonal elements, so there's no need to compute the full covariance matrix—just compute the variance of each gradient component and sum them. In data-parallel scenarios, the gradient variance can be directly estimated using gradients computed on each device.

## Data Efficiency

Building on the above results, we can derive an asymptotic relationship between the amount of training data and the number of training steps. The derivation is straightforward: substituting equation (10) into the loss function yields that, under the optimal learning rate, the decrease in the loss function per iteration is:

$$
\Delta L = L(\theta) - \mathbb{E}[L(\theta - \eta^* \tilde{g}^B)] \approx \frac{\Delta L_{\text{max}}}{1 + \frac{B_{\text{noise}}}{B}}
$$

where:

$$
\Delta L_{\text{max}} = \frac{(g^\top g)^2}{2 g^\top H g}
$$

The key interpretation of this result is as follows:

- When $B \to \infty$ (i.e., full-batch SGD), each step achieves the maximum loss decrease $\Delta L_{\text{max}}$, allowing the target point to be reached with the fewest training steps ($S_{\text{min}}$).
- When $B$ is finite, each step's average loss decrease is $\Delta L$, implying that approximately $1 + \frac{B_{\text{noise}}}{B}$ steps are needed to achieve the same loss decrease as full-batch SGD. Therefore, the total number of training steps is roughly $S = (1 + \frac{B_{\text{noise}}}{B}) S_{\text{min}}$.

Since the batch size is $B$, the total number of samples consumed during training is $E = B S = (B + B_{\text{noise}}) S_{\text{min}}$. This is an increasing function of $B$, and as $B \to 0$, $E \to B_{\text{noise}} S_{\text{min}}$, indicating that using a sufficiently small batch size can reduce the total number of training samples needed, at the cost of a large number of training steps.

## Adaptive Versions

The analysis approach for Adam is similar to that for SGD, both based on second-order expansion. The difference lies in replacing the direction vector $\tilde{g}^B$ with a general vector $\tilde{u}^B$. In this case, we have:

$$
\mathbb{E}[L(\theta - \eta \tilde{u}^B)] \approx L(\theta) - \eta \mathbb{E}[\tilde{u}^B]^\top g + \frac{1}{2} \eta^2 \text{Tr}(\mathbb{E}[\tilde{u}^B \tilde{u}^{B \top} ] H)
$$

Now, we need to determine $\tilde{u}^B$ and compute the corresponding $\mathbb{E}[\tilde{u}^B]$ and $\mathbb{E}[\tilde{u}^B \tilde{u}^{B \top}]$. Since only an asymptotic relationship is needed, we choose $\text{SignSGD}$, i.e., $\tilde{u}^B = \text{sign}(\tilde{g}^B)$, as an approximation for Adam.

## Intuitive Understanding of the Surge Phenomenon

The Surge Phenomenon can be intuitively understood as a manifestation of the suboptimality of adaptive learning rate strategies. Taking the approximation $\tilde{u}^B = \text{sign}(\tilde{g}^B)$ as an example, as $B$ increases, $\tilde{g}^B$ becomes more accurate and approaches $\text{sign}(g)$ as $B \to \infty$. However, $\text{sign}(g)$ is not necessarily the most scientifically accurate update direction, especially in the later stages of training where adaptive strategies might have negative effects. Therefore, when $B$ is appropriately sized, the noise in $\text{sign}(\tilde{g}^B)$ can help correct this suboptimality. As $B$ continues to increase and the noise decreases, the opportunity for such corrections diminishes, necessitating a more cautious reduction in the learning rate.

