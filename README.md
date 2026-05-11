# Business-Analytics
# Demand Forecasting & Monte Carlo Simulation for Inventory and Profit Planning

> An Excel-based Operations Analytics case study combining **time series forecasting**, **safety stock optimisation**, and **Monte Carlo risk simulation** to support data-driven inventory and profitability decisions for a T-Shirt manufacturer.

![Tool](https://img.shields.io/badge/Tool-Microsoft%20Excel-217346?logo=microsoftexcel&logoColor=white)
![Domain](https://img.shields.io/badge/Domain-Operations%20Analytics-blue)
![Methods](https://img.shields.io/badge/Methods-Forecasting%20%7C%20Monte%20Carlo-orange)
![Course](https://img.shields.io/badge/Course-7036SSL%20MSc%20Business%20Analytics-lightgrey)

---

## 1. Business Context

A consumer apparel company needs to plan inventory and profitability for **2025–2026** (an 8-quarter horizon). Historical quarterly demand from 2018–2024 shows a downturn (2018–2020), stabilisation, and recovery from 2023 onward, with strong seasonal patterns.

The business problem is twofold:

- **How much should we order, and when?** — to avoid stock-outs while controlling holding costs.
- **What profit can we realistically expect, and how risky is it?** — given uncertainty in both total market demand and the company's market share.

This project answers both questions using a transparent, spreadsheet-based analytical workflow that operations managers can audit and extend.

---

## 2. Project Objectives

1. Explore and **decompose** the historical demand series into trend, seasonality, and noise.
2. Build and benchmark **three forecasting models** (Moving Average, Simple Exponential Smoothing, Linear Exponential Smoothing) and select the best performer.
3. Translate the chosen forecast into an **operational policy**: safety stock and reorder point at a 95% service level.
4. Quantify uncertainty through a **Monte Carlo simulation** (250 trials) of lead-time demand and profit, then assess downside risk.

---

## 3. Repository Structure

| File | Description |
|------|-------------|
| `Darshil_Zadaphiya_CW2_Data.xlsx` | Full Excel workbook containing data, models, charts, and simulation. |
| `Darshil_Zadaphiya_Operations_CW2.docx` | Final written report with methodology, findings, and recommendations. |
| `README.md` | This file. |

### Workbook Contents

| Sheet | Purpose |
|-------|---------|
| `Time Series Plot` | Raw quarterly demand 2018–2024 and visual exploration. |
| `A.1 Time Series Exploration` | Additive decomposition: trend, seasonal indices, and noise. |
| `A.2 & A.3 Time Series Models` | MAV, SES, and LES models with train/test split and error metrics. |
| `A.4 Safety Stock` | Lead-time demand, RMSE-based standard deviation, safety stock & ROP calculation. |
| `B.1 & B.2 Simulation` | Monte Carlo simulation (250 iterations) for lead-time demand and profit. |

---

## 4. Methodology

### Part A — Forecasting

**Decomposition** (additive model: `Y = Trend + Seasonality + Noise`)
- Quarters 2 and 3 consistently exceed trend; Quarters 1 and 4 fall below.
- Noise contained within roughly ±15 units, indicating a strong model fit.
- Reconstructed series closely tracks the original, validating the decomposition.

**Train/Test Split**
- Training: 2018–2022 (20 quarterly observations)
- Test: 2023–2024 (8 observations, matching the forecast horizon)

**Models Compared**

| Model | Parameters | MAD | RMSE | MAPE |
|-------|-----------|-----|------|------|
| Moving Average (4-period) | n = 4 | 72 | 78 | 30% |
| Simple Exponential Smoothing | α = 0.6 | 64 | 71 | 27% |
| **Linear Exponential Smoothing** | **α = 0.5, β = 0.8** | **37** | **43** | **15%** |

**LES** is the chosen model — it captures both level and trend, with seasonality already handled through decomposition.

### Inventory Policy — Safety Stock & Reorder Point

Using a **95% service level** (Z = 1.645) and a lead time of 8 quarters:

```
Lead-time demand (μ_LTD) ≈ 1,901 thousand units
σ_LTD = √(lead time) × RMSE = √8 × 43 ≈ 121.7 thousand units
Safety Stock = Z × σ_LTD ≈ 1.645 × 121.7 ≈ 200 thousand units
Reorder Point (ROP) = μ_LTD + Safety Stock ≈ 2,101 thousand units
```

### Part B — Monte Carlo Simulation

A **250-iteration Monte Carlo simulation** models two sources of uncertainty:

1. **Total lead-time demand** (variability around the LES forecast).
2. **Company market share**, drawn from a discrete probability distribution:

| Share | 20% | 30% | 40% | 50% | 60% | 70% |
|-------|-----|-----|-----|-----|-----|-----|
| Probability | 0.05 | 0.10 | 0.15 | 0.20 | 0.25 | 0.25 |

For each trial:
```
Company Demand = Simulated Share × Simulated Total Demand
Profit = Company Demand × (Selling Price − Unit Cost) = Demand × (£8 − £4)
```

---

## 5. Key Findings

### Operational
- **Reorder Point: ~2.1 million units** of T-shirts (at 95% service level over the 8-quarter lead time).
- **Recommended safety stock: ~200,000 units**, balancing stock-out risk against holding cost.

### Financial (from 250-trial simulation)
- **Expected company demand:** ~994,500 units over the lead time.
- **Expected profit:** **£3.98 million** (average across all trials).
- **Profit range:** £1.43M (worst case) to £6.21M (best case).
- **75% of trials** fall between **£3.6M and £5.5M** — a reliable mid-range for planning.
- **Only 0.4%** of trials fall below £2M, indicating limited downside risk.
- The profit distribution is **positively skewed**, with the modal range £4.2M–£4.9M.

---

## 6. Business Recommendations

1. **Adopt LES-based forecasts** for the 2025–2026 planning cycle — it materially outperforms simpler methods (MAPE 15% vs 27–30%).
2. **Set the reorder trigger at ~2.1M units** to maintain a 95% service level over the lead-time horizon.
3. **Plan around an expected ~£4M profit baseline**, with a realistic working range of £3.6M–£5.5M; treat outcomes above £6M as upside scenarios, not the plan.
4. **Re-run the simulation quarterly** as new demand data arrives — the workbook is structured to accept rolling updates without restructuring.

---

## 7. Tools & Techniques

| Category | Techniques Used |
|----------|----------------|
| Data Exploration | Quarterly time series plots, trendline fitting (polynomial) |
| Decomposition | Classical additive decomposition (trend / seasonality / noise) |
| Forecasting | Moving Average (MAV), Simple Exponential Smoothing (SES), Linear Exponential Smoothing (LES) |
| Accuracy Metrics | MAD, RMSE, MAPE |
| Inventory Theory | Safety Stock, Reorder Point (ROP), Service Level (Z-score) |
| Simulation | Monte Carlo (250 iterations), discrete probability sampling via `LOOKUP(RAND(), ...)` |
| Risk Analysis | Profit distribution, percentile analysis, downside-risk quantification |

---

## 8. Skills Demonstrated

- Translating raw business data into a structured analytical workflow.
- Applying classical operations-analytics techniques in a reproducible spreadsheet model.
- Choosing models on the basis of **out-of-sample** error metrics rather than in-sample fit.
- Linking statistical forecasts to **operational levers** (safety stock, ROP) and **financial outcomes** (profit risk profile).
- Communicating quantitative findings clearly for both technical and business audiences.

---

## 9. How to Use This Repository

1. Download `Darshil_Zadaphiya_CW2_Data.xlsx` and open it in Microsoft Excel (2016 or later recommended).
2. Read the report (`Darshil_Zadaphiya_Operations_CW2.docx`) for the full narrative and reasoning.
3. Navigate sheets in order: `Time Series Plot` → `A.1` → `A.2 & A.3` → `A.4` → `B.1 & B.2`.
4. To re-run the Monte Carlo simulation, press **F9** to recalculate the workbook — the `RAND()` draws will refresh and the simulation summary will update.

---

## 10. Author

**Darshil Zadaphiya**
MSc Business Analytics — Coventry University
Student ID: 15692900
Module: **7036SSL — Operations Analytics**

---

## 11. Acknowledgements

Excel templates and tutorials adapted from materials by **Shazib Shaikh (2025)** for the 7036SSL Operations Analytics module at Coventry University:
- *Time Series Decomposition — an Excel-based Tutorial*
- *Forecasting Methods — an Excel-based Tutorial*
- *Safety Stock Forecasting — an Excel-based Tutorial*
- *Monte Carlo Simulation — an Excel-based Tutorial*

---

> *This project was completed as academic coursework. All figures and recommendations are based on the dataset and assumptions provided in the module brief.*
