# 2022 LOL Competitive Match Analysis

## Introduction and Question Identification
The dataset we are working with contains statistics from all of the Official 2022 League of Legends Competitive Matches. League of Legends has the biggest esports scene of any game with its annual events racking in millions of viewers all over the world. We set out to find interesting trends to learn more about how teams win in such a competitive game. While asking and exploring many different ideas, we landed on the leading question of: **Do high action teams tend to win more?** Action is a fairly arbitrary term, so we decided to put our definition of action as a weighted summation of kills, kill assists, and damage per minute done by each player on a team, then averaged it across the team. The initial dataset has 149232 rows and 123 columns. The row count is semi-inflated, because there are 12 rows for each game, to represent each player's statistic in a 10 person game, as well as each team's statistics. 
There are two teams, a Blue team and a Red team, with 5 players each filling different roles in a game of League of Legends: top, jungle, middle, bot, and support. These are represented in the order they will appear in the rows. The first 5 rows of a game will be the 5 Blue team players. The next 5 rows of a game will be the 5 Red team players. Then the 11th row will be the Blue team's statistics as a group, and the 12th is the Red team's statistics as a group. 

Columns in this large dataset that are relevant to our analysis and that are important to understand are: 
1. league: represents the circuit that the competitive match is played in
2. patch: the version that the competitive match is played on, as League of Legends updates every two weeks.
3. participantid: a number from 1 to 10, or 100/200 depending on the position the player plays, and which team they are on.
4. side: Blue or Red, depending on the side that the team is
5. position: the role the individual/team plays. Can be either [top/jng/mid/bot/sup/team]
6. playername: the individual player's in game name (IGN)
7. teamname: the team that the player is playing for's name
8. gamelength: the length of the competitive match in seconds
9. result: whether the individual/team won or lost the game (0 for loss, 1 for win)
10. kills: the amount of kills the individual/team gets
11. assists: the amount of assists the individual/team gets (an assist is obtained when someone assists the person getting the kill, either by helping with dealing damage, or supporting them)
12. dpm: damage dealt to enemy players over the amount of minutes in the game.
13. monsterkillsownjungle: the amount of monsters that are killed on the side of the map belonging to their own team. These have values kept as NaN for our missingness test. 

## Cleaning and EDA

### Data Cleaning
This dataset is not perfect, as such we to clean it before we can use it. For one, there were many NaN values in this dataset. However, they were in columns that we did not use. The dataset has a lot of information and statistics in it, more than we used in our analysis. So we will trim down the amount of columns we are working with to the 13 we listed above. This got rid of all of the NaN values, and made our workspace cleaner. Additionally, we had the column result that was in 0 and 1s, which we changed to True/False.

We also split our data into two dataframes, one with the data from the team's performance in the game (participantid = 100/200), and with the data from the player's perfomrance in each game. We had to further clean the dataframe about the teams to remove the playername column from it, as they ended up all being NaN, as they are Missing by Design(MD) 

**Team DataFrame**
<iframe src="assets/teams_df_head.html" width=800 height=600 frameBorder=0></iframe>

**Players DataFrame**
<iframe src="assets/players_df_head.html" width=800 height=600 frameBorder=0></iframe>

### Univariate Analysis
<iframe src="assets/game_length_distribution.html" width=800 height=600 frameBorder=0></iframe>
We decided to graph distribution of the length of the games(seconds) using a histogram. We see that the distribution of game lengths was roughly normal throughout the year, though slightly more skewed for longer games than shorter ones. 

<iframe src="assets/kills_distribution.html" width=800 height=600 frameBorder=0></iframe>
We also decided to graph the distribution for the number of kills in a game using a histogram. We can observe from this that it has double peaks and is slightly skewed toward more kills. 

### Bivariate Analysis
<iframe src="assets/wr_vs_time.html" width=800 height=600 frameBorder=0></iframe>
We decided to graph a team's average game length vs the team's average winrate in a scatter plot to see if there is a correlation. We see here that there is no strong correlation in a team's average game time affecting that team's winrate. So we decided to not proceed with this question

