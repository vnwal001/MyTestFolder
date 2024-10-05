# Course: CS 625
## Name: Victor Nwala
## Homework 3

**Due:** October 6, 2024 by 11:59 PM

### Report

### Part 1: PRE-PROCESSING



- 1) I used the real-world data of VIDEO GAMES SALES of various publishers. My intent was to focus on the Nintendo Brand and from the years 2001 to 2010
- 2) I deleted the rows with 0 sales across any region listed in the data.
- 3) This reduces my dataset from over 16,000 rows to just 203 rows
- *Please note that the data pre-processing steps are shown within my python code in each question answered*

  
### Part 2: MY CHARTS

#### 1. STACKED BAR CHART 
<img src="https://github.com/vnwal001/MyTestFolder/blob/main/stackedBar.png" alt="Sales Across Regions (2001-2010)" width="989" height="590">

**Answer:**
   ```
   import matplotlib.pyplot as plt
   import pandas as pd
   df = pd.read_csv("https://raw.githubusercontent.com/vnwal001/MyTestFolder/refs/heads/main/vgsales.csv", sep=",")
   df = df[(df != 0).all(axis=1)]
   df = df[(df['Year'].between(2001, 2010)) & (df['Publisher'] == 'Nintendo')]
   df['Year'] = df['Year'].astype(int)
   # Filter for years between 2001 and 2010
   df_filtered = df[(df['Year'] >= 2001) & (df['Year'] <= 2010)]
   df_filtered.to_csv('output.csv', index=False, sep=',', header=True, encoding='utf-8')
   # Group by Year and sum the sales
   sales_by_year = df_filtered.groupby('Year')[['NA_Sales', 'JP_Sales', 'EU_Sales', 'Other_Sales']].sum().reset_index()
   # Create a stacked bar plot
   plt.figure(figsize=(10, 6))
   plt.bar(sales_by_year['Year'].astype(str), sales_by_year['NA_Sales'], label='NA Sales', color='blue')
   plt.bar(sales_by_year['Year'].astype(str), sales_by_year['JP_Sales'], bottom=sales_by_year['NA_Sales'], label='JP Sales', color='orange')
   plt.bar(sales_by_year['Year'].astype(str), sales_by_year['EU_Sales'], bottom=sales_by_year['NA_Sales'] + sales_by_year['JP_Sales'], label='EU Sales', color='green')
   plt.bar(sales_by_year['Year'].astype(str), sales_by_year['Other_Sales'], 
         bottom=sales_by_year['NA_Sales'] + sales_by_year['JP_Sales'] + sales_by_year['EU_Sales'], 
         label='Other Sales', color='red')
   # Add labels and title
   plt.title('Stacked Bar Plot of Nintendo Sales by Year (2001-2010)')
   plt.xlabel('Year')
   plt.ylabel('Total Sales (in millions)')
   plt.xticks(rotation=45)
   plt.legend()
   plt.tight_layout()
   plt.show()
   ```

  Idiom: Bar Chart / Mark: Bar
| Data: Attribute | Data: Attribute Type  | Encode: Channel | 
| --- |---| --- |
| Total Sales | key, categorical | length of bars (y-axis) |
| Year | value, quantitative | horizontal spatial region (x-axis) |

#### 2. MULTI-LINE CHART 

<img src="https://github.com/vnwal001/MyTestFolder/blob/main/MultiLineChart.png" alt="Total Nintendo Sales Across Regions (2001-2010)" width="989" height="590">

**Answer:**

