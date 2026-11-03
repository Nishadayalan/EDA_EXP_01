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
<img width="585" height="597" alt="image" src="https://github.com/user-attachments/assets/b3d5a1a6-582a-4aa7-979d-62e24a983a02" />



<img width="282" height="355" alt="image" src="https://github.com/user-attachments/assets/66151851-dbac-4090-8e03-c577a63cd8dc" />



<img width="308" height="222" alt="image" src="https://github.com/user-attachments/assets/b7659d38-6270-47f4-80c9-aefd427b3a12" />



<img width="286" height="361" alt="image" src="https://github.com/user-attachments/assets/fb8c0da0-3cd6-4c8f-9624-725f554c75f9" />



<img width="272" height="140" alt="image" src="https://github.com/user-attachments/assets/e388daf5-ba02-4fcc-abfd-3b49e0343862" />



<img width="458" height="407" alt="image" src="https://github.com/user-attachments/assets/54bc8fe8-387e-472e-95fc-8994207338ff" />



<img width="293" height="202" alt="image" src="https://github.com/user-attachments/assets/53e552d9-c9e0-448f-aed2-731a463ce118" />



<img width="397" height="638" alt="image" src="https://github.com/user-attachments/assets/d5f8c494-07a0-49e9-ad2d-a47433efb304" />



<img width="372" height="135" alt="image" src="https://github.com/user-attachments/assets/1c68faff-062f-4f5f-8c27-b98c797356cb" />



<img width="765" height="366" alt="image" src="https://github.com/user-attachments/assets/3be4a6ca-0153-400a-8967-0fa65568db84" />



<img width="236" height="122" alt="image" src="https://github.com/user-attachments/assets/acb299cf-59f4-4e46-9192-9503587397cf" />



<img width="216" height="205" alt="image" src="https://github.com/user-attachments/assets/f2cb2f59-35f5-4738-af0c-ec207f129843" />



 # Result:
 
  This experiment is executed successfully