<iframe src="assets/wr_vs_patch.html" width=800 height=600 frameBorder=0></iframe>
(For a better viewing experience, double click one of the teams to only see that team's winrate graphed, instead of all of them by default)

For the other bivariate analysis, we wanted to look at how a team's winrate changes in every version of the game in a line graph, since patches can be represented as periods of time. It gave us an idea of a team's performance throughout the year, and how the state of game balance affected them. 

## Interesting Aggregates
 
<iframe src="assets/team_wr_side_pivot.html" width=800 height=600 frameBorder=0></iframe>

We made a pivot table comparing the winrates of teams split between when they got blue side and when they got red side. Though it may seem irrelevant which side you get, League of Legends is not perfectly balanced between the two sides you can roll, and there have been arguments even recently about whether one side has an unfair advantage or not in professional play. Here we can see that some teams have drastically different winrates, depending on the side they play on. The circuits try to balance this by giving side selection to each team once, by coin toss, or by seeding. 

## Assessment of Missingness 
Assessment of Missingness
We do not believe that there are columns that are NMAR, which means there doesn’t seem to be any columns that are missing based on the value of missingness itself. Actually many of the columns are missing by design because they do not apply to that row. For example, there are rows that are made up of aggregated team stats during a game, while there are rows that represent the stats of an individual player during a game. This results in certain statistics not making sense for player or team rows. Therefore, there will be missing values in playername for team rows and the same applies for many other NaNs values.

To see if we could use monsterkillsownjungle column for our calculation of ACS, we wanted to test its missingness. If the value was missing depending on a certain column, it could affect the ACS calculation and be a bad indicator of ACS. 
First, we performed a missingness permutation test on the kills. Both of our permutation tests have a significance value of 0.01. Our null hypothesis would be the distribution of ‘kills’ when ‘monsterkillsownjungle’ is missing the the same as the distribution of ‘kills’ when the ‘monsterkillsownjungle’ is not missing

<iframe src="assets/hist_kills_by_missing.html" width=800 height=600 frameBorder=0></iframe>

Since the centers of kills between rows with missing monsterkillsownjungle and rows without missing values for monsterkillsownjungle is so similar, it would be better to compare the distribution of values. Therefore, we will be using a KS statistic. 
The p-value we eventually ended up with 0.4075. We fail to reject the null hypothesis due to 0.01 < 0.4075, so it seems like the ‘monsterkillsownjungle’ column is not dependent on ‘kills’. Therefore, the ‘monsterkillsownjungle’ column, according to this test, seems to be missing completely at random

Next, we performed a missingness permutation test on league(aka region). Our null hypothesis would be that the proportion of missingness in ‘monsterkillsownjungle’ is the same across all leagues. Our null hypothesis is that the proportion of missingness is different across different leagues. Since we are using proportions, we will use a TVD as our test statistic

<iframe src="assets/bar_region_missingness.html" width=800 height=600 frameBorder=0></iframe>

Our p-value is 0. We reject the null hypothesis due to 0.01 > 0.0, so it seems like the ‘monsterkillsownjungle’ column is dependent on ‘league. Therefore, the ‘monsterkillsownjungle’ column seems to be MAR according to this test.


## Hypothesis Testing
**Our Null Hypothesis:** "In our population, ACS of teams that tend to win vs teams that tend to lose have the same distribution and any differences in our samples are due to random chance"  <br>  
**Alternative hypothesis:** "In our population, the ACS of teams that tend to win are higher compared to teams that tend to lose and any differences cannot be explained by random chance."
<iframe src="assets/acs_by_groups_hist.html" width=800 height=600 frameBorder=0></iframe>

We decided to use a difference in group means where the groups are high and low. High means that the winrate is higher than or equal to 50% or 0.5, while low is lower than a 50% winrate. We set our significance level to 0.01. After running a permutation test, we got a p-value of 0. The p-value tells us that there is close to a 0% chance that the difference in acs between high and low winrate teams is just due to random chance. Since our significance level is 0.01, we reject the null hypothesis that the high and low winrate teams were drawn from the same distribution for acs. Using a difference in group means is a good choice because the visualization of the distributions of ACS between high and low win rate teams show that their centers are quite different. A real difference in group means that there is evidence that teams with higher ACS also have higher win rates.

<iframe src="assets/Group_Means_in_ACS.html" width=800 height=600 frameBorder=0></iframe>
The red line represents our observed statistic, while the histogram represents the plot of the 1000 test statistis we made from the random permutation test. As you can see there are no generated test statistics that are either more extreme or equal to our observed test statistic. 


