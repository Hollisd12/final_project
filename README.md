# final_project

## Plan

### Topic and dataset
Predict MVP of basketball game
What is the most important statistic which defines who will be the MVP?
we’re basically trying to model how the media will vote for players this season
Target value for our task will be Share column, between 0-1
this is a task of ranking, try to approach as a regression problem

### Create a mockup of a machine learning model
Model:
Supervised as the data set has labels
Output of data:
Regression
Algorithim:
Linear Regression

### Roles
ETL: Delilah 
ML: Delilah & Diego
Dashboard: Samuel & Li
Readme: All

### Technologies
Python
Tableau

### Sources
Primary source of data: https://www.kaggle.com/datasets/danchyy/nba-mvp-votings-through-history
- https://towardsdatascience.com/predicting-2018-19-nbas-most-valuable-player-using-machine-learning-512e577032e3
- https://github.com/danchyy/Basketball_Analytics/blob/master/Scripts/2018_19_season/mvp_predictions/Predicting%20MVP.ipynb

Second source: https://www.kaggle.com/code/samfenske/predicting-nba-mvp-with-advanced-stats/notebook

Third source: https://medium.com/@atharvjoshi/predicting-the-2022-2023-nba-mvp-using-python-76cabf4422fd

## ETL
Need to clean up data and compare relationships
- Add column that lets us know who won MVP
- MVP is chosen by number of votes
- There are 38 seasons in the data set
- Lists the stats for all MVP candidates for each season
- Varies from 10 players (2015-2016) to 31 players (1980-1981)

### Background
What do the stats mean? (source: https://www.basketball-reference.com/about/glossary.html)
- Fga( Field Goals Attempted ) : Definition The number of field goals that a player or team has attempted(include 2 and 3 score)
- Fg3a(3-Point Field Goal Attempts) : The number of 3-point field goals that a team or player attempts.
- fta(Free Throws Attempted) : The number of free throws that a team or player attempts.
- bpm(Box Plus/Minus) : Is a statistic used to evaluate basketball players’ quality and contribution to the team
- g(games) : number of games that player has played
- ws(Win Share) : Win Share is a measure that is assigned to players based on their offense, defense, and playing time.
- ws_per_48 : WS/48 is win shares per 48 minutes
- ts_pct(True Shooting_Percentile) : A shooting percentage that factors in the value of 3-point field goals and free throws in addition to the total number of field goal attempts.
- usg_pct(Usage Percentile) : percentage of team plays a player was involved in while he was on the floor
- fg_pct(Field Goal Percentile) :  The percentage of field goals that a team or player makes(include 2 and 3 score)
- fg3_pct(3-Point Field Goal Percentile) : Field Goal that players take in every games
- ft_pct(Free Throws Percentile) : The percentage of free thow attempts that a team or player makes
- pts_per_g(Points per games) : The number of points a team or player has scored in every games
- trb_per_g(Total rebound per games) : number of rebound that player has taken in every games
- ast_per_g(Assists per games) :Numbers of assist that players earn for making a pass that leads directly to a teammate's made basket in every games
- stl_per_g(Steals per games) : Numbers of steal that players get in every games.
- blk_per_g(blocks per games) : Numbers of blocks that players do in every games
- per (player efficiency rating): A calculation of all positives and negatives simple stats
- mp_per_g (minutes per game): minutes per games played

What are the different types of statistics?
- Counting stats: simple frequency-based numbers such as: games played, minutes per game, points per game, assists per game, etc.
- Advanced statistics: go beyond basic stats. for example: player efficiency rating, win shares, box plus/minus, usage percentage, etc.

### Exploratory analysis

Data is quite unbalanced, histogram of award share values, more than half of examples lie between 0.0 and 0.2 which can cause issues for models
![award_share_occurrences](https://user-images.githubusercontent.com/112137694/217980130-8ae3fd2a-71aa-43cd-8cbb-38d4a3c3e578.png)

#### Evaluate some advanced statistics compared to award share
![pts_per_g_vs_award_share](https://user-images.githubusercontent.com/112137694/217980144-82d77897-827c-46a8-9789-841b12a11b9d.png)
![win_percentage_vs_award_share](https://user-images.githubusercontent.com/112137694/217980156-d36a0d97-0d4a-4c84-a208-cd0bf5a385b3.png)
![win_shares_per_48_vs_award_share](https://user-images.githubusercontent.com/112137694/217980162-bc34f769-dfc9-4cd9-846f-af388d80a8ba.png)

#### Identifying the most important statistics
Created a correlation heatmap to visualize which variables are linearly related
![heatmap](https://user-images.githubusercontent.com/112137694/217980171-486b2ecc-add0-4bde-963a-86c66bfb59ef.png)

- Notice that the advanced stats are the correlated the strongest with award share.
- Remove stats that are highly correlated or features that will basically represent the same thing or can be represented by some scalar multiplications
- BPM and PER represent similar stat.
- Points per game is directly connected with usage
- Can remove percentages because true shooting percentage is enough. Also, WS per 48 can be removed since it is just a scaled value of WS
- Also, the attempts will be removed since they are included in usage stat.
- Since per and BPM model similiar stat, only need to use one stat in the model

## Dashboard
Interactive charts that displays 
- Points Per Game vs Award Share
- Win shares per 48 vs award share
- Win percentage vs award share

## Machine learning
Training with different stats to see which affect the award share outcome
First attempt with the following stats:
- ts_pct
- bpm
- mp_per_g
- pts_per_g
- trb_per_g
- ast_per_g
- stl_per_g
- blk_per_g
- ws
- win_pct
