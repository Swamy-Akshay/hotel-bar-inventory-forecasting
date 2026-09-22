Yes. The current version still sounds like something being **read aloud**. For the repo, it should read more like a **project explanation/documentation** that you can use as a reference while recording.

Use this version instead:

# Project Video Walkthrough

**Target duration:** 4.5–5 minutes

This document explains the main parts of the project and the points to cover during the video. The focus is on the business problem, the overall approach, and the final results rather than explaining individual lines of code.

## 1. Business Problem

**Notebook section:** `Step 1 — Data Cleaning`

The project focuses on inventory management for hotel bars. The main goal is to keep enough stock to meet demand while avoiding unnecessary inventory.

The dataset contains transaction-level records for purchases, consumption, and bottle balances. The data is first checked for consistency and then converted into daily consumption for each Bar × Brand combination.

The final dataset contains **96 Bar × Brand series**.

## 2. Data Preparation and Analysis

**Notebook sections:**
`Verify inventory conservation`
`Create the complete daily grid`
`Assign ABC classes`
`Plot day-of-week demand`
`Stockout candidates by brand`

Daily aggregation makes the data easier to analyze and use for forecasting.

Zero-consumption days are also included because many brands are not used every day. Keeping these days is important for understanding the actual demand pattern.

The analysis then covers product velocity, weekday demand, and historical stockout patterns. These results help explain the nature of the inventory problem before forecasting is applied.

## 3. Demand Forecasting

**Notebook sections:**
`Step 3 — Demand Forecasting`
`Step 3.1 — Clean Walk-Forward Validation`
`Step 3.2 — Intermittent Demand Forecasting Experiment`

The forecasting process uses chronological validation. Earlier data is used to make forecasts for later periods, which avoids using future information during model selection.

Different approaches are tested, including rolling averages, machine-learning models, and intermittent-demand methods.

An important part of the evaluation is checking both forecast error and forecast behavior. This is useful because a model that predicts zero very often can sometimes show lower error without being useful for inventory planning.

## 4. Final Forecasting Model

**Notebook sections:**
`Step 3.3 — Final Model Selection`
`Step 3.4 — Final Test Performance Comparison`
`Step 3.5 — Development Blend Experiment`
`Step 3.6 — Final Model Lock`

The final model is **Same Weekday Mean 8W**.

For each Bar × Brand combination, the forecast uses recent demand from the same weekday over the previous eight weeks. This helps account for the weekly pattern in bar demand.

The final test period is **20 October 2023 to 1 January 2024**.

Final test performance:

| Metric |       Result |
| ------ | -----------: |
| MAE    | **59.14 ml** |
| WAPE   |  **106.16%** |

The model also performed better than the 14-day rolling-mean baseline on the locked test period.

## 5. Inventory Policy

**Notebook sections:**
`Calculate the dynamic policy`
`Policy summary`
`Visualize the dynamic policy`

The demand forecast is then used to set inventory targets.

The policy assumes a **2-day supplier lead time** and a **95% service level**. The Par Level combines expected demand during the lead time with a safety-stock buffer based on demand variation.

Results across the 96 series:

| Metric            |        Result |
| ----------------- | ------------: |
| Average Par Level | **340.29 ml** |
| Maximum Par Level | **949.70 ml** |

This creates a separate inventory target for each Bar × Brand combination.

## 6. Inventory Simulation

**Notebook sections:**
`Simulation engine`
`Run the simulation for all 96 series`
`Overall business KPIs`
`Plot inventory behavior`

The simulation represents the daily replenishment process.

It tracks incoming orders, available inventory, fulfilled demand, lost demand, ending inventory, and new replenishment orders. Orders are received after the assumed two-day lead time.

Final simulation results:

| KPI                  |            Result |
| -------------------- | ----------------: |
| Total demand         | **395,762.81 ml** |
| Fulfilled demand     | **287,291.29 ml** |
| Lost volume          | **108,471.52 ml** |
| Service level        |        **72.59%** |
| Stockout series-days |           **586** |
| Average inventory    |  **27,965.57 ml** |
| Order events         |         **1,163** |
| Inventory turnover   |         **14.15** |

The inventory plots provide a visual view of how stock levels change relative to the calculated Par Levels.

## 7. Final Takeaway

**Notebook section:** `Step 6 — Final Results & Business Summary`

The project covers the complete process from raw inventory data to daily demand forecasting, inventory policy calculation, and historical simulation.

The main challenge is intermittent demand. Some Bar × Brand combinations have many zero-consumption days, which makes positive-demand prediction difficult.

The current solution provides a practical baseline and a complete decision flow. Future versions could use additional information such as promotions, holidays, events, real-time POS data, and supplier lead-time changes to improve the results.

This version is much better suited for a **GitHub project explanation**: you can glance at each section while recording without having to read it word-for-word.


Video url : https://drive.google.com/drive/folders/1bDi3I97ZUxvlhYsgbzSNY8rld4NSOW_u?usp=sharing
