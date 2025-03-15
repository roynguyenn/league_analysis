# League of Legends first tower Analysis
League of Legends Tower Analysis is an extensive data science study carried out at UCSD. The project navigates through several stages—from initial exploratory data analysis and statistical testing, to developing baseline predictive models and assessing fairness. Its main aim is to uncover how towers in League of Legends relate to crucial in-game metrics and overall match results.

by: Royce Nguyen (ron005@ucsd.edu)

## Introduction

### What is League of Legends?
League of Legends is without a doubt one of the world's most popular mulitplayer online battle arena (MOBA) game out there. A basic outline of how the game works includes two teams of five, where they face against each other to see who destroys the other home-base first! Each match has a map comprising of three lanes that leads to each team's base: top, middle, bottom. Within these lanes is this concept of "towers", a big statue that does damage to the enemy player if they get within range, essentially protecting a team's homebase. Destruction of towers not only grants a team a better economic advantage, but also a better strategic advantage.

This leads us into the central question we'll be investigating: **"how well do the early game metrics contribute to teams getting the first tower in a match?"**. Where we'll be doing data analysis to unravel the correlation between the early game variables and whether or not that actually contributed to a team getting the first tower within their match. Later throughout this project, we'll be using these early game variables and more to see whether or not it is able to predict the amount of towers a team gets in a match. The models we're going to create down the line will influence strategic-decision making, prioritization of certain objectives, team compositions, which will all lead to better overall team performance throughout the game.

### Dataset Introduction
In our project, we are going to be working with Oracle Elixir's League of Legends professional match's history throughout 2022. This datatset consists of 150,588 rows alongside 161 columns. Of those columns, some of them are important metrics defining the key gameplay variables recorded from each match that would help go more in depth in answering our question and predictive analysis.

- `gameid`: A unique identifier for every profession match played in 2022.

- `firsttower`: A binary flag indicating whether the team secured the first tower of the match (1 if they did, 0 if not).

- `goldat10`: The total gold a team has accumulated by the 10-minute mark.

- `xpat10`: The total experience points (XP) earned by a team at the 10-minute mark.

- `csat10`: The cumulative number of minions killed by a team in lane by the 10-minute mark.

- `killsat10`: The total number of enemy champions that a team has slain by the 10-minute mark.

- `assistsat10`: The total number of assists contributed by the team toward enemy champion kills at the 10-minute mark.

- `deathsat10`: The total number of times team members have died by the 10-minute mark.

- `towers`: The overall number of towers destroyed by the team throughout the match.

- `monsterkills`: The aggregate count of neutral monsters killed by the team during the match.

- `visionscore`: A metric reflecting how effectively the team maintained vision control across the map.

- `goldspent`: The total amount of gold expended by the team during the match.

- `dragons, barons, heralds, elders`: These columns indicate the number of each respective neutral objective secured by the team, with each objective providing distinct strategic advantages.

## Data Cleaning and Exploratory Data Analysis
For this part, we'll be keeping the relevant columns to help explore our question more in depth. We'll be keeping the columns: 'firsttower', 'goldat10', 'side', 'gamelength', 'xpat10', 'csat10', 'killsat10', 'assistsat10', 'deathsat10'. The dataset is organized so that in each match, there are 5 rows each representing a player in a team then a row that represents the team aggregate of the data. Since there are 2 teams, we'll have 12 rows in total for each match. For this part, we'll only be needing all of the team aggregate rows which will be used in our analysis here. Reason why we are using "at10" columns is because first tower usually gets taken after 10 minutes (although I acknowledege there are instances where this is not the case, however those are outliers)

Here is the top 5 rows of our cleaned dataset:

| firsttower | side | gamelength | csat10 | killsat10 | deathsat10 |
|------------|------|------------|--------|-----------|------------|
| 1.0        | Blue | 1713       | 322.0  | 3.0       | 0.0        |
| 1.0        | Red  | 2114       | 344.0  | 3.0       | 1.0        |
| 1.0        | Blue | 1972       | 368.0  | 0.0       | 1.0        |
| 1.0        | Blue | 2488       | 297.0  | 4.0       | 2.0        |
| 1.0        | Blue | 2020       | 331.0  | 0.0       | 2.0        |

(This dataframe will also be used for our missingness and hypothesis test)

### Univariate Analysis
Distribution of single columns

Below is distribution of kills at 10 between all teams
<iframe
  src="assets/distribution_killsat10.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
The histogram shows us that most teams have relatively few kills near the start of the game, with the distribution being heavily skewed to the right. Majority of teams will have between 0-5 kills with a small fraction having more than that, and its unimodal with peak at 1 kill.

Here, we also plotted the distribution of cs (minion kills) at 10 minutes across all teams
<iframe
  src="assets/distribution_csat10_new.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
The distribution here shows us that it is approximately normal. This tells us that within the early game, majority of teams will get around 320 minion kills which will stay consistent and useful for analyzing team behavior in the early game.

