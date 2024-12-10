# League of Legends Red Versus Blue Analysis

## Introduction

### Background Information and Motivation

League of Legends(LoL) is a popular video game developed by Riot Games that centers around working as a team to advance to your opponents base. LoL falls under the video game category of "multiplayer online battle arena" or MOBA for short.

MOBAs in general all follow the same basic principles. Each player on a team controls their own character, and together, the team aims to destroy their opponents base. Each team is also given a respective side, typically denoted as "red" or "blue" (Note: Some MOBAs do not explicitly display the side a team is on, in which case "true red" and "true blue" would be used in substitute). The significance of the "side" a team starts on is because the map is not symetrical and interactions may be different based on starting "side".

The main focus of the project is to determine **if the side a team is playing on contributes to the results of a match.** The motivation behind this project is how in chess, the player starting with white is at a very minor advantage. This project serves to determine if the "side" creates a minor advantage for players, similarly to chess.

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

# Data Cleaning
