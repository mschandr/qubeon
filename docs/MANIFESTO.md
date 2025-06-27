# Qubeon³: The Cube Paradigm Manifesto

<!-- NAVIGATION -->
**Qubeon³ Documentation**

[⬅️ Back to README](../README.md) | [📐 Calculus](theory/CALCULUS.md) | [🔁 Transposition](theory/TRANSPOSITION.md) | [⚙️ Operators Index](theory/OPERATORS_INDEX.md)


> "Flat data is an artifact of legacy thinking." — Qubeon³

## 📦 The Problem

### The history of the problem

For 50 years, relational databases have dominated how we store and think about data. They were a brilliant solution in a world of:

- 64KB RAM
- 80x24 green terminals
- Flat files and batch jobs

The **relational model**, built on tuples and 2D tables, worked — because it had to. But it **flattened multidimensional data into rows**, and reintroducing that dimensionality required increasingly complex joins, groupings, and pivoting. Time-series queries became exercises in date arithmetic. Geospatial structures were forcibly reduced to latitude-longitude pairs. Marketing analytics became fragile as it joined networks across entity tables. Simulation data? Sliced thin and exported until all context was lost.

Please feel free to stop me if you've ever attempted something similar.

Check out the following table schemas:

```sql
CREATE TABLE campaigns (
  id VARCHAR(16) PRIMARY KEY,
  name VARCHAR(100),
  owner VARCHAR(100),
  start_date DATE,
  end_date DATE
);

-- Create ENUM type first
CREATE TYPE device_type AS ENUM ('mobile', 'desktop', 'tablet');

-- Then use it in your table
CREATE TABLE devices (
  id SERIAL PRIMARY KEY,
  device_type device_type NOT NULL
);

CREATE TABLE marketing_metrics (
  id SERIAL PRIMARY KEY,
  campaign_id VARCHAR(16) REFERENCES campaigns(id),
  device_id INT REFERENCES devices(id),
  date DATE NOT NULL,
  clicks INT NOT NULL,
  impressions INT NOT NULL
);
```

### 🎯 Here's your mission, should you choose to accept

Obtain the daily click-through rate (CTR) per campaign and device type for May 2025, specifically for campaigns 'CAMP001' and 'CAMP002'.

Here's how you can accomplish that in a 2-dimensional world:

```sql
SELECT
  c.id AS campaign_id,
  d.device_type,
  m.date,
  SUM(m.clicks) AS total_clicks,
  SUM(m.impressions) AS total_impressions,
  CASE
    WHEN SUM(m.impressions) = 0 THEN NULL
    ELSE ROUND(SUM(m.clicks)::NUMERIC / SUM(m.impressions), 4)
  END AS ctr
FROM
  marketing_metrics m
JOIN campaigns c ON m.campaign_id = c.id
JOIN devices d ON m.device_id = d.id
WHERE
  m.date BETWEEN '2025-05-01' AND '2025-05-31'
  AND c.id IN ('CAMP001', 'CAMP002')
GROUP BY
  c.id,
  d.device_type,
  m.date
ORDER BY
  c.id,
  d.device_type,
  m.date;
```

That is undoubtedly a somewhat daunting query.

## 🩵 What NoSQL Got Right… and Wrong

NoSQL emerged as a reaction to the rigidity of the relational model, and it wasn’t wrong.

Document stores, key-value pairs, and wide-column databases gave developers:

- Schema flexibility
- Developer-first ergonomics
- Scalability and replication out of the box

But NoSQL wasn’t designed for structural depth; it was intended for schema chaos.

Instead of flattening the world into rows, NoSQL blurred it into blobs:

- Nested documents became hard to query meaningfully
- Relationships became harder to enforce
- Complex data was sharded, scattered, and normalized, but not structured

### Here's the same problem as before, but in NoSQL (MongoDB)

```json
{
  "campaign_id": "CAMP001",
  "device_type": "mobile",
  "date": "2025-05-07",
  "clicks": 320,
  "impressions": 4000
}
```

You’d typically drop foreign keys entirely and store `device_type` and `campaign_id` redundantly for fast lookup.

### ❌ The Aggregation Query in MongoDB

