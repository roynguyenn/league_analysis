# League of Legends first tower Analysis
League of Legends Tower Analysis is an extensive data science study carried out at UCSD. The project navigates through several stages—from initial exploratory data analysis and statistical testing, to developing baseline predictive models and assessing fairness. Its main aim is to uncover how towers in League of Legends relate to crucial in-game metrics and overall match results.

## Introduction

### What is League of Legends?
League of Legends is without a doubt one of the world's most popular mulitplayer online battle arena (MOBA) game out there. A basic outline of how the game works includes two teams of five, where they face against each other to see who destroys the other home-base first! Each match has a map comprising of three lanes that leads to each team's base: top, middle, bottom. Within these lanes is this concept of "towers", a big statue that does damage to the enemy player if they get within range, essentially protecting a team's homebase. Destruction of towers not only grants a team a better economic advantage, but also a better strategic advantage.

This leads us into the central question we'll be investigating: "how well do the early game metrics contribute to teams getting the first tower in a match?". Where we'll be doing data analysis to unravel the correlation between the early game variables and whether or not that actually contributed to a team getting the first tower within their match. Later throughout this project, we'll be using these early game variables and more to see whether or not it is able to predict the amount of towers a team gets in a match. The models we're going to create down the line will influence strategic-decision making, prioritization of certain objectives, team compositions, which will all lead to better overall team performance throughout the game.

### Dataset Introduction
In our project, we are going to be working with Oracle Elixir's League of Legends professional match's history throughout 2022. This datatset consists of 150,588 rows alongside 161 columns. Of those columns, some of them are important metrics defining the key gameplay variables recorded from each match that would help go more in depth in answering our question and predictive analysis.

-firsttower: A binary flag indicating whether the team secured the first tower of the match (1 if they did, 0 if not).

-goldat10: The total gold a team has accumulated by the 10-minute mark.

-xpat10: The total experience points (XP) earned by a team at the 10-minute mark.

-csat10: The cumulative number of minions killed by a team in lane by the 10-minute mark.

-killsat10: The total number of enemy champions that a team has slain by the 10-minute mark.

-assistsat10: The total number of assists contributed by the team toward enemy champion kills at the 10-minute mark.

-deathsat10: The total number of times team members have died by the 10-minute mark.

-towers: The overall number of towers destroyed by the team throughout the match.

-monsterkills: The aggregate count of neutral monsters killed by the team during the match.

-visionscore: A metric reflecting how effectively the team maintained vision control across the map.

-goldspent: The total amount of gold expended by the team during the match.

-dragons, barons, heralds, elders: These columns indicate the number of each respective neutral objective secured by the team, with each objective providing distinct strategic advantages.


<iframe
  src="assets/distribution_killsat10.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

