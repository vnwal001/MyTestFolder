# Course: CS 625
## Name: Victor Nwala
## Homework 5

**Due:** November 3, 2024 by 11:59 PM


## PART 1

### A) DATA MANIPULATION

##### To prepare my data for use I did the following:
- The original file was in xls format, I created a new csv file to place my data
- I copied over the values of the only data I needed for my analysis, I copied the state population data for just 1970, 1985, 1995 and 2009.
- I for the specified years, if they had both and April and July data, I picked the July data for that year because most years data were collected in July. I only took the April data if there was not July available
- I rename the Years column to just the Year and removed the month. 

### B) TABLE 12

#### 1) BOXPLOT : Show the distributions of the population of all states in 1970, 1985, 1995, and 2009. This should result in 4 separate boxplot glyphs in a single chart

<img src="https://github.com/vnwal001/MyTestFolder/blob/main/h1.png" alt="Boxplot of Population distribution For 50 States for 1970, 1985, 1995, and 2009" width="1189" height="590">

- *Description of the chart and how is was created (explain the code you used and include code snippets)*
- Answer: Code Snippet and Explanation

```
#pandas: Used for data manipulation and analysis.
#matplotlib.pyplot: A plotting library for creating static, animated, and interactive visualizations.
#seaborn: A statistical data visualization library based on Matplotlib, providing a high-level interface for drawing attractive graphics.

import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

# Step 1: Load the data
data = pd.read_csv('https://raw.githubusercontent.com/vnwal001/MyTestFolder/refs/heads/main/population4.csv')


# Step 2: Extract relevant columns (removing unnecessary columns)
years = ['1970', '1985', '1995', '2009']
pop_data = data[['State'] + years]  # Only keep State and relevant year columns

# Step 3: Melt the DataFrame to long format
pop_melted = pop_data.melt(id_vars=['State'], value_vars=years, var_name='Year', value_name='Population')

# Convert Population to numeric (if not already)
pop_melted['Population'] = pd.to_numeric(pop_melted['Population'], errors='coerce')

# Step 4: Exclude Washington, D.C.
pop_melted = pop_melted[pop_melted['State'] != 'District of Columbia']

# Step 5: Create the boxplots
plt.figure(figsize=(12, 6))
sns.boxplot(x='Year', y='Population', data=pop_melted)

# Set the y-axis to start from 0
plt.ylim(0, pop_melted['Population'].max() * 1.1)  # Adjust the upper limit as needed

# Add titles and labels
plt.title('Population Distribution by State for Selected Years (Excluding D.C.)')
plt.xlabel('Year')
plt.ylabel('Population In Thousands')
plt.xticks(rotation=45)
plt.grid(True)

# Show the plot
plt.tight_layout()
plt.show()

```
- Explanation
  ```
  Each box represents the interquartile range (IQR) of the population data for each year, indicating where the middle 50% of data lies.
  The line inside each box represents the median population.
  Whiskers extend to show the range of the data (excluding outliers).
  Outliers are plotted as individual points outside the whiskers
   ```
  
- *Discuss the advantages and disadvantages of each type of distribution chart idiom for showing these distributions (talk specifically about these distributions, not just their advantages and disadvantages in general)*
- Answer:
```
Advantages
1) This Boxplots have provide a concise summary of the dataset by displaying the median, quartiles, and potential outliers, making it easy to grasp the distribution characteristics at a glance.
2) It has helped to facilitate comparisons between different categories (in this case, different years), highlighting differences in central tendency and variability.
3) This Boxplot has  effectively identified outliers, which can be crucial for understanding anomalies in the data.

Disadvantages
1) It does convey information about the number of data points or the specific values
2) In this box plot you cannot clearly tell if outlier points overlap
```

- *Name 1-2 simple observations you can draw from each chart*
- Answer:
- 1) Every year compared had state population outliers
- 2) The median population does not change by so much, yes there is a steady increase but not by much across the years compared. 




#### 2) Histogram: Show the distribution of the population of all states in one of the years (your chart title must indicate which year) your histogram should use a reasonable bin size for the data

<img src="https://github.com/vnwal001/MyTestFolder/blob/main/h2.png" alt="Histogram of Population distribution For 50 States for 2009" width="1189" height="590">


- *Description of the chart and how is was created (explain the code you used and include code snippets)*
- Answer: Code Snippet and Explanation

