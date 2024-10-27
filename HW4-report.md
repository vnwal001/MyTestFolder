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

- *Explanation of how the idiom used in your chart is appropriate for your datasets and question/task*
- Answer: I am doing a comparision of two quantitative attributes, so a scatter plot is the right plot.
- *Discussion of any insights gained about the data from your chart*
- I found that Alaska was a popout in the plot having the highest water and land area
- *Discussion of any design decisions you made*
- I decided to use a log scale to space out the points to prevent overcrowding 
- *Discussion of any special customizations you used*
- I added labels to the points on the plot to help identify each state.
- *Further Questions - What further questions does your exploration of the dataset prompt? What hypotheses do you have about what the answers might be? Are there other tables that might help you address these questions?*
- My further question will be, have the total water and land area changed over time? My hypothesis will be that it has due to climate change. I would love to know what has changed, if water area has increased and land area has shrunk over time. I don't have other tables to help address this question.  



### Q2: Pick 10 states and compare the proportion of their total area that is land and the proportion that is water. You may pick the 10 states however you wish (e.g., 10 largest, 10 smallest, 10 largest based on land area, 10 largest based on water area, 10 favorite states), but you must discuss how you chose the states.

### ANSWERS

### STACKED BAR CHART
<img src="https://github.com/vnwal001/MyTestFolder/blob/main/q2.png" alt="Total Land Area vs Total Water Area Comparision" width="1389" height="690">

**Python Code**

```
import pandas as pd
import matplotlib.pyplot as plt

# Load the data from the CSV file
file_path = 'https://raw.githubusercontent.com/vnwal001/MyTestFolder/main/QU1DATA.csv'  # Change this to your CSV file path
df = pd.read_csv(file_path)

# Select relevant columns
df = df[['POST OFFICE ABBR', 'Total area SQ MILE', 'Land area SQ MILE', 'Total Water SQ MILE']]

# Rename columns for easier access
df.columns = ['Post Office ABBR', 'Total Area (sq mi)', 'Land Area (sq mi)', 'Total Water Area (sq mi)']

# Convert columns to numeric
df['Total Area (sq mi)'] = pd.to_numeric(df['Total Area (sq mi)'], errors='coerce')
df['Land Area (sq mi)'] = pd.to_numeric(df['Land Area (sq mi)'], errors='coerce')
df['Total Water Area (sq mi)'] = pd.to_numeric(df['Total Water Area (sq mi)'], errors='coerce')

# Drop rows with NaN values
df = df.dropna()

# Sort by Total Area and get the top 10 largest states
largest_states = df.nlargest(10, 'Total Area (sq mi)')

# Plotting
plt.figure(figsize=(14, 7))

# Create a stacked bar chart
plt.bar(largest_states['Post Office ABBR'], largest_states['Land Area (sq mi)'], 
        color='royalblue', label='Land Area (sq mi)')
plt.bar(largest_states['Post Office ABBR'], largest_states['Total Water Area (sq mi)'], 
        bottom=largest_states['Land Area (sq mi)'], color='lightskyblue', label='Total Water Area (sq mi)')

# Adding titles and labels
plt.title('Total Area (sq mi) = Land Area (sq mi) + Total Water Area (sq mi) for the 10 Largest States')
plt.xlabel('States')
plt.ylabel('Area (square miles)')

# Set y-axis limits to ensure visibility
plt.ylim(0, largest_states['Total Area (sq mi)'].max() * 1.1)

# Add gridlines for better readability
plt.grid(True, which="both", ls="--", linewidth=0.5)

plt.legend()

# Annotate both land and water portions
for index, row in largest_states.iterrows():
    plt.text(row['Post Office ABBR'], row['Land Area (sq mi)'] / 2, 
             f"{row['Land Area (sq mi)']:.1f}", 
             ha='center', va='center', color='white')
    plt.text(row['Post Office ABBR'], row['Land Area (sq mi)'] + row['Total Water Area (sq mi)'] / 2, 
             f"{row['Total Water Area (sq mi)']:.1f}", 
             ha='center', va='center', color='black')

# Show the plot
plt.tight_layout()
plt.show()

```

