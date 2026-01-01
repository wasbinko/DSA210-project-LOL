# What Wins the Bot Lane: A League of Legends Data Analysis Project

## Project Overview
This project investigates the relationship between champion draft synergies, early player performance, and match outcomes in the 2v2 bottom lane in League of Legends. By analyzing historical data from high-ranked matches, this project identifies whether early laning leads (Gold Difference @ 10 min) predict the final game winner and evaluates champion synergy using Machine Learning.

## Research Questions
To address the project goals, this analysis focuses on the following key questions:
* **RQ 1:** To what extent does an early economic lead (Gold Difference at 10 minutes) correlate with the final match victory?
* **RQ 2:** Which specific ADC + Support combinations demonstrate the highest win rates in the current meta?
* **RQ 3:** Can a Machine Learning model (Random Forest) accurately predict the match winner based solely on the four champions selected in the bot lane?
* **RQ 4:** Can we automatically classify bot lane duos into performance tiers (e.g., 'OP' vs. 'Weak') without human bias?

---

## Motivation
Picking the right champion in League of Legends is a critical component of the game. Many players believe the match is often won or lost based on the draft phase alone. For bottom lane players—specifically the ADC (main damager) and Support—the pressure to select a synergistic pair is high. Currently, most players rely on intuition or anecdotal evidence. This project uses data from thousands of real games to replace guesswork with statistical evidence.

---

## Data Sources
I utilized a hybrid data collection strategy to build a comprehensive dataset:
1.  **Kaggle Dataset (Seed Data):** Used as a baseline to obtain valid Match IDs and final game outcomes (Win/Loss) for high-ELO matches.
2.  **Riot Games API (Enrichment):**
    * **Match-V5 Endpoint:** Extracted the specific champions played in the ADC and Support roles.
    * **Timeline-V5 Endpoint:** Calculated the granular "Gold Difference at 10 Minutes" metric for every match.

---

## Methodology

### 1. Data Collection & Processing
* **Tools:** Python, Pandas, `riotwatcher` (for API rate limiting), `tqdm` (progress tracking).
* **Pipeline:**
    * Iterated through match IDs from the seed dataset.
    * Filtered out invalid matches (e.g., remakes, non-standard lanes).
    * Calculated the **Gold Differential** (Blue Bot Gold - Red Bot Gold) at frame 10 (10:00).
    * Aggregated data into a structured CSV (`bot_lane_dataset.csv`) containing Champion names, Gold Stats, and Win Results.

### 2. Analysis & Modeling 
* **Statistical Testing:** Performed a **Student's T-Test** to verify the significance of the relationship between early gold leads and victory.
* **Visualization:** Created Boxplots (with jitter) to visualize gold distributions and Ranked Bar Charts to display Duo Win Rates.

---

## Key Findings (Preliminary)

### 1. Early Leads Predict Wins
* **Visual Evidence:** Boxplot analysis reveals a clear distinction in distributions. Winning teams consistently hold a higher median gold lead at 10 minutes compared to losing teams.
* **Statistical Significance:** The Hypothesis Test resulted in a **P-Value < 0.05**, allowing us to reject the Null Hypothesis. This statistically confirms that gaining a gold advantage in the first 10 minutes is a significant predictor of winning the game.

### 2. Duo Synergy
* **Meta Analysis:** The analysis successfully identified high-performing pairs. The ranked bar chart highlighted distinct pairings with 100% win rates in the sample set, contrasting them with lower-performing off-meta combinations.

