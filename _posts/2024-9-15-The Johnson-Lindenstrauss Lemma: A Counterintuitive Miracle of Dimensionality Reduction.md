---
title: 'The Johnson-Lindenstrauss Lemma: A Counterintuitive Miracle of Dimensionality Reduction'
date: 2024-9-15
permalink: /posts/2024/7/Johnson-Lindenstrauss/
tags:
  - Johnson-Lindenstrauss
  - Dimensionality Reduction
  - Probability Inequalities
---

Let’s dive into the **Johnson-Lindenstrauss Lemma**, or as we’ll call it to save breath, the **JL Lemma**. This is one of those magical results about dimensionality reduction and a prime example of the counterintuitive nature of high-dimensional spaces—what people sometimes call the “curse of dimensionality.” In fact, the JL Lemma underpins many machine learning techniques like dimensionality reduction and hashing. It also provides theoretical support for understanding and tuning dimensional parameters in modern models. Pretty neat, huh?

---

### The Power of Logarithmic Dimensions

At its core, the JL Lemma is like saying:

**Plain English JL Lemma**: You can fit $$N$$ vectors into just $$O(\log N)$$-dimensional space.  

Here’s the kicker: no matter how high-dimensional your original $$N$$ vectors are, you can squash them into $$O(\log N)$$ dimensions and still control the relative error in distances. That’s wild! Imagine you’re working on a vector retrieval problem where the original vectors are ridiculously high-dimensional. Checking distances between all of them sounds painful, computationally speaking. But the JL Lemma says, "Hey, relax! You can map those vectors into a much smaller $$O(\log N)$$-dimensional space, and guess what? The retrieval accuracy is *almost* the same." Talk about a life-saver!

---

### Is the Method Complex?

Now you might wonder: with such a strong conclusion, is the dimensionality reduction process some sort of black-magic math? The answer is a big *nope*! The method is *astonishingly simple*. All you need is **random linear projections**. In fact, people often joke that proving the JL Lemma is easier than intuitively understanding why it works. The math checks out, but wrapping your head around such counterintuitive results can be tricky.

---

### A Few Related Mind-Benders

Before we dig into the nitty-gritty, let’s recall two other counterintuitive results from high-dimensional spaces:

1. **Almost Orthogonal Vectors**: In high dimensions, any two random vectors are almost perpendicular. Wild, right? Not something you’d guess from our comfy low-dimensional intuition.
2. **Nearly Orthogonal Matrices**: Randomly sampling an $$n \times n$$ matrix from a Gaussian distribution like $$\mathcal{N}(0, 1/n)$$ gives you something close to an orthogonal matrix. That’s a far cry from our usual notion that orthogonality is a strict, rare property.

These results aren’t just fun facts; they’re closely related to the JL Lemma and form its foundation. To make sense of the JL Lemma quantitatively, we’ll need to dive into some probabilities.

---

### Probability Inequalities: The Tools We Need

### Markov Inequality

If $$x$$ is a non-negative random variable and $$a > 0$$, then:

$$
P(x \geq a) \leq \frac{\mathbb{E}[x]}{a}.
$$

This result doesn’t require any fancy assumptions about the distribution of $$x$$ -- it just needs to be non-negative. The proof is simple:

$$
\mathbb{E}[x] = \int_0^\infty x p(x) dx \geq \int_a^\infty x p(x) dx \geq \int_a^\infty a p(x) dx = a P(x \geq a).
$$

### Chebyshev Inequality
If $$x$$ isn’t non-negative, we can tweak things. For instance, \(|x - \mathbb{E}[x]|\) is always non-negative. Applying Markov’s inequality to $$(x - \mathbb{E}[x])^2$$, we get the **Chebyshev Inequality**:

$$
P(|x - \mathbb{E}[x]| \geq a) = P((x - \mathbb{E}[x])^2 \geq a^2) \leq \frac{\text{Var}[x]}{a^2}.
$$

### Cramér-Chernoff Method

Another useful tool is the **Cramér-Chernoff method**, which transforms any random variable into a non-negative one using an exponential function. For any $$\lambda > 0$$, we have:

