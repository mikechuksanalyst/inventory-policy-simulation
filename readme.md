# Probabilistic Inventory System – (M, N) Policy Simulation

Monte Carlo simulation of a periodic-review inventory system to identify the optimal policy parameters M (maximum stock level) and N (review period length) under stochastic demand and lead time.

---

## Problem Statement

A retailer operates a periodic-review inventory system. Every N days the inventory is checked and an order is placed to bring stock back up to level M. Both customer demand and supplier lead time are random variables. The goal is to find the combination of M and N that minimises total inventory cost over 10 simulated cycles.

**Policies evaluated:**

| Policy | M (Max Level) | N (Review Period) |
|--------|--------------|-------------------|
| A      | 11           | 5 days            |
| B      | 11           | 6 days            |
| C      | 12           | 5 days            |
| D      | 12           | 6 days            |

---

## Demand and Lead Time Distributions

**Daily Demand**

| Demand | Probability | Cumulative |
|--------|-------------|------------|
| 0      | 0.10        | 0.10       |
| 1      | 0.25        | 0.35       |
| 2      | 0.35        | 0.70       |
| 3      | 0.21        | 0.91       |
| 4      | 0.09        | 1.00       |

**Lead Time**

| Lead Time (Days) | Probability | Cumulative |
|-----------------|-------------|------------|
| 1               | 0.60        | 0.60       |
| 2               | 0.30        | 0.90       |
| 3               | 0.10        | 1.00       |

---

## Cost Structure

| Cost Component | Rate |
|---|---|
| Holding cost | $2.00 per unit per day |
| Shortage cost | $1.50 per unit per day |
| Purchase cost | $50.00 per unit ordered |
| Ordering cost | $10.00 per order placed |

**Initial conditions:** 3 units on-hand, 8 units arriving in 2 days.

---

## Results

| Policy | M | N | Avg Cost / Cycle | Rank |
|--------|---|---|-----------------|------|
| **A**  | **11** | **5** | **$433.75** | **1 – OPTIMAL** |
| C      | 12 | 5 | $464.15         | 2    |
| B      | 11 | 6 | $516.85         | 3    |
| D      | 12 | 6 | $547.45         | 4    |

**Policy A (M=11, N=5) is the optimal policy.**

---

## Conclusion

Policy A achieves the lowest average cost per cycle ($433.75) across 10 Monte Carlo simulated cycles. The shorter review period (N=5 days rather than 6) replenishes stock more frequently, reducing the exposure to stockouts during demand peaks and lead-time variability. The lower maximum level (M=11 rather than M=12) avoids the holding cost penalty of carrying excess inventory. Together, these parameters produce a lean and responsive replenishment rhythm that outperforms all other combinations under this demand and lead-time profile.

Policy D (M=12, N=6) performs worst at $547.45 per cycle. Holding more stock for longer periods without proportional service-level improvement makes it the most expensive configuration -- the additional holding cost is not justified by reduced shortage events.

The margin between the best and worst policy is roughly $114 per cycle. Over a full year of 52 review cycles (assuming N=5), this compounds to approximately $5,900 in avoidable cost annually.

---

## Methodology

**Monte Carlo Simulation via Random Number Mapping**

Random numbers uniformly distributed on [0, 1] are generated and mapped to demand and lead-time values using the cumulative probability distributions above. For each day:

1. A random number maps to a daily demand value
2. If an order is placed (end of every N-day cycle), a second random number maps to lead time
3. The order arrives on the day calculated as: current day + lead time
4. End-of-day inventory = beginning inventory - demand (floored at 0)
5. Shortage = max(0, demand - beginning inventory)
6. Daily cost = holding cost + shortage cost + purchase cost (on order days) + ordering cost (on order days)

The same sequence of random numbers is used across all four policies so the comparison is fair -- any cost difference reflects the policy parameters, not random variation.

---

## Workbook Structure

| Sheet | Contents |
|---|---|
| Cover | Problem summary, parameter overview, optimal policy callout |
| Assumptions | All input parameters with colour-coded convention, demand and lead-time tables with RN mapping ranges |
| Policy A | Full 50-day simulation table with cycle cost breakdown |
| Policy B | Full 60-day simulation table (10 cycles × N=6) |
| Policy C | Full 50-day simulation table |
| Policy D | Full 60-day simulation table |
| Summary & Chart | Cost comparison table, cycle-by-cycle line chart, average cost bar chart, conclusion |

**Colour convention (industry standard):**
- Blue text: hardcoded input values (change to run scenarios)
- Black text: formulas and calculated outputs

---

## Files

```
├── simulation_project_enhanced.xlsx   # Full simulation workbook
└── README.md                          # This file
```

---

## Tools Used

- Microsoft Excel (Monte Carlo simulation, charting)
- openpyxl (Python) for workbook construction and formatting

---

## References

- Law, A.M. (2015). *Simulation Modeling and Analysis*. 5th ed. McGraw-Hill.
- Hillier, F.S. and Lieberman, G.J. (2015). *Introduction to Operations Research*. 10th ed. McGraw-Hill. Chapter 18: Inventory Theory.