```js
db.marketing_metrics.aggregate([
  {
    $match: {
      campaign_id: { $in: ["CAMP001", "CAMP002"] },
      date: { $gte: ISODate("2025-05-01"), $lte: ISODate("2025-05-31") }
    }
  },
  {
    $group: {
      _id: {
        campaign_id: "$campaign_id",
        device_type: "$device_type",
        date: "$date"
      },
      total_clicks: { $sum: "$clicks" },
      total_impressions: { $sum: "$impressions" }
    }
  },
  {
    $project: {
      _id: 0,
      campaign_id: "$_id.campaign_id",
      device_type: "$_id.device_type",
      date: "$_id.date",
      total_clicks: 1,
      total_impressions: 1,
      ctr: {
        $cond: {
          if: { $eq: ["$total_impressions", 0] },
          then: null,
          else: { $divide: ["$total_clicks", "$total_impressions"] }
        }
      }
    }
  },
  {
    $sort: {
      campaign_id: 1,
      device_type: 1,
      date: 1
    }
  }
]);
```

🧕 What's Wrong With This?

- Indexing is fragile unless all fields are pre-indexed (especially `campaign_id`, `date`)
- Data model must be denormalized — error-prone if devices or campaigns change
- Aggregation logic is verbose and error-prone
- No native notion of "dimensions", just field matching and grouping

NoSQL helped us store multidimensional data, but it didn't help us think in multidimensional terms. It gave us depth, but no shape.

---

### 📦 Why cubes?

Why cubes? Because the world isn’t flat, and neither is your data.

A **Rubik’s Cube** is a good real-world analogy: it has layers, sides, and positions — dimensions that relate to each other in space. You don’t just *store* the colours; you *rotate* and *observe* how positions evolve. Cubes are inherently spatial, temporal, and relational.

In data terms:

- A cube allows you to observe **changes across multiple dimensions** simultaneously.
- It enables native operations like **slicing**, **pivoting**, and **alignment** without rewriting entire queries.
- It mirrors how **we experience the real world** — across time, space, and context.

Whether it's sales across products, regions, and quarters — or sensor outputs over altitude, time, and temperature — **cubes preserve structure**, not discard it.

They aren’t a new abstraction — they’re the **original structure** that SQL and NoSQL tried to flatten.

Cubes are the natural home of multidimensional insight. Let’s give them the first-class status they’ve always deserved.

---

## 👥 Who Is This For?

- **Simulation engineers** who are modelling changing environments or physical processes
- **Marketing analysts** navigating campaign/device/time intersections
- **Data scientists** wrangling tensors and model inputs
- **Game developers** storing voxel-like world states
- **Urban planners** working with multi-layer geospatial data
- **Quantitative traders** analyzing time-windowed multidimensional market signals

---

## 🔎 What’s Missing from Today’s Systems?

| System             | What It Offers                    | What It's Missing                     |
| ------------------ | --------------------------------- | ------------------------------------- |
| Flat SQL DBs       | Proven joins, ACID compliance     | No native multi-dimensionality        |
| OLAP Cubes         | Dimensions for reporting          | Static, rigid, rarely write-optimized |
| NumPy / PyTorch    | Dynamic tensor operations         | In-memory only, not persistent        |
| NoSQL (Mongo, etc) | Schema freedom, dev velocity      | Weak relationships, blob logic        |
| Tensor DBs (niche) | ML-centric storage of tensor data | No query model, not general-purpose   |

Qubeon³ is meant to fill the gap: cube-native thinking, with storage, access, and query support to match.

---

## 🧪 What Would a Qubeon³ Query Look Like?

```cube
SELECT CTR
FROM marketing_metrics
SLICE campaign['CAMP001', 'CAMP002'], device_type['mobile', 'desktop'], date['2025-05']
```

Readable. Slicable. Aligned.

No joins. No GROUP BY headaches. Just dimensions, directly addressed.

---

## 🚀 Future Extensions: Beyond the Cube

Cubes are only the beginning.

- **4D simulations**: evolving spatial data over time
- **Volumetric video**: depth x frame x color
- **Tesseract-style ML**: multi-modal tensor-based training and evaluation
- **Quantum state encoding**: energy levels x spin x time x entanglement

Qubeon³ begins with 3D — but its core is extensibility.

---

## 🌍 You Are Here

You're reading this because you know:

- Data isn't flat
- Tables aren't enough
- And you're ready to build something that honours the **shape of knowledge**

Welcome to Qubeon³.

---

## 🛡️ What Is Qubeon³?

Qubeon³ is an open-source initiative to rethink how we store and query data — not in rows or blobs, but in native multidimensional structures. It is:

- A new data engine for cube-first storage and analysis
- A declarative query language inspired by tensor and scientific computation
- A reimagining of relational and NoSQL assumptions, built for the data modeling needs of the present and future

This project is open to all who share our vision — free to use, fork, and contribute to under a clearly defined license.

Join us in reshaping how data is shaped.


