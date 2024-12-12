---
title: 'Asymptotically Flat Spacetime and the Positive Mass Theorem'
date: 2022-02-03
permalink: /posts/2024/12/Asymptotically-Flat/
tags:
  - ADM Mass
  - Positive Mass Theorem
  - Riemannian Geometry
math: true
---

<script type="text/javascript" async
    src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js">
</script>

### 1. What does it mean for a Spacetime to be "Flat"?

In this section, we explore the **Positive Mass Theorem**, a cornerstone result in classical general relativity. The theorem rigorously demonstrates that, under suitable physical and geometric assumptions, the total ADM mass of an asymptotically flat spacetime is non-negative, with equality holding only for Minkowski spacetime.

#### 1.1 Energy-Momentum in General Relativity

The gravitational field’s energy-momentum is challenging to localize due to the **Equivalence Principle**. The Riemann curvature tensor $$R_{\mu\nu\rho\sigma}$$, which encodes gravitational effects, can be locally "eliminated" through coordinate transformations in regions of spacetime free of matter. Formally, we express the connection as:

$$
\Gamma^\lambda_{\mu\nu} = \frac{1}{2} g^{\lambda\sigma} \left( \partial_\mu g_{\nu\sigma} + \partial_\nu g_{\mu\sigma} - \partial_\sigma g_{\mu\nu} \right),
$$

and under coordinate transformations, the components of $$\Gamma^\lambda_{\mu\nu}$$can be locally reduced to zero. Thus, energy-momentum cannot be represented as a tensor field globally in general relativity.

#### 1.2 ADM Mass and Asymptotic Flatness

To define a total energy-momentum for an isolated system, we require the spacetime to be **asymptotically flat**. For a metric $$g_{\mu\nu}$$, asymptotic flatness is characterized by:

1. $$g_{\mu\nu} = \eta_{\mu\nu} + h_{\mu\nu}$$, where $$h_{\mu\nu} = O(r^{-1})$$,

2. $$\partial_\rho h_{\mu\nu} = O(r^{-2})$$,

3. $$\partial_\sigma \partial_\rho h_{\mu\nu} = O(r^{-3})$$.

Here, $$r = \sqrt{x^2 + y^2 + z^2}
$$denotes the radial coordinate in a Cartesian-like coordinate system. These conditions ensure that $$g_{\mu\nu}$$approaches the Minkowski metric $$\eta_{\mu\nu}$$at spatial infinity.

The ADM mass $$M_{\text{ADM}}$$is defined using the spatial metric $$g_{ij}$$and its first derivatives on a spacelike hypersurface $$\Sigma$$. Explicitly, in a coordinate system where $$\Sigma$$is defined by $$t = \text{const}$$, we have:

$$M_{\text{ADM}} = \frac{1}{16\pi} \lim_{r \to \infty} \int_{S_r} \left( \partial_j g_{ij} - \partial_i g_{jj} \right) n^i \, dS,$$

where $$S_r$$is a large sphere of radius $$r$$, $$n^i$$is the unit normal vector to $$S_r$$, and $$dS$$is the surface element. This integral captures both the matter and gravitational contributions to the total energy.

#### 1.3 Modern Definition via Conformal Compactification

To avoid the coordinate dependence of the naive definition, the **modern approach** compactifies spacetime using a **conformal transformation**:

$$
\tilde{g}_{\mu\nu} = \Omega^2 g_{\mu\nu},
$$

where the conformal factor $$\Omega$$satisfies:

$$
\Omega \to 0, \quad \nabla_\mu \Omega \neq 0 \quad \text{as } r \to \infty.
$$

Under this transformation, the "infinity" of the original spacetime becomes a boundary of the conformally rescaled spacetime $$(\tilde{\mathcal{M}}, \tilde{g}_{\mu\nu})$$. The key properties of the transformed spacetime include:

1. The causal structure, determined by the light cone $$g_{\mu\nu} dx^\mu dx^\nu = 0$$, remains invariant because conformal transformations preserve angles.

2. Null geodesics in $$g_{\mu\nu}$$remain null in $$\tilde{g}_{\mu\nu}$$.

As an example, consider the Minkowski metric:

$$
ds^2 = -dt^2 + dr^2 + r^2 d\Omega^2,
$$

where $$r$$and $$t$$are unbounded. Applying the transformations:

1. $$u = t - r, \, v = t + r$$,

2. $$
\tilde{g}_{\mu\nu} = \frac{4}{(1 + u^2)(1 + v^2)} g_{\mu\nu}
$$,

3. $$T = \tan^{-1}(v) + \tan^{-1}(u), \, R = \tan^{-1}(v) - \tan^{-1}(u),$$

the rescaled metric becomes:

$$
\tilde{ds}^2 = -dT^2 + dR^2 + \sin^2 R \, d\Omega^2,
$$

where $$R$$and $$T$$are bounded, with $$R \in [0, \pi]$$and $$T \in [-\pi, \pi]$$.

#### 1.4 Geometric Tools and Ricci Tensor

The Positive Mass Theorem relies on the interplay of geometry and partial differential equations. A key tool is the **Ricci tensor** $$R_{\mu\nu}$$, derived from the Riemann curvature tensor $$R^\rho_{\sigma\mu\nu}$$:

$$
R_{\mu\nu} = R^\rho_{\mu\rho\nu}, \quad R = g^{\mu\nu} R_{\mu\nu}.
$$

The Einstein field equations $$G_{\mu\nu} = 8\pi T_{\mu\nu}$$relate the geometry of spacetime to its energy content, where $$G_{\mu\nu} = R_{\mu\nu} - \frac{1}{2} g_{\mu\nu} R$$.

#### 1.5 Penrose's Framework

In Penrose's compactification framework, spacetime infinity is represented as conformal infinity $$\mathcal{I}$$, divided into:

1. Past and future null infinities ($$\mathcal{I}^-$$, $$\mathcal{I}^+$$),

2. Spacelike infinity ($$i^0$$),

3. Timelike infinities ($$i^-$$, $$i^+$$).

Each of these is characterized by specific behaviors of the conformal factor $$\Omega$$and the rescaled metric $$\tilde{g}_{\mu\nu}$$.

#### 1.6 Weak Energy Condition and Positivity

The Positive Mass Theorem is proven under the **Weak Energy Condition**:

$$
T_{\mu\nu} v^\mu v^\nu \geq 0, \quad \forall v^\mu \text{ timelike},
$$

which ensures non-negative energy density for any observer. Schoen and Yau’s proof employs techniques from **minimal surface theory** in Riemannian geometry, while Witten’s proof uses spinors and properties of the Dirac operator $$\mathcal{D}$$:

$$
\mathcal{D}\psi = \gamma^\mu \nabla_\mu \psi,
$$

where $$\psi$$is a spinor field and $$\gamma^\mu$$are the gamma matrices satisfying the Clifford algebra.

---
