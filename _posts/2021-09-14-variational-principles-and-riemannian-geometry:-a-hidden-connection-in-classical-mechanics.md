---
redirect_from:
  - /2012/08/14/blog-post-1/
---
### **Can Riemannian Geometry be not about general relativity?**
Riemannian geometry in general relativity? Sure, you've heard about that. It's the rockstar of mathematical physics, weaving together gravity and spacetime. But let me tell you a secret: Riemannian geometry isn't just the darling of general relativity—it’s lurking quietly in classical mechanics, too. Yes, Newton’s good ol’ mechanics has Riemannian geometry in its DNA. Let’s unpack this and see how it works.

---

## The Starting Point: What Makes Geometry Tick?

The heart of Riemannian geometry is the **Riemannian metric**, a mathematical gadget that lets us compute distances and angles on curved spaces. It’s like having a high-tech ruler for warped surfaces. With a Riemannian metric, you can use variational principles to find geodesics—the shortest paths, or “straightest” curves, on the surface. Think of geodesics as the lazy paths: nature loves efficiency.

Now, here's the fun twist. If a Riemannian metric gives you a variational principle, could we reverse the process? Could a variational principle give us a Riemannian metric? It turns out, for many systems, yes! Variational principles show up everywhere: the principle of least action in mechanics, minimum potential energy in physics, maximum entropy in probability—nature just loves extremes.

For quadratic variational principles, the connection is particularly juicy. Let’s dive into an example to see this magic in action.

---

## From the Principle of Least Action to Riemannian Geometry

Let’s start with the principle of least action in classical mechanics. Consider a simple two-dimensional conservative system. The trajectory \( x(t), y(t) \) minimizes the action\(\sigma\):

\[
S = \int \left[ \frac{1}{2} \left( \left( \frac{dx}{dt} \right)^2 + \left( \frac{dy}{dt} \right)^2 \right) - U(x, y) \right] dt
\]

Here, \( U(x, y) \) is the potential energy, and we’ve set \( m = 1 \) for simplicity. Varying \( S \) gives the Euler-Lagrange equations:

\[
\frac{d^2 x}{dt^2} = -\frac{\partial U}{\partial x}, \quad \frac{d^2 y}{dt^2} = -\frac{\partial U}{\partial y}
\]

These are the equations of motion—nothing new so far.

Now let’s throw in energy conservation:

\[
\frac{1}{2} \left( \left( \frac{dx}{dt} \right)^2 + \left( \frac{dy}{dt} \right)^2 \right) + U(x, y) = E
\]

We can use this to eliminate \( dt \). Rearrange the energy equation:

\[
U(x, y) = E - \frac{1}{2} \left( \left( \frac{dx}{dt} \right)^2 + \left( \frac{dy}{dt} \right)^2 \right)
\]

Substitute this back into \( S \):

\[
S = \int \left( \left( \frac{dx}{dt} \right)^2 + \left( \frac{dy}{dt} \right)^2 - E \right) dt
\]

The term \( -E \, dt \) is a total differential—it doesn’t affect the variational result. So, the equivalent action is:

\[
S = \int \left( \left( \frac{dx}{dt} \right)^2 + \left( \frac{dy}{dt} \right)^2 \right) dt
\]

But wait! Let’s express \( dt^2 \) using the energy equation:

\[
dt^2 = \frac{dx^2 + dy^2}{2(E - U)}
\]

Substituting \( dt^2 \) into the action:

\[
S = \int \sqrt{2(E - U)(dx^2 + dy^2)}
\]

Now, drop the constant factor (variation doesn’t care about it), and we have:

\[
S = \int \sqrt{(E - U)(dx^2 + dy^2)}
\]

Here’s the punchline: this action has the form of a Riemannian metric:

\[
ds^2 = (E - U)(dx^2 + dy^2)
\]

The geodesics of this metric describe the trajectories of the system! Physics just got geometric.

---

## From Riemannian Geometry to Equations of Motion

Let’s confirm this. Suppose the metric is:

\[
ds^2 = (E - U)(dx^2 + dy^2)
\]

Its geodesics minimize the action:

\[
S = \int \sqrt{(E - U)(dx^2 + dy^2)}
\]

Varying this action gives:

\[
\delta S = \int \delta \sqrt{(E - U)(dx^2 + dy^2)}
\]

After some algebra and integration by parts (trust me, it’s messy but doable), you recover:

\[
\frac{d^2 x}{dt^2} + \frac{\partial U}{\partial x} = 0, \quad \frac{d^2 y}{dt^2} + \frac{\partial U}{\partial y} = 0
\]

The geodesics match the equations of motion! Voilà—mechanics and geometry are two sides of the same coin.

---

## Generalization

This idea generalizes beautifully. For a conservative system with the action:

\[
S = \int \left[ \frac{1}{2} g_{\mu\nu} \frac{dx^\mu}{dt} \frac{dx^\nu}{dt} - U(x) \right] dt
\]

the trajectory of energy \( E \) corresponds to the geodesics of the metric:

\[
ds^2 = [E - U(x)] g_{\mu\nu} dx^\mu dx^\nu
\]

The process works for any quadratic variational principle. Surprisingly, Jacobi discovered this way back in 1837!

---

## Practical Applications: Solving Geodesics

This isn’t just theoretical fun—it’s practical, too. For example, consider the metric:

\[
ds^2 = f(x, y)(dx^2 + dy^2)
\]

The geodesic equations are:

\[
\frac{d^2 x}{ds^2} = -\frac{1}{2f} \frac{\partial f}{\partial x} \left( \frac{dx}{ds} \right)^2 + \dots
\]

Solving these directly? Painful. But if we switch to the time parameter \( dt \), we simplify things:

\[
dt = \sqrt{\frac{dx^2 + dy^2}{2f(x, y)}}
\]

This reduces the geodesic equation to:

\[
\frac{d^2 x}{dt^2} = \frac{\partial f}{\partial x}, \quad \frac{d^2 y}{dt^2} = \frac{\partial f}{\partial y}
\]

For simple cases, like \( f(x, y) = \frac{1}{2}(x^2 + y^2) \), these become linear and solvable.

---

## Wrapping It All Up

So, there you have it. Riemannian geometry isn’t just about Einstein and spacetime—it’s been hiding in classical mechanics all along. Geometrizing mechanics links physics and geometry in profound ways, giving us a unified framework to explore everything from trajectories to field theories. And who knows? The next time you see a variational principle, you might just uncover a hidden geometry waiting to be explored.
