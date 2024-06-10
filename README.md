# League of Legends Snowball Effect
Our data science and analysis project for DSC 80 at UCSD  

## Introduction  
In League of Legends, the primary objective is to destroy the Nexus, which is the only win condition. However, for a team to reach their opponent's Nexus, they must first destroy and get past their opponent's turrets, or towers. Each team has eleven towers on each side, which are spread out along the three lanes. Not all towers need to be destroyed to reach the nexus, just all in one lane. Towers are also an indicator of how successful a team is in a certain match. We wanted to answer the question: **Does getting the first towers improve the chance of a team winning?**  

The dataset we used was provided by [Oracle's Elixir](https://oracleselixir.com/tools/downloads), and we focused on the 2023 season statistics, as it was the most recent complete dataset. The dataset has 125,904 rows and 131 columns. Each row represents a player's performance in a given match, and there are 5 players in a team. Additionally, there is one row after all players in the team denoting the team's overall performance, and can be easily identified by `NaN`  values for the player's name. Within the dataset, the columns we are interested in using are `teamname`, `result`, `minoinkills`, `monsterkills`, `wardskilled`, `total cs`, `firsttower`,  `firstmidtower`, `firstherald`, and `firsttothreetowers`.  

* `teamname` is simply the name of the team that we are observing. We will not be using this in our calcualtion, but rather as an index to keep track of the teams.
* `datacompleteness` is a column whose values are 'complete' and 'partial'. In our data cleaning and exploration section, we will be discussing why we chose to use 'complete' data.
* `result` is a boolean value denoting whether the team in question won the match or not.
* `minionkills` is the total amount of minions that a team killed in a match.
* `monsterkills` is the total number of monsters a team killed in a match.
* `wardskilled` is the total number of wards a team killed in a match.
* `total cs` is simply `minionkills` + `monsterkills` + `wardkills`, which is an indicator of the team's overall strength.
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
|   firsttothreetowers |   result |   firstmidtower |   totalgold |   heralds |   hextechs |   team kpm |   minionkills |   firsttower |   inhibitors |   pentakills |   playoffs |   kills |   monsterkills |     dpm |
|---------------------:|---------:|----------------:|------------:|----------:|-----------:|-----------:|--------------:|-------------:|-------------:|-------------:|-----------:|--------:|---------------:|--------:|
|                    0 | 0.209221 |        0.177204 |     53246   |  0.641708 |   0.244336 |   0.334783 |       805.233 |     0.224739 |     0.373131 |   0.00634345 |   0.231763 | 10.7876 |        166.91  | 2076.79 |
|                    1 | 0.790732 |        0.822796 |     60448.9 |  1.34285  |   0.450487 |   0.588414 |       823.284 |     0.77521  |     1.51462  |   0.0175617  |   0.231815 | 17.7854 |        191.392 | 2466.15 |  


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
Our baseline model consisted of the X_train and X_test having `kills`, `minionkills`, `monsterkills`, and `dpm` as columns, and our y_train and y_test simply being the `firsttothreetowers` column. All of the features we used are quantitative, so encoding was not necessary for our base model. We used a Random Forest Classifier because we believed that the model would avoid fitting better than a standard decision tree.  
The base model achieved an accuracy of 0.74, which we believe is quite good, but can be improved on.  

## Final Model  
*coming soon*

## Fairness Analysis  
*coming soon*
