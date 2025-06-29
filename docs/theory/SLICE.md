# SLICE Identity in Q³ Calculus


<!-- NAVIGATION -->
**Qubeon³ Documentation**\
[⬅️ Back to README](../../README.md) | [📘 Manifesto](../MANIFESTO.md) | [📐 Calculus](CALCULUS.md) | [✂️ Slice](SLICE.md) | [📤 Project](PROJECT.md) | [🔽 Contract](CONTRACT.md) | [🔁 Transposition](TRANSPOSITION.md) | [🧩 Align](ALIGN.md) | [🗺️ Operators Index](OPERATORS_INDEX.md)


> "To slice a cube is to interrogate its shape with intention."

---

## 🧮 Proposition

For any cube 𝒬 and any axis-based predicate `P(q)`, the result of `SLICE(𝒬, P)` is a valid subcube:

$`SLICE(𝒬, P) := { q ∈ 𝒬 | P(q) }`$

This subcube $`𝒮`$ is guaranteed to:

- Contain only tuples from $`𝒬`$
- Preserve axis structure and typing

---

## 🧠 Why It Holds

Cubes in Q³ are defined as finite sets of axis-typed tuples. Since SLICE applies only axis-aware filters:

- Each $`q ∈ SLICE(𝒬, P)`$ is also in $`𝒬`$
- The tuple shape and axis schema are unchanged
- No transformation, reordering, or reduction is applied

Thus:

$`SLICE(𝒬, P) ⊆ 𝒬 ∧ is_valid_cube(SLICE(𝒬, P))`$

---

## 🧪 Code Verification

```python
import numpy as np
import pandas as pd

def slice_cube(df, predicate_fn):
    return df[predicate_fn(df)]

# Example cube (DataFrame form)
data = pd.DataFrame([
    {"product": "WidgetA", "region": "NA", "year": 2023},
    {"product": "WidgetA", "region": "EU", "year": 2024},
    {"product": "WidgetB", "region": "NA", "year": 2023},
])

# Slice by year
sliced = slice_cube(data, lambda d: d["year"] == 2024)
print(sliced)
```

---

## 🧾 Propositional Form

Let $`P(q)`$ be any axis-aware predicate. Then:

- $`SLICE(𝒬, P) := { q ∈ 𝒬 | P(q) }`$
- $`∀q ∈ SLICE(𝒬, P) ⇒ q ∈ 𝒬`$
- $`∀q ∈ SLICE(𝒬, P), q has shape and types consistent with 𝒬`$

### Identity Law:

If $`P(q) = true`$ for all $`q`$, then:

$`SLICE(𝒬, λq. true) = 𝒬`$

### Compositional Law:

$`SLICE(SLICE(𝒬, P₁), P₂) = SLICE(𝒬, λq. P₁(q) ∧ P₂(q))`$

---

## 📍 Implications

- SLICE is **non-destructive**: it does not alter cube structure
- SLICE is **closed**: result is always a valid cube
- SLICE supports **predicate chaining** and **identity preservation**
- SLICE enables precise cube targeting while remaining schema-safe

---

<!-- NAVIGATION -->
**Qubeon³ Documentation**\
[⬅️ Back to README](../../README.md) | [📘 Manifesto](../MANIFESTO.md) | [📐 Calculus](CALCULUS.md) | [✂️ Slice](SLICE.md) | [📤 Project](PROJECT.md) | [🔽 Contract](CONTRACT.md) | [🔁 Transposition](TRANSPOSITION.md) | [🧩 Align](ALIGN.md) | [🗺️ Operators Index](OPERATORS_INDEX.md)
