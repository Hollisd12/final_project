![image](https://user-images.githubusercontent.com/112137694/220790464-f94fa080-620b-4a33-b119-047441765c53.png)

### Roles
ETL: Delilah

ML Model: Diego & Delilah

Dashboard: Samuel & Li

Readme: All

## Overview of the project
This project is designed to analyze basketball player statistics and determine if we can accurately predict who will win the MVP (most valuable player) of the season using machine learning. Essentially, we are attempting to model how the media will vote for players based on their statistics. 

### Data Source:
[Kaggle data link](https://www.kaggle.com/datasets/danchyy/nba-mvp-votings-through-history)

### Key questions to be answered:
- Can we predict the NBA MVP based on statistics using machine learning?

## ETL Process
First, we reviewed our data set to see what we could learn about our data:

- Our data set includes key statistics for all NBA MVP candidates for each season since from 1980-81 through 2017-18
- There are 38 seasons in the data set
- This data set includes statistics for all MVP candidates (players) for each season
- Each season has between 10-31 players

![image](https://user-images.githubusercontent.com/112137694/220791298-e482e5c2-e6df-4556-b22c-c1ac2676af23.png)

Then, we checked for null values and determined there were none:

![image](https://user-images.githubusercontent.com/112137694/220791453-077e14d9-d343-4e46-9be3-52f97b8f50aa.png)

### Background
To better understand our data, we had to perform research on how the NBA MVP is chosen and what each statistic (column) means. 

#### What do the stats mean?
![image](https://user-images.githubusercontent.com/112137694/220790365-df2db091-03ec-4cf0-9210-822ffe5d9ffb.png)

[source](https://www.basketball-reference.com/about/glossary.html)

There are different types of statistics being taken into consideration for our MVPs:

- Simple statistics: Simple frequency-based numbers such as: games played, minutes per game, points per game, assists per game, etc.
- Advanced statistics: Go beyond basic stats. For example: player efficiency rating, win shares, box plus/minus, usage percentage, etc.

Additionally, there are some columns and stats that we cannot use for our machine learning model as they directly tie into who won the MVP.

- votes_first
- points_won
- points_max
- award_share

#### How is the MVP selected?
The NBA MVP (Most Valuable Player) is selected by a panel of sportswriters and broadcasters from the United States and Canada, who vote at the end of the regular season. The voting process involves each panelist selecting five players for the award, ranked in order from first to fifth place.

The NBA uses a points system to determine the winner, with each first-place vote worth 10 points, each second-place vote worth 7 points, each third-place vote worth 5 points, each fourth-place vote worth 3 points, and each fifth-place vote worth 1 point. The player with the highest point total is awarded the NBA MVP.

In our data set, we can determine the MVP for each season based on points_won and award_share. award_share = points_won/points_max

To show who won the MVP in our dataset, we added a column called Mvp? and set all values to No.

![image](https://user-images.githubusercontent.com/112137694/220793484-291991f4-2b15-4c5b-80d0-94fc6125b397.png)

Then, we looped through our dataset and updated the column to be Yes for the player with the highest points_won for each season.

![image](https://user-images.githubusercontent.com/112137694/220793520-37c98c28-41bc-4c68-9839-93ae4fec14cb.png)
![image](https://user-images.githubusercontent.com/112137694/220793560-3ea71acb-f500-4606-abc4-5b8de5fd2cde.png)

### Identifying the most important statistics
We started our exploratory analysis of our data to try and visualize the relationship between some advanced statistics and award_share

The first graph we created was for points_per_g vs award_share

![pts_per_g_vs_award_share](https://user-images.githubusercontent.com/112137694/220794361-6536a573-fd97-44b7-b1e3-2437ab401d75.png)

In the graph we plotted all players from all seasons and then we changed the color of the points to depend on if the player was the MVP or not. From a quick glance, it seems like there is a minor correlation between points per game and award shares. However, we can't ignore all those players with high points per game and low award shares. This means there must be other factors affecting award shares.

Next, we analyzed win_pct vs award_share:

![win_percentage_vs_award_share](https://user-images.githubusercontent.com/112137694/220794408-f17f3444-1c79-4a82-b63f-d947f3b3cb18.png)

Again, we can see a small trend between MVPs and their win percentage. However, there has to be other factors in play in determining the award_shares, not just win percentage.

The last graph we created was for ws vs award_share:

![win_shares_per_48_vs_award_share](https://user-images.githubusercontent.com/112137694/220794387-c52a5dc1-7d08-484a-bb0b-1726afb1edef.png)

Again, this graph looks similar to the previous two graphs. It seems our advanced statistics have some slight correlation with who will win the MVP, however, other factors must be in play.

#### Correlation matrix

To help identify which statistics in our data set that have the biggest impact on who earns the MVP, we created a correlation heatmap. 

![heatmap](https://user-images.githubusercontent.com/112137694/220793614-644a8d40-0dec-461d-8d8d-6ef9c0fba402.png)

The correlation matrix is a good way of visualizing which features are very correlated and thus can be used to highlight duplicated infromation which in some situations doesn't help the model. We can review stats that represent similar statsitics and remove them from the model. 

Additionally, we can review which statistics are more correlated with award_share and thus the player becoming MVP. 

Based on our analysis and research we determined the following:

- Remove stats that are highly correlated or features that will basically represent the same thing or can be represented by some scalar multiplications
- BPM and PER represent similar stat.
- Points per game is directly connected with usage
- Can remove percentages because true shooting percentage is enough. Also, WS per 48 can be removed since it is just a scaled value of WS
- Also, the attempts will be removed since they are included in usage stat.
- Since per and BPM model similiar stat, only need to use one stat in the model

#### Stastics to be used in our model
From our evaluation and exploratory analysis, the final metrics we have decided to use in our model include:
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

## Machine Learning Models
In our machine learning model, we are attempting to predict how the media will vote for players to be the MVP based on their statistics. The target value for our task will be the award_share column which is between 0-1. Our model will be a supervised learning model as our data set has labels.

### Questions to answer
- Can we predict how the media will vote for players this season and if they will be the MVP?

### Setting up the data
![image](https://user-images.githubusercontent.com/112137694/220798322-b9551c0a-dbc4-49f6-bd22-1505e85ea12a.png)

- the dataframe we used included our mvp_statistics cvs file
- we dropped the first column which was unnamed and had no data

### The model

#### Decision Tree Regressor
In our first attempt, we created a Decision Tree Regressor model
(describe the first model here - decision tree)
 
![image](https://user-images.githubusercontent.com/112137694/220798521-713e2ba3-a18e-4491-bb40-8656ee0b1862.png)

(add a quick description of the code above, what we are importing, the different training vs testing seasons, etc.)

Next, we created a new dataframe from the validation features and added a prediction column. The prediction column is where the model will predict the award_shares.

![image](https://user-images.githubusercontent.com/112137694/220798902-924f62ff-8db6-4e93-8e7e-e3869d3b265d.png)
![image](https://user-images.githubusercontent.com/112137694/220798986-19f06ef9-0cc3-4cde-805e-969a467afd49.png)

We then added code to add a new column to our dataframe of validation features to display who the predicted MVP for each season is, based on the prediction column. 

![image](https://user-images.githubusercontent.com/112137694/220799119-0542cc81-097c-41ca-8df9-05ce267a0cec.png)

##### Results of the model

The Decision Tree Regressor model was able to predict the MVP for 4 out of the 8 seasons (50%).

We then calculated the mean squared error (MSE), mean absolute error (MAE), and r2 (r-quared).

![image](https://user-images.githubusercontent.com/112137694/220799296-1553cd72-3e95-4ec0-86f5-8aaa8e6c41a6.png)

- MSE of 0.07881568253968253 indicates that, on average, the model's predictions deviate by 0.08 from the actual target values. This value is expressed in the units of the target variable, so you can use it to determine the magnitude of the error. The smaller the MSE, the better the model's predictions.
- MAE of 0.1669682539682539 indicates that, on average, the model's predictions deviate by 0.14 from the actual target values. This value is expressed in the units of the target variable and gives a more interpretable measure of the model's accuracy, since it is expressed in the same units as the target variable.
- The R-Squared value of -0.1454585206389547 indicates the proportion of variance in the target variable that can be explained by the features. In general, a high R-Squared value indicates that the model is a good fit for the data, while a low R-Squared value indicates that the model may not be a good fit for the data.

#### Linear Regression Model
(describe the second model here - maybe what this model is, why we chose it (to look for a more accurate model))

![image](https://user-images.githubusercontent.com/112137694/220799940-928f7771-41da-40fb-8104-3d2db23ca4d4.png)

(Describe the code above, similar to our first code except the imports)

![image](https://user-images.githubusercontent.com/112137694/220800068-fa975933-bc93-4e98-b0b6-00bce77539a8.png)

(This code is the same as the first model: we created a new DF, added the column for mvp_prediction, added a column to show who won mvp based on predicition)

##### Results of the model

The Linear Regression model predicted the MVPs correctly for 5 out of the 8 seasons (62.5%).

We then calculated the mean squared error (MSE), mean absolute error (MAE), and r2 (r-quared).

![image](https://user-images.githubusercontent.com/112137694/220800284-9e235907-b480-4774-80da-532fcc43063b.png)

- MSE of 0.029840141347939504 indicates that, on average, the model's predictions deviate by 0.03 from the actual target values. This value is expressed in the units of the target variable, so you can use it to determine the magnitude of the error. The smaller the MSE, the better the model's predictions.
- MAE of 0.1351533088374249 indicates that, on average, the model's predictions deviate by 0.14 from the actual target values. This value is expressed in the units of the target variable and gives a more interpretable measure of the model's accuracy, since it is expressed in the same units as the target variable.
- The R-Squared value of 0.5663217894882959 indicates the proportion of variance in the target variable that can be explained by the features. An R-Squared value of 0.566 means that 56.6% of the variance in the target variable can be explained by the features. 

Overall, this model is way more accurate than the decision tree model, as seen by the statistics and our MVP predictions.

### Random Forest Regressor Model
The last model we tried was the random forest regressor model. We chose this model because __. We started the model out similarly to the previous two, by creating the same initial dataframe.

![image](https://user-images.githubusercontent.com/112137694/220800548-b03cfa20-a63d-4040-9cb2-143a6c72c84f.png)

In this model, we imported the RandomForestRegressor algorithm from the sklearn library. We used the same seasons for training and testing as the previous two models. 

![image](https://user-images.githubusercontent.com/112137694/220800785-d698976c-cc34-4091-9b42-03a714fd8336.png)

We then took the same steps of creating a new dataframe with the features used in the model, the player, and season. We then added a new column called mvp_predictions where we populated the models prediction of award_share for each player. Then, we added a column that showed us which players the model would predict would win the MVP based on mvp_prediction (predicted award_shares). 

![image](https://user-images.githubusercontent.com/112137694/220801002-33fcf655-ff2c-4259-bb70-03b1c7e06941.png)

This model was able to accurately predict who would win the MVP for 6 out of the 8 testing seasons, for a 75% accuracy.

We then calculated the mean squared error (MSE), mean absolute error (MAE), and r2 (r-quared).

![image](https://user-images.githubusercontent.com/112137694/220801180-24c3358b-817f-4730-8f7d-663ed42b2498.png)

- MSE of 0.022307539351587306 indicates that, on average, the model's predictions deviate by 0.022 from the actual target values. This value is expressed in the units of the target variable, so you can use it to determine the magnitude of the error. The smaller the MSE, the better the model's predictions.
- MAE of 0.09895563492063493 indicates that, on average, the model's predictions deviate by 0.099 from the actual target values. This value is expressed in the units of the target variable and gives a more interpretable measure of the model's accuracy, since it is expressed in the same units as the target variable.
- The R-Squared value of 0.6757959811881447 indicates the proportion of variance in the target variable that can be explained by the features. An R-Squared value of 0.675 means that 67.5% of the variance in the target variable can be explained by the features, while 32.5% of the variance is unexplained. 

This model is the most accurate out of every model we attempted. An r-squared value of 0.675 is good for our model

## Dashboard
[Tableau Dashboard](https://public.tableau.com/app/profile/li.yan.shao/viz/Final_Project_16757209444750/1_1?publish=yes)


## Conculsion
Data is quite unbalanced, histogram of award share values, more than half of examples lie between 0.0 and 0.2 which can cause issues for models
![award_share_occurrences](https://user-images.githubusercontent.com/112137694/217980130-8ae3fd2a-71aa-43cd-8cbb-38d4a3c3e578.png)


### Sources
Primary source of data: https://www.kaggle.com/datasets/danchyy/nba-mvp-votings-through-history
- https://towardsdatascience.com/predicting-2018-19-nbas-most-valuable-player-using-machine-learning-512e577032e3
- https://github.com/danchyy/Basketball_Analytics/blob/master/Scripts/2018_19_season/mvp_predictions/Predicting%20MVP.ipynb

Second source: https://www.kaggle.com/code/samfenske/predicting-nba-mvp-with-advanced-stats/notebook

Third source: https://medium.com/@atharvjoshi/predicting-the-2022-2023-nba-mvp-using-python-76cabf4422fd


