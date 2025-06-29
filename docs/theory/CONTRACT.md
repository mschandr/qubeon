# CONTRACT Identity in Q³ Calculus

<!-- NAVIGATION -->
**Qubeon³ Documentation**\
[⬅️ Back to README](../../README.md) | [📘 Manifesto](../MANIFESTO.md) | [📐 Calculus](CALCULUS.md) | [✂️ Slice](SLICE.md) | [📤 Project](PROJECT.md) | [🔽 Contract](CONTRACT.md) | [🔁 Transposition](TRANSPOSITION.md) | [🧩 Align](ALIGN.md) | [🗺️ Operators Index](OPERATORS_INDEX.md)


> "To contract a cube is to speak in summaries — not sentences."

---

## 🧼 Proposition

For any cube 𝕋 and aggregation function `f`, contracting along axis `a` yields a new cube where axis `a` is removed and values are aggregated over its domain:

```
CONTRACT(𝕋, a, f) := aggregate_over(𝕋 \ a, f)
```

The resulting cube:

- Removes axis `a`
- Aggregates values for remaining axis combinations

---

## 🧠 Why It Holds

Cubes are finite collections of tuples with axis-aligned values. Aggregation across an axis partitions the cube by the remaining axes, then applies `f` to each partition:

- Group by all axes except `a`
- For each group, compute `f(values along a)`

Since:

- Grouping respects axis types
- `f` operates on finite numeric or categorical sets

the result is deterministic and valid.

```
CONTRACT(𝕋, a, f) has shape Axes(𝕋) - {a}
```

---

## 🧪 Code Verification

```python
import pandas as pd

# Example cube (DataFrame form)
data = pd.DataFrame([
    {"product": "WidgetA", "region": "NA", "year": 2023, "sales": 100},
    {"product": "WidgetA", "region": "NA", "year": 2024, "sales": 150},
    {"product": "WidgetA", "region": "EU", "year": 2023, "sales": 90},
])

# Contract along "year" using sum
contracted = data.groupby(["product", "region"], as_index=False)["sales"].sum()
print(contracted)
```

---

## 🧾 Propositional Form

Let `a` be the axis to contract and `f` the aggregation function:

- $`CONTRACT(𝕋, a, f) := { (q \ a, f(values for q \ a)) }`$
- $`∀q' ∈ CONTRACT(𝕋, a, f) ⇒ axes(q') = axes(q) - {a}`$

### Identity Law (Aggregation Neutrality):

If `f` is identity (e.g. `lambda x: x[0]`) then:

```
CONTRACT(𝕋, a, f) = PROJECT(𝕋, axes(𝕋) - {a})
```

### Aggregation Law:

If `f` is associative (e.g. sum, count, max):

```
CONTRACT(CONTRACT(𝕋, a1, f), a2, f) = CONTRACT(𝕋, {a1, a2}, f)
```

---

## 📍 Implications

- CONTRACT reduces **dimensional scope** and performs **value summarization**
- Enables pivoting from data to statistics (e.g. sums, averages)
- Preserves schema integrity except for contracted axis
- Compatible with SLICE and PROJECT to target subcubes

---

<!-- NAVIGATION -->
**Qubeon³ Documentation**\
[⬅️ Back to README](../../README.md) | [📘 Manifesto](../MANIFESTO.md) | [📐 Calculus](CALCULUS.md) | [✂️ Slice](SLICE.md) | [📤 Project](PROJECT.md) | [🔽 Contract](CONTRACT.md) | [🔁 Transposition](TRANSPOSITION.md) | [🧩 Align](ALIGN.md) | [🗺️ Operators Index](OPERATORS_INDEX.md)