Idiom: STACKED BAR CHART / Mark: Bar
| Data: Attribute | Data: Attribute Type  | Encode: Channel | 
| --- |---| --- |
| Area | value, quantitative| Position: The height or length of each segment in the bar (y-axis) |
| States |  key, categorical | Label: Text labels can be used to indicate the states (x-axis) |

- *Explanation of how the idiom used in your chart is appropriate for your datasets and question/task*
- Answer: I used a stacked bar chart to show the fraction of water and land in the total area
- *Discussion of any insights gained about the data from your chart*
- I found that Alaska has the highest bar, it's size is more than double the size of Texas also I found the the water are of Alaska is almost the size of the entire state of Wyoming
- *Discussion of any design decisions you made*
- I selected the top 10 largest states in the US
- *Discussion of any special customizations you used*
- I added labels to the bars, to visualize the actual values
- *Further Questions - What further questions does your exploration of the dataset prompt? What hypotheses do you have about what the answers might be? Are there other tables that might help you address these questions?*
- I would to know how many states will I sum to get the size of Alaska, I think about 5 of the top 10 states. Yes, there is a table to help address this question. I would also love to know where Alaska would rank by population from the top 10 states. 






### Q3 Combine this table with Table 12 (Resident Population, from Section 1 - Population) to show the relationship between land area and 2008 population for each state.

#### STACKED BAR CHART

<img src="https://github.com/vnwal001/MyTestFolder/blob/main/q3.png" alt="Resident Population Vs Land Area" width="1189" height="590">

**Python Code**

```
import pandas as pd
import matplotlib.pyplot as plt

# Load the data from the CSV file
file_path = 'https://raw.githubusercontent.com/vnwal001/MyTestFolder/refs/heads/main/QU1DATA_EXTRA1.csv'
df = pd.read_csv(file_path)

# Select relevant columns and rename for easier access
df = df[['POST OFFICE ABBR', 'Land area SQ MILE', 'RESIDENT POPULATION']]

# Convert columns to numeric, forcing string values to NaN
df['Resident Population'] = pd.to_numeric(df['RESIDENT POPULATION'], errors='coerce')
df['Land Area (sq mi)'] = pd.to_numeric(df['Land area SQ MILE'], errors='coerce')

# Drop rows with NaN values
df = df.dropna()

# Exclude Washington, D.C.
df = df[df['POST OFFICE ABBR'] != 'DC']

# Prepare the data for a stacked bar chart
df_grouped = df.groupby('POST OFFICE ABBR').sum()

# Create the bar chart
plt.figure(figsize=(12, 6))

# Plotting the populations and land areas as a stacked bar chart
plt.bar(df_grouped.index, df_grouped['Resident Population'], label='Resident Population', color='skyblue')
plt.bar(df_grouped.index, df_grouped['Land Area (sq mi)'], label='Land Area (sq mi)', 
        bottom=df_grouped['Resident Population'], color='orange')

# Adding resident population value labels for specified states
for state in ['AK', 'WV', 'WY', 'DE', 'HI', 'NE', 'MT', 'ME', 'VT', 'NH', 'RI', 'SD', 'ID']:
    if state in df_grouped.index:
        plt.text(state, df_grouped['Resident Population'].loc[state] + df_grouped['Land Area (sq mi)'].loc[state] / 2,
                 f"{int(df_grouped['Resident Population'].loc[state])}", 
                 ha='center', va='center', fontsize=10, color='black')

# Adding titles and labels
plt.title('Stacked Bar Chart of Resident Population and Land Area by State (Excluding DC)')
plt.xlabel('Post Office Abbreviation')
plt.ylabel('Population in Thousands')
plt.xticks(rotation=45)
plt.legend()

# Show the plot
plt.tight_layout()
plt.show()

```

