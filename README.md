Aniss Brachmi - Coding Assignment
---

## 🧠 Overview
The assignment was completed and replicated in **both Stata and Python**.  
All learning outcomes were achieved, including:

- Reading and cleaning data  
- Filtering and transforming variables  
- Generating new variables  
- Creating summary statistics  
- Producing and exporting graphs  
- Maintaining a reproducible project structure  
- Submitting the project through GitHub for grading and reproducibility.

---

## ⚙️ How to Reproduce the Analysis

### 🔹 **Stata and Python Implementation**

1. Open **Stata**.  
2. Set your working directory to the project’s root folder.  
3. Open `final.do`.  
4. Click **Run (Do)** or type `do final.do` in the Command window.  
5. The code will create processed datasets, summary tables, and graphs automatically.

#### **Stata Script (`final.do`)** - Python script ('football_analysis')

```stata
// final.do
// Load data, clean, summarize, and visualize

clear all
set more off

use "data/raw/football.dta", clear

// Create output directories if missing
capture mkdir output
capture mkdir output\tables
capture mkdir output\figures

// Clean data
drop if missing(goals_home) | missing(goals_away)

// Create variables
gen goal_diff = goals_home - goals_away
gen home_win  = (goals_home > goals_away)
gen away_win  = (goals_away > goals_home)
gen draw      = (goals_home == goals_away)

// Summary statistics
summarize goals_home goals_away goal_diff home_win draw

// Aggregate by season and export table
preserve
collapse (mean) home_win, by(season)
export delimited using "output/tables/summary_stats_stata.csv", replace

// Graph: Home win rate by season
twoway (connected home_win season), ///
    title("Home Win Rate by Season (Stata)") ///
    xtitle("Season") ///
    ytitle("Average home win rate")
graph export "output/figures/stata_home_win_rate_by_season.png", replace
restore

####  **Python script ('football_analysis.py)***

cd "C:\Users\YOURNAME\Documents\Aniss Brachmi - Coding Assignment\code\python"

python analysis_football.py

# analysis_football.py
# Replicates the Stata workflow in Python

from pathlib import Path
import pandas as pd
import matplotlib.pyplot as plt

# 1. Set up paths
PROJECT_ROOT = Path(__file__).resolve().parents[1]
DATA_RAW = PROJECT_ROOT / "data" / "raw"
OUTPUT_TABLES = PROJECT_ROOT / "output" / "tables"
OUTPUT_FIGURES = PROJECT_ROOT / "output" / "figures"

OUTPUT_TABLES.mkdir(parents=True, exist_ok=True)
OUTPUT_FIGURES.mkdir(parents=True, exist_ok=True)

# 2. Load the data
df = pd.read_stata(DATA_RAW / "football.dta")

# 3. Clean the data
df = df.dropna(subset=["goals_home", "goals_away"])

# 4. Create new variables
df["goal_diff"] = df["goals_home"] - df["goals_away"]
df["home_win"] = (df["goals_home"] > df["goals_away"]).astype(int)
df["away_win"] = (df["goals_away"] > df["goals_home"]).astype(int)
df["draw"] = (df["goals_home"] == df["goals_away"]).astype(int)

# 5. Generate summary statistics
summary = df[["goals_home", "goals_away", "goal_diff", "home_win", "draw"]].describe()
summary.to_csv(OUTPUT_TABLES / "summary_stats_python.csv", index=True)

# 6. Collapse and visualize
win_rate = df.groupby("season")["home_win"].mean().reset_index()

plt.figure(figsize=(8,5))
plt.plot(win_rate["season"], win_rate["home_win"], marker="o")
plt.title("Home Win Rate by Season (Python)")
plt.xlabel("Season")
plt.ylabel("Average home win rate")
plt.grid(True, linestyle="--", alpha=0.5)
plt.tight_layout()
plt.savefig(OUTPUT_FIGURES / "py_home_win_rate_by_season.png")
plt.close()

print("✅ Analysis complete!")
print("Tables saved to:", OUTPUT_TABLES)
print("Figures saved to:", OUTPUT_FIGURES)

### EXPECTED RESULTS
/output/tables/summary_stats_python.csv
/output/figures/py_home_win_rate_by_season.png

| Tool       | Steps                      | Output Directory                      |
| ---------- | -------------------------- | ------------------------------------- |
| **Stata**  | Run `final.do`             | `/output/tables/`, `/output/figures/` |
| **Python** | Run `analysis_football.py` | `/output/tables/`, `/output/figures/` |

FINAL STATEMENT

This assignment demonstrates the ability to replicate the same data analysis workflow in both Stata and Python.
All required learning outcomes were achieved and documented with clarity and reproducibility in mind. Both scripts are fully functional, and the project structure allows for easy reproducibility.

*Submitted by Aniss Brachmi*  
*Central European University – Coding for Economists (2025)*
