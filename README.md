# What Wins the Bot Lane: A League of Legends Data Analysis Project

##  Project Overview
This project investigates the relationship between champion draft synergies, early player performance and match outcomes in the 2v2 bottom lane in League of Legends. By analyzing historical data from high ranked matches from the 2024 and 2025 seasons, this project aims to point out the factors which best predict the winning lane (more kills or gold by minute 10), which mostly also shows who will even win the whole game in the end.

The key research questions addressed in this project include:
* How strongly does a champion duo's preexisting synergy, like their win rate together correlate with laning success, which is, for example, like who has more gold at minute 10 of the game.
* Is achieving an early gold or kill lead more predictive of the final result or is it having a statistically favorable counterpick matchup from the draft?
* Can a model accurately predict the gold difference in the bot lane based simply on the four champions selected?

---

##  Motivation
Picking your champion in League of Legends is the most essential part. Many players believe the game is often won or lost based on these picks. For bottom lane players, the ADC (main damager) and Support, it's really stressful to pick the right two champions. Most players just guess what is good based on what is popular or what they have heard is strong.

This project will use data from thousands of real games to find the real answers. I want to help myself and maybe my friends make smarter picks.

---

##  Objectives
* Find out if getting an early lead actually helps a player win the whole game.
* Make a list of the strongest and weakest ADC (main damager) + Support pairs in the current meta.
* Build a program that can predict who will win the laning phase just by looking at the four champions in the matchup.
* Create charts to make the results easy to understand, like a heat map showing the win rate of every common bot lane combo.

---

##  Data Sources
I will combine data from three places:

* **Riot Games API:** This is my main source for raw match data.
    * **Match Data:** Tells me who won, which champions were in the game, and final scores (KDA).
    * **Timeline Data:** Gives me the minute details, like how many minions each player had at the 10 minute mark.

* **Kaggle Datasets:** Prepackaged data files (CSVs) that others have already collected. They are great because the data is already clean and organized.

* **Stats Websites :** Scrape data from these sites.
    * Riot's data doesn't tell me a champion's overall win rate.
    * Get stats like the win rate as a pair, or a specific champion's winrate against another.
    * Use these numbers to measure synergy and counter pick strength.

---

##  Data Collection Plan

### 1. Using the Riot API 
* **Tool:** Python.
* **How:** I'll use Python to connect to the Riot API after getting a developer key. The general process involves getting a list of high ranked players and then pulling their recent match histories. For each match, I'll download the main data like champions, and the timeline data like gold at 10 minutes. I will then use pandas to clean and organize this raw data.

### 2. Using Kaggle
I will use the according public dataset on Kaggle that will give me concrete information of kills, gold, exp and etc. to get more insight.

### 3. Using Web Scraping 
* **Tool:** Python
* **How:** A simple script that visits a stats website (like U.gg or OP.gg). It will read the site to find the win rates for champion pairs. I'll copy these numbers into my own file.

## Expected Outcomes
* A clear, data-driven answer on whether early-game leads or good champion matchups are more important for winning.
* A Synergy Matrix that visualizes the win rates of common ADC and Support combinations.
* A simple predictive model that can estimate who will win the laning phase based on the four champions picked.