Idiom: STACKED BAR CHART / Mark: Bar
| Data: Attribute | Data: Attribute Type  | Encode: Channel | 
| --- |---| --- |
| Magnitude in Thousands | value, quantitative| Position: The height or length of each segment in the bar (y-axis) |
| States |  key, categorical | Label: Text labels can be used to indicate the states (x-axis) |


- *Explanation of how the idiom used in your chart is appropriate for your datasets and question/task*
- Answer: I used a stacked bar chart to show the fraction of population and land in the area
- *Discussion of any insights gained about the data from your chart*
- I found some states whose population is significantly smaller compared to its land area. 
- *Discussion of any design decisions you made*
- I decided to use a log scale so I can visualize the population ratios, a linear scale gave me poor results. 
- *Discussion of any special customizations you used*
- I added labels to the bar whose population hue did not show because of how small it is. 
- *Further Questions - What further questions does your exploration of the dataset prompt? What hypotheses do you have about what the answers might be? Are there other tables that might help you address these questions?*
- I would to know for those states which significantly smaller population compare to their land size, why less people live there despite how large the are. Hypothesis could be that they have less opportunities. I do not have a table to address this question.  


### Q4: Show how the amount of water withdrawals attributed to the public supply has changed over time.

### ANSWERS

#### 4 LINE CHART

<img src="https://github.com/vnwal001/MyTestFolder/blob/main/q4.png" alt="Public Supply Widthrawal over time" width="1190" height="590">

**Python Code**

```

import pandas as pd
import matplotlib.pyplot as plt

# Load the data from the CSV file
file_path = 'https://raw.githubusercontent.com/vnwal001/MyTestFolder/refs/heads/main/Q2UDATA.csv'  # Change this to your CSV file path
df = pd.read_csv(file_path)

# Select relevant columns
df = df[['Year', 'Public Supply']]

# Clean the Year column by removing unwanted characters
df['Year'] = df['Year'].str.replace(r'\D', '', regex=True)  # Remove non-digit characters

# Retain only the first four characters (the year)
df['Year'] = df['Year'].str[:4]

# Convert the cleaned Year to numeric
df['Year'] = pd.to_numeric(df['Year'], errors='coerce')

# Convert Public Supply to numeric
df['Public Supply'] = pd.to_numeric(df['Public Supply'], errors='coerce')

# Drop rows with NaN values
df = df.dropna()

# Plotting
plt.figure(figsize=(12, 6))
plt.plot(df['Year'], df['Public Supply'], marker='o', linestyle='-', color='blue')

# Adding titles and labels
plt.title('Changes in Water Withdrawals Attributed to Public Supply Over Time')
plt.xlabel('Year')
plt.ylabel('Water Withdrawals (billion gallons)')
plt.grid()

# Show the plot
plt.tight_layout()
plt.show()
```

Idiom: LINE CHART / Mark: Points
| Data: Attribute | Data: Attribute Type  | Encode: Channel | 
| --- |---| --- |
| Water Withdrawals | value, quantitative| Position: Showing measure value of water (y-axis) |
| Year |  key, categorical | Position: Showing Years (x-axis) |

- *Explanation of how the idiom used in your chart is appropriate for your datasets and question/task*
- Answer: I used a line chart, because it is best used to show a trend
- *Discussion of any insights gained about the data from your chart*
- I found a continous increase in water withdrawals each year.
- *Discussion of any design decisions you made*
- I use a linear scale  
- *Discussion of any special customizations you used*
- no special customizations bacause the line chart is clear
- *Further Questions - What further questions does your exploration of the dataset prompt? What hypotheses do you have about what the answers might be? Are there other tables that might help you address these questions?*
- I would love to know if the increase in water withdrawals continued until recent years. My hypothesis will be yes it did, I have no table to address this issue.

### Q5: Which of the end uses has contributed the most to the growth over time?

#### ANSWER: THERMO-ELECTRIC POWER

<img src="https://github.com/vnwal001/MyTestFolder/blob/main/q5.png" alt="Water Withdrawals by End Use over Time" width="620" height="470">

