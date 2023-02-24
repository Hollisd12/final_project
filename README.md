# final_project

## Segment 1 tasks

### Topic and dataset
Predict MVP of basketball game

What is the most important statistic which defines who will be the MVP?

Dataset = mvp_votings.csv

### Create a mockup of a machine learning model

Model:
Supervised as the data set has labels

Output of data:
Regression? - predicting the value/outcome

Algorithim:
Linear Regression

### Roles

ETL: Delilah 

ML: Delilah & Diego

Dashboard: Samuel & Li

Readme: 

### Technologies
Python

Tableau


### ETL
Need to clean up data and compare relationships

Add column that lets us know who won MVP

- MVP is chosen by number of votes

### Sources
Primary source of data: 
https://www.kaggle.com/datasets/danchyy/nba-mvp-votings-through-history
- His article explaining his project - https://towardsdatascience.com/predicting-2018-19-nbas-most-valuable-player-using-machine-learning-512e577032e3
- His github - https://github.com/danchyy/Basketball_Analytics/blob/master/Scripts/2018_19_season/mvp_predictions/Predicting%20MVP.ipynb

Second source: 
https://www.kaggle.com/code/samfenske/predicting-nba-mvp-with-advanced-stats/notebook
- uses the data set from the primary source to create a machine learning model and visualizations

Third source: 
https://medium.com/@atharvjoshi/predicting-the-2022-2023-nba-mvp-using-python-76cabf4422fd
- just an article talking about this type of machine learning model. can pull inspiration from


Graph A. Provides a visual look into what player accumalated more votes first coupled with FG Pct
  >Votes First= number of votes per player recieved for the MVP 
  >FG PCT= percentage of field goals that a team or player makes 2 pcts
  
 https://public.tableau.com/app/profile/samuel.harding.iv/viz/Final_Project_16754853759490/Sheet1?publish=yes
 
 ## Conclusion

### Results
We were able to create a machine learning model with good predicitve power for determining how the media would vote for the NBA MVP based on simple and advanved statistics. Our model was able to indicate that 67.5% of the variance in award_share is explained by the features (statistics) we selected for our model. However, 32.5% of the variance is unexplained. This variance may be because we do not have enough statistics taken into consideration in our model. Additionally, there may be other factors that affect how the media will vote other than the simple and advanced statistics.

For example, voters may look at things like media coverage. A player's narrative and storyline throughout the season and how it has impacted the league may also be considered. Additionally, voters may consider a player's overall career achievements and how their performance in a given season compares to other great seasons in NBA history. All of these items are not included in our data set or may hard to include in a model.

### Recommendations for future analysis
- More data could be beneficial in providing a more adequate model. For example: media coverage
- Knowledge of voting history and trends may be helpful. Voting has probably changed over the years and the stats that may have been more important in the past may not be as important today.

### What would we have done differently if we had more time allocated for the project
- Our dataset only includes data for NBA MVP candidates up until the 2017-2018 season. If we had more time, we could scrape and collect data for the last 5 seasons.
- With the data for the last few seasons included, we could also scrape player statistics for the current 2022-2023 seasons to attempt to predict who will win the MVP for this season. 

### Sources
https://www.kaggle.com/datasets/danchyy/nba-mvp-votings-through-history

https://www.basketball-reference.com/about/glossary.html