$$
x \geq a \implies \lambda x \geq \lambda a \implies e^{\lambda x} \geq e^{\lambda a}.
$$

Using Markov’s inequality:  

$$
P(x \geq a) = P(e^{\lambda x} \geq e^{\lambda a}) \leq e^{-\lambda a} \mathbb{E}[e^{\lambda x}].
$$

Here’s the genius part: since $$\lambda > 0$$ is arbitrary, you can optimize over $$\lambda$$ to minimize the bound:  

$$
P(x \geq a) \leq \min_{\lambda > 0} e^{-\lambda a} \mathbb{E}[e^{\lambda x}].
$$


### The Foundation: Unit Norm Lemma

This lemma forms the backbone of the JL Lemma:  

> **Unit Norm Lemma**: Let $$u \in \mathbb{R}^n$$ be a random vector where each entry is sampled i.i.d. from $$\mathcal{N}(0, 1/n)$$, and let $$\epsilon \in (0, 1)$$. Then:

> $$
> P\big(|\|u\|_2 - 1| \geq \epsilon\big) \leq 2 \exp\left(-\frac{\epsilon^2 n}{8}\right).
> $$

This says that for sufficiently large $$n$$, the length of $$u$$ (its $$\ell_2$$-norm) will almost always be close to 1. The probability of deviation shrinks exponentially fast with $$n$$.


### Proof of the Unit Norm Lemma
We use the **Cramér-Chernoff method** here. Decompose $$|\|u\|_2^2 - 1| \geq \epsilon$$ into two cases: $$\|u\|_2^2 - 1 \geq \epsilon$$ or $$1 - \|u\|_2^2 \geq \epsilon$$. Focus on the first case,  

$$
\mathbb{P}\big(\|u\|_2^2 - 1 \geq \epsilon\big) \leq \min_{\lambda > 0} e^{-\lambda (\epsilon + 1)} \mathbb{E}\big[e^{\lambda \|u\|_2^2}\big]
$$

Since $$u$$ is sampled from $$\mathcal{N}(0, 1/n)$$, each component $$u_i$$ is independent, and we can write:  

$$
\mathbb{E}[e^{\lambda \|u\|_2^2}] = \prod_{i=1}^{n} \mathbb{E}[e^{\lambda u_i^2}] = \left(\mathbb{E}[e^{\lambda u_1^2}]\right)^n.
$$

For Gaussian $$u_i$$, it’s straightforward to compute:  

$$
\mathbb{E}[e^{\lambda u_1^2}] = \sqrt{\frac{n}{n - 2\lambda}}.
$$

Plugging this back and minimizing gives the desired bound. The rest of the proof follows similarly for the second case.


### Putting It Together: The JL Lemma

Finally, let’s formalize the JL Lemma:  

> **Mathematical JL Lemma**: Let $$v_1, v_2, \dots, v_N \in \mathbb{R}^m$$ be $$N$$ vectors. For $$n > \frac{24 \log N}{\epsilon^2}$$, if $$A \in \mathbb{R}^{n \times m}$$ is a random matrix sampled i.i.d. from $$\mathcal{N}(0, 1/n)$$, then with probability at least $$1 - \frac{1}{N}$$:  

> $$
> (1 - \epsilon) \|v_i - v_j\|_2^2 \leq \|A v_i - A v_j\|_2^2 \leq (1 + \epsilon) \|v_i - v_j\|_2^2,
> $$
> 
> for all $$i \neq j$$.

In short, we can squish $$N$$ vectors into $$O(\log N)$$ dimensions while approximately preserving their pairwise distances. The proof combines the Unit Norm Lemma with basic properties of random projections.

### Why This Matters

The JL Lemma is more than just theoretical fun. It’s practical and simple to use: take a Gaussian random matrix, project your data, and you’re good to go! Whether it’s for approximate nearest neighbors, hash functions, or model tuning, the JL Lemma is a versatile tool. And remember, this is just for random projections—there are more refined techniques out there that can push these bounds even further.
