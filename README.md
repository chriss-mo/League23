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
One interesting aggregation we found was by grouping the dataframe by `firsttothreetowers` and taking the mean of the resulting dataframe. Some interesting statistics from this aggregation are the win rate at 79% for teams that were the first to three towers, and the first mid tower taken was 82% for teams that were the first to take three towers. 

## Assessment of Missingness  
The `Heralds` column in the dataset is NMAR. When looking into the data it has not dependency on other columns. It only depends on it's own column where if no heralds were taken then the value in the `Heralds` column is NaN. To find more about the data a column could be added called `Herald_Taken` which is true if a herald was taken and false it no herald was taken. This could be used to find a dependency in the missingness.  

## Hypothesis Testing  
*coming soon*  

## Framing a Prediction Problem  
*coming soon*  

## Baseline Model  
*coming soon*  

## Final Model  
*coming soon*

## Fairness Analysis  
*coming soon*
