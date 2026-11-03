# Experiment 1: EDA in IPL Dataset

## Name : Nisha D
## Reg No. : 212223230143

# Aim:
To perform Exploratory Data Analysis (EDA) on the IPL matches dataset and derive insights about matches per season, winning teams, toss decisions, and top venues.

# Algorithm / Procedure:

**1.Import Libraries**
  Import pandas for data handling.
  Import matplotlib and seaborn for visualization.
  
**2.Load Dataset**
  Use pd.read_csv() to load the IPL matches dataset.
  Check dataset shape using .shape.
  View first 5 rows using .head().
**3.Matches per Season (Univariate Analysis)**

  Group data by season and count matches.
  Plot a bar chart to visualize growth/decline in matches.
  
**4.Top Winning Teams (Univariate Analysis)**
  Use value_counts() on the winner column.
  Plot top 5 winning teams in a bar chart.
  
**5.Toss Decisions (Univariate Analysis)**
  Count toss decision preferences (bat vs field).
  Plot results using a bar chart.
  
**6.Top Venues (Univariate Analysis)**
  Count matches per venue.
  Display top 5 venues with a horizontal bar chart.
  
**7.Draw Insights**
  Observe patterns in toss decisions.
  Identify teams with consistent winning trends.
  
  # Program 
```
import pandas as pd
# Load the IPL dataset
df = pd.read_csv("matches.csv")
```
```
print("\nQ1. DATASET UNDERSTANDING")
print("Rows:", df.shape[0])
print("Columns:", df.shape[1])
print(df.head())
```
```
print("\nQ2. COLUMNS AND DATA TYPES")
print(df.dtypes)
```
```
print("\nQ4. DATA CLEANING")
print("Missing values:")
print(df.isnull().sum()[df.isnull().sum() > 0])
print("Duplicate records before cleaning:", df.duplicated().sum())

df["city"] = df["city"].fillna("Unknown")
df = df.drop_duplicates().copy()

print("Missing city values after cleaning:", df["city"].isnull().sum())
print("Duplicate records after cleaning:", df.duplicated().sum())
```
```
print("\nQ5. SEASON ANALYSIS")
season_matches = df.groupby("season").size()
print(season_matches)
print("Season with most matches:", season_matches.idxmax())
print("Highest number of matches:", season_matches.max())
```
```
print("\nQ6. TOP 5 WINNING TEAMS")
team_wins = (
    df.dropna(subset=["winner"])
      .groupby("winner")
      .size()

      .sort_values(ascending=False)
)
print(team_wins.head(5))
```
```
print("\nQ7. CSK MATCHES")
csk = df[df["winner"] == "Chennai Super Kings"]
print(csk[["season", "team1", "team2", "winner"]].head(10))
print("Total CSK wins:", len(csk))
```
```
print("\nQ8. TOSS DECISION ANALYSIS")
toss_count = df["toss_decision"].value_counts()
toss_percent = df["toss_decision"].value_counts(normalize=True) * 100
print("Frequency:")
print(toss_count)
print("Percentage:")
print(toss_percent.round(2))
print("Most preferred:", toss_count.idxmax())
```
```
print("\nQ9. SEASON-WISE TOSS ANALYSIS")
toss_season = pd.crosstab(df["season"], df["toss_decision"])
print(toss_season)
print("Most preferred decision by season:")
print(toss_season.idxmax(axis=1))
```
```
print("\nQ10. TOP 5 VENUES")
venues = df.groupby("venue").size().sort_values(ascending=False)
print(venues.head(5))
```
```
print("\nQ11. WINNING MARGIN ANALYSIS")
margin_df = df.dropna(subset=["result_margin"])
largest = margin_df.loc[margin_df["result_margin"].idxmax()]
print("Largest winning margin:", largest["result_margin"])
print(largest[[
    "season", "team1", "team2", "winner",
    "result", "result_margin"
]])

print("\nTop 10 winning margins:")
print(
    margin_df.sort_values("result_margin", ascending=False)
    [["season", "team1", "team2", "winner", "result_margin"]]
    .head(10).to_string(index=False)
)
```
```
print("\nQ12. MATCH RESULT ANALYSIS")
print(df["result"].value_counts())
```
```
print("\nQ13. DATETIME CONVERSION")
print("Before conversion:", df["date"].dtype)
df["date"] = pd.to_datetime(df["date"], errors="coerce")
print("After conversion:", df["date"].dtype)
```
```
print("\nQ14. YEAR EXTRACTION")
df["year"] = df["date"].dt.year
print(df[["date", "year"]].head(10).to_string(index=False))
```
```
df.to_csv("IPL_Matches_Cleaned.csv", index=False)
print("\nCleaned dataset saved as IPL_Matches_Cleaned.csv")
```



  # Output
 ```

Q1. DATASET UNDERSTANDING
Rows: 1095
Columns: 20
       id   season        city        date match_type player_of_match  \
0  335982  2007/08   Bangalore  2008-04-18     League     BB McCullum   
1  335983  2007/08  Chandigarh  2008-04-19     League      MEK Hussey   
2  335984  2007/08       Delhi  2008-04-19     League     MF Maharoof   
3  335985  2007/08      Mumbai  2008-04-20     League      MV Boucher   
4  335986  2007/08     Kolkata  2008-04-20     League       DJ Hussey   

                                        venue                        team1  \
0                       M Chinnaswamy Stadium  Royal Challengers Bangalore   
1  Punjab Cricket Association Stadium, Mohali              Kings XI Punjab   
2                            Feroz Shah Kotla             Delhi Daredevils   
3                            Wankhede Stadium               Mumbai Indians   
4                                Eden Gardens        Kolkata Knight Riders   

                         team2                  toss_winner toss_decision  \
0        Kolkata Knight Riders  Royal Challengers Bangalore         field   
1          Chennai Super Kings          Chennai Super Kings           bat   
2             Rajasthan Royals             Rajasthan Royals           bat   
3  Royal Challengers Bangalore               Mumbai Indians           bat   
4              Deccan Chargers              Deccan Chargers           bat   

                        winner   result  result_margin  target_runs  \
0        Kolkata Knight Riders     runs          140.0        223.0   
1          Chennai Super Kings     runs           33.0        241.0   
2             Delhi Daredevils  wickets            9.0        130.0   
3  Royal Challengers Bangalore  wickets            5.0        166.0   
4        Kolkata Knight Riders  wickets            5.0        111.0   

   target_overs super_over method    umpire1         umpire2  
0          20.0          N    NaN  Asad Rauf     RE Koertzen  
1          20.0          N    NaN  MR Benson      SL Shastri  
2          20.0          N    NaN  Aleem Dar  GA Pratapkumar  
3          20.0          N    NaN   SJ Davis       DJ Harper  
4          20.0          N    NaN  BF Bowden     K Hariharan
```

