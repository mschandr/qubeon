# Q³ Operators Index

<!-- NAVIGATION -->
**Qubeon³ Documentation**  

[⬅️ Back to README](../../README.md) | [📘 Manifesto](../MANIFESTO.md) | [📐 Calculus](CALCULUS.md) | [🔁 Transposition](TRANSPOSITION.md) | [⚙️ Operators Index](OPERATORS_INDEX.md)

> "Operators are the verbs of the Q³ language — slicing, reshaping, and reorienting the data cube."

---

## 📦 Overview

Q³ Calculus introduces a set of native operations that act on structured cubes in multidimensional space. These operators extend relational logic into volumetric data manipulation.

Each operator modifies or filters cube data along one or more axes while preserving or transforming its structure.

---

## 🧮 Operator Definitions

| Operator   | Description |
|------------|-------------|
| **SLICE**    | Filters cube cells based on axis-aligned constraints. |
| **ALIGN**    | Conforms axes between cubes to ensure structural compatibility. |
| **CONTRACT** | Reduces one or more axes using an aggregation function. |
| **TRANSPOSE**| Rearranges the order of axes via a permutation. |
| **REMAP**    | Renames axes or recodes values within axes. |

---

## 🔍 Detailed Descriptions

### **SLICE**
Extracts a subcube from a larger cube based on axis-aligned constraints.

**Example:**

$`{ q | q ∈ 𝒬 ∧ q["region"] = "NA" ∧ q["year"] ∈ {2023, 2024} }`$

This is analogous to a filtered `SELECT` in SQL.

---

### **ALIGN**
Standardizes axis order and labels across multiple cubes for joint operations.

Use case: before joining or comparing cubes with mismatched axis orders or cardinalities.

**Example:**

$`ALIGN(𝒬1, 𝒬2) → both share axes ["date", "product", "region"]`$

---

### **CONTRACT**
Collapses one or more axes by applying a reduction function such as sum, mean, max.

**Example:**

$`CONTRACT(𝒬, axis="region", agg=SUM)`$

Equivalent to `GROUP BY` + aggregation in SQL but natively over cube structure.

---

### **TRANSPOSE**
Permutes the order of axes.

**Example:**

$`TRANSPOSE(𝒬, (2, 0, 1)) → rearranges [x, y, z] → [z, x, y]`$

Triple cyclic transposition restores original form (see [Transposition](TRANSPOSITION.md)).

---

### **REMAP**
Renames axis labels or recodes values.

**Example:**

$`REMAP(𝒬, axis="region", mapping={"NA": "North America", "EU": "Europe"})`$

Useful for projection-like operations or harmonizing external inputs.

---

## 📚 Notes

- All operators are **pure** and **composable**
- Intermediate representations are cubes unless reduced to scalar values
- Most operators are closed over cube space (cube in → cube out)

---

<!-- NAVIGATION -->
**Qubeon³ Documentation**  

[⬅️ Back to README](../../README.md) | [📘 Manifesto](../MANIFESTO.md) | [📐 Calculus](CALCULUS.md) | [🔁 Transposition](TRANSPOSITION.md) | [⚙️ Operators Index](OPERATORS_INDEX.md)
