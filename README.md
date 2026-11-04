# Pharma Cold-Chain Supply Chain Optimization

An integrated **EOQ + Linear Programming + Monte Carlo** model for a pharmaceutical cold chain — deciding how much to order, which warehouse serves which region, and how demand uncertainty affects cost and service level over 90 days.

**Course:** Optimization and Business Decision Analysis (OBDA), PGDBA  
**Team:** Hritajit, Sandeep, Mainak, Varsha Das

## Problem
- **5 warehouses** (Mumbai, Delhi, Bengaluru, Chennai, Kolkata; 30k–50k units capacity)
- **5 products** across vaccines, biologics and oncology (₹14 influenza vaccine to ₹1,200 trastuzumab), including ultra-cold (−80 °C) mRNA vaccines
- **6 customer zones** with daily demand from 350 (North-East) to 1,400 units (West)
- Stochastic lead time N(5, 1.5) days and random temperature excursions

## Approach
| Stage | Method |
|---|---|
| Inventory policy | EOQ with cold-storage holding surcharge, safety stock and reorder point under stochastic lead time; newsvendor critical ratio for vaccines |
| Distribution | Balanced transportation LP (SciPy `linprog`, HiGHS) at ₹2.5/unit/km with category surcharges (vaccines 1.5×, biologics 1.2×) |
| Risk | Monte Carlo simulation — 500 runs × 90 days, tracking inventory, transit pipeline, excursions and 10× shortage penalty |

## Key results
- LP finds a **hub-and-spoke** network (Mumbai −80 °C hub → North; Delhi → Central; Bengaluru/Chennai → South; Kolkata → East); total transport cost ₹18.46 Cr, with influenza vaccine the largest share.
- Ultra-cold mRNA vaccine EOQ held to **2,811 units** by its storage premium; refrigerated products favour larger, less frequent orders.
- Simulated **service level > 99.8 %** for all products (target 94–98 %), though stockouts still occur in 6.0–10.4 % of runs.
- **Shortage cost dominates** total cost for high-value biologics (Adalimumab ₹2.95 L, Trastuzumab ₹4.05 L) → policy should prioritise service over cost.

## Repository
```
notebooks/cold_chain_optimization.ipynb   # full model: EOQ, LP, Monte Carlo, dashboard
results/cold_chain_optimization_results.xlsx
docs/Final_Report.docx
```

## Tech
Python 3.11 · NumPy · pandas · SciPy (HiGHS LP) · Matplotlib · Seaborn