### Bivariate Analysis

Here, I created a bivariate graph, demonstrating the relationship between the amount of kills to cs in the early game.
<iframe
  src="assets/distribution_cs_and_kills.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
This scatterplot reveals the slightly negative correlation it has between kills and cs at 10 minutes. This suggests that when teams are more focused on slaying enemy champions, they are sacrificing minion kills.

Another bivariate plot I've created is the distribution of cs kills at 10 minutes between teams who got first tower and didn't.
<iframe
  src="assets/distribution_cs_btwn_towers.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
In this plot, we see a slight shift in the distribution, where teams that did get first tower had the distribution shifted to the right. Indicating that teams that gets the first tower on average would have a higher minion kill score by 10 minutes. (we'll see later on that this can also visualize our hypothesis test)

### Aggregate Comparison Amongst Teams
Here, I've aggregated data using the mean of popular team matches, comparing them side by side where they did get first tower to matches where they didn't.

This is the aggregate statistics of teams where they did get first tower

| firsttower | teamname              | csat10  | killsat10 | assistsat10 | deathsat10 |
|------------|-----------------------|---------|-----------|-------------|------------|
| 1.0        | DRX                   | 331.06  | 1.70      | 3.08        | 1.49       |
| 1.0        | Dplus KIA             | 327.27  | 1.82      | 3.04        | 1.26       |
| 1.0        | Evil Geniuses         | 324.90  | 2.35      | 3.58        | 1.42       |
| 1.0        | T1                    | 335.03  | 2.15      | 3.33        | 1.69       | 
| 1.0        | Team Liquid Academy   | 323.03  | 2.41      | 3.40        | 1.72       | 

This is the aggregate statistics of the same teams that didn't get first tower

| firsttower | teamname            | csat10 | killsat10 | assistsat10 | deathsat10 |
|------------|---------------------|--------|-----------|-------------|------------|
| 0.0        | DRX                 | 324.87 | 1.22      | 1.97        | 1.97       |
| 0.0        | Dplus KIA           | 329.82 | 1.30      | 2.07        | 1.64       |
| 0.0        | Evil Geniuses       | 318.04 | 1.50      | 2.19        | 2.08       |
| 0.0        | T1                  | 322.77 | 1.77      | 3.03        | 2.38       |
| 0.0        | Team Liquid Academy | 320.20 | 1.49      | 2.43        | 2.09       |

Here, I grouped by the firsttower and teamname, then I got its mean aggregate for each team within each first tower categorization. Comparing the two, we see that the same teams that got first tower actually had higher killsat10, assistsat10, and lower deathsat10. Indicating domination of the early game which could lead to first tower being taken.

## Assessment of Missingness

### NMAR Analysis

In our dataset, I believe that columns that are NMAR (Not Missing at Random) would be doublekills', 'triplekills', 'quadrakills', 'pentakills'. Reason why I think this is the case is because it depends on whether or not the actual player gets those type of kills consecutively. Support role players usually wouldn't get anything higher than a triplekill, making it so these values appear as missing is because the player can't even get those type of kills to begin with. An extra column we might add to make this MAR, is a 'had_killstreak' column, where it's a binary indicator of whether or not the player had 2 or more kills in a row in a match. 1 indicating that they did, 0 if not. 

### Missingness Dependency

Here, we're going to see whether or not missing values in 'firsttower' is dependent on other columns. The other two columns we'll be using are 
'gamelength' and 'side'.

I'm going to perform a permutation test on firsttower missingness and gamelength.

**Null Hypothesis**: distribution of 'gamelength' is the same when 'firsttower' is missing and when 'firsttower' is not missing.
**Alternative Hypothesis**: distribution of 'gamelength' is **NOT** the same when 'firsttower' is missing and when 'firsttower' is not missing.

Here is the distribution of our test statistic (absolute difference in means)
<iframe
  src="assets/missingness_gamelength.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
Here, I found out that the observed test statistic is 47.24143596845647, with a p-value of 0.0, meaning that we REJECT the null hypothesis in favor of the alternative since it is below our minimum threshhold of 0.05 (we use standard alpha = 0.05 here). So 'firsttower' missingness is dependent on the gamelength column.

Another permutation test I performed was on 'firsttower' missingness and 'side'.

**Null Hypothesis**: distribution of 'side' is the same when 'firsttower' is missing and when 'firsttower' is not missing.
**Alternative Hypothesis**: distribution of 'side' is **NOT** the same when 'firsttower' is missing and when 'firsttower' is not missing.

Heres the distribution of our test statistic in this test (Total Variation Distance)
<iframe
  src="assets/missingness_side.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
Here, the observed test statistic (TVD) is 0.0, with a p-value of 1. This means that we accept the null and we can confidently say that the missingness of 'firsttower' does not depend on 'side' column. 

## Hypothesis Testing