```

Q2. COLUMNS AND DATA TYPES
id                   int64
season              object
city                object
date                object
match_type          object
player_of_match     object
venue               object
team1               object
team2               object
toss_winner         object
toss_decision       object
winner              object
result              object
result_margin      float64
target_runs        float64
target_overs       float64
super_over          object
method              object
umpire1             object
umpire2             object
dtype: object
```
```

Q4. DATA CLEANING
Missing values:
city                 51
player_of_match       5
winner                5
result_margin        19
target_runs           3
target_overs          3
method             1074
dtype: int64
Duplicate records before cleaning: 0
Missing city values after cleaning: 0
Duplicate records after cleaning: 0
```
```

Q5. SEASON ANALYSIS
season
2007/08    58
2009       57
2009/10    60
2011       73
2012       74
2013       76
2014       60
2015       59
2016       60
2017       59
2018       60
2019       60
2020/21    60
2021       60
2022       74
2023       74
2024       71
dtype: int64
Season with most matches: 2013
Highest number of matches: 76
```
```

Q6. TOP 5 WINNING TEAMS
winner
Mumbai Indians                 144
Chennai Super Kings            138
Kolkata Knight Riders          131
Royal Challengers Bangalore    116
Rajasthan Royals               112
dtype: int64
```
```

Q7. CSK MATCHES
     season                        team1                  team2  \
1   2007/08              Kings XI Punjab    Chennai Super Kings   
7   2007/08          Chennai Super Kings         Mumbai Indians   
11  2007/08          Chennai Super Kings  Kolkata Knight Riders   
14  2007/08  Royal Challengers Bangalore    Chennai Super Kings   
27  2007/08             Delhi Daredevils    Chennai Super Kings   
30  2007/08          Chennai Super Kings        Kings XI Punjab   
41  2007/08        Kolkata Knight Riders    Chennai Super Kings   
52  2007/08              Deccan Chargers    Chennai Super Kings   
56  2007/08          Chennai Super Kings        Kings XI Punjab   
62     2009  Royal Challengers Bangalore    Chennai Super Kings   

                 winner  
1   Chennai Super Kings  
7   Chennai Super Kings  
11  Chennai Super Kings  
14  Chennai Super Kings  
27  Chennai Super Kings  
30  Chennai Super Kings  
41  Chennai Super Kings  
52  Chennai Super Kings  
56  Chennai Super Kings  
62  Chennai Super Kings  
Total CSK wins: 138
```
```

Q8. TOSS DECISION ANALYSIS
Frequency:
toss_decision
field    704
bat      391
Name: count, dtype: int64
Percentage:
toss_decision
field    64.29
bat      35.71
Name: proportion, dtype: float64
Most preferred: field
```
```

Q9. SEASON-WISE TOSS ANALYSIS
toss_decision  bat  field
season                   
2007/08         26     32
2009            35     22
2009/10         39     21
2011            25     48
2012            37     37
2013            45     31
2014            19     41
2015            25     34
2016            11     49
2017            11     48
2018            10     50
2019            10     50
2020/21         27     33
2021            16     44
2022            15     59
2023            21     53
2024            19     52
Most preferred decision by season:
season
2007/08    field
2009         bat
2009/10      bat
2011       field
2012         bat
2013         bat
2014       field
2015       field
2016       field
2017       field
2018       field
2019       field
2020/21    field
2021       field
2022       field
2023       field
2024       field
dtype: object
```
```

Q10. TOP 5 VENUES
venue
Eden Gardens                                 77
Wankhede Stadium                             73
M Chinnaswamy Stadium                        65
Feroz Shah Kotla                             60
Rajiv Gandhi International Stadium, Uppal    49
dtype: int64
```
```

Q11. WINNING MARGIN ANALYSIS
Largest winning margin: 146.0
season                       2017
team1            Delhi Daredevils
team2              Mumbai Indians
winner             Mumbai Indians
result                       runs
result_margin               146.0
Name: 620, dtype: object

Top 10 winning margins:
 season                       team1                       team2                      winner  result_margin
   2017            Delhi Daredevils              Mumbai Indians              Mumbai Indians          146.0
   2016 Royal Challengers Bangalore               Gujarat Lions Royal Challengers Bangalore          144.0
2007/08 Royal Challengers Bangalore       Kolkata Knight Riders       Kolkata Knight Riders          140.0
   2015 Royal Challengers Bangalore             Kings XI Punjab Royal Challengers Bangalore          138.0
   2013 Royal Challengers Bangalore               Pune Warriors Royal Challengers Bangalore          130.0
   2019         Sunrisers Hyderabad Royal Challengers Bangalore         Sunrisers Hyderabad          118.0
   2023 Royal Challengers Bangalore            Rajasthan Royals Royal Challengers Bangalore          112.0
   2011             Kings XI Punjab Royal Challengers Bangalore             Kings XI Punjab          111.0
   2024       Kolkata Knight Riders              Delhi Capitals       Kolkata Knight Riders          106.0
2007/08            Delhi Daredevils            Rajasthan Royals            Rajasthan Royals          105.0
```
```

Q12. MATCH RESULT ANALYSIS
result
wickets      578
runs         498
tie           14
no result      5
Name: count, dtype: int64
```
```

Q13. DATETIME CONVERSION
Before conversion: object
After conversion: datetime64[ns]
```
```

Q14. YEAR EXTRACTION
      date  year
2008-04-18  2008
2008-04-19  2008
2008-04-19  2008
2008-04-20  2008
2008-04-20  2008
2008-04-21  2008
2008-04-22  2008
2008-04-23  2008
2008-04-24  2008
2008-04-25  2008
```

```
Cleaned dataset saved as IPL_Matches_Cleaned.csv
```


 # Result:
 
  This experiment is executed successfully

