# Qubeon³ Relational Calculus Primer

<!-- NAVIGATION -->
**Qubeon³ Documentation**\
[⬅️ Back to README](../../README.md) | [📘 Manifesto](../MANIFESTO.md) | [📐 Calculus](CALCULUS.md) | [✂️ Slice](SLICE.md) | [📤 Project](PROJECT.md) | [🔽 Contract](CONTRACT.md) | [🔁 Transposition](TRANSPOSITION.md) | [🧩 Align](ALIGN.md) | [🗺️ Operators Index](OPERATORS_INDEX.md)


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

```
{ q | q ∈ 𝓈 ∧ q[axis1] ∈ A ∧ q[axis2] = v }
```

Where:

- $`𝓈`$ is a cube
- $`q`$ is a cube cell reference
- $`axis1`$, $`axis2`$ are axis selectors
- $`A`$ is a domain or subdomain
- $`v`$ is a scalar value

> "Return all cells $`q`$ in cube $`𝓈`$ where axis1 is in set A and axis2 equals v."

---

## 📀 Cuboid Calculus

> Theory and formalism for expressing spatial structures as logical objects.

We define **cuboids** as 3D objects with orthogonal boundaries in Euclidean space that can be described using the minimal bounding rectangle approach.

Each cuboid is defined by a minimum and maximum point in ℝ³.

We denote a single cuboid as a 6-tuple of real numbers:

```
c = (xmin, ymin, zmin, xmax, ymax, zmax)
```

To express the set of all **valid** cuboids, we define:

```
q := { c ∈ ℝ⁶ : c₀ < c₃ ∧ c₁ < c₄ ∧ c₂ < c₅ }
```

This enforces that for any element `c` in set `q`, the minimums are strictly less than the corresponding maximums in each dimension — ensuring each tuple describes a non-degenerate cuboid with real spatial extent.

---

## 🧪 Sparse vs. Dense Cubes

In classical data warehousing, a “cube” is often defined as a **dense** structure: a full block of values across all axis combinations.

Q³ deliberately relaxes this assumption.

> **In Q³, a cube is a set of axis-typed tuples.**\
> There is no requirement for completeness across dimensions.

This means:

- Slices and projections may produce **sparse** or irregular subcubes
- Operators still behave consistently, even when data coverage is partial
- Cube validity in Q³ is structural (i.e., tuple shape), not density-based

---

<!-- NAVIGATION -->
**Qubeon³ Documentation**\
[⬅️ Back to README](../../README.md) | [📘 Manifesto](../MANIFESTO.md) | [📐 Calculus](CALCULUS.md) | [✂️ Slice](SLICE.md) | [📤 Project](PROJECT.md) | [🔽 Contract](CONTRACT.md) | [🔁 Transposition](TRANSPOSITION.md) | [🧩 Align](ALIGN.md) | [🗺️ Operators Index](OPERATORS_INDEX.md)


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

```
{ q | q ∈ 𝓈 ∧ q[axis1] ∈ A ∧ q[axis2] = v }
```

Where:

- $`𝓈`$ is a cube
- $`q`$ is a cube cell reference
- $`axis1`$, $`axis2`$ are axis selectors
- $`A`$ is a domain or subdomain
- $`v`$ is a scalar value

> "Return all cells $`q`$ in cube $`𝓈`$ where axis1 is in set A and axis2 equals v."

---

## 📀 Cuboid Calculus

> Theory and formalism for expressing spatial structures as logical objects.

We define **cuboids** as 3D objects with orthogonal boundaries in Euclidean space that can be described using the minimal bounding rectangle approach.

Each cuboid is defined by a minimum and maximum point in ℝ³.

We denote a single cuboid as a 6-tuple of real numbers:

```
c = (xmin, ymin, zmin, xmax, ymax, zmax)
```

To express the set of all **valid** cuboids, we define:

```
q := { c ∈ ℝ⁶ : c₀ < c₃ ∧ c₁ < c₄ ∧ c₂ < c₅ }
```

This enforces that for any element `c` in set `q`, the minimums are strictly less than the corresponding maximums in each dimension — ensuring each tuple describes a non-degenerate cuboid with real spatial extent.

---

## 🧪 Sparse vs. Dense Cubes

In classical data warehousing, a “cube” is often defined as a **dense** structure: a full block of values across all axis combinations.

Q³ deliberately relaxes this assumption.

> **In Q³, a cube is a set of axis-typed tuples.**\
> There is no requirement for completeness across dimensions.

This means:

- Slices and projections may produce **sparse** or irregular subcubes
- Operators still behave consistently, even when data coverage is partial
- Cube validity in Q³ is structural (i.e., tuple shape), not density-based

---

**Qubeon³ Documentation**\
[⬅️ Back to README](../../README.md) | [📘 Manifesto](../MANIFESTO.md) | [📐 Calculus](CALCULUS.md) | [✂️ Slice](SLICE.md) | [📤 Project](PROJECT.md) | [🔽 Contract](CONTRACT.md) | [🔁 Transposition](TRANSPOSITION.md) | [🧩 Align](ALIGN.md) | [🗺️ Operators Index](OPERATORS_INDEX.md)