```
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

# Step 1: Load the data
data = pd.read_csv('https://raw.githubusercontent.com/vnwal001/MyTestFolder/refs/heads/main/population4.csv')

# Step 2: Extract relevant columns
year = '2009'  # Change to the desired year
pop_data = data[['State', year]]

# Step 3: Filter out Washington D.C.
pop_data = pop_data[pop_data['State'] != 'District of Columbia']

# Step 4: Convert the Population to numeric (if not already)
pop_data[year] = pd.to_numeric(pop_data[year], errors='coerce')

# Step 5: Create the histogram using Seaborn
plt.figure(figsize=(12, 6))
sns.histplot(pop_data[year].dropna(), bins=15, color='skyblue', kde=True)

# Add titles and labels
plt.title(f'Population Distribution of States in {year} (Excluding D.C.)')
plt.xlabel('Population In Thousands')
plt.ylabel('Number of States')
plt.grid(axis='y')

# Show the plot
plt.tight_layout()
plt.show()

```

- Explanation
  ```
1) The resulting histogram displays the distribution of state populations for the year 2009. X-Axis (Population in Thousands) represents the population of the states, segmented into bins. Y-Axis (Number of States)
represents the number of states that fall within each population bin.
2) The histogram visualizes how the populations of the states are distributed, allowing for quick identification of population ranges where most states fall. The overlayed KDE provides a smooth estimate of the population density
.
  ```

- *Discuss the advantages and disadvantages of each type of distribution chart idiom for showing these distributions (talk specifically about these distributions, not just their advantages and disadvantages in general)*
Answer:


```
Advantages 

1) The histogram effectively displays how state populations are distributed across a range, allowing for quick insights into the overall population distribution.
Exclusion of D.C.:

2) By excluding Washington, D.C., the histogram focuses solely on the states, which provides a clearer understanding of state populations without the outlier effect of the capital.
Kernel Density Estimate (KDE):

3) The inclusion of a KDE overlay smooths the distribution, helping to highlight underlying trends and patterns.

Disadvantages

1) Like all histograms, this one aggregates data into bins, which means that individual variations within those bins are not visible. This could mask important insights.

2) Viewers might misinterpret the distribution if they are not aware of the effects of binning and KDE smoothing, leading to incorrect assumptions about the data's underlying distribution.

3) While the KDE adds useful information, it could also create confusion if the audience misinterprets it as a distinct data series rather than a smoothed estimate of the histogram.
   
4) This histogram represents only the year 2009. It doesn’t allow for comparisons across multiple years or an understanding of population trends over time, limiting its analytical depth.
   
```
- *Name 1-2 simple observations you can draw from each chart*
- Answer:
```
  1) I observed that about 25 states have populations clustered around the first bin which is between 4 million and 5 million people
  2) I observed that this is a rightly skewed distribution. A right-skewed distribution often indicates the presence of outliers or extreme values on the high end of the scale.
 ```



#### 3) eCDF: Show the distributions of the population of all states in two of the years (your legend must indicate which years)
<img src="https://github.com/vnwal001/MyTestFolder/blob/main/h3.png" alt="eCDF Distributions of the population of all states for 1995 and 2009" width="989" height="590">

- *Description of the chart and how is was created (explain the code you used and include code snippets)*
- Answer: Code Snippet and Explanation

```
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt

# Step 1: Load the data
data = pd.read_csv('https://raw.githubusercontent.com/vnwal001/MyTestFolder/refs/heads/main/population4.csv')

# Step 2: Exclude Washington D.C.
data = data[data['State'] != 'District of Columbia']

# Step 3: Extract population data for the years 1995 and 2009
population_1995 = pd.to_numeric(data['1995'], errors='coerce').dropna()
population_2009 = pd.to_numeric(data['2009'], errors='coerce').dropna()

# Step 4: Create the eCDF plots using Seaborn
plt.figure(figsize=(10, 6))

# Plot eCDF for 1995
sns.ecdfplot(population_1995, label='1995', color='skyblue', linewidth=2)

# Plot eCDF for 2009
sns.ecdfplot(population_2009, label='2009', color='salmon', linewidth=2)

# Add titles and labels
plt.title('Empirical Cumulative Distribution Function (eCDF) of State Populations (Excluding D.C.)')
plt.xlabel('Population In Thousands')
plt.ylabel('ECDF')
plt.legend(loc='upper left')
plt.grid(True)

# Show the plot
plt.tight_layout()
plt.show()
```