**Python Code**

```
import pandas as pd
import matplotlib.pyplot as plt

# Load the data from the CSV file
file_path = 'https://raw.githubusercontent.com/vnwal001/MyTestFolder/refs/heads/main/Q2UDATA.csv'  # Change this to your CSV file path
df = pd.read_csv(file_path)

# Select relevant columns
columns_of_interest = [
    'Year', 'Public Supply', 'Self Supply Domestic', 'Livestock', 
    'Irrigation', 'Thermo-electric power', 'Self-supplied industrial', 
    'Mining', 'Commercial', 'Aquaculture'
]
df = df[columns_of_interest]

# Clean the Year column
df['Year'] = df['Year'].str.replace(r'\D', '', regex=True).str[:4]
df['Year'] = pd.to_numeric(df['Year'], errors='coerce')

# Replace non-numeric values in end use columns with zero
for col in columns_of_interest[1:]:  # Exclude the 'Year' column
    df[col] = pd.to_numeric(df[col], errors='coerce').fillna(0)

# Drop rows with NaN values in 'Year' after cleaning
df = df.dropna(subset=['Year'])

# Group by Year and sum the values for each end use
annual_withdrawals = df.groupby('Year').sum()

# Plotting
plt.figure(figsize=(14, 7))
annual_withdrawals.plot(kind='line', marker='o', alpha=0.8)

# Adding titles and labels
plt.title('Water Withdrawals by End Use Over Time')
plt.xlabel('Year')
plt.ylabel('Water Withdrawals (Billion gallons')
plt.grid()

# Place the legend outside the plot
plt.legend(title='End Uses', bbox_to_anchor=(1.05, 1), loc='upper left')

# Show the plot
plt.tight_layout()
plt.show()
```


Idiom: Multi-Line Chart / Mark: Line
| Data: Attribute | Data: Attribute Type  | Encode: Channel | 
| --- |---| --- |
| Water Widthdrawals | value, quantitative| Continuous and color (y-axis) |
| Year |  key, categorical | Continuous and color (x-axis) |

- *Explanation of how the idiom used in your chart is appropriate for your datasets and question/task*
- Answer: I used a multi-line chart, because it is best used to show a trend across the various uses over time
- *Discussion of any insights gained about the data from your chart*
- I found that mining,  aquaculture and Commercial did not significantly increase over time. I also found that irrigation use is begining to decline
- *Discussion of any design decisions you made*
- I used distinct colors to represent each end use  
- *Discussion of any special customizations you used*
- no special customizations bacause the multi-line chart is clear
- *Further Questions - What further questions does your exploration of the dataset prompt? What hypotheses do you have about what the answers might be? Are there other tables that might help you address these questions?*
- I would love to know why the sharp increase from 1950 to 1980 in themo-electric power and sudden decline and plateau from 1980. I would also like to know what is causing the decline in irrigation.

### Q6: Combine this table with Table 354 (Land Cover/Use by Type) to show the relationship between the change in area of cropland and pastureland to rural water withdrawal (domestic and livestock).

### ANSWERS

<img src="https://github.com/vnwal001/MyTestFolder/blob/main/q6a.png" alt="Estimating Water Withdrawal values for Missing Years" width="1189" height="590">

**Python Code**

