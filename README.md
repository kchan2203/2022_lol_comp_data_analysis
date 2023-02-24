# 2022_lol_comp_data_analysis

<h2>Introduction and Question Identification<h2>
The dataset we are working with contains statistics from all of the Official 2022 League of Legends Competitive Matches. League of Legends has the biggest esports scene of any game with its annual events racking in millions of viewers all over the world. We set out to find interesting trends to learn more about how teams win in such a competitive game. While asking and exploring many different ideas, we landed on the leading question of: **Do high action teams tend to win more.** Action is a fairly arbitrary term, so we decided to put our definition of action as a weighted summation of kills, kill assists, and damage per minute done by each player on a team, then averaged it across the team. The initial dataset has 149232 rows and 123 columns. The row count is semi-inflated, because there are 12 rows for each game, to represent each player's statistic in a 10 person game, as well as each team's statistics. 
There are two teams, a Blue team and a Red team, with 5 players each filling different roles in a game of League of Legends: top, jungle, middle, bot, and support. These are represented in the order they will appear in the rows. The first 5 rows of a game will be the 5 Blue team players. The next 5 rows of a game will be the 5 Red team players. Then the 11th row will be the Blue team's statistics as a group, and the 12th is the Red team's statistics as a group. 

Columns in this large dataset that are relevant to our analysis and that are important to understand are: 
1. league: represents the circuit that the competitive match is played in
2. patch: the version that the competitive match is played on, as League of Legends updates every two weeks.
3. participantid: a number from 1 to 10, or 100/200 depending on the position the player plays, and which team they are on.
4. playername: the individual player's in game name (IGN)
5. teamname: the team that the player is playing for's name
6. gamelength: the length of the competitive match in seconds
7. result: whether the individual/team won or lost the game (0 for loss, 1 for win)
8. kills: the amount of kills the individual/team gets
9. assists: the amount of assists the individual/team gets (an assist is obtained when someone assists the person getting the kill, either by helping with dealing damage, or supporting them)
10. dpm: damage dealt to enemy players over the amount of minutes in the game. 

##Cleaning and EDA##

##Assessment of Missingness##

##Hypothesis Testing##
