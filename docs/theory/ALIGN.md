# 🧩 ALIGN Identity in Q³ Calculus

**Qubeon³ Documentation**\
[⬅️ Back to README](../../README.md) | [📘 Manifesto](../MANIFESTO.md) | [📐 Calculus](CALCULUS.md) | [✂️ Slice](SLICE.md) | [📤 Project](PROJECT.md) | [🔽 Contract](CONTRACT.md) | [🔁 Transposition](TRANSPOSITION.md) | [🧩 Align](ALIGN.md) | [🗺️ Operators Index](OPERATORS_INDEX.md)

> "Two cubes must first agree before they can speak the same language."

---

## 📊 Proposition

Given two cubes $`ℚ`$ and $`ℝ`$, `ALIGN($`ℚ`$, $`ℝ`$)` transforms both such that:

- Axes match in name, order, and domain
- Missing domains are filled with neutral elements (e.g., NaN, zero)
- The resulting pair is **join-compatible**

Mathematically:

$`ALIGN(ℚ, ℝ) := (ℚ′, ℝ′) ⇒ Axes(ℚ′) = Axes(ℝ′) ∧ Domains match`$

---

## 🧠 Why It Holds

Cubes are structured by axis sets $`α, β`$. When combining $`ℚ`$ and $`ℝ`$:

- If Axes($`ℚ`$) $`≠`$ Axes($`ℝ`$), no natural join is possible
- Alignment introduces **missing axes** with neutral filler values
- All axes are ordered identically post-alignment

Example:

$`ℚ`$: axes = (product, year)
$`ℝ`$: axes = (region, product)
ALIGN($`ℚ, ℝ`$) ⇒ (product, region, year)
```

Each cube is expanded with default values to enable safe composition.

---

## 🔪 Code Verification

```python
import pandas as pd
from itertools import product

# Define neutral filler
def align_cubes(df1, df2, fill_value=None):
    all_axes = sorted(set(df1.columns) | set(df2.columns))
    df1_aligned = df1.reindex(columns=all_axes, fill_value=fill_value)
    df2_aligned = df2.reindex(columns=all_axes, fill_value=fill_value)
    return df1_aligned, df2_aligned

# Example usage
df1 = pd.DataFrame([{"product": "A", "year": 2023}])
df2 = pd.DataFrame([{"region": "NA", "product": "A"}])
df1a, df2a = align_cubes(df1, df2)
print(df1a)
print(df2a)
```

---

## 📐 Propositional Form

Let ℚ and ℝ be cubes with arbitrary axis sets. Then:

```
ALIGN(ℚ, ℝ) = (ℚ′, ℝ′) such that:
Axes(ℚ′) = Axes(ℝ′) = Axes(ℚ) ∪ Axes(ℝ)
Domains match, default fill values apply
```

### Identity Law:

```
ALIGN(ℚ, ℚ) = (ℚ, ℚ)
```

### Symmetry Law:

```
ALIGN(ℚ, ℝ) = ALIGN(ℝ, ℚ)
```

---

## 📍 Implications

- Alignment is required for UNION, DIFF, and binary JOIN
- Enables multi-cube computation
- Non-destructive: original data is not changed
- Canonicalizes axis order for cube algebra

---

**Qubeon³ Documentation**\
[⬅️ Back to README](../../README.md) | [📘 Manifesto](../MANIFESTO.md) | [📐 Calculus](CALCULUS.md) | [✂️ Slice](SLICE.md) | [📤 Project](PROJECT.md) | [🔽 Contract](CONTRACT.md) | [🔁 Transposition](TRANSPOSITION.md) | [🧩 Align](ALIGN.md) | [🗺️ Operators Index](OPERATORS_INDEX.md)

