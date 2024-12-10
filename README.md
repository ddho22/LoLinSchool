# League of Legends Red Versus Blue Analysis

## Introduction

### Background Information

League of Legends(LoL) is a popular video game developed by Riot Games that centers around working as a team to advance to your opponents base. LoL falls under the video game category of "multiplayer online battle arena" or MOBA for short.

MOBAs in general all follow the same basic principles. Each player on a team controls their own character, and together, the team aims to destroy their opponents base. Each team is also given a respective side, typically denoted as "red" or "blue" (Note: Some MOBAs do not explicitly display the side a team is on, in which case "true red" and "true blue" would be used in substitute).

The main focus of the project is on determining the degree of impact that the starting side has on the results of a professional match. This will be further elaborated in a further section.

### Data information

The underlying data will come from Tim Sevenhuysen's aggregated 2022 Professional LoL Esports data set on Oracle Elixir. The data set contains over 150,000 entries and 161 features. The features are very exhaustive and not all features are relevant to this project. The relevant features are as follows:

- `**side**`
