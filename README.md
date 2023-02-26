# Most Exciting Professional League in League of Legends
Orlando Fang's DSC 80 Project 3
by Orlando Fang (tifang@ucsd.edu)

## Introduction and Question Identification
The project aims performs an analysis on League of Legends Competitive Matches in 2022. Data obtained from Oracle’s Elixir. The dataset contains information of players and teams from over 10,000 League of Legends competitive matches.
The analysis will utilize the dataset and answer the following questions:
Looking at tier-one professional leagues, which league has the most “action-packed” games? 
Action-packed measured by the average total team kpm(kill per minute) over all games in the league.
Is the amount of “action” in this league significantly different than in other leagues? 
For league of legends players and esports fans, this dataset and question is important because it provides information on the "activeness" of games in each league. Like many traditional sports, audiences of League of Legends Competitive Matches, for example myself, would perfer to watch exciting and action-packed matches instead of boring matches where the match goes slowly. My based by assessment on team kpm because team kpm measures how often kills are achieved, and a high team kpm means that fights and kills are often, thus meaning that the matches are action-packed. This dataset can help new and old audiences to find the best league to watch if they want to watch exciting games.
These are a total 149232 rows and 123 columns in the original dataset. Of which I will need 6078 rows because I only need data on the games of tier-one leagues. I removed all rows regarding non-tier-one leagues and rows of individual players in each game. I only kept rows on team information, which contains values like team kpm.
In this analysis, I used the following columns:
- 'league', which is name of the league that the matches played in
- 'team kpm', the kills per minute for the entire team on a match
- 'teamkills', the total kills for the entire team on a match
- 'gamelength', the length of each match
- 'side', Blue or Red, represent the two opposing sides in League of Legends
- 'ban5', the 5th champion that the team decided to ban
- 'ban4', the 4th champion that the team decided to ban

## Cleaning and EDA (Exploratory Data Analysis)
### Data Cleaning
First I read in the csv file and generated a dataframe on the file. Because many columns are stored as 0 or 1 when they meant True or False, I want to change them to boolean. I first find all the columns that contains only two unique values, of which I found the columns that can only contains 0 or 1 and changed them to boolean where 1 is replaced by True and 0 is replaced by 0. For all the column names that start with 'first' I knew it need to be changed to boolean because the columns storing data on 'first' can only be either True, the team got first on something, or False, the team failed to be the first on reaching objectives.
As shown in the dataframe below, I've successfully changed data values to booleans.
Note that I only showed the columns with exactly 2 unique values because there is a total of 123 columns and I do want to show them all. Also the columns I need to perform analysis later does not contains any values that should be boolean.
|    | datacompleteness   |   year | playoffs   | side   | result   | firstblood   | firstbloodkill   | firstbloodassist   | firstbloodvictim   |   firstdragon |   firstherald |   firstbaron |   firsttower |   firstmidtower |   firsttothreetowers |\n|---:|:-------------------|-------:|:-----------|:-------|:---------|:-------------|:-----------------|:-------------------|:-------------------|--------------:|--------------:|-------------:|-------------:|----------------:|---------------------:|\n|  0 | complete           |   2022 | False      | Blue   | False    | False        | False            | False              | False              |           nan |           nan |          nan |          nan |             nan |                  nan |\n|  1 | complete           |   2022 | False      | Blue   | False    | True         | False            | True               | False              |           nan |           nan |          nan |          nan |             nan |                  nan |\n|  2 | complete           |   2022 | False      | Blue   | False    | False        | False            | False              | False              |           nan |           nan |          nan |          nan |             nan |                  nan |\n|  3 | complete           |   2022 | False      | Blue   | False    | True         | False            | True               | False              |           nan |           nan |          nan |          nan |             nan |                  nan |\n|  4 | complete           |   2022 | False      | Blue   | False    | True         | True             | False              | False              |           nan |           nan |          nan |          nan |             nan |                  nan |

