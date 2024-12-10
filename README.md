# League of Legends Red Versus Blue Analysis

## Introduction

### Background Information and Motivation

League of Legends(LoL) is a popular video game developed by Riot Games that centers around working as a team to advance to your opponents base. LoL falls under the video game category of "multiplayer online battle arena" or MOBA for short.

MOBAs in general all follow the same basic principles. Each player on a team controls their own character, and together, the team aims to destroy their opponents base. Each team is also given a respective side, typically denoted as "red" or "blue" (Note: Some MOBAs do not explicitly display the side a team is on, in which case "true red" and "true blue" would be used in substitute). The significance of the "side" a team starts on is because the map is not symetrical and interactions may be different based on starting "side".

The main focus of the project is to determine **if the side a team is playing on contributes to the results of a match.** The motivation behind this project is how in chess, the player starting with white is at a very minor advantage. This project serves to determine if the "side" creates a minor advantage for players, similarly to chess.

Author: Darren Ho

*Note: League of Legends and subsequently "LoL" will represent its mobile counterpart League of Legends Wild Rift*

### Data information

The underlying data will come from Tim Sevenhuysen's aggregated 2022 Professional LoL Esports data set on Oracle Elixir. The data set contains over 150,000 entries and 161 features. The features are very exhaustive and not all features are relevant to this project. The relevant features are as follows:

- `position`: (Str) The position that a player was during a game. Valid values are either "top", "jng", "mid", "bot", "sup", or "team". Entries where "position" is "team" represent aggregated data for a team throughout the duration of a game. For the purposes of this project, "team" entries will be used (Further addressed in Data Cleaning).

- `side`: (Str) The side that a team was on for the duration of a game. Values are either "Red" or "Blue". This feature is included as it is the primary focus of this project.

- `result`: (int) The outcome of a match. 1 representing a victory, and 0 representing a loss. This feature is included as it is also a primary focus of this project.

- `kills`: (int) The number of kills that a player or team made in a game. This feature is included as kills are a common indicator of performance.

- `deaths`: (int) The number of deaths that a player or team made in a game. This feature is included as deaths are used to regulate the impact of kills in later models.

- `assists`: (int) The number of assists that a player or team made in a game. Assists are recorded when a player does damage to an enemy that was killed by their teamate. This feature is included as it is also an indicator of performance.

- `totalgold`: (int) The total amount of gold that a player or team generated throughout the duration of a match. This feature is included as gold is a fundamental part of LoL and is a common indicator of performance beyond kills and deaths.

## Data Cleaning and Exploratory Data Analysis

### Data Cleaning

1. Given that my focus is at the team level, I selected only rows where the "position" was "team". This reduced the number of rows to just over 25,000.

2. To reduce the time and resources required in further steps, I selected only the relevant columns (position, side, result, kills, deaths, assists, and totalgold) mentioned above.

The resulting dataset has 25,030 rows and 6 columns. Furthemore, no data was missing in any of the columns.

Below is the head of the cleaned data frame that will be used for hypothesis testing and the prediction model:

### Univariate Analysis
I performed a univariate analysis of the total kills on a team. The resulting histogram is embeded below:

<iframe
  src="Assets/kills_distribution.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

The distribution seems to be bimodal, however the Bivariate Analysis explains the cause for this seemingly bimodal distribution.


### Bivariate Analysis
To explore the bimodal distribution from Univariate Analysis, I created conditional distrubtions of "kills" on "result":

<iframe
  src="Assets/winner_kills_distribution.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

<iframe
  src="Assets/loser_kills_distribution.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

The resulting distributions are almost normally distributed, being slightly right skewed. This suggests that kills, conditional on results, are distributed in a balanced manner. According to these distriutions, the distribution of kills for the team that won is to the right of the distribution of kills for the team that won. This is to be expected as higher kills are typically associated with winning a game.

I also performed a Bivariate Analysis of "result" conditioned on "side". The resulting plot is below:

<iframe
  src="Assets/wins_by_side.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

Based on the resulting plot, the team on the "Blue" side won nearly 5% more of the games than the "Red" side. This suggests that "side" may impact "resuult". This relationship will be further explored in later sections.

### Interesting Aggregates
Here is an interesting aggregate that I found:



Upon aggregating based on "result" I found that the winning team had 109% more kills and 122% more assits. However, the difference in the mean total gold was only ~20%. This suggests that the winning team is not as advantaged in gold as I would have expected, and that "totalgold" does not differ as much as kills and assists do. Furthermore, the difference between the increase in kills is less than the increase in assists, conditional on result. This suggests that wining teams fought better as a team than losing teams and one person did not individual outperform the rest of their team by a larger margin than the respective individual on the losing team.

## Assessment of Missingness

## Hypothesis Testing
The purpose of this hypothesis test is to determine if the blue team is statistically more likely to win or if the observed difference can be explained by random chance. This test is neccessary to determine if "side" will be a viable feature for determining the results of a game.

**Null Hypothesis:** The proportion of wins belonging to teams starting on the "Blue" side are the same as "Red". 

**Alterantive Hypothesis:** The proportion of wins belonging to teams starting on the "Blue" side are greater than those starting on the "Red" side.

**Test Statistic:** Proportion wins belonging to teams starting on the "Blue" side.

**Significance Level:** 10%

To perform this test, I did a hypothesis test using 0.5 as the probability of "Blue" winning. The probability is set to 0.5 as the null hypothesis states that the proportions of wins of "Red" and "Blue" winning are the same. The test statistic of proportions of wins belonging to "Blue" side is a valid test statistic as I am testing only the proportion of wins from "Blue" and not from red. 

Below is a histogram representing the results of this hypothesis test:

Based on this hypothesis test, the observed *p-value is 0*, leading me to reject the null hypothesis in favor of the alternative hypothesis. The results imply that the team starting on the "Blue" side may be more likely to win a match. This suggests that the starting side may impact the result of a match, which will be used in our prediction problem.

## Framing a Prediction Problem
From the hypothesis test, I found that there may exists a correlation between "side" and "result". However, there are likely several other columns where such a correlation with "result" eixists. As such, I would like to build a model on the Prediction Problem: *Can a team's outcome in a match be determined by their in-game statistics? *

The foundational principles of this model is a **binary classification**, determining between whether or not a team won or lost. The response variable will be the **"result"** of a match. To do this, I had to encode the "result" values using Binarizer. I chose to predict the result of a match because it is the most interesting to me to see if it is possible to predict a winner based on data alone. Or in other words, is there a pattern to winning statistics?

The metric that I am using is accuracy because the data is even distributed, with there being the same number of winning teams as losing teams in the dataset. I am not using precision nor recall as there is no true positive that I am trying to predict and care about both of the predicted values.

At the time of prediction, the only information that is available are the team's: **kills, deaths, assists, totalgold, and starting side.** These statistics were generated throughout the duration fo the game and will be used to train a Baseline and Final Model.

## Baseline Model
The Baseline Model is a RandomForestClassifier, with the default parameters. The features used to train teh first model are "kills" and "side". The only preprocessing that was done was to use Binarizer to encode the "side" column. 

To train the model, I split the data into 
