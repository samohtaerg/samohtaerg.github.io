---
title: 'From the Pythagorean Theorem to Riemannian Metrics'
date: 2021-7-11
permalink: /posts/2021/7/riemannian-metrics/
tags:
  - Riemannian Geometry
  - Differential Geometry
  - do Carmo
---


### **What Is a Riemannian Metric?**

Geometry—what comes to mind? If you’re thinking “measuring the Earth,” you’re spot on! The word "geometry" originates from "Earth measurement." To measure, you need a reference frame and a way to compute distances.

Enter the **Pythagorean theorem**, the cornerstone of Euclidean geometry:

$$
ds^2 = dx^2 + dy^2 
$$

This formula assumes flatness and orthogonality, as described in **do Carmo’s Proposition 1.4**. But what if the surface is curved or the coordinates aren’t orthogonal? That’s where the idea of the **Riemannian metric** takes center stage.

---

### **Curvilinear Coordinates and Non-Euclidean Spaces**

Let’s take a step beyond Cartesian coordinates. In polar coordinates, for instance, distances are measured as:

\[
ds^2 = dr^2 + r^2 d\theta^2 \tag{2}
\]

Here, the radial and angular components stretch or shrink, and the basis vectors \(\partial / \partial r\) and \(\partial / \partial \theta\) are no longer unit-length or orthogonal. As outlined in **do Carmo’s Definition 1.2**, this transformation requires the **metric tensor**, which generalizes the Pythagorean theorem to arbitrary coordinates.

In the most general 2D case:

\[
ds^2 = E(x_1, x_2)(dx^1)^2 + 2F(x_1, x_2)dx^1 dx^2 + G(x_1, x_2)(dx^2)^2 \tag{3}
\]

The coefficients \(E, F, G\) encode the inner products of the coordinate basis vectors, forming the metric tensor \(g_{\mu\nu}\), as detailed in **do Carmo’s Equation (1.7)**.

---

### **Curvature and Geodesics**

The **first fundamental form**, \((ds^2)\), defines intrinsic properties of a surface. For curved surfaces, the geodesics—paths that locally minimize distance—are solutions to a variational principle. **do Carmo’s Proposition 3.5** elegantly connects geodesics to the Euler-Lagrange equations, illustrating the deep interplay between mechanics and geometry.

---

### **Examples of Riemannian Metrics**

#### **Flat Space in Polar Coordinates**

Switching to polar coordinates on a flat plane yields:

\[
ds^2 = dr^2 + r^2 d\theta^2
\]

This example, aligned with **do Carmo’s Example 1.8**, highlights how even flat spaces require non-trivial metrics when represented in curvilinear coordinates.

#### **The Sphere**

On a unit sphere, parameterized as:

\[
\begin{cases}
x = \sin\theta \cos\phi, \\
y = \sin\theta \sin\phi, \\
z = \cos\theta,
\end{cases}
\]

the metric becomes:

\[
ds^2 = d\theta^2 + \sin^2\theta \, d\phi^2 \tag{6}
\]

This induced metric, derived from the ambient Euclidean space, is explored in **do Carmo’s Example 1.14**. The \(\sin^2\theta\) factor reflects how distances compress near the poles.

#### **A Temperature-Dependent Ruler**

Imagine a flat plane where temperature varies, causing rulers to stretch or shrink locally. The resulting metric:

\[
ds^2 = f(x, y)(dx^2 + dy^2) \tag{7}
\]

describes a conformally flat space. This concept, linked to **do Carmo’s Example 2.12**, illustrates how metrics adapt to local scaling factors.

---

### **Practical Tools: The Metric Tensor**

Using the metric tensor \(g = [g_{\mu\nu}]\), we can formalize distances and angles. For a vector \(A = [A^\mu]\):

\[
|A|^2 = g_{\mu\nu} A^\mu A^\nu \tag{10}
\]

And for two vectors \(A\) and \(B\), the inner product is:

\[
\langle A, B \rangle = g_{\mu\nu} A^\mu B^\nu \tag{11}
\]

The determinant of \(g\) also defines the **volume element**:

\[
\sqrt{\det(g)} \, dx^1 \wedge dx^2 \wedge \cdots \wedge dx^n \tag{14}
\]

This plays a pivotal role in integration on manifolds, as detailed in **do Carmo’s Theorem 2.5**.

---

### **Wrapping Up**

Riemannian metrics generalize the Pythagorean theorem to any space, flat or curved, capturing distances, angles, and volumes. From polar coordinates to spheres, this mathematical structure forms the backbone of both differential geometry and physics. Thanks to foundational texts like **do Carmo’s**, we can rigorously explore the beauty of geometry in both local and global contexts.

---

**Tags:** #DifferentialGeometry #ManfredodoCarmo #RiemannianMetrics
