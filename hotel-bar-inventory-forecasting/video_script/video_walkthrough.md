# Project Video Walkthrough

**Target duration:** 4.5–5 minutes

This document is an explanatory guide for the project video. It is intentionally written as presentation content rather than a word-for-word script.

## 1. Opening and business context — ~30 seconds

**Notebook to show:** `Step 1 — Data Cleaning`

The project addresses a practical hotel-bar inventory problem: keeping enough stock available to meet customer demand without holding unnecessary inventory. Stockouts can create unmet demand, while excess stock ties up cash and storage space.

The analysis starts with transaction-level bottle records containing timestamps, opening balance, purchases, consumption, and closing balance. The first stage checks inventory consistency and converts the transactions into a daily demand view for each Bar × Brand combination.

## 2. Data preparation and EDA — ~45 seconds

**Notebook to show:**
- `Verify inventory conservation`
- `Create the complete daily grid`
- `Assign ABC classes`
- `Plot day-of-week demand`
- `Stockout candidates by brand`

The complete daily grid is important because many products are not consumed every day. Zero-consumption days are therefore kept explicitly instead of being treated as missing observations.

The exploratory analysis then looks at product velocity, weekday demand patterns, and historical stockout candidates. These checks provide context for the forecasting problem and show why intermittent demand is important for this dataset.

## 3. Forecasting approach — ~1 minute 40 seconds

**Notebook to show:**
- `Step 3 — Demand Forecasting`
- `Step 3.1 — Clean Walk-Forward Validation`
- `Step 3.2 — Intermittent Demand Forecasting Experiment`

Forecasting is evaluated chronologically. The model learns from earlier dates and is evaluated on later dates, so information from the future is not used during model selection.

Several approaches are tested, including rolling baselines, machine-learning models, and methods designed for intermittent demand.

A key finding is that a method can show a lower error simply by predicting zero too often. Because of this, forecast activity is checked alongside MAE and WAPE rather than relying on one number alone.

## 4. Final model — ~40 seconds

**Notebook to show:**
- `Step 3.3 — Final Model Selection`
- `Step 3.4 — Final Test Performance Comparison`
- `Step 3.5 — Development Blend Experiment`
- `Step 3.6 — Final Model Lock`

The final selected method is **Same Weekday Mean 8W**. For a given date, the method uses recent observations from the same weekday for the same Bar × Brand series.

The final test period is locked to **20 October 2023 through 1 January 2024**. The model achieved **59.14 ml MAE** and **106.16% WAPE** on this period.

The final comparison also shows that the selected method produced lower MAE and WAPE than the 14-day rolling-mean baseline on the locked test set.

## 5. From forecast to inventory policy — ~45 seconds

**Notebook to show:**
- `Calculate the dynamic policy`
- `Policy summary`
- `Visualize the dynamic policy`

The forecast is converted into an inventory target using a two-day supplier lead time and a 95% service-level assumption.

The Par Level combines expected demand during the lead time with a safety-stock buffer for demand variation.

Across the 96 Bar × Brand series, the average Par Level is **340.29 ml**, with a maximum of **949.70 ml**.

In practice, this provides a daily target that can adapt to the expected demand and historical variability of each series.

## 6. Inventory simulation and results — ~50 seconds

**Notebook to show:**
- `Simulation engine`
- `Run the simulation for all 96 series`
- `Overall business KPIs`
- `Plot inventory behavior`

The simulation works like a simplified daily replenishment process. It receives pending orders after the two-day lead time, fulfills actual demand when inventory is available, records lost demand during stockouts, and places replenishment orders to restore inventory toward the Par Level.

Across the final test period, total demand was **395,762.81 ml** and fulfilled demand was **287,291.29 ml**. The simulation recorded **108,471.52 ml** of lost volume and a **72.59% simulated service level**.

There were **586 stockout series-days**, **1,163 order events**, and average total inventory of about **27,965.57 ml**. Inventory turnover was **14.15**.

## 7. Closing — ~25 seconds

**Notebook to show:** `Step 6 — Final Results & Business Summary`

The project provides a complete workflow from raw inventory transactions to demand forecasting, dynamic Par Levels, and historical inventory simulation.

The main limitation is intermittent demand: predicting exactly when a positive-consumption day will occur is still difficult. A stronger production version could add promotions, holidays, events, real-time POS signals, supplier lead-time variation, and more specialized intermittent-demand methods.

The current notebook should therefore be presented as a practical, validated baseline for inventory planning with clear opportunities for future improvement.