```
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

# Create the dataset
data = {
    'Year': [1950, 1955, 1960, 1965, 1970, 1975, 1980, 
             1985, 1990, 1995, 2000, 2005],
    'Domestic': [2.1, 2.1, 2.0, 2.3, 2.6, 2.8, 3.4, 
               3.32, 3.39, 3.39, 3.58, 3.83],
    'Livestock': [1.5, 1.5, 1.6, 1.7, 1.9, 2.1, 2.2, 
               2.23, 2.25, 2.28, 2.38, 2.14]
}

# Convert to DataFrame
df = pd.DataFrame(data)

# Set the Year as the index for easier plotting and interpolation
df.set_index('Year', inplace=True)

# Specify the years for estimation
years_to_estimate = [1982, 1987, 1992, 1997, 2001, 2002, 2003]

# Interpolate the values for the specified years
df_estimated = df.reindex(df.index.union(years_to_estimate)).interpolate()

# Plot the original and estimated values
plt.figure(figsize=(12, 6))
plt.plot(df.index, df['Domestic'], marker='o', label='Domestic (Original)', color='blue')
plt.plot(df.index, df['Livestock'], marker='o', label='Livestock (Original)', color='orange')
plt.plot(df_estimated.index, df_estimated['Domestic'], linestyle='--', color='blue', alpha=0.5, label='Domestic (Estimated)')
plt.plot(df_estimated.index, df_estimated['Livestock'], linestyle='--', color='orange', alpha=0.5, label='Livestock (Estimated)')

# Highlight the estimated points
for year in years_to_estimate:
    plt.scatter(year, df_estimated.loc[year, 'Domestic'], color='blue')
    plt.scatter(year, df_estimated.loc[year, 'Livestock'], color='orange')

# Adding titles and labels
plt.title('Original and Estimated Values Over Years')
plt.xlabel('Year')
plt.ylabel('Billion Gallons')
plt.xticks(np.arange(1950, 2006, 5))
plt.legend()
plt.grid()
plt.tight_layout()
plt.show()

# Display estimated values
print("Estimated values:")
print(df_estimated.loc[years_to_estimate])
```

```
Estimated values:
        Domestic    Livestock
Year                     
1982    3.3600      2.215
1987    3.3550      2.240
1992    3.3900      2.265
1997    3.4850      2.330
2001    3.6425      2.320
2002    3.7050      2.260
2003    3.7675      2.200

```
#### RURAL =  DOMESTIC + LIVESTOCK

```
import pandas as pd

# Create the dataset
data = {
    'Self Supply Domestic': [3.36, 3.355, 3.39, 3.485, 3.6425, 3.705, 3.7675],
    'Livestock': [2.215, 2.24, 2.265, 2.33, 2.32, 2.26, 2.2]
}

# Convert to DataFrame
df = pd.DataFrame(data)

# Calculate the new column 'Rural'
df['Rural'] = df['Self Supply Domestic'] + df['Livestock']

# Display the updated DataFrame
print(df)
```


```
 Self Supply Domestic  Livestock   Rural
0                3.3600      2.215  5.5750
1                3.3550      2.240  5.5950
2                3.3900      2.265  5.6550
3                3.4850      2.330  5.8150
4                3.6425      2.320  5.9625
5                3.7050      2.260  5.9650
6                3.7675      2.200  5.9675

```
#### Multi-Line Chart
<img src="https://github.com/vnwal001/MyTestFolder/blob/main/q6b.png" alt="Change in Cropland, Pastureland and Rural Water Withdrawals Over the Years" width="1189" height="590">

```
import pandas as pd
import matplotlib.pyplot as plt

# Create the dataset
data = {
    'Year': [1982, 1987, 1992, 1997, 2001, 2002, 2003],
    'Cropland': [420.4, 406.2, 381.2, 376.4, 369.6, 368.4, 367.9],
    'Pastureland': [131.4, 127.2, 125.1, 119.5, 116.9, 117.3, 117],
    'Rural Water Withdrawal': [5.575, 5.595, 5.655, 5.815, 5.9625, 5.965, 5.9675]
}

# Convert to DataFrame
df = pd.DataFrame(data)

# Plotting the changes over the years
plt.figure(figsize=(12, 6))

# Plot for Cropland
plt.plot(df['Year'], df['Cropland'], marker='o', label='Cropland', color='green')
# Plot for Pastureland
plt.plot(df['Year'], df['Pastureland'], marker='o', label='Pastureland', color='brown')
# Plot for Rural Water Withdrawal
plt.plot(df['Year'], df['Rural Water Withdrawal'], marker='o', label='Rural Water Withdrawal', color='blue')

# Adding titles and labels
plt.title('Change in Cropland, Pastureland, and Rural Water Withdrawal Over the Years')
plt.xlabel('Year')
plt.ylabel('Area (acres) / Water Withdrawal (million gallons)')
plt.xticks(df['Year'])  # Ensure all years are shown
plt.legend()
plt.grid()
plt.tight_layout()
plt.show()

```