```
import matplotlib.pyplot as plt
import pandas as pd
df = pd.read_csv("https://raw.githubusercontent.com/vnwal001/MyTestFolder/refs/heads/main/vgsales.csv", sep=",")
df = df[(df != 0).all(axis=1)]
df = df[(df['Year'].between(2001, 2010)) & (df['Publisher'] == 'Nintendo')]
df['Year'] = df['Year'].astype(int)
# Filter for years between 2001 and 2010
df_filtered = df[(df['Year'] >= 2001) & (df['Year'] <= 2010)]
# Group by Year and sum the sales
sales_by_year = df_filtered.groupby('Year')[['NA_Sales', 'JP_Sales', 'EU_Sales', 'Other_Sales']].sum().reset_index()
# Create a multi-line plot
plt.figure(figsize=(10, 6))
# Plot each sales category
plt.plot(sales_by_year['Year'].astype(str), sales_by_year['NA_Sales'], label='NA Sales', marker='o', color='blue')
plt.plot(sales_by_year['Year'].astype(str), sales_by_year['JP_Sales'], label='JP Sales', marker='o', color='orange')
plt.plot(sales_by_year['Year'].astype(str), sales_by_year['EU_Sales'], label='EU Sales', marker='o', color='green')
plt.plot(sales_by_year['Year'].astype(str), sales_by_year['Other_Sales'], label='Other Sales', marker='o', color='red')
# Add labels and title
plt.title('Multi-Line Plot of Sales by Year (2001-2010) For Nintendo')
plt.xlabel('Year')
plt.ylabel('Total Sales (in millions)')
plt.xticks(rotation=45)
plt.legend()
plt.tight_layout()
plt.grid()

# Show the plot
plt.show()
```

#### 4. Create a New Column "Verdict"

**Answer:**
I created a new column "Verdict" based on the "Rating" column:

| Rating       | Verdict     |
|--------------|-------------|
| 0            | Not known   |
| >0 and <=4.5 | Flop        |
| >4.5 and <=6.5| Average    |
| >6.5 and <=8.0| Hit        |
| >8.0         | Super Hit   |

**Transformations:**
-  Created a new column, Vedict based on column RATING by filling with it "Not Known"  for 0 values, GREL,  `if(value == 0, "Not known", value)`
- `if(value > 8.0, "Super Hit", value)` if value is greater than 8 in Verdict replace with "Super Hit"
- `if(isNumeric(value), if ((value > 0).and(value <= 4.5), "Flop", value), value)` Replace values >0 and <=4.5 with Flop and ignore none numeric values in verdict
- `if(isNumeric(value), if ((value > 4.5).and(value <= 6.5), "Average", value), value)` Replace values >4.5 and <=6.5 with  Average and ignore none numeric values in verdict
- `if(isNumeric(value), if ((value > 6.5).and(value <= 8.0), "Hit", value), value)` Replace values >6.5 and <=8.0 with  Hit and ignore none numeric values in verdict

#### Additional Cleaning of Data based of invalid date values
- I created new column convertToDateStartDate based on column startYear converting all the values to date data type, grel, `toDate(value)`
- I Performed a text facet on convertToDateStartDate and saw 5 blank rows, because they have invalid Date values, I deleted those 5 matching rows
- I removed convertToDateStartDate column

After cleaning the file:
- Exported as `HW2-Movies.csv`.
- Extracted JSON scripts containing all operations and saved as `HW2-Movies.json`.

### Part 2: Analyze Cleaned Data

1. **How many movies were listed as “Super Hit” in the year 2021?**
   - [Insert answer with explanation]

2. **Which movie got the lowest rating in the years 2018 to 2020 (excluding 0)?**
   - [Insert answer with explanation]

3. **List the top 3 genres (no duplicates) that got the highest number of votes (excluding 0).**
   - [Insert answer with explanation]

4. **Name the director who directed the 10-minute runtime movie in 2020 that received the lowest number of votes.**
   - [Insert answer with explanation]

5. **List the top 5 movies that received the highest number of votes but are categorized as “Flop.”**
   - [Insert answer with explanation]

*Note any cleaning decisions made that could impact the answers.*

## Submission

Your GitHub repository should include the following files:
- `HW2-report.md` - Your report
- `HW2-Movies.csv` - Cleaned CSV
- `HW2-Movies.json` - Operations used to clean the data in JSON format
- Any images referenced in your report

Submit the URL of your report in Canvas under HW2. This should look like:  
`https://github.com/odu-cs625-datavis/Fall24-asv-*username*/blob/main/HW2-report.md`

*If you make changes after submitting, the last commit time in your repo will be considered as your submission time.*

**References:**
- Dataset: [5 Datasets to Practice Data Cleaning](https://medium.com/@FranciscoHinojosaLuna/5-datasets-to-practice-data-cleaning-27378f422e1c)
- Movies: [Kaggle Movies Dataset](https://www.kaggle.com/datasets/bharatnatrayn/movies-dataset-for-feature-extraction-prediction?resource=download)
