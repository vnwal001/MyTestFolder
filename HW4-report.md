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






### Extra Credit [2 points]: Combine this table with Table 12 (Resident Population, from Section 1 - Population) to show the relationship between land area and 2008 population for each state.

#### 3. STACKED BAR CHART

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