Idiom: Multi-Line Chart / Mark: Line
| Data: Attribute | Data: Attribute Type  | Encode: Channel | 
| --- |---| --- |
| Water Widthdrawals | value, quantitative| Continuous and color (y-axis) |
| Year |  key, categorical | Continuous and color (x-axis) |


- *Explanation of how the idiom used in your chart is appropriate for your datasets and question/task*
- Answer: I used a multi-line chart to show the change of Cropland, Pastureland and Rural Water Withdrawals over the years 
- *Discussion of any insights gained about the data from your chart*
- I found that Rural water withdrawal is holding steady over the years I selected.
- *Discussion of any design decisions you made*
- Firstly from the table 358 there were no data of year 1982, 1987, 1992, 1997, 2001, 2002 and 2003. I had to interlopate to get values of those specific years from the existing values. With the values of I got, I combined it with table 354, which had cropland and pastureland water values for those specific years. 
- *Discussion of any special customizations you used*
- I used distinct colors to represent the various water uses.
- *Further Questions - What further questions does your exploration of the dataset prompt? What hypotheses do you have about what the answers might be? Are there other tables that might help you address these questions?*
- I would like to know if the cropland water withdrawal is holding steady or still declining till date. My hypothesis is that it is still holding steady. I have no tables to address this question.

### Q7 Choose 5 cities and compare their record highs in each month. You may pick the 5 cities however you wish, but you must discuss how you chose the cities.

#### ANSWERS

#### Multi-Line Chart
<img src="https://github.com/vnwal001/MyTestFolder/blob/main/q7.png" alt="Record High Temperatures of Selected Cities" width="1389" height="690">

```
import pandas as pd
import matplotlib.pyplot as plt

# Load the data from the CSV file
file_path = 'https://raw.githubusercontent.com/vnwal001/MyTestFolder/refs/heads/main/Q3UDATA.csv'  # Change this to your CSV file path
df = pd.read_csv(file_path)


# Strip whitespace from column names
df.columns = df.columns.str.strip()

# Convert specified columns to numeric values
columns_to_convert = [
    'Length of record (years)', 'January', 'February', 'March', 'April', 
    'May', 'June', 'July', 'August', 'September', 'October', 'November', 
    'December', 'Annual \\1'  # Note: use double backslash to escape
]

# Apply conversion
for col in columns_to_convert:
    df[col] = pd.to_numeric(df[col], errors='coerce').fillna(0)

# Filter the DataFrame for the specified cities
cities_of_interest = ['Los Angeles', 'Mobile', 'Chicago', 'Little Rock', 'Denver']
df_filtered = df[df['Cities'].isin(cities_of_interest)]

# Set the index to cities for easier plotting
df_filtered.set_index('Cities', inplace=True)

# Extract the monthly temperatures
monthly_temps = df_filtered[['January', 'February', 'March', 'April', 'May', 'June',
                              'July', 'August', 'September', 'October', 'November', 'December']]

# Plotting
plt.figure(figsize=(14, 7))

# Create a line plot for each city
for city in cities_of_interest:
    plt.plot(monthly_temps.columns, monthly_temps.loc[city], marker='o', label=city)

# Adding titles and labels
plt.title('Monthly Record High Temperatures for Selected Cities')
plt.xlabel('Month')
plt.ylabel('Temperature (°F)')
plt.xticks(rotation=45)
plt.legend(title='Cities', loc='upper left', bbox_to_anchor=(1, 1))

# Show the plot
plt.grid()
plt.tight_layout()
plt.show()
```


Idiom: Multi-Line Chart / Mark: Line
| Data: Attribute | Data: Attribute Type  | Encode: Channel | 
| --- |---| --- |
| Temperature | value, quantitative| Continuous and color (y-axis) |
| Month |  key, categorical | Continuous and color (x-axis) |


