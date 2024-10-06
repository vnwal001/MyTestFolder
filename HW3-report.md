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

   #Output Processed file to be used in Tableau
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
| Total Sales | value, quantitative | length of bars (y-axis) |
| Year |  key, categorical  | horizontal spatial region (x-axis) |

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

Idiom: Multi-Line Chart / Mark: Line
| Data: Attribute | Data: Attribute Type  | Encode: Channel | 
| --- |---| --- |
| Total Sales | value, quantitative| Continuous and color (y-axis) |
| Year |  key, categorical | Continuous and color (x-axis) |


#### 3. SCATTER PLOT 

<img src="https://github.com/vnwal001/MyTestFolder/blob/main/ScatterPlot.png" alt="Total Nintendo Sales from Other Regions Vs Total Global Sales From (2001-2010)" width="989" height="590">

**Answer:**

```
import matplotlib.pyplot as plt
import pandas as pd

df = pd.read_csv("https://raw.githubusercontent.com/vnwal001/MyTestFolder/refs/heads/main/vgsales.csv", sep=",")
df = df[(df != 0).all(axis=1)]
df = df[(df['Year'].between(2001, 2010)) & (df['Publisher'] == 'Nintendo')]
df['Year'] = df['Year'].astype(int)
df_filtered = df[(df['Year'].between(2001, 2010)) & (df['Publisher'] == 'Nintendo')]
# Group by Year and sum the Other_Sales and Global_Sales
total_sales_by_year = df_filtered.groupby('Year')[['Other_Sales', 'Global_Sales']].sum().reset_index()
# Create a scatter plot for Total Other_Sales vs Total Global_Sales
plt.figure(figsize=(10, 6))
# Plot all points
plt.scatter(total_sales_by_year['Other_Sales'], total_sales_by_year['Global_Sales'], color='blue', alpha=0.6, edgecolors='w', s=100)
# Find the index of the highest value in Global Sales
max_index = total_sales_by_year['Global_Sales'].idxmax()
max_row = total_sales_by_year.iloc[max_index]
# Highlight the maximum point
plt.scatter(max_row['Other_Sales'], max_row['Global_Sales'], color='red', s=100, edgecolors='black', label='Max Value')
# Label the maximum point
plt.text(max_row['Other_Sales'], max_row['Global_Sales'], '', fontsize=10, ha='right', color='red')
# Label each point with the corresponding year, without the .0
for i, row in total_sales_by_year.iterrows():
    plt.text(row['Other_Sales'], row['Global_Sales'], str(int(row['Year'])), fontsize=10, ha='right')
# Add labels and title
plt.title('Scatter Plot of Total Other Sales vs Total Global Sales (2001-2010)')
plt.xlabel('Total Other Sales (in millions)')
plt.ylabel('Total Global Sales (in millions)')
plt.grid()
plt.legend()
# Show the plot
plt.tight_layout()
plt.show()
```

Idiom: Multi-Line Chart / Mark: Dots
| Data: Attribute | Data: Attribute Type  | Encode: Channel | 
| --- |---| --- |
| Total Other Sales | value, quantitative| color (y-axis) |
| Total Global Sales |  value, quantitative | color (x-axis) |

#### 4. SCATTER PLOT FROM TABLEAU

<img src="https://github.com/vnwal001/MyTestFolder/blob/main/ScatterPlot.png" alt="Total Nintendo Sales from Other Regions Vs Total Global Sales From (2001-2010)" width="989" height="590">

**References:**
- Dataset: [5 Datasets to Practice Data Cleaning](https://medium.com/@FranciscoHinojosaLuna/5-datasets-to-practice-data-cleaning-27378f422e1c)
- Movies: [Kaggle Movies Dataset](https://www.kaggle.com/datasets/bharatnatrayn/movies-dataset-for-feature-extraction-prediction?resource=download)
