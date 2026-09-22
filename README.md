# Hotel Bar Inventory Forecasting & Par Level Recommendation

A time-series forecasting and inventory policy project for hotel bar operations. The project converts transaction-level bottle records into daily Bar × Brand demand, evaluates forecasting approaches for intermittent demand, calculates dynamic Par Levels and safety stock, and backtests the resulting inventory policy.

## Project objective

The goal is to support daily inventory decisions by balancing two practical problems:

- stockouts of products that customers are likely to request
- unnecessary inventory tied up in slower-moving products

The analysis covers **96 Bar × Brand series** from **1 January 2023 to 1 January 2024**.

## Approach

1. **Data cleaning and aggregation**
   - Parse transaction timestamps.
   - Verify `Closing Balance = Opening Balance + Purchase - Consumed`.
   - Aggregate consumption to daily Bar × Brand level.
   - Create a complete daily grid so zero-consumption days are represented explicitly.

2. **Exploratory analysis**
   - ABC / velocity analysis.
   - Day-of-week demand analysis.
   - Historical stockout audit.
   - Overall demand trend analysis.

3. **Demand forecasting**
   - Use chronological walk-forward validation rather than randomized cross-validation.
   - Test rolling-average, seasonal/intermittent-demand, Random Forest, and LightGBM approaches during development.
   - Final model: **Same Weekday Mean 8W**.
   - Final test period: **20 October 2023 to 1 January 2024**.

4. **Inventory policy**
   - Lead time: **2 days**.
   - Service level: **95%** (`Z = 1.645`).
   - `Par Level = Lead-Time Demand + Safety Stock`.

5. **Inventory simulation**
   - Simulate daily demand fulfillment and replenishment across all 96 series.
   - Track stockouts, lost demand, inventory, order events, and turnover.

## Final results

### Forecasting

| Metric | Result |
|---|---:|
| Selected model | Same Weekday Mean 8W |
| Final-test MAE | **59.14 ml** |
| Final-test WAPE | **106.16%** |

The selected model was also compared with a 14-day rolling-mean baseline on the locked final test period. The same-weekday method produced lower MAE and WAPE in that comparison.

### Inventory policy

| Metric | Result |
|---|---:|
| Lead time | **2 days** |
| Service level assumption | **95%** |
| Average Par Level | **340.29 ml** |
| Maximum Par Level | **949.70 ml** |

### Inventory simulation

| KPI | Result |
|---|---:|
| Total demand | **395,762.81 ml** |
| Fulfilled demand | **287,291.29 ml** |
| Lost volume | **108,471.52 ml** |
| Simulated service level | **72.59%** |
| Stockout series-days | **586** |
| Stockout calendar days | **72** |
| Average total inventory | **27,965.57 ml** |
| Order events | **1,163** |
| Ordered volume | **276,885.94 ml** |
| Inventory turnover | **14.15** |

## Important limitation

Demand is highly intermittent. The final forecast therefore still has difficulty identifying every positive-demand day, and the simulated service level remains below the 95% policy assumption. The current results should be treated as a baseline inventory policy rather than a claim of production-ready accuracy.

Potential future improvements include promotion and holiday signals, event calendars, real-time POS data, supplier lead-time variability, product substitution, and more specialized intermittent-demand models.

## Repository contents

```text
hotel-bar-inventory-forecasting/
├── data/
│   ├── raw/
│   │   └── Consumption Dataset.xlsx
│   └── processed/
│       └── daily_bar_consumption.csv
├── notebooks/
│   └── inventory_forecasting_solution.ipynb
├── report/
│   ├── business_report.pdf
│   └── business_report.tex
├── video_script/
│   └── video_walkthrough.md
├── README.md
├── requirements.txt
└── .gitignore
```

## Running the notebook

Create a virtual environment, install the dependencies, and open the notebook in Jupyter or VS Code:

```bash
python -m venv .venv
```

Windows:

```bash
.venv\Scripts\activate
```

macOS / Linux:

```bash
source .venv/bin/activate
```

Install dependencies:

```bash
pip install -r requirements.txt
```

Open:

```text
notebooks/inventory_forecasting_solution.ipynb
```

The notebook reads the authoritative dataset from:

```text
data/raw/Consumption Dataset.xlsx
```

## Deliverables

- `notebooks/inventory_forecasting_solution.ipynb` — complete analysis and simulation
- `report/business_report.pdf` — 1–2 page business report
- `video_script/video_walkthrough.md` — explanatory walkthrough for the 3–5 minute project video
