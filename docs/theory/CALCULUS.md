# Qubeon³ Relational Calculus Primer

> "If tables were arithmetic, cubes are geometry."

---

## 🎯 Goal

To extend the principles of relational calculus into a form that supports three (or more) dimensions natively, enabling expressive and intuitive queries over structured cube data. (For now we're limiting ourselves to 3 dimensions because anything higher gives me headaches.)

This document introduces **Q³ Calculus**, a dimensional extension of relational calculus.

---

## 🧠 Background: Classical Relational Calculus

Relational calculus traditionally exists in two forms:

* **Tuple Relational Calculus (TRC)**: Describes what data to retrieve based on conditions over tuples.

Syntax: `{ t | P(t) }` where `t` is a tuple and `P(t)` is a predicate.

* **Domain Relational Calculus (DRC)**: Operates over domain variables rather than tuples.

Syntax: `{ <x1, x2, ..., xn> | P(x1, x2, ..., xn) }`

Both assume a flat, tabular model of data where a relation is a **set of tuples**.

## 🧩 Limitation: The Dimensional Ceiling

**Relational calculus was never built to model structured layers, grids, or axes.** It only supports flat projections of dimensional data. To model cube-shaped data, you'd typically:

- Encode each axis as a separate column
- Duplicate context
- Aggregate manually across pseudo-dimensions

This leads to unnatural queries and brittle logic.

## 🔮 Introducing Q³ Calculus

Let us define a cube-native formalism:

### Core Concepts

- **Qube (𝒬)**: A first-class multidimensional data object

- **Slice**: A subset of a Qube along one or more axes

- **Axis-aware predicates**: Conditions aware of axis context

- **Cube Comprehension**: An expression that returns a cube slice matching specific structural and value constraints

### Syntax Sketch

`{ q | q ∈ 𝒬 ∧ q[axis1] ∈ A ∧ q[axis2] = v }`

Where:

- `𝒬` is a cube
- `q` is a cube cell reference
- `axis1`, `axis2` are axis selectors
- `A` is a domain or subdomain
- `v` is a scalar value

You could read the above as:

> "Return all cells q in cube 𝒬 where axis1 is in set A and axis2 equals v."

## 📐 Operators

| Operator | Meaning |
| --- | --- |
| SLICE | Subset based on axis constraints |
| ALIGN | Reshape to align axes |
| CONTRACT | Reduce along one or more axes |
| TRANSPOSE | Permute axes |
| REMAP | Rename/rekey axes or values |

These mirror and extend relational operations like `SELECT`, `PROJECT`, and `JOIN`, but operate natively in cube space.

### Operator Index

SLICE: Filter cube cells based on values along specified axes.

ALIGN: Conform axis order and cardinality across cubes.

CONTRACT: Reduce cube by collapsing one or more axes using an aggregation function (e.g. sum, mean).

TRANSPOSE: Rearrange the order of axes via permutation.

REMAP: Change axis names or values (similar to projection or renaming).

## ✳️ Example Expression

Let cube `𝒬` contain marketing data along axes: `campaign`, `device_type`, and `date`.

`{ q | q ∈ 𝒬 ∧ q["campaign"] ∈ {"CAMP001", "CAMP002"} ∧ q["device_type"] = "mobile" ∧ q["date"] ∈ May2025 }`

So, how do we prove all this? Well, I'm glad you asked.