Secondly, I removed all rows on individual players because I only need stats of each team on the entire game. Next, I removed all rows that are not matches in tier_one leagues because tier_one leagues matches are most popular and most audiences only care about tier_one leagues.
Tier-one leagues includes: 'LCK','LPL','LEC','LCS','PCS','VCS','CBLOL','LJL','LLA'
The resulting the dataframe is as following, containing only the columns I am going to use in my analysis
|    | league   |   team kpm |   teamkills |   gamelength | side   | ban5    | ban4     |\n|---:|:---------|-----------:|------------:|-------------:|:-------|:--------|:---------|\n|  0 | LPL      |     0.5714 |          13 |         1365 | Blue   | Camille | Jayce    |\n|  1 | LPL      |     0.2637 |           6 |         1365 | Red    | Rumble  | LeBlanc  |\n|  2 | LPL      |     0.9141 |          22 |         1444 | Blue   | Camille | Jayce    |\n|  3 | LPL      |     0.3324 |           8 |         1444 | Red    | Akali   | LeBlanc  |\n|  4 | LPL      |     0.3803 |          12 |         1893 | Blue   | Leona   | Nautilus |

Thirdly, I did not replace any data with NaN because this data is generated by matches where there can hardly be any missingness that are not missing at random. I scanned through the dataset and did not find any possible data that might need to be replaced with NaN. For all the zeros, they likely represent that the data value is actually 0 instead of missing.

### Univariate Analysis
The following histogram in a distribution of the counts of different team kpm values. From the plot I can see that the most frequent team kpm is between 0.27 to 0.2899. The distribution is unimodal and slightly skewed to the right, meaning that high team kpm are rare.
<iframe src="assets/teamkpm.html" width=800 height=600 frameBorder=0></iframe>

### Bivariate Analysis
The following scatter plot performs a bivariate analysis on the relationship between team kills per minute and team kills for the entire game. The plot shows a moderate positive correlation between team kills per minute and team kills, which means that when the total number of team kills increase, team kills per minute is also likely going to increase.
<iframe src="assets/teamkpm_over_teamkills.html" width=800 height=600 frameBorder=0></iframe>

### Interesting Aggregates
The following pivot table compares how does choosing different sides affect team kill per minute for different tier-one leagues. The significance of this table is that is shows the audiences which side is more action-packed, and audiences of different league can decide which side to watch based on their preference on action-packed matches. From the table we can see that the Red side teams in VCS have the highest mean team kpm, and for audiences of the PCS league, the Blue side is significantly more action-packed than the Red side.
| league   |     Blue |      Red |\n|:---------|---------:|---------:|\n| CBLOL    | 0.456072 | 0.418165 |\n| LCK      | 0.342713 | 0.357575 |\n| LCS      | 0.36656  | 0.366808 |\n| LEC      | 0.420367 | 0.384361 |\n| LJL      | 0.382385 | 0.39612  |\n| LLA      | 0.414917 | 0.388797 |\n| LPL      | 0.437027 | 0.402299 |\n| PCS      | 0.460437 | 0.372922 |\n| VCS      | 0.5154   | 0.538168 |

## Assessment of Missingness
### NMAR Analysis
I do not believe there is a column in my dataset that is NMAR. I looked into one of the URLs provided in the dataset and it seems like the data are collected and posted on the league's official websites.
Since the data generating process is probably not based on survey but based on some automated algorithms, it is unlikely to contain NMAR columns because a team or player cannot refuse to report their statistics just because they perform poorly in the match.
Also, as I looked through the data, it seems like all missingness are MD, where rows for players do have not values for columns that store values of the entire team and vice versa, MAR, where some leagues do not have values for some columns for all matches, for example LCK matches do not have URL, and possibly MCAR. Thus, it would be hardly possibly for any column to be NMAR.