- *Explanation of how the idiom used in your chart is appropriate for your datasets and question/task*
- Answer: I used a multi-line chart to show the monthly record high temperatures
- *Discussion of any insights gained about the data from your chart*
- I selected 5 cities from the various regions of  the united states namely: Los Angeles: West, Mobile: South, Chicago: Midwest, Little Rock: South, Denver: Mountain. Also from the Month of July to August, only Mobile and Los Angeles are trending upwards. 
- *Discussion of any design decisions you made*
- I used distinct colors to represent each city chosen
- *Discussion of any special customizations you used*
- no special customizations. 
- *Further Questions - What further questions does your exploration of the dataset prompt? What hypotheses do you have about what the answers might be? Are there other tables that might help you address these questions?*
- I noticed that from August to Septempter only Los Angeles is trending upwards, I would like to see if there are other cities with this behaviour. My hypothesis is yes, escpecially in the west. There are tables to address this question.

### Q8 Using the data from all of the cities, which month most often has the highest high? (Hint: For each month, show the number of times it had the highest high for a city.)

### ANSWER IS JULY

#### BAR CHART

<img src="https://github.com/vnwal001/MyTestFolder/blob/main/q8.png" alt="Frequency of the Highest Temperature" width="841" height="597">

**Python Code**
```
import pandas as pd
import matplotlib.pyplot as plt
import numpy as np

# Load the data from the CSV file
file_path = 'https://raw.githubusercontent.com/vnwal001/MyTestFolder/refs/heads/main/Q3UDATA.csv'  # Change this to your CSV file path
df = pd.read_csv(file_path)

# Strip whitespace from column names
df.columns = df.columns.str.strip()

# Convert the monthly temperature columns to numeric values
monthly_columns = ['January', 'February', 'March', 'April', 
                   'May', 'June', 'July', 'August', 
                   'September', 'October', 'November', 'December']

for col in monthly_columns:
    df[col] = pd.to_numeric(df[col], errors='coerce').fillna(0)

# Create a DataFrame to store the month with the highest temperature for each city
highest_months = []

# Loop through each city to find the month with the highest record high
for index, row in df.iterrows():
    city_highest_month = row[monthly_columns].idxmax()
    highest_months.append(city_highest_month)

# Count occurrences of each month being the highest
month_counts = pd.Series(highest_months).value_counts()

# Display the results
print("Number of times each month had the highest high temperature:")
print(month_counts)

# Generate unique colors for each bar
colors = plt.cm.tab10(np.linspace(0, 1, len(month_counts)))

# Plot the results with unique colors for each bar
plt.figure(figsize=(10, 6))
bars = plt.bar(month_counts.index, month_counts.values, color=colors)
plt.title('Frequency of Highest High Temperatures by Month')
plt.xlabel('Month')
plt.ylabel('Number of Cities with Highest Record High')
plt.xticks(rotation=45)
plt.grid(axis='y')


for bar in bars:
    yval = bar.get_height()
    plt.text(bar.get_x() + bar.get_width()/2, yval, int(yval), ha='center', va='bottom')

plt.show()
```


Idiom: BAR Chart / Mark: BAR
| Data: Attribute | Data: Attribute Type  | Encode: Channel | 
| --- |---| --- |
| Frequency | value, quantitative| Height of Bar (y-axis) |
| Month |  key, categorical |  color (x-axis) |

- *Explanation of how the idiom used in your chart is appropriate for your datasets and question/task*
- Answer: I used a Bar chart because it best shows frequency with the height of the bars.
- *Discussion of any insights gained about the data from your chart*
- I found the months of July and August had the the same frequecy of highest temperatures
- *Discussion of any design decisions you made*
- I decided to give each Bar a unique color to represent the months
- *Discussion of any special customizations you used*
- I added the frequency as a label to the bars 
- *Further Questions - What further questions does your exploration of the dataset prompt? What hypotheses do you have about what the answers might be? Are there other tables that might help you address these questions?*
- I would like to know if the frequency of highest temperatures have changed over the years. My Hypothesis is yes, because of climate change. I do not have a table to address this questions but there could be. 

