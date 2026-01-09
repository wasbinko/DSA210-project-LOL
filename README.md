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

### 3. Machine Learning Models
* **Win Prediction (Random Forest):**
   * **Features:** Champion IDs and early game gold difference.
       * **Evaluation:** Analyzed accuracy and the confusion matrix to understand prediction errors.
* **Duo Clustering (K-Means):**
   *  **Model:** K-Means Clustering with **k=4**.
       * **Features:** Win Rate and Average Gold Difference @ 10.
       * **Goal:** Group bot lane pairs into S, A, B, and C performance tiers.
---

## Key Findings

### The Impact of Early Gold
* **Result:** There is an overwhelming correlation between early gold leads and victory.
* **Evidence:** The T-Test yielded a **P-Value of ~1.34e-51**, confirming the results are extremely statistically significant ($p < 0.05$).
* **Visualization:** Shows that winning teams have a significantly higher median gold lead at 10 minutes.
<img src="output/gold_diff_boxplot.png" alt="" style="height: 500px; width:800px;"/>

### Strongest & Weakest Duos
* **Top Performers:** **Jinx + Thresh** (~72% Win Rate) and **Twitch + Milio** (~70% Win Rate) represent high-synergy pairings.
* **Underperformers:** **Kaisa + Braum** (~20% Win Rate) and **Kaisa + Alistar** (~30% Win Rate) show significantly lower synergy in the current dataset.
* **Visualization:** (Mostly shows the top and mid tier duos, with the lower tiers not seen due to the sheer amount of them and inability to show that all in a graph)

 <img src="output/top_bot_horizbar.png" alt="" style="height: 500px; width:800px;"/>

### Predicting Wins (Machine Learning)
* **Model Accuracy:** The Random Forest model achieved an accuracy of **61.75%**.
* **Confusion Matrix Analysis:** * The model correctly identified 61.75% of outcomes. 
    * It is particularly accurate at identifying losses when the bot lane falls behind (True Positive). 
    * **False Positives:** The main error type occurred when the model predicted a win due to a bot lane lead, but the team still lost. This highlights that while bot lane is influential, mid/late-game "throws" or other lanes feeding can still override a bot lane advantage.
* **Visualization:**
  
  <img src="output/win_confusionmatrix.png" alt="" style="height: 500px; width:500px;"/>

### Automated Tier List (Clustering)
Using **K-Means Clustering (k=4)**, the algorithm automatically categorized bot lane duos into tiers:
* **S-Tier (Dominant) (Cluster ID: 2):** Win Rates **> 60%** and consistent positive gold leads. These are the "Meta Kings" (e.g., Jinx + Thresh).
* **A-Tier (Strong):** Win Rates between **52% and 58%**. Consistent, reliable meta staples.
* **B-Tier (Balanced):** Win Rates hovering around **48-51%**. These are situational picks that do not provide an inherent statistical advantage.
* **C-Tier (Suboptimal):** Win Rates **< 45%** with negative average gold differences. These pairs statistically put their team at a disadvantage.

**Visualization:** We can confirm that the S-Tier cluster(Cluster ID:2), has the biggest positive gold difference and subsequently the highest win rate, and in contrast C-tier (Cluster ID: 3) have the biggest negative gold difference, and the lowest win rate.

<img src="output/performance_kmeans.png" alt="" style="height: 500px; width:800px;"/>

