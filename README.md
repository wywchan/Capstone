# Capstone Project - <i>"He Got Game So Show Him The Money"</i>

## Problem Statement

Player/agent negotiations with teams have the potential to be contentious which can cause anxiety to the player, team management as well as the fanbase. Time, energy and money are all at stake during contract negotiations so it would be of great benefit to have a means of accelerating this process by having a regression model that is agnostic to any biases and present a fair contract amount based on anticipated player production. Performance will be guided by r2 score. Baseline will be measured by RMSE and success will be the magnitude of the reduction against the base.

## Executive Summary

A neural network was successfully created that scored reasonably well in terms of r2 score at 74%. RMSE was reduced from baseline by almost 50%. Residuals from predictions are also mostly within calculated standard error of just under $5M. With more time and data, the model can be further refined and improved.

The model performs well enough that it should provide a good starting point for any contract negotiations. Further uses could be for fantasy sports that use an auction method for drafting.

### Methodology

The project is conducted across three jupyter notebooks. The first being Data Acquisition, followed by EDA and then finally Modelling.

I used the NBA historical stats dataset from Kaggle and merged it with scraped salary information from hoopshype.com. I used 10 seasons worth of statistics (2008-2017) which covers a decade of data that includes the last update of the NBA historical stats dataset. Once assembled, I added in player details such as height, weight, year entered into NBA and college. I then searched for outliers for salary compared to winshares (WS) as well as minutes played. This should have also removed outliers due to injury.

I scaled the results as part of preprocessing and encoded the position before checking for correlation and adding possible interaction terms. I initially set the test size at 0.25 but decreased it to 0.2 as the model was overfitting.

The quality of the source for the dataset (basketball-reference.com) was excellent so no significant cleaning was needed. Additional challenges are that players that were traded during the season had multiple rows which were removed. I retained the 'TOT' row for that player instead which contained the entire season's production.

A major assumption is that we will only need to predict a single year's salary for our output. This may cause issues in practice as contracts can run for multiple years which is beyond the scope of this project. Additionally, features such as "Bird Rights" and various salary cap exceptions will not be considered as these features are generally not widely tracked in NBA statistical databases.

---

### Data Dictionary

|Feature|Type|Description|
|---|---|---|
|year|int64|The NBA season of the observation|
|player|object|The player's name|
|pos|object|The player's position|
|tm|object|The player's team. If he was traded during the season, we take the total|
|g|float64|Games played|
|gs|float64|Games started|
|mp|float64|Miutes played|
|per|float64|Player efficiency rating - a per minute rating of a player's performance|
|ts%|float64|True shooting % - a measure of shooting efficiency|
|3par|float64|3 pt attempt rate|
|ftr|float64|Free throw attempt rate|
|orb%|float64|Offensive rebound % - an estimate of offensive rebounds a player grabbed while on the floor|
|drb%|float64|Defensive rebound % - an estimate of defensive rebounds a player grabbed while on the floor|
|trb%|float64|Total rebound % - an estimate of total rebounds a player grabbed while on the floor|
|ast%|float64|Assist % - an estimate of the percentage of teammate field goals a player assisted on|
|stl%|float64|Steal % - an estimate of opponent possessions ended by a steal by the player|
|blk%|float64|Block % - an estimate of opponent 2 pt field goal attempts blocked by the player|
|tov%|float64|Turnover % - estimate of turnovers over 100 plays|
|usg%|float64|Player usage - estimate of percentage of team plays utilizing the player|
|ows|float64|Offensive winshares|
|dws|float64|Defensive winshares|
|ws|float64|Winshares - an estimate of # of wins contributed by a player|
|ws/48|float64|Winshares/48 min - an estimate of # of wins contributed by a player per 48 min|
|ws|float64|Winshares - an estimate of # of wins contributed by a player|
|obpm|float64|Offensive box plus/minus|
|dbpm|float64|Defensive box plus/minus|
|bpm|float64|Box plus/minus|
|vorp|float64|Value over replacement player|
|fg|float64|Field goals made|
|fga|float64|Field goals attempted|
|fg%|float64|Field goals made %|
|3p|float64|3 pt shots made|
|3pa|float64|3 pt shots attempted|
|3p%|float64|3 pt made %|
|2p|float64|2 pt shots made|
|2pa|float64|2 pt shots attempted|
|2p%|float64|2 pt made %|
|efg%|float64|Effective field goal % - adjusts fg% to account for the fact that a 3 pt shot is worth more than a 2 pt shot|
|ft|float64|Free throws made|
|fta|float64|Free throws attempted|
|ft%|float64|Free throws made %|
|orb|float64|Offensive rebounds|
|drb|float64|Defensive rebounds|
|trb|float64|Total rebounds|
|ast|float64|Assists|
|stl|float64|Steals|
|blk|float64|Blocks|
|tov|float64|Turnovers|
|pf|float64|Personal fouls|
|pts|float64|Points scored|
|salary|int64|The player's salary for that season|
|year_start|int64|The year the player entered the NBA|
|height|int64|The player's height in inches|
|weight|float64|The player's weight in lbs|
|college|float64|The college the player attended. "No college" if player didn't attend college|
|rpg|float64|Rebounds per game|
|apg|float64|Assists per game|
|ppg|float64|Points per game|
|spg|float64|Steals per game|
|bpg|float64|Blocks per game|
|yrs_in_league|int64|How many years the player had been in the NBA for during that season|

More information about calculated stats can be found at https://www.basketball-reference.com/about/glossary.html
#### Provided Data

Provided datasets:

- [Player dataset #1 from Kaggle](./Data/player_data.csv)
- [Player dataset #2 from Kaggle](./Data/Players.csv)
- [Historical NBA season stats from Kaggle](./Data/Season_Stats.csv)
- [Salary and season stats merged](./Data/stats_merged.csv)
- [Enriched merged stats containing player details](./Data/stats_enriched.csv)
- [Final cleaned dataset](./Data/stats_cleaned.csv)
- [Cleaned player data](./Data/playerdata_clean.csv)

#### Acknowledgements
Many thanks to Omri Goldstein for compiling and posting the dataset to Kaggle and Abid Rahman for updating it. The dataset can be found at https://www.kaggle.com/drgilermo/nba-players-stats

Also thanks to [basketball-reference](https://www.basketball-reference.com) for curating NBA historical data in a consumable way and [Hoopshype](https://www.hoopshype.com) for making salary data available to the public.

---

### Conclusions and Recommendations
We were able to create a neural network that had moderate success statistically in predicting salaries based on production. The r2 test score was 74% and RMSE was reduced from baseline by almost 50%. In general, the features with the highest amount of correlation are related to scoring such as points and field goals as well as winshares. Furthermore, there appears to be a premium paid for higher profile athletes that is not related to their production.

The model should sufficiently provide predictions to kick off negotiations at a level that are reasonably close to historical reality. Some limitations are as mentioned in the opening such as length of contract, increasing salary cap considerations as well as rights for some teams to pay more and offer longer term for their own players (Bird rights). Additionally, the model requires many inputs making it cumbersome to use.

Further uses of the model could be for fantasy sports where players are drafted by auction instead of draft pick selection. Values can be better assigned to avoid overpaying a superstar for similar production of another lower profile player.

Next steps include possibly adding in additional historical data since the model is a data hungry one. Furthermore, it seems a modifier is needed to account for "superstar" status as there is a premium paid to these players that isn't production based. Additionally, it could be interesting to put the salaries into bands and turn this into a classification problem. Since the model already performs well as a regression model this could be a better use of the neural network.