### Q8 Combine this table with Table 380 (Lowest Temperature of Record) to show the relationship between each city's annual highest high and annual lowest low.

#### ANSWERS

<img src="https://github.com/vnwal001/MyTestFolder/blob/main/q9.png" alt="Annual Highest and Lowest temperatures by City" width="1189" height="790">

**Python Code**

```
import pandas as pd
import matplotlib.pyplot as plt

# Load the data from the CSV file
file_path = 'https://raw.githubusercontent.com/vnwal001/MyTestFolder/refs/heads/main/Q3UDATA1_EXTRA.csv'  # Replace with your actual file path
df = pd.read_csv(file_path)

# Clean column names by stripping whitespace
df.columns = df.columns.str.strip()

# Select relevant columns
df = df[['Cities', 'AnnualHighest', 'AnnualLowest']]

# Convert temperature columns to numeric
df['AnnualHighest'] = pd.to_numeric(df['AnnualHighest'], errors='coerce')
df['AnnualLowest'] = pd.to_numeric(df['AnnualLowest'], errors='coerce')

# Drop rows with NaN values
df = df.dropna()

# Identify the cities with the highest and lowest temperatures
highest_temp_city = df.loc[df['AnnualHighest'].idxmax()]
lowest_temp_city = df.loc[df['AnnualLowest'].idxmin()]

# Create a horizontal bar chart
plt.figure(figsize=(12, 8))

# Plot Annual Highest and Lowest
bars_highest = plt.barh(df['Cities'], df['AnnualHighest'], color='skyblue', label='Annual Highest', alpha=0.7)
bars_lowest = plt.barh(df['Cities'], df['AnnualLowest'], color='orange', label='Annual Lowest', alpha=0.7)

# Highlight the city with the highest temperature
for bar in bars_highest:
    if bar.get_y() + bar.get_height() / 2 == highest_temp_city.name:
        bar.set_color('red')  # Change color to highlight highest temperature city

# Highlight the city with the lowest temperature
for bar in bars_lowest:
    if bar.get_y() + bar.get_height() / 2 == lowest_temp_city.name:
        bar.set_color('black')  # Change color to highlight lowest temperature city

# Adding titles and labels
plt.title('Annual Highest and Lowest Temperatures by City')
plt.xlabel('Temperature (°F)')
plt.ylabel('Cities')
plt.legend()

# Show the plot
plt.tight_layout()
plt.show()

```

Idiom: Horizontal BAR Chart / Mark: BAR
| Data: Attribute | Data: Attribute Type  | Encode: Channel | 
| --- |---| --- |
| Cities | Categorical| Position: Position of Bar (y-axis) |
| Temperature |  value, quantitative |  color (x-axis) |


- *Explanation of how the idiom used in your chart is appropriate for your datasets and question/task*
- Answer: I used a Horizontal Bar chart so I could visual encode the temperature by the length of the Bar
- *Discussion of any insights gained about the data from your chart*
- I found that Phoenix had the highest temperature and Bismarck had the lowest temperature
- *Discussion of any design decisions you made*
- I decided not to give all the cities unique colors, it would too colorful and confusing. 
- *Discussion of any special customizations you used*
- I decided to popout the highest and lowest temperatures 
- *Further Questions - What further questions does your exploration of the dataset prompt? What hypotheses do you have about what the answers might be? Are there other tables that might help you address these questions?*
- I would like to know, over the years if more cities are having lower minimun temperatures and higher maximum temperatures. I want to know where the trend lies, my hypothesis is yes, and there could be data to address this question. 


**References:**
- https://chatgpt.com/
- https://www.census.gov/library/publications/2009/compendia/statab/129ed/geography-environment.html
- https://www.census.gov/library/publications/2009/compendia/statab/129ed.html