- Explanation
  
  ```
  1) This code effectively visualizes the cumulative distribution of state populations for the years 1995 and 2009 using eCDF plots.

  2) The eCDF shows a clear comparisons of population distributions over time and provides insights into how the population landscape of the states has evolved, all while excluding Washington,
     D.C. for the years 1995 and 2009
  3) X-Axis (Population In Thousands) represents the population size of states while Y-Axis (ECDF), represents the cumulative proportion of states that have populations less than or equal to the values on the x-axis.
  
  ```
  - *Discuss the advantages and disadvantages of each type of distribution chart idiom for showing these distributions (talk specifically about these distributions, not just their advantages and disadvantages in general)*
- Answer:
```
Advantages:

1) This eCDF plot shows the cumulative proportion of states with populations below a certain value, making it easy to understand the distribution of populations at a glance.

2) By displaying eCDFs for both 1995 and 2009, the plot allows for straightforward comparisons between the two years, helping to highlight changes in population distributions over time.

Disadvantages:

1) While this eCDFs provide cumulative proportions, it does not show individual data points or the specific number of states within certain population ranges. This can mask important details about the data distribution.

2) This eCDFs does convey information about the spread or variability of the data within each population range, unlike histograms or boxplots.

```

- *Name 1-2 simple observations you can draw from each chart*
- Answer:
  1) I observed that about 90% of the states in both 1995 and 2009 are have a population of 15 million or less people. 
  2) I observed that this is a rightly skewed distribution. A right-skewed distribution often indicates the presence of outliers or extreme values on the high end of the scale.
     


## PART 2 : FURTHER ANALYSIS

### The first Boxplot showed some outliers I wanted to answer the following question:
### 1) What states population were the outliers 1970, 1985, 1995 and 2009.
### 2) Were the same states consistently outliers in each of those years and why were they outliers in those years or not 


#### 1) What states population were the outliers 1970, 1985, 1995 and 2009.
```
Outliers can indicate variability in the data, suggesting that there may be extreme values that differ from the majority of the data points.
In this case we had very high population values compared to the other states as outliers
```

 ##### Answer: To answer this question, I decided to plot a Bar chart of all the outliers across these years

 <img src="https://github.com/vnwal001/MyTestFolder/blob/main/h4.png" alt="eCDF Distributions of the population of all states for 1995 and 2009" width="1189" height="590">

  ##### My First Deductions from the BAR CHART
  
  ###### In 1970 the population boxplot had the following outliers
  - California
  - Illinois
  - New York
  - Pennsylvania
  - Ohio
  - Texas

    
 ###### In 1985 the population boxplot had the following outliers
  - California
  - New York
  - Texas

    
 ###### In 1995 the population boxplot had the following outliers
  - California
  - New York
  - Texas
  - Florida
    

 ###### In 2009 the population boxplot had the following outliers
  - California
  - New York
  - Texas
  - Florida

 ##### My Second Deductions from the BAR CHART
 - I was amazed to find that 1970 had six states that were outliers because visually I could only count 5. It appears 2 points overlapped

 #### 1)  Were the same states consistently outliers in each of those years and why were they outliers in those years or not
 - California, New York and Texas were consistent outliers in 1970, 1985, 1995 and 2009.
 - Ohio and Pennsylvania were outliers in 1970 but by 1985, they were no longer outliers while in 1995 AND 2009 Florida was an outlier.
 
```
In 1970, the populations of California, New York, Texas, Illinois, Pennsylvania, and Ohio were indeed significantly higher than those of many other states. These states represented key economic centers and had large urban populations, which contributed to their higher numbers. However, demographic trends were starting to shift, with some states like New York beginning to lose population to more rapidly growing states like California and Texas.

By 1985, the combination of economic decline, outmigration, demographic shifts, and the rapid growth of other states contributed to Pennsylvania and Ohio no longer having significantly high populations compared to states like California and Texas. These trends reflected broader changes in the U.S. economy and population movements during that period.


 ### Other Observations 
 ##### I wanted to see the average population growth or decline of the states that were consistenly outliers this will help me determine the state with fastest population growth rate.
 ##### The states I compared were California, New York and Texas

 | <img src="https://github.com/vnwal001/MyTestFolder/blob/main/h5.png" alt="Average Population Growth from 1970 to 2009" width="500"/> | <img src="https://github.com/vnwal001/MyTestFolder/blob/main/h6.png" alt="Average Population over those Years" width="500"/> |
|------------------------------------------------------|------------------------------------------------------|

```
I OBSERVED THAT THOUGH CALIFORNIA HAS THE HIGHER POPULATION, TEXAS HAS THE HIGHEST GROWTH RATE
Average Percentage Growth the years 1970, 1985, 1995 and 2009:
California: 22.96%
New York: 2.38%
Texas: 30.84%
```
**Python Code**

```
import pandas as pd
import matplotlib.pyplot as plt

