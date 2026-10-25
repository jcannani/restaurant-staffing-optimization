# Demand-Driven Staffing Optimization

**Can we cut labor costs without hurting service by staffing to predicted demand instead of a fixed schedule?**

This project rebuilds, on public data, the same type of analysis I used at Tokyo Restaurant Group to reduce operating expenses by 8% - forecasting demand and aligning staffing to it rather than staffing on a flat, intuition-based schedule.

## Business Problem

Most retail/restaurant operations staff on a fixed weekly schedule that doesn't flex with actual demand. That creates two costly failure modes: overstaffing during predictably slow periods, and understaffing during demand spikes. 

**Goal:** Build a demand forecast, translate it into a staffing recommendation, and quantify the real trade-off vs. a naive fixed-staffing baseline.

## Data

[Rossmann Store Sales](https://www.kaggle.com/competitions/rossmann-store-sales) (Kaggle) - daily sales for 1.115 stores, including promotions, holidays, and store metadata.

Rossmann doesn't include actual staffing data, so I built a transparent, documented staffing rule from sales volume (2 minimum staff + 1 per $1,500 in daily sales) - a normal step in operational analytics when real staffing data isn't public.

## Method

1. Explored demand patterns for a sample store (Store #85)
2. Built a baseline forecast using day-of-week averages
3. Built an improved forecast using gradient boosting, adding promotions and holiday features
4. Converted the forecast into a staffing recommendation using a documented rule
5. Compared total labor cost: demand-driven staffing vs. a flat fixed schedule

## Results

**Store 85 deep dive** (test period: May-Jul 2015, 92 open days)
- Forecast error (MAPE): 13.0% day-of-week baseline -> 8.7% gradient boosting
- Demand-driven staffing cost $2,160 more than a flat 7-person schedule (+2.8%)
- The flat schedule would be understaffed on 23 of 92 days (25.0%), all high-demand days (mostly Sundays and Mondays)

**30 randomly sampled stores** (`random_state=42`; per-store results in `data/results_30_stores.csv`)
- Model beat the baseline in 30 of 30 stores: average MAPE 16.7% -> 11.1%
- Demand-driven staffing cost +0.4% on average (cheaper in 14 of 30 stores)
- A flat schedule would be understaffed on 30.5% of test days

**Assumptions and limits**
- Staffing rule is an assumption (2 minimum staff + 1 per $1,500 predicted sales; $15/hr, 8-hour shifts), since real schedules are not public
- One 3-month test window; features are calendar and promotion signals only

![Demand vs. Staffing](dashboard/demand_vs_staffing.png)

## Tools

Python (pandas, scikit-learn), matplotlib

## Author

Joanna Ni - [Linkedin](https://linkedin.com/in/joannani)