### Missingness Dependency
As I search through all the columns that contains missing values, it seems to me that many columns have the same amount of missing values compared with other values, which I believe are MAR because some leagues do not record some values for all of their matches. Out of the columns with missing values, I chose to perform permutation tests to analyze the dependency of the missingness of the ban5 on other columns like the league column. Values in the ban5 column record the chamption that is being banned by the team for the match. Banning is the important part of professional games because is allows the team to ban a champion that the opponent prefers. The 5th ban, which is the last ban is especially important because it is the last step of the team's banning strategy. However, some values are missing. Missing values means that the team did not ban any champion for the 5th ban. First I can eliminate the possibility of MD and NMAR because the team can I cannot determine the missing value exactly by looking at the other columns, and it values itselves cannot be the reason for its missingness. Thus, I want to test if missingness of ban5 depend on other columns. I am interest in testing ban5 missingness on the league column because I wonder if some leagues are more likely to miss ban5. 
<iframe src="assets/ban5_missing_league.html" width=800 height=600 frameBorder=0></iframe>
From the distribution above, we can see that the probability of ban5 missingness for each league is quite different. LPL and CBLOL seems to have a higher change to missing ban5.
The next step would be performing permutation tests to analyze the dependency of the missingness of ban5 on league.
<iframe src="assets/ban5_missingness.html" width=800 height=600 frameBorder=0></iframe>
The distrition above is the result of 500 repetitions of permutation tests on the league column. I am comparing two distributions, the distribution of 'league' when 'ban5' is missing and the distribution of 'league' when 'ban5 is not missing. Since distributions of the league column are qualitative (categorical), I used the total variation distance (TVD) as my test statistics. From the distribution we can see that for most repetitions, the TVDs are lower then the observed TVD. 
However, the p-value is 0.16, hence, we conclude that the missingness in the 'ban5' column is not dependent on 'league'.

This does not mean that ban5 missingness is MCAR, when I performed permutation tests on different columns, column ban4 results in a p-value of 0.026, so at a significance level of 0.05 we can reject the null hypothesis the distribution of 'ban4' when 'ban5' is missing is the same as the distribution of 'ban4' when 'ban5' is not missing, and thus conclude that ban5 missingness is MAR.
<iframe src="assets/ban5_missing_ban4.html" width=800 height=600 frameBorder=0></iframe>
I used TVD as test statistics because ban4 contains champion names which are categorical. For most repetitions, the TVDs are lower then the observed TVD. Along with the low p-value, we can conclude that different values for ban4 can lead to different possibility for ban5 to be missing.

## Hypothesis Testing
null hypothesis: League and team kpm are not related – the amount of “action” in the most "action-packed" tier-one league is due to chance alone
alternative hypothesis: League and team kpm are related – the amount of “action” in the most "action-packed" tier-one league is not due to chance alone
The null and alternative hypothesis would be useful in answer my question because it can determine whether or not the amount of “action” in the most "action-packed" league significantly different than in other leagues, and audiences who want to watch "action-packed" matches would want to make sure if League and team kpm are related or not so that they can decide whether or not to watch matches based on the league.
Average team kpm is my test statistic because team kpm is continuous, and the average of team kpm would allow me to provide audiences expected numbers of team kpm for all matches for each league.
Significance level of 0.01 to be highly significant and to minimize the risk of concluding that a difference exists when there is no actual difference.
After grouping the data by leagues and sorting the values, I found that VCS league has the highest team kpm mean.
To test if this result is due to chance alone, I performed the following hypothesis test.
I drew 10000 samples of size 646 from all team kpm, where 646 is the count for the number of games in VCS league. I got the following distribution where the red line is the observed team kpm mean.
<iframe src="assets/hypo_test.html" width=800 height=600 frameBorder=0></iframe>
Because I am testing if the high team kpm mean for VCS is due to chance alone, I compute p-value by find the mean of the number of repetitions that is larger than my observed mean.
The resulting p-value is 0.0
Since the p-value is smaller than the significance level of 0.01, we reject the null.