# Load the data
data = pd.read_csv('https://raw.githubusercontent.com/vnwal001/MyTestFolder/refs/heads/main/population4.csv')

# Clean 'State' column
data['State'] = data['State'].str.strip()

# Extract relevant columns
years = ['1970', '1985', '1995', '2009']
pop_data = data[['State'] + years]

# Melt the DataFrame to long format
pop_melted = pop_data.melt(id_vars=['State'], value_vars=years, var_name='Year', value_name='Population')

# Convert Population to numeric
pop_melted['Population'] = pd.to_numeric(pop_melted['Population'], errors='coerce')

# Exclude Washington, D.C.
pop_melted = pop_melted[pop_melted['State'] != 'District of Columbia']

# Filter for California, Texas, and New York
selected_states = pop_melted[pop_melted['State'].isin(['California', 'Texas', 'New York'])]

# Pivot the data for easier calculations
pivot_data = selected_states.pivot(index='Year', columns='State', values='Population')

# Calculate growth rates
growth_rates = pivot_data.pct_change() * 100  # Percentage change
growth_rates = growth_rates.dropna()  # Remove NaN values from the first row

# Create a bar plot for growth rates
plt.figure(figsize=(12, 6))
growth_rates.plot(kind='bar', color=['skyblue', 'salmon', 'lightgreen'], alpha=0.7)

# Add titles and labels
plt.title('Average Growth Rate of Population (in %) for California, Texas, and New York')
plt.xlabel('Year')
plt.ylabel('Growth Rate (%)')
plt.xticks(rotation=0)
plt.grid(axis='y')

# Show the plot
plt.tight_layout()
plt.legend(title='State')
plt.show()

```
 
```
import pandas as pd
import matplotlib.pyplot as plt

# Load the data
data = pd.read_csv('https://raw.githubusercontent.com/vnwal001/MyTestFolder/refs/heads/main/population4.csv')

# Clean 'State' column
data['State'] = data['State'].str.strip()

# Extract relevant columns
years = ['1970', '1985', '1995', '2009']
pop_data = data[['State'] + years]

# Melt the DataFrame to long format
pop_melted = pop_data.melt(id_vars=['State'], value_vars=years, var_name='Year', value_name='Population')

# Convert Population to numeric
pop_melted['Population'] = pd.to_numeric(pop_melted['Population'], errors='coerce')

# Exclude Washington, D.C.
pop_melted = pop_melted[pop_melted['State'] != 'District of Columbia']

# Filter for California, Texas, and New York
selected_states = pop_melted[pop_melted['State'].isin(['California', 'Texas', 'New York'])]

# Pivot the data for easier calculations
pivot_data = selected_states.pivot(index='Year', columns='State', values='Population')

# Calculate growth rates
growth_rates = pivot_data.pct_change() * 100  # Percentage change
growth_rates = growth_rates.dropna()  # Remove NaN values from the first row

# Calculate the average percentage growth for each state
average_growth = growth_rates.mean()

# Print the average growth rates for each state
print("Average Percentage Growth from 1985 to 2009:")
for state in average_growth.index:
    print(f"{state}: {average_growth[state]:.2f}%")

# Create a bar plot for average growth rates
plt.figure(figsize=(8, 5))
average_growth.plot(kind='bar', color=['skyblue', 'salmon', 'lightgreen'], alpha=0.7)

# Add titles and labels
plt.title('Average Percentage Growth of Population (1985-2009)')
plt.xlabel('State')
plt.ylabel('Average Growth Rate (%)')
plt.xticks(rotation=0)
plt.grid(axis='y')

# Show the plot
plt.tight_layout()
plt.show()

```

























**References:**
- https://chatgpt.com/
- https://www.census.gov/library/publications/2009/compendia/statab/129ed/geography-environment.html
- https://www.census.gov/library/publications/2009/compendia/statab/129ed.html
