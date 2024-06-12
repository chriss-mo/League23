# League of Legends Snowball Effect
Our data science and analysis project for DSC 80 at UCSD  

## Introduction  
In League of Legends, the primary objective is to destroy the Nexus, which is the only win condition. However, for a team to reach their opponent's Nexus, they must first destroy their opponent's turrets, or towers. Each team has eleven towers on each side, which are spread out along the three lanes. Not all towers need to be destroyed to reach the nexus, just all in one lane. Towers are also an indicator of how successful a team is in a certain match. We wanted to answer the question: **How does getting three turrets first affect the game?**

The dataset we used was provided by [Oracle's Elixir](https://oracleselixir.com/tools/downloads), and we focused on the 2023 season statistics, as it was the most recent complete dataset. The dataset has 125,904 rows and 131 columns. Each row represents a player's performance in a given match, and there are 5 players in a team. Additionally, there is one row after all players in the team denoting the team's overall performance, and can be easily identified by `NaN`  values for the player's name. Within the dataset, the columns we are interested in using are `gameid`, `result`, `minionkills`, `monsterkills`, `totalgold`, `firsttower`,  `firstmidtower`, `heralds`, `gameid`, `league`, `kills`, `dpm`, and `firsttothreetowers`.  

* `gameid` is a column that contains the id for every game played
* `league` stores the name of the league it was played in but also includes major tournaments that are not a part of the regular season
* `totalgold` is the total gold for each team
* `kills` stores the number of kills for each team
* `dpm` stores the damage per minute for each team
* `datacompleteness` is a column whose values are 'complete' and 'partial'. In our data cleaning and exploration section, we will be discussing why we chose to use 'complete' data.
* `result` is a boolean value denoting whether the team in question won the match or not.
* `minionkills` is the total amount of minions that a team killed in a match.
* `monsterkills` is the total number of monsters a team killed in a match.
* `firsttower` is a boolean value denoting whether or not the team in question destroyed the first tower in a match.
* `firstmidtower` is a boolean value denoting whether or not the team in question destroyed the first tower in the middle lane in a match.
* `firstherald` is a boolean value denoting whether or not the team in question killed the first herald, another objective that helps players to destroy towers.
* `firsttothreetowers` is a boolean value denoting whether or not the team in question beat their opponent to destroy 3 of the 11 towers.

## Data Cleaning and Exploratory Data Analysis  
For our dataset, we didn't want to spend too much time imputing large amounts of data, so we wanted to only look at games whose `datacompleteness` is 'complete'. This still gives us many observations to analyze, and allows us to keep moving in our project. We also wanted to look at the overall performance of team instead of a player-by-player analysis, so we filtered it further to only look at the summary rows for each team in a game.  
For our univariate analysis, we decided to plot the distribution of `minionkills` as a histogram, and we saw that it was roughly normal:  
![image](https://github.com/chriss-mo/League23/assets/156863651/c62e08ad-a4d2-4ea5-9e59-82509de6d8ec)  
For our bivariate analysis, we wanted to observe the relationship between being the first to getting three towers and winning the game, and we visualized this using a pie chart:  
![image](https://github.com/chriss-mo/League23/assets/156863651/c3b74e95-7264-440e-b1c0-f571b97e482a)  
One interesting aggregation we found was by grouping the dataframe by `firsttothreetowers` and taking the mean of the resulting dataframe. Some interesting statistics from this aggregation are the win rate at 79% for teams that were the first to three towers, and the first mid tower taken was 82% for teams that were the first to take three towers. The table is shown here:  
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>result</th>
      <th>firstmidtower</th>
      <th>totalgold</th>
      <th>heralds</th>
      <th>hextechs</th>
      <th>team kpm</th>
      <th>minionkills</th>
      <th>firsttower</th>
      <th>inhibitors</th>
      <th>pentakills</th>
      <th>playoffs</th>
      <th>kills</th>
      <th>monsterkills</th>
      <th>dpm</th>
    </tr>
    <tr>
      <th>firsttothreetowers</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0.0</th>
      <td>0.209221</td>
      <td>0.177204</td>
      <td>53245.981423</td>
      <td>0.641708</td>
      <td>0.244336</td>
      <td>0.334783</td>
      <td>805.232556</td>
      <td>0.224739</td>
      <td>0.373131</td>
      <td>0.006343</td>
      <td>0.231763</td>
      <td>10.787608</td>
      <td>166.909946</td>
      <td>2076.788635</td>
    </tr>
    <tr>
      <th>1.0</th>
      <td>0.790732</td>
      <td>0.822796</td>
      <td>60448.867777</td>
      <td>1.342851</td>
      <td>0.450487</td>
      <td>0.588414</td>
      <td>823.283594</td>
      <td>0.775210</td>
      <td>1.514616</td>
      <td>0.017562</td>
      <td>0.231815</td>
      <td>17.785407</td>
      <td>191.391910</td>
      <td>2466.149385</td>
    </tr>
  </tbody>
</table>  

## Assessment of Missingness  
The `Heralds` column in the dataset is NMAR. When looking into the data it has not dependency on other columns. It only depends on it's own column where if no heralds were taken then the value in the `Heralds` column is NaN. To find more about the data a column could be added called `Herald_Taken` which is true if a herald was taken and false it no herald was taken. This could be used to find a dependency in the missingness.  
We decided to try and see if the missingness of the `split` column depended on the `league` column in our dataset, since each league has multiple splits, and certain leagues may not have any splits. We performed a permutation test with the total variation distance as the test statistic on the following:  
*Null hypothesis:* The distribution of `league` when `split` is missing is the same as distribution of `league` when `split` is not missing.  
*Alternative hypothesis:* The distribution of `league` when `split` is missing is **not** the same as distribution of `league` when `split` is not missing.  
The results of this permutation test yielded a p-value of 0.0, which means we can reject the null hypothesis at the standard 5% significance level. The visualization is shown here:  
![image](https://github.com/chriss-mo/League23/assets/156863651/9692c6e9-fcdf-48a1-869a-fa853d4a48cc)  
We tried to see if the missingness of `split` depended on the `teamid` in our dataset by performing a permutation test with the total variation distance as the test statistic on the following:  
*Null hypothesis:* The distribution of `teamid` when `split` is missing is the same as distribution of `teamid` when `split` is not missing.  
*Alternative hypothesis:* The distribution of `teamid` when `split` is missing is **not** the same as distribution of `teamid` when `split` is not missing.  
The results of this permutation test yielded a p-value of 0.0, which means we fail to reject the null hypothesis at the standard 5% significance level. The visualization is shown here:  
![image](https://github.com/chriss-mo/League23/assets/156863651/df2a3a4a-5f8c-424a-bd09-2c3c33be2e0b)  

## Hypothesis Testing  
For our permutation test, we want to create an experiment with the following:
*Null hypothesis:* The `result` for teams with `firsttothreetowers` and the teams without `firsttothreetowers` are drawn from the same distribution.
*Alternate hypothesis:* The `result` for teams with `firsttothreetowers` and the teams without `firsttothreetowers` are **not** drawn from the same distribution.
To do this experiment, we shuffled the `result` column and used the difference of means as the test statistic. We found the the p-value of our observed statistic to be 0.0, which means we can reject the null hypothesis that the `result` for teams with `firsttothreetowers` and the teams without `firsttothreetowers` are drawn from the same distribution at the standard 5% significance level. The visualization of the test statistics and observed value is shown below:  
![image](https://github.com/chriss-mo/League23/assets/156863651/54a00797-c858-455f-8564-a387208c89cd)  

## Framing a Prediction Problem  
Our prediction model aims to answer the question: **How likely can a team get the first to three towers?** In other words, given the statistics that are an indicator of team strength, can we predict whether or not a team will be able to beat their opponent to destroy three towers?  
This problem is a binary classification problem, as we are trying to predict the binary column `firsttothreetowers`. For our test statistic, we will use accuracy, as we want to reduce false positives, and can easily do further analysis on false negatives. We want to use `kills`, `minionkills`, `monsterkills`, and `dpm` as our features, since they are indicators of team strength and we hypothesize that stronger teams will be the first to get three towers in a game.  

## Baseline Model  
Our baseline model consisted of the X_train and X_test having `kills`, `minionkills`, `monsterkills`, and `dpm` as columns, and our y_train and y_test simply being the `firsttothreetowers` column. All of the features we used are quantitative, so encoding was not necessary for our base model. We used a Decision Tree Classifier because we wanted a simple model to test our data on. The base model achieved an accuracy of 77%, which we believe is quite good, but can be improved on since the training accuracy is 100%, which suggests that our decision tree has overfit to the training data.  

## Final Model  
For our final model, we decided to add more features with `firsttower`, `firstmidtower`, and `totalgold`. `firsttower` and `firstmidtower` are binary nominal features, which are by nature encoded, while `totalgold` is quantitative and does not require encoding either. We believe that adding the features like `firsttower` and `firstmidtower` are good indicators of map control of a team in the game, which can be a good predicator of `firsttothreetowers`. Additionally, `totalgold` was added to provide a glimpse into a team's economic strength in a given game, which further supports the idea that stronger teams will get three towers first.  
We decided to use a Random Forest model instead of a decision tree in our base model, as we believed that a Random Forest model is less prone to overfit by design commpared to a decision tree. We also decided to incorporate some preprocessing techniques like a standard scaler to make sure that no column was weighted more than others, and we performed hyperparameter tuning and cross validation using `GridSearchCV` to give us the best parameters. We primarily experimented with the max depth (`max_depth`) of the trees and the number of trees (`num_estimators`) in the Random Forest, and found the best parameters to be a `max_depth` of 10 and `n_estimators` of 19. The accuracy of our best estimator was 84%, which is an improvement on the base model's performance. The confusion matrix of our best estimator is shown below:  
![image](https://github.com/chriss-mo/League23/assets/156863651/d2cea824-2c67-4996-9758-ef4444b75e6a)  

## Fairness Analysis  
For our fairness analysis, we wanted to see if our trained model performed better for teams with more kills. We set the threshold at 25 kills, and performed a permutation test on the following:  
*Null hypothesis:* Our model is fair. Its accuracy for teams with less than 25 kills and teams with 25 kills or more are roughly the same, and any differences are due to random chance.  
*Alternate hypothesis:* Our model is unfair. Its accuracy for teams with less than 25 total kills is lower than its precision for teams with 25 kills or more. Our p-value for this pemutation test was 0.31, which means we fail to reject the null hypothesis at the standard 5% significance level, and we can say that our model is not unfair based on the number of kills a team has. The visualization of our test results is shown below:  
![image](https://github.com/chriss-mo/League23/assets/156863651/ffdd982e-f36c-4324-839c-071faa7c0e0e)
