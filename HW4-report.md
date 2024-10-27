# Course: CS 625
## Name: Victor Nwala
## Homework 4

**Due:** October 27, 2024 by 11:59 PM


### Q1: For each state, show the relationship between the state's land area and its total water area. Consider only the 50 US states (Alabama - Wyoming), ignore DC and the territories. Highlight any interesting outliers.

### ANSWERS

#### SCATTER PLOT
<img src="https://github.com/vnwal001/MyTestFolder/blob/main/q1.png" alt="Total Land Area vs Total Water Area" width="1188" height="708">

**Python Code**
   ```
  import pandas as pd
import matplotlib.pyplot as plt

# Load the data from the CSV file
file_path = 'https://raw.githubusercontent.com/vnwal001/MyTestFolder/main/QU1DATA.csv'  # Corrected CSV file path
df = pd.read_csv(file_path)

# Select relevant columns
df = df[['POST OFFICE ABBR', 'Land area SQ MILE', 'Total Water SQ MILE']]

# Rename columns for easier access
df.columns = ['Post Office ABBR', 'Land Area (sq mi)', 'Total Water Area (sq mi)']

# Convert columns to numeric, taking absolute values
df['Land Area (sq mi)'] = pd.to_numeric(df['Land Area (sq mi)'], errors='coerce').abs()
df['Total Water Area (sq mi)'] = pd.to_numeric(df['Total Water Area (sq mi)'], errors='coerce').abs()

# Drop rows with NaN values
df = df.dropna()

# Ignore District of Columbia
df = df[df['Post Office ABBR'] != 'DC']

# Plotting
plt.figure(figsize=(12, 8))

# Plot all states except District of Columbia with the default color
plt.scatter(df['Land Area (sq mi)'], df['Total Water Area (sq mi)'], 
            color='blue', alpha=0.5)

# Identify the highest point
max_point = df.loc[df['Total Water Area (sq mi)'].idxmax()]

# Plot the highest point with a different color
plt.scatter(max_point['Land Area (sq mi)'], max_point['Total Water Area (sq mi)'], 
            color='red', s=100, label=f'Highest: {max_point["Post Office ABBR"]}', edgecolor='black')

# Add labels for each point
for i in range(len(df)):
    plt.text(df['Land Area (sq mi)'].iloc[i], df['Total Water Area (sq mi)'].iloc[i], 
             df['Post Office ABBR'].iloc[i], fontsize=8, ha='right')

# Adding titles and labels
plt.title('Land Area vs. Total Water Area by POST OFFICE ABBR (Excluding District of Columbia)')
plt.xlabel('Land Area (square miles)')
plt.ylabel('Total Water Area (square miles)')
plt.xscale('log')  # Optional: Logarithmic scale for better visualization
plt.yscale('log')  # Optional: Logarithmic scale for better visualization
plt.legend(title='Significant Points', bbox_to_anchor=(1.05, 1), loc='upper left')
plt.grid(True)

# Show the plot
plt.show()
   ```

  Idiom: Scatter Plot / Mark: Point
| Data: Attribute | Data: Attribute Type  | Encode: Channel | 
| --- |---| --- |
| Land Area | value, quantitative | horizontal spatial region (x-axis) |
| Total Water Area |  value, quantitative  | vertical spatial region (y-axis) |

- Explanation of how the idiom used in your chart is appropriate for your datasets and question/task
- Discussion of any insights gained about the data from your chart
- Discussion of any design decisions you made
- Discussion of any special customizations you used










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

Idiom: Scatter Plot / Mark: Dots
| Data: Attribute | Data: Attribute Type  | Encode: Channel | 
| --- |---| --- |
| Total Other Sales | value, quantitative| color (y-axis) |
| Total Global Sales |  value, quantitative | color (x-axis) |


#### 4. SCATTER PLOT FROM TABLEAU

<img src="https://github.com/vnwal001/MyTestFolder/blob/main/TableauPlot.jpg" alt="Total Nintendo Sales Across Regions (2001-2010)" width="1024" height="791">

Idiom: Multi-Line Chart / Mark: Line
| Data: Attribute | Data: Attribute Type  | Encode: Channel | 
| --- |---| --- |
| Total Sales | value, quantitative| Continuous and color (y-axis) |
| Year |  key, categorical | Continuous and color (x-axis) |

#### Reflection
It was easier to plot this multi-line plot on Tableau, but for this use case I will prefare blotting this in python just because it handles data pre-processing and plotting in one instance.
I had to export a preprocess data to use for my Tableau plot. 

**References:**
- https://chatgpt.com/
