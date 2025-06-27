# Qubeon³ Relational Calculus Primer

<!-- NAVIGATION -->
**Qubeon³ Documentation**
[⬅️ Back to README](../../README.md) | [📘 Manifesto](../MANIFESTO.md) | [📐 Calculus](CALCULUS.md) | [🔁 Transposition](TRANSPOSITION.md) | [⚙️ Operators Index](OPERATORS_INDEX.md)

> "If tables were arithmetic, cubes are geometry."

---

## 🎯 Goal

To extend the principles of relational calculus into a form that supports three (or more) dimensions natively, enabling expressive and intuitive queries over structured cube data.
*(For now we're limiting ourselves to 3 dimensions because anything higher gives me headaches.)*

This document introduces **Q³ Calculus**, a dimensional extension of relational calculus.

---

## 🧠 Background: Classical Relational Calculus

Relational calculus traditionally exists in two forms:

- **Tuple Relational Calculus (TRC)**:
  $`{ t | P(t) }`$ where $`t`$ is a tuple and $`P(t)`$ is a predicate.

- **Domain Relational Calculus (DRC)**:
  $`{ <x1, x2, ..., xn> | P(x1, x2, ..., xn) }`$

Both assume a flat, tabular model of data where a relation is a set of tuples.

---

## 🧩 Limitation: The Dimensional Ceiling

Relational calculus was never built to model structured layers, grids, or axes. To model cube-shaped data, you'd typically:

- Encode each axis as a separate column
- Duplicate context
- Aggregate manually across pseudo-dimensions

This leads to unnatural queries and brittle logic.

---

## 🔮 Introducing Q³ Calculus

Let us define a cube-native formalism:

### Core Concepts

- **Qube (𝒬)**: A first-class multidimensional data object
- **Slice**: A subset of a Qube along one or more axes
- **Axis-aware predicates**: Conditions aware of axis context
- **Cube Comprehension**: An expression that returns a cube slice matching structural and value constraints

### Syntax Sketch

<!-- NAVIGATION -->
**Qubeon³ Documentation**

[⬅️ Back to README](../../README.md) | [📘 Manifesto](../MANIFESTO.md) | [📐 Calculus](CALCULUS.md) | [🔁 Transposition](TRANSPOSITION.md) | [⚙️ Operators Index](OPERATORS_INDEX.md)
