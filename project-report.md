# Course: CS 625
## Name: Victor Nwala
## FINAL PROJECT REPORT

**Due:** November 30, 2024 by 11:59 PM


### SECTION 1: A brief description of your chosen datasets (including links to the original sources of the data)
#### I have chosen the datset from the The National Hurricane Center (NHC), it conducts a post-storm analysis of each tropical cyclone in its area of responsibility to determine the official assessment of the cyclone's history. In this dataset you find the all types of cyclones recorded across the Pacific and Atlantic Ocean.
#### TD – Tropical cyclone of tropical depression intensity (< 34 knots)
#### TS – Tropical cyclone of tropical storm intensity (34-63 knots)
#### HU – Tropical cyclone of hurricane intensity (> 64 knots)
#### EX – Extratropical cyclone (of any intensity)
#### SD – Subtropical cyclone of subtropical depression intensity (< 34 knots)
#### SS – Subtropical cyclone of subtropical storm intensity (> 34 knots)
#### LO – A low that is neither a tropical cyclone, a subtropical cyclone, nor an extratropical cyclone (of any intensity)
#### WV – Tropical Wave (of any intensity)
#### DB – Disturbance (of any intensity)
#### I have focussed my analysic just on Hurricanes i.e. Tropical cyclone of hurricane intensity (> 64 knots)
#### Source: https://www.kaggle.com/datasets/noaa/hurricane-database/


### SECTION 2: Final questions that I addressed
- Hurricane Intensity and Frequency Analysis
- A) Is the frequency of hurricanes increasing over time?
- B) Are hurricane wind speeds becoming stronger over time?
- C) What are the potential factors contributing to any observed changes in hurricane frequency and intensity?

#### 2) Comparative Analysis of Atlantic vs. Pacific Hurricanes
- How do trends in hurricane frequency and intensity differ between the Atlantic and Pacific Oceans over years?

#### 3) Impact of Category 5 Hurricanes on Population Migration
- For hurricanes that make landfall in the U.S., is there a noticeable migration response in the aftermath of Category 5 hurricanes? Specifically, does the population of selected cities decrease significantly a year  after the hurricane, and can any patterns be identified?



##### To prepare my data for use I did the following:
- The original file was in xls format, I created a new csv file to place my data
- I copied over the values of the only data I needed for my analysis, I copied the state population data for just 1970, 1985, 1995 and 2009.
- For the specified years, if they had both and April and July data, I picked the July data for that year because most years data were collected in July. I only took the April data if there was not a July date available
- I renamed the Years column to just the Year and removed the month.
- 

#### SECTION 2 QUESTION A ANSWERS 
#### ANALYZING HURRICANE FREQUENCIES
#### ATLANTIC HURRICANES FREQUENCIES
<img src="https://github.com/vnwal001/MyTestFolder/blob/main/HF2.png" alt="Atlantic Hurricane Frequency" width="910" height="525">

- *Description of the chart and how is was created (explain the code you used and include code snippets)*
- Answer: Code Snippet and Explanation

```
import numpy as np    # linear algebra
import pandas as pd   # data processing, CSV file I/O (e.g. pd.read_csv)
import plotly.express as px
import warnings
import re

# Suppress warnings
warnings.simplefilter("ignore")

# Step 1: Read the dataset
data = pd.read_csv("https://raw.githubusercontent.com/vnwal001/MyTestFolder/refs/heads/main/atlantic.csv")

# Step 2: Clean the dataset
# Remove duplicates
data.drop_duplicates(inplace=True)

# Convert the latitude and longitude Column to numeric type.
data['Latitude'] = data['Latitude'].apply(lambda x: re.match('[0-9]{1,3}.[0-9]{0,1}' , x)[0])
data['Longitude'] = data['Longitude'].apply(lambda x: re.match('[0-9]{1,3}.[0-9]{0,1}' , x)[0])

# Convert date column to datetime format
data['Date'] = pd.to_datetime(data['Date'], format='%Y%m%d')
data['Month'] = data['Date'].apply(lambda x: x.month)
data['Year'] = data['Date'].apply(lambda x: x.year)

# Step 3: Filter data for 'HU' (Hurricane) status
# Ensure 'Status' column is cleaned and consistent for filtering
data['Status'] = data['Status'].str.strip()  # Remove leading/trailing spaces
hu_data = data[data['Status'] == 'HU']


# Step 4: Filter data from 1960 onwards
hu_data = hu_data[hu_data['Year'] >= 1960]

# Step 5: Group by Year and unique Hurricane ID to get the count of unique hurricanes per year
hurricane_counts = hu_data.groupby([ 'ID','Year']).size().reset_index(name='Count')

Year_Count = hurricane_counts['Year'].value_counts().reset_index()

Year_Count.columns = ['Year', 'Frequency']
Year_Count = Year_Count.sort_values(by='Year')


# Step 6: Aggregate to get the total number of unique hurricanes per year
#hurricane_counts_per_year = hurricane_counts.groupby('Year')['Count'].sum().reset_index(name='Frequency')

# Step 7: Plot with Plotly (Interactive plot)
fig = px.line(Year_Count, x='Year', y='Frequency',
              title='Atlantic Hurricane Frequency (1960 - 2015)',
              labels={'Year': 'Year', 'Frequency': 'Frequency'},
              markers=True)

# Step 8: Customize hover to display both Year and Frequency
fig.update_traces(mode='lines+markers', hovertemplate='Year: %{x}<br>Frequency: %{y}')


fig.add_annotation(
    x=2005,  # X-coordinate for the high point (year)
    y=15,   # Y-coordinate for the wind speed
    text="Peak Frequency = 15",  # Text to display
    showarrow=True,  # Show arrow pointing to the point
    arrowhead=2,  # Style of the arrowhead
    font=dict(
        size=12,  # Font size
        color="red",  # Font color
        family="Arial"  # Font family
    ),
    bgcolor="white",  # Background color of the annotation box
    borderpad=4,  # Padding around the text box
    align="center",  # Align text horizontally
    valign="middle"  # Align text vertically
)

# Show the plot
fig.update_layout(
    xaxis_title="Year",
    yaxis_title="Atlantic Hurricane Frequency",
    title_x=0.5,  # Center the title
    template="plotly"  # Polished default appearance similar to Seaborn
)



# Show the interactive plot
fig.show()
```

####  THE LAST 50 YEARS FROM 2015 COMPARISM

<img src="https://github.com/vnwal001/MyTestFolder/blob/main/atav.png" alt="Average Atlantic Hurricane Frequency" width="995" height="525">

```
import numpy as np    # linear algebra
import pandas as pd   # data processing, CSV file I/O (e.g. pd.read_csv)
import plotly.express as px
import warnings
import re

# Suppress warnings
warnings.simplefilter("ignore")

# Step 1: Read the dataset
data = pd.read_csv("https://raw.githubusercontent.com/vnwal001/MyTestFolder/refs/heads/main/atlantic.csv")

# Step 2: Clean the dataset
# Remove duplicates
data.drop_duplicates(inplace=True)

# Convert the latitude and longitude Column to numeric type.
data['Latitude'] = data['Latitude'].apply(lambda x: re.match('[0-9]{1,3}.[0-9]{0,1}' , x)[0])
data['Longitude'] = data['Longitude'].apply(lambda x: re.match('[0-9]{1,3}.[0-9]{0,1}' , x)[0])

# Convert date column to datetime format
data['Date'] = pd.to_datetime(data['Date'], format='%Y%m%d')
data['Month'] = data['Date'].apply(lambda x: x.month)
data['Year'] = data['Date'].apply(lambda x: x.year)

# Step 3: Filter data for 'HU' (Hurricane) status
# Ensure 'Status' column is cleaned and consistent for filtering
data['Status'] = data['Status'].str.strip()  # Remove leading/trailing spaces
hu_data = data[data['Status'] == 'HU']


# Step 3: Filter data for 'HU' (Hurricane) status
hu_data = hu_data[hu_data['Year'] >= 1960]

# Step 4: Filter data from 1960 onwards
hu_data = hu_data[hu_data['Year'] >= 1960]

# Step 5: Group by Year and unique Hurricane ID to get the count of unique hurricanes per year
hurricane_counts = hu_data.groupby([ 'ID','Year']).size().reset_index(name='Count')

Year_Count = hurricane_counts['Year'].value_counts().reset_index()

Year_Count.columns = ['Year', 'Frequency']
Year_Count = Year_Count.sort_values(by='Year')

# Step 6: Calculate the average frequency for two periods: 1965-1989 and 1990-2015
# Define the two periods
period_1 = Year_Count[(Year_Count['Year'] >= 1964) & (Year_Count['Year'] <= 1989)]
period_2 = Year_Count[(Year_Count['Year'] >= 1990) & (Year_Count['Year'] <= 2015)]

# Calculate the average frequency for each period
avg_freq_period_1 = period_1['Frequency'].mean()
avg_freq_period_2 = period_2['Frequency'].mean()

# Prepare data for the bar chart
avg_freq_data = pd.DataFrame({
    'Period': ['1964-1989', '1990-2015'],
    'Average Frequency': [avg_freq_period_1, avg_freq_period_2]
})

# Step 7: Create a bar chart to compare average frequencies
fig = px.bar(avg_freq_data, x='Period', y='Average Frequency',
             title='Comparison of Average Hurricane Frequency Over the Atlantic (1964-1989 vs. 1990-2015)',
             labels={'Average Frequency': 'Average Frequency', 'Period': 'Time Period'},
             color='Period', text='Average Frequency')  # Add text labels to bars

# Update the layout to position the text labels inside the bars
fig.update_traces(texttemplate='%{text:.2f}', textposition='inside')

# Show the plot
fig.update_layout(
    xaxis_title="Year",
    yaxis_title="Averge Frequency",
    title_x=0.5,  # Center the title
    template="plotly"  # Polished default appearance similar to Seaborn
)

# Show the plot
fig.show()

```

#### BOXPLOT OF THE LAST 50 YEARS FROM 2015 COMPARISM

<img src="https://github.com/vnwal001/MyTestFolder/blob/main/atbox.png" alt="Atlantic Hurricane Frequency" width="995" height="525">

```
import numpy as np
import pandas as pd
import plotly.express as px
import warnings
import re

# Suppress warnings
warnings.simplefilter("ignore")

# Step 1: Read the dataset
data = pd.read_csv("https://raw.githubusercontent.com/vnwal001/MyTestFolder/refs/heads/main/atlantic.csv")

# Step 2: Clean the dataset
# Remove duplicates
data.drop_duplicates(inplace=True)

# Convert the latitude and longitude Column to numeric type.
data['Latitude'] = data['Latitude'].apply(lambda x: re.match('[0-9]{1,3}.[0-9]{0,1}' , x)[0])
data['Longitude'] = data['Longitude'].apply(lambda x: re.match('[0-9]{1,3}.[0-9]{0,1}' , x)[0])

# Convert date column to datetime format
data['Date'] = pd.to_datetime(data['Date'], format='%Y%m%d')
data['Month'] = data['Date'].apply(lambda x: x.month)
data['Year'] = data['Date'].apply(lambda x: x.year)

# Step 3: Filter data for 'HU' (Hurricane) status
# Ensure 'Status' column is cleaned and consistent for filtering
data['Status'] = data['Status'].str.strip()  # Remove leading/trailing spaces
hu_data = data[data['Status'] == 'HU']


# Step 3: Filter data for hurricanes from 1960 onwards
hu_data = hu_data[hu_data['Year'] >= 1960]


# Step 4: Filter data from 1960 onwards
hu_data = hu_data[hu_data['Year'] >= 1960]

# Step 5: Group by Year and unique Hurricane ID to get the count of unique hurricanes per year
hurricane_counts = hu_data.groupby([ 'ID','Year']).size().reset_index(name='Count')

Year_Count = hurricane_counts['Year'].value_counts().reset_index()

Year_Count.columns = ['Year', 'Frequency']
Year_Count = Year_Count.sort_values(by='Year')

# Step 6: Define the two periods for comparison
period_1 = Year_Count[(Year_Count['Year'] >= 1964) & (Year_Count['Year'] <= 1989)]
period_2 = Year_Count[(Year_Count['Year'] >= 1990) & (Year_Count['Year'] <= 2015)]

# Combine the two periods for boxplot plotting
period_1['Period'] = '1964-1989'
period_2['Period'] = '1990-2015'

# Concatenate both periods into a single dataframe
combined_data = pd.concat([period_1, period_2])

# Step 7: Create a boxplot to compare frequency distributions
fig = px.box(combined_data, x='Period', y='Frequency', 
             title='Atlantic Frequency Distribution of Hurricanes (1964-1989 vs. 1990-2015)',
             labels={'Frequency': 'Hurricane Frequency', 'Period': 'Time Period'},
             color='Period')

# Adding frequency labels to the major parts of the boxplot
for period in ['1964-1989', '1990-2015']:
    # Get the statistics for the period
    period_data = combined_data[combined_data['Period'] == period]
    q1 = period_data['Frequency'].quantile(0.25)
    median = period_data['Frequency'].median()
    q3 = period_data['Frequency'].quantile(0.75)
    whisker_min = period_data['Frequency'].min()
    whisker_max = period_data['Frequency'].max()
    
    # Add labels to the boxplot
    fig.add_annotation(
        x=period, y=q1, text=f"Q1: {q1:.2f}",
        showarrow=True, arrowhead=2, font=dict(size=12, color="blue"),
        bgcolor="white", borderpad=4
    )
    fig.add_annotation(
        x=period, y=median, text=f"Median: {median:.2f}",
        showarrow=True, arrowhead=2, font=dict(size=12, color="black"),
        bgcolor="white", borderpad=4
    )
    fig.add_annotation(
        x=period, y=q3, text=f"Q3: {q3:.2f}",
        showarrow=True, arrowhead=2, font=dict(size=12, color="green"),
        bgcolor="white", borderpad=4
    )
    fig.add_annotation(
        x=period, y=whisker_min, text=f"Min: {whisker_min}",
        showarrow=True, arrowhead=2, font=dict(size=12, color="red"),
        bgcolor="white", borderpad=4
    )
    fig.add_annotation(
        x=period, y=whisker_max, text=f"Max: {whisker_max}",
        showarrow=True, arrowhead=2, font=dict(size=12, color="red"),
        bgcolor="white", borderpad=4
    )

# Show the plot
fig.update_layout(
    xaxis_title="Year",
    yaxis_title="Average Frequency",
    title_x=0.5,  # Center the title
    template="plotly"  # Polished default appearance similar to Seaborn
)

# Show the plot
fig.show()

```




#### *THE HURRICANE FREQUENCY STORY OF THE LAST 50 YEARS FROM 2015 OVER THE ATLANTIC OCEAN*

```
On the look of my first line plot, I noticed that a slight increase in the hurricane frequency over the past 50 years but,
I wanted to see more clearly into the data. I decided to break my data in two 25 year time period.
By doing this I can clearly compare both time periods to verify my suspicion. In doing this I observed the following:

- The hurriance average frequency per year increased from 5.38 to 6.88 from the time periods 1964-1989 to 1990-2015
- The boxplot clearly shows a positive trend in hurricane frequency, the median is higher in the right boxplot, it suggests that the center of the data is shifted toward higher frequency compared to the left boxplot.
- 1990-2015 time period has a higher max frequncy value, I should not that this max value is not an outlier or anomaly .i.e it is not outside the normal range compared to others. 

```











#### PACIFIC HURRICANES  FREQUENCIES

- *Description of the chart and how is was created (explain the code you used and include code snippets)*
- Answer: Code Snippet and Explanation

<img src="https://github.com/vnwal001/MyTestFolder/blob/main/HF3.png" alt="Pacific Hurricane Frequency" width="910" height="525">

```
import numpy as np    # linear algebra
import pandas as pd   # data processing, CSV file I/O (e.g. pd.read_csv)
import plotly.express as px
import warnings
import re

# Suppress warnings
warnings.simplefilter("ignore")

# Step 1: Read the dataset
data = pd.read_csv("https://raw.githubusercontent.com/vnwal001/MyTestFolder/refs/heads/main/pacific.csv")

# Step 2: Clean the dataset
# Remove duplicates
data.drop_duplicates(inplace=True)

# Convert the latitude and longitude Column to numeric type.
data['Latitude'] = data['Latitude'].apply(lambda x: re.match('[0-9]{1,3}.[0-9]{0,1}' , x)[0])
data['Longitude'] = data['Longitude'].apply(lambda x: re.match('[0-9]{1,3}.[0-9]{0,1}' , x)[0])

# Convert date column to datetime format
data['Date'] = pd.to_datetime(data['Date'], format='%Y%m%d')
data['Month'] = data['Date'].apply(lambda x: x.month)
data['Year'] = data['Date'].apply(lambda x: x.year)

# Step 3: Filter data for 'HU' (Hurricane) status
# Ensure 'Status' column is cleaned and consistent for filtering
data['Status'] = data['Status'].str.strip()  # Remove leading/trailing spaces
hu_data = data[data['Status'] == 'HU']


# Step 4: Filter data from 1960 onwards
hu_data = hu_data[hu_data['Year'] >= 1960]

# Step 5: Group by Year and unique Hurricane ID to get the count of unique hurricanes per year
hurricane_counts = hu_data.groupby([ 'ID','Year']).size().reset_index(name='Count')

Year_Count = hurricane_counts['Year'].value_counts().reset_index()

Year_Count.columns = ['Year', 'Frequency']
Year_Count = Year_Count.sort_values(by='Year')


# Step 6: Aggregate to get the total number of unique hurricanes per year
#hurricane_counts_per_year = hurricane_counts.groupby('Year')['Count'].sum().reset_index(name='Frequency')

# Step 7: Plot with Plotly (Interactive plot)
fig = px.line(Year_Count, x='Year', y='Frequency',
              title='Pacific Hurricane Frequency from 1960-2015',
              labels={'Year': 'Year', 'Frequency': 'Frequency'},
              markers=True)

# Step 8: Customize hover to display both Year and Frequency
fig.update_traces(mode='lines+markers', hovertemplate='Year: %{x}<br>Frequency: %{y}')

fig.add_annotation(
    x=1990,  # X-coordinate for the high point (year)
    y=16,   # Y-coordinate for the wind speed
    text="Peak",  # Text to display
    showarrow=True,  # Show arrow pointing to the point
    arrowhead=2,  # Style of the arrowhead
    font=dict(
        size=12,  # Font size
        color="red",  # Font color
        family="Arial"  # Font family
    ),
    bgcolor="white",  # Background color of the annotation box
    borderpad=4,  # Padding around the text box
    align="center",  # Align text horizontally
    valign="middle"  # Align text vertically
),


fig.add_annotation(
    x=1992,  # X-coordinate for the high point (year)
    y=16,   # Y-coordinate for the wind speed
    text="Peak",  # Text to display
    showarrow=True,  # Show arrow pointing to the point
    arrowhead=2,  # Style of the arrowhead
    font=dict(
        size=12,  # Font size
        color="red",  # Font color
        family="Arial"  # Font family
    ),
    bgcolor="white",  # Background color of the annotation box
    borderpad=4,  # Padding around the text box
    align="center",  # Align text horizontally
    valign="middle"  # Align text vertically
)

fig.add_annotation(
    x=2014,  # X-coordinate for the high point (year)
    y=16,   # Y-coordinate for the wind speed
    text="Peak",  # Text to display
    showarrow=True,  # Show arrow pointing to the point
    arrowhead=2,  # Style of the arrowhead
    font=dict(
        size=12,  # Font size
        color="red",  # Font color
        family="Arial"  # Font family
    ),
    bgcolor="white",  # Background color of the annotation box
    borderpad=4,  # Padding around the text box
    align="center",  # Align text horizontally
    valign="middle"  # Align text vertically
)

# Show the plot
fig.update_layout(
    xaxis_title="Year",
    yaxis_title="Pacific Hurricane Frequency",
    title_x=0.5,  # Center the title
    template="plotly"  # Polished default appearance similar to Seaborn
)


# Show the interactive plot
fig.show()

```

####  THE LAST 50 YEARS FROM 2015 AVERAGE PACIFIC HURRICANE COMPARISM 

<img src="https://github.com/vnwal001/MyTestFolder/blob/main/pcav.png" alt="Pacific Atlantic Hurricane Frequency" width="995" height="525">

```
import numpy as np    # linear algebra
import pandas as pd   # data processing, CSV file I/O (e.g. pd.read_csv)
import plotly.express as px
import warnings
import re

# Suppress warnings
warnings.simplefilter("ignore")

# Step 1: Read the dataset
data = pd.read_csv("https://raw.githubusercontent.com/vnwal001/MyTestFolder/refs/heads/main/pacific.csv")

# Step 2: Clean the dataset
# Remove duplicates
data.drop_duplicates(inplace=True)

# Convert the latitude and longitude Column to numeric type.
data['Latitude'] = data['Latitude'].apply(lambda x: re.match('[0-9]{1,3}.[0-9]{0,1}' , x)[0])
data['Longitude'] = data['Longitude'].apply(lambda x: re.match('[0-9]{1,3}.[0-9]{0,1}' , x)[0])

# Convert date column to datetime format
data['Date'] = pd.to_datetime(data['Date'], format='%Y%m%d')
data['Month'] = data['Date'].apply(lambda x: x.month)
data['Year'] = data['Date'].apply(lambda x: x.year)

# Step 3: Filter data for 'HU' (Hurricane) status
# Ensure 'Status' column is cleaned and consistent for filtering
data['Status'] = data['Status'].str.strip()  # Remove leading/trailing spaces
hu_data = data[data['Status'] == 'HU']


# Step 3: Filter data for 'HU' (Hurricane) status
hu_data = hu_data[hu_data['Year'] >= 1960]

# Step 4: Filter data from 1960 onwards
hu_data = hu_data[hu_data['Year'] >= 1960]

# Step 5: Group by Year and unique Hurricane ID to get the count of unique hurricanes per year
hurricane_counts = hu_data.groupby([ 'ID','Year']).size().reset_index(name='Count')

Year_Count = hurricane_counts['Year'].value_counts().reset_index()

Year_Count.columns = ['Year', 'Frequency']
Year_Count = Year_Count.sort_values(by='Year')

# Step 6: Calculate the average frequency for two periods: 1965-1989 and 1990-2015
# Define the two periods
period_1 = Year_Count[(Year_Count['Year'] >= 1964) & (Year_Count['Year'] <= 1989)]
period_2 = Year_Count[(Year_Count['Year'] >= 1990) & (Year_Count['Year'] <= 2015)]

# Calculate the average frequency for each period
avg_freq_period_1 = period_1['Frequency'].mean()
avg_freq_period_2 = period_2['Frequency'].mean()

# Prepare data for the bar chart
avg_freq_data = pd.DataFrame({
    'Period': ['1964-1989', '1990-2015'],
    'Average Frequency': [avg_freq_period_1, avg_freq_period_2]
})

# Step 7: Create a bar chart to compare average frequencies
fig = px.bar(avg_freq_data, x='Period', y='Average Frequency',
             title='Comparison of Average Hurricane Frequency Over the Pacific (1964-1989 vs. 1990-2015)',
             labels={'Average Frequency': 'Average Frequency', 'Period': 'Time Period'},
             color='Period', text='Average Frequency')  # Add text labels to bars

# Update the layout to position the text labels inside the bars
fig.update_traces(texttemplate='%{text:.2f}', textposition='inside')

# Show the plot
fig.update_layout(
    xaxis_title="Year",
    yaxis_title="Average Frequency",
    title_x=0.5,  # Center the title
    template="plotly"  # Polished default appearance similar to Seaborn
)

# Show the plot
fig.show()


```


####  THE LAST 50 YEARS FROM 2015 AVERAGE PACIFIC HURRICANE COMPARISM 

<img src="https://github.com/vnwal001/MyTestFolder/blob/main/pcbox.png" alt="Average Pacific Hurricane Frequency" width="995" height="525">

```
import numpy as np
import pandas as pd
import plotly.express as px
import warnings
import re

# Suppress warnings
warnings.simplefilter("ignore")

# Step 1: Read the dataset
data = pd.read_csv("https://raw.githubusercontent.com/vnwal001/MyTestFolder/refs/heads/main/pacific.csv")

# Step 2: Clean the dataset
# Remove duplicates
data.drop_duplicates(inplace=True)

# Convert the latitude and longitude Column to numeric type.
data['Latitude'] = data['Latitude'].apply(lambda x: re.match('[0-9]{1,3}.[0-9]{0,1}' , x)[0])
data['Longitude'] = data['Longitude'].apply(lambda x: re.match('[0-9]{1,3}.[0-9]{0,1}' , x)[0])

# Convert date column to datetime format
data['Date'] = pd.to_datetime(data['Date'], format='%Y%m%d')
data['Month'] = data['Date'].apply(lambda x: x.month)
data['Year'] = data['Date'].apply(lambda x: x.year)

# Step 3: Filter data for 'HU' (Hurricane) status
# Ensure 'Status' column is cleaned and consistent for filtering
data['Status'] = data['Status'].str.strip()  # Remove leading/trailing spaces
hu_data = data[data['Status'] == 'HU']


# Step 3: Filter data for hurricanes from 1960 onwards
hu_data = hu_data[hu_data['Year'] >= 1960]


# Step 4: Filter data from 1960 onwards
hu_data = hu_data[hu_data['Year'] >= 1960]

# Step 5: Group by Year and unique Hurricane ID to get the count of unique hurricanes per year
hurricane_counts = hu_data.groupby([ 'ID','Year']).size().reset_index(name='Count')

Year_Count = hurricane_counts['Year'].value_counts().reset_index()

Year_Count.columns = ['Year', 'Frequency']
Year_Count = Year_Count.sort_values(by='Year')

# Step 6: Define the two periods for comparison
period_1 = Year_Count[(Year_Count['Year'] >= 1964) & (Year_Count['Year'] <= 1989)]
period_2 = Year_Count[(Year_Count['Year'] >= 1990) & (Year_Count['Year'] <= 2015)]

# Combine the two periods for boxplot plotting
period_1['Period'] = '1964-1989'
period_2['Period'] = '1990-2015'

# Concatenate both periods into a single dataframe
combined_data = pd.concat([period_1, period_2])

# Step 7: Create a boxplot to compare frequency distributions
fig = px.box(combined_data, x='Period', y='Frequency', 
             title='Pacific Frequency Distribution of Hurricanes (1964-1989 vs. 1990-2015)',
             labels={'Frequency': 'Hurricane Frequency', 'Period': 'Time Period'},
             color='Period')

# Adding frequency labels to the major parts of the boxplot
for period in ['1964-1989', '1990-2015']:
    # Get the statistics for the period
    period_data = combined_data[combined_data['Period'] == period]
    q1 = period_data['Frequency'].quantile(0.25)
    median = period_data['Frequency'].median()
    q3 = period_data['Frequency'].quantile(0.75)
    whisker_min = period_data['Frequency'].min()
    whisker_max = period_data['Frequency'].max()
    
    # Add labels to the boxplot
    fig.add_annotation(
        x=period, y=q1, text=f"Q1: {q1:.2f}",
        showarrow=True, arrowhead=2, font=dict(size=12, color="blue"),
        bgcolor="white", borderpad=4
    )
    fig.add_annotation(
        x=period, y=median, text=f"Median: {median:.2f}",
        showarrow=True, arrowhead=2, font=dict(size=12, color="black"),
        bgcolor="white", borderpad=4
    )
    fig.add_annotation(
        x=period, y=q3, text=f"Q3: {q3:.2f}",
        showarrow=True, arrowhead=2, font=dict(size=12, color="green"),
        bgcolor="white", borderpad=4
    )
    fig.add_annotation(
        x=period, y=whisker_min, text=f"Min: {whisker_min}",
        showarrow=True, arrowhead=2, font=dict(size=12, color="red"),
        bgcolor="white", borderpad=4
    )
    fig.add_annotation(
        x=period, y=whisker_max, text=f"Max: {whisker_max}",
        showarrow=True, arrowhead=2, font=dict(size=12, color="red"),
        bgcolor="white", borderpad=4
    )

# Show the plot
fig.update_layout(
    xaxis_title="Year",
    yaxis_title="Pacific Hurricane Frequency",
    title_x=0.5,  # Center the title
    template="plotly"  # Polished default appearance similar to Seaborn
)

# Show the plot
fig.show()

```


#### *THE HURRICANE FREQUENCY STORY OF THE LAST 50 YEARS FROM 2015 OVER THE PACIFIC OCEAN*

```
Just like the data from the atlantic ocean, I wanted to make sense of the peaks and valleys in frequency trend plot so I decided to break my data in two 25 year time period.
By doing this I can clearly compare both time periods to verify what's actually happening . In doing this I observed the following:

- The hurriance average frequency per year increased from 8.15 to 9.05 from the time periods 1964-1989 to 1990-2015
- The boxplot also clearly shows a positive trend in hurricane frequency, the median is higher in the right boxplot, it suggests that the center of the data is shifted toward higher frequency compared to the left boxplot.
- 1990-2015 time period also has a higher max frequncy value.

```
#### SECTION 2 B QUESTION A ANSWERS
#### ANALYZING HURRICANE MAX WIND SPEEDS

#### ATLANTIC HURRICANES MAX WIND SPEEDS


<img src="https://github.com/vnwal001/MyTestFolder/blob/main/atws.png" alt="Atlantic Hurricane Max Wind" width="910" height="525">

```
# Step 1: Import necessary libraries
import numpy as np
import pandas as pd
import plotly.express as px
import seaborn as sns
import matplotlib.colors as mcolors
import warnings

# Suppress warnings
warnings.simplefilter("ignore")

# Set Seaborn style
sns.set(style="whitegrid")

# Step 2: Read the dataset
data = pd.read_csv("https://raw.githubusercontent.com/vnwal001/MyTestFolder/refs/heads/main/atlantic.csv")

# Step 3: Clean the dataset
# Remove leading and trailing spaces from all string columns
data = data.applymap(lambda x: x.strip() if isinstance(x, str) else x)

# Remove duplicates
data.drop_duplicates(inplace=True)

# Ensure the 'Date' column is in datetime format
data['Date'] = pd.to_datetime(data['Date'], format='%Y%m%d')

# Extract the year from the 'Date' column
data['Year'] = data['Date'].dt.year

# Step 4: Filter data for Status = 'HU' (Hurricane)
hu_data = data[data['Status'] == 'HU']
unique_ids_hurr = set(hu_data['ID'])
hu_data = data[data['ID'].isin(unique_ids_hurr)]

# Step 5: Handle Missing or Invalid Maximum Wind Values
# Drop rows where 'Maximum Wind' is missing or invalid
hu_data = hu_data.dropna(subset=['Maximum Wind'])
# Step 6: Get the maximum wind speed for each year
max_wind_per_year = hu_data.groupby('Year')['Maximum Wind'].max().reset_index()

# Filter the data to only include years from 1960 onwards
max_wind_per_year = max_wind_per_year[max_wind_per_year['Year'] >= 1960]

# Step 7: Plot using Plotly for an interactive graph with Seaborn style
fig = px.line(max_wind_per_year, x='Year', y='Maximum Wind',
              markers=True, title='Atlantic Max Wind Speed by Year for Hurrianes')

# Add hover data to highlight speed and year on hover
fig.update_traces(
    hovertemplate="<b>Year:</b> %{x}<br><b>Max Wind (knots):</b> %{y}<extra></extra>"
)

# Apply Seaborn's color palette, and convert it to a hex format for Plotly
palette = sns.color_palette("muted", 1)  # Muted color palette from Seaborn
hex_color = mcolors.to_hex(palette[0])  # Convert RGB tuple to hex using matplotlib

# Update the line color with the hex color
fig.update_traces(line=dict(color=hex_color))


fig.add_annotation(
    x=1980,  # X-coordinate for the high point (year)
    y=165,   # Y-coordinate for the wind speed
    text="Highest",  # Text to display
    showarrow=True,  # Show arrow pointing to the point
    arrowhead=2,  # Style of the arrowhead
    font=dict(
        size=12,  # Font size
        color="red",  # Font color
        family="Arial"  # Font family
    ),
    bgcolor="white",  # Background color of the annotation box
    borderpad=4,  # Padding around the text box
    align="center",  # Align text horizontally
    valign="middle"  # Align text vertically
)

# Show the plot
fig.update_layout(
    xaxis_title="Year",
    yaxis_title="Max Wind (knots)",
    title_x=0.5,  # Center the title
    template="plotly"  # Polished default appearance similar to Seaborn
)

fig.show()

```


#### ATLANTIC HURRICANES AVERAGE MAX WIND SPEEDS FOR THE LAST 50 YEARS FROM 2015


<img src="https://github.com/vnwal001/MyTestFolder/blob/main/atavwp.png" alt="Atlantic Hurricane Max Wind" width="910" height="525">

```
import numpy as np
import pandas as pd
import plotly.express as px
import seaborn as sns
import matplotlib.colors as mcolors
import warnings

# Suppress warnings
warnings.simplefilter("ignore")

# Set Seaborn style
sns.set(style="whitegrid")

# Step 2: Read the dataset
data = pd.read_csv("https://raw.githubusercontent.com/vnwal001/MyTestFolder/refs/heads/main/atlantic.csv")

# Step 3: Clean the dataset
# Remove leading and trailing spaces from all string columns
data = data.applymap(lambda x: x.strip() if isinstance(x, str) else x)

# Remove duplicates
data.drop_duplicates(inplace=True)

# Ensure the 'Date' column is in datetime format
data['Date'] = pd.to_datetime(data['Date'], format='%Y%m%d')

# Extract the year from the 'Date' column
data['Year'] = data['Date'].dt.year

# Step 4: Filter data for Status = 'HU' (Hurricane)
hu_data = data[data['Status'] == 'HU']
unique_ids_hurr = set(hu_data['ID'])
hu_data = data[data['ID'].isin(unique_ids_hurr)]

# Step 5: Handle Missing or Invalid Maximum Wind Values
# Drop rows where 'Maximum Wind' is missing or invalid
hu_data = hu_data.dropna(subset=['Maximum Wind'])

# Step 6: Calculate the average Maximum Wind for each year
average_wind_per_year = hu_data.groupby('Year')['Maximum Wind'].mean().reset_index()

# Filter the data to only include years from 1960 onwards
average_wind_per_year = average_wind_per_year[average_wind_per_year['Year'] >= 1960]

# Step 7: Calculate the average wind speed for two periods: 1964-1989 and 1990-2015
# Define the two periods
period_1 = average_wind_per_year[(average_wind_per_year['Year'] >= 1964) & (average_wind_per_year['Year'] <= 1989)]
period_2 = average_wind_per_year[(average_wind_per_year['Year'] >= 1990) & (average_wind_per_year['Year'] <= 2015)]

# Calculate the average wind speed for each period
avg_wind_period_1 = period_1['Maximum Wind'].mean()
avg_wind_period_2 = period_2['Maximum Wind'].mean()

# Prepare data for the bar chart
avg_wind_data = pd.DataFrame({
    'Period': ['1964-1989', '1990-2015'],
    'Average Wind Speed (knots)': [avg_wind_period_1, avg_wind_period_2]
})

# Step 8: Create a bar chart to compare average wind speeds
fig = px.bar(avg_wind_data, x='Period', y='Average Wind Speed (knots)',
             title='Comparison of Average Atlantic Hurricane Wind Speed (1964-1989 vs. 1990-2015)',
             labels={'Average Wind Speed (knots)': 'Average Wind Speed (knots)', 'Period': 'Time Period'},
             color='Period')

# Add labels on top of the bars
fig.update_traces(text=avg_wind_data['Average Wind Speed (knots)'].round(2),  # Add rounded wind speed values as labels
                  textposition='outside',  # Position the labels outside the bars
                  texttemplate='%{text}')  # Format the text to show the exact value
# Show the plot
fig.update_layout(
    xaxis_title="Year",
    yaxis_title="Average Wind Speed (Knots)",
    title_x=0.5,  # Center the title
    template="plotly"  # Polished default appearance similar to Seaborn
)

# Show the plot
fig.show()

```

#### ATLANTIC HURRICANES MAX WIND SPEEDS BOX PLOT 

<img src="https://github.com/vnwal001/MyTestFolder/blob/main/atavwsbox.png" alt="Pacific Hurricane Frequency" width="910" height="525">


```
import numpy as np
import pandas as pd
import plotly.express as px
import seaborn as sns
import warnings

# Suppress warnings
warnings.simplefilter("ignore")

# Set Seaborn style
sns.set(style="whitegrid")

# Step 2: Read the dataset
data = pd.read_csv("https://raw.githubusercontent.com/vnwal001/MyTestFolder/refs/heads/main/atlantic.csv")

# Step 3: Clean the dataset
# Remove leading and trailing spaces from all string columns
data = data.applymap(lambda x: x.strip() if isinstance(x, str) else x)

# Remove duplicates
data.drop_duplicates(inplace=True)

# Ensure the 'Date' column is in datetime format
data['Date'] = pd.to_datetime(data['Date'], format='%Y%m%d')

# Extract the year from the 'Date' column
data['Year'] = data['Date'].dt.year

# Step 4: Filter data for Status = 'HU' (Hurricane)
hu_data = data[data['Status'] == 'HU']
unique_ids_hurr = set(hu_data['ID'])
hu_data = data[data['ID'].isin(unique_ids_hurr)]

# Step 5: Handle Missing or Invalid Maximum Wind Values
# Drop rows where 'Maximum Wind' is missing or invalid
hu_data = hu_data.dropna(subset=['Maximum Wind'])

# Step 6: Create Periods for Comparison
# Define the two periods
period_1 = hu_data[(hu_data['Year'] >= 1964) & (hu_data['Year'] <= 1989)]
period_2 = hu_data[(hu_data['Year'] >= 1990) & (hu_data['Year'] <= 2015)]

# Add a column for the period (for boxplot comparison)
period_1['Period'] = '1964-1989'
period_2['Period'] = '1990-2015'

# Combine the two periods into a single dataframe
combined_data = pd.concat([period_1, period_2])

# Step 7: Create a boxplot to compare wind speed distributions
fig = px.box(combined_data, x='Period', y='Maximum Wind',
             title='Atlantic Distribution of Hurricane Wind Speeds (1964-1989 vs. 1990-2015)',
             labels={'Maximum Wind': 'Wind Speed (knots)', 'Period': 'Time Period'},
             color='Period')


# Show the plot
fig.update_layout(
    xaxis_title="Year",
    yaxis_title="Wind Speed (Knots)",
    title_x=0.5,  # Center the title
    template="plotly"  # Polished default appearance similar to Seaborn
)

# Show the plot
fig.show()

```

#### *THE HURRICANE MAX WIND SPEED STORY OF THE LAST 50 YEARS FROM 2015 OVER THE ATLANTIC OCEAN*

```
I wanted to make sense of the peaks and valleys in frequency trend plot so I decided to break my data in two 25 year time period.
By doing this I can clearly compare both time periods to verify what's actually happening . In doing this I observed the following:

- The hurriance average frequency has relatively been the same in periods 1964-1989 to 1990-2015, 1990-2015 has very small or negligible increase. 
- The boxplot distribution also clearly shows almost the same for both time periods with outliers  (relatively higher than normal) existing in both time periods 

```


#### ANALYZING PACIFIC HURRICANES MAX WIND SPEEDS

#### PACIFIC HURRICANES MAX WIND SPEEDS

<img src="https://github.com/vnwal001/MyTestFolder/blob/main/pcws.png" alt="Pacific Max Wind Speed" width="910" height="525">

```
# Step 1: Import necessary libraries
import numpy as np
import pandas as pd
import plotly.express as px
import seaborn as sns
import matplotlib.colors as mcolors
import warnings

# Suppress warnings
warnings.simplefilter("ignore")

# Set Seaborn style
sns.set(style="whitegrid")

# Step 2: Read the dataset
data = pd.read_csv("https://raw.githubusercontent.com/vnwal001/MyTestFolder/refs/heads/main/pacific.csv")

# Step 3: Clean the dataset
# Remove leading and trailing spaces from all string columns
data = data.applymap(lambda x: x.strip() if isinstance(x, str) else x)

# Remove duplicates
data.drop_duplicates(inplace=True)

# Ensure the 'Date' column is in datetime format
data['Date'] = pd.to_datetime(data['Date'], format='%Y%m%d')

# Extract the year from the 'Date' column
data['Year'] = data['Date'].dt.year

# Step 4: Filter data for Status = 'HU' (Hurricane)
hu_data = data[data['Status'] == 'HU']
unique_ids_hurr = set(hu_data['ID'])
hu_data = data[data['ID'].isin(unique_ids_hurr)]

# Step 5: Handle Missing or Invalid Maximum Wind Values
# Drop rows where 'Maximum Wind' is missing or invalid
hu_data = hu_data.dropna(subset=['Maximum Wind'])

# Step 6: Get the maximum wind speed for each year
max_wind_per_year = hu_data.groupby('Year')['Maximum Wind'].max().reset_index()

# Step 7: Plot using Plotly for an interactive graph with Seaborn style
fig = px.line(max_wind_per_year, x='Year', y='Maximum Wind',
              markers=True, title='Pacific Max Wind Speed by Year for Hurricanes')

# Add hover data to highlight speed and year on hover
fig.update_traces(
    hovertemplate="<b>Year:</b> %{x}<br><b>Max Wind (knots):</b> %{y}<extra></extra>"
)

# Apply Seaborn's color palette, and convert it to a hex format for Plotly
palette = sns.color_palette("muted", 1)  # Muted color palette from Seaborn
hex_color = mcolors.to_hex(palette[0])  # Convert RGB tuple to hex using matplotlib

# Update the line color with the hex color
fig.update_traces(line=dict(color=hex_color))

# Add vertical lines for the periods of lower, intermediate, and higher wind speeds
fig.add_vline(x=1970, line=dict(color="black", dash="dot"), annotation_text="", annotation_position="top left")
fig.add_vline(x=1990, line=dict(color="black", dash="dot"), annotation_text="", annotation_position="top left")

# Annotate the periods to make it clearer
fig.add_annotation(
    x=1960,  # Position near the first period
    y=max_wind_per_year['Maximum Wind'].min() + 10,  # Position vertically slightly above the minimum wind speed
    text="Lower Wind Speeds\n(before 1970)",
    showarrow=False,
    font=dict(size=12, color="blue"),
    bgcolor="white",
    borderpad=4
)

fig.add_annotation(
    x=1980,  # Position near the second period
    y=max_wind_per_year['Maximum Wind'].min() + 80,  # Position vertically slightly above the minimum wind speed
    text="Intermediate Wind Speeds\n(1970-1990)",
    showarrow=False,
    font=dict(size=12, color="green"),
    bgcolor="white",
    borderpad=4
)

fig.add_annotation(
    x=2005,  # Position near the third period
    y=max_wind_per_year['Maximum Wind'].min() + 100,  # Position vertically slightly above the minimum wind speed
    text="Higher Wind Speeds\n(after 1990)",
    showarrow=False,
    font=dict(size=12, color="red"),
    bgcolor="white",
    borderpad=4
)

# Add text annotation at the high point
fig.add_annotation(
    x=2015,  # X-coordinate for the high point (year)
    y=185,   # Y-coordinate for the wind speed
    text="Highest",  # Text to display
    showarrow=True,  # Show arrow pointing to the point
    arrowhead=2,  # Style of the arrowhead
    font=dict(
        size=12,  # Font size
        color="red",  # Font color
        family="Arial"  # Font family
    ),
    bgcolor="white",  # Background color of the annotation box
    borderpad=4,  # Padding around the text box
    align="center",  # Align text horizontally
    valign="middle"  # Align text vertically
)

# Show the plot
fig.update_layout(
    xaxis_title="Year",
    yaxis_title="Max Wind (knots)",
    title_x=0.5,  # Center the title
    template="plotly"  # Polished default appearance similar to Seaborn
)

fig.show()

```

#### PACIFIC HURRICANES MAX WIND SPEEDS BAR PLOT OF THE LAST 50 YEARS FROM 2025

<img src="https://github.com/vnwal001/MyTestFolder/blob/main/pcavws.png" alt="Average Pacific Max Wind Speed" width="910" height="525">

```
import numpy as np
import pandas as pd
import plotly.express as px
import seaborn as sns
import matplotlib.colors as mcolors
import warnings

# Suppress warnings
warnings.simplefilter("ignore")

# Set Seaborn style
sns.set(style="whitegrid")

# Step 2: Read the dataset
data = pd.read_csv("https://raw.githubusercontent.com/vnwal001/MyTestFolder/refs/heads/main/pacific.csv")

# Step 3: Clean the dataset
# Remove leading and trailing spaces from all string columns
data = data.applymap(lambda x: x.strip() if isinstance(x, str) else x)

# Remove duplicates
data.drop_duplicates(inplace=True)

# Ensure the 'Date' column is in datetime format
data['Date'] = pd.to_datetime(data['Date'], format='%Y%m%d')

# Extract the year from the 'Date' column
data['Year'] = data['Date'].dt.year

# Step 4: Filter data for Status = 'HU' (Hurricane)
hu_data = data[data['Status'] == 'HU']
unique_ids_hurr = set(hu_data['ID'])
hu_data = data[data['ID'].isin(unique_ids_hurr)]

# Step 5: Handle Missing or Invalid Maximum Wind Values
# Drop rows where 'Maximum Wind' is missing or invalid
hu_data = hu_data.dropna(subset=['Maximum Wind'])

# Step 6: Calculate the average Maximum Wind for each year
average_wind_per_year = hu_data.groupby('Year')['Maximum Wind'].mean().reset_index()

# Filter the data to only include years from 1960 onwards
average_wind_per_year = average_wind_per_year[average_wind_per_year['Year'] >= 1960]

# Step 7: Calculate the average wind speed for two periods: 1965-1989 and 1990-2015
# Define the two periods
period_1 = average_wind_per_year[(average_wind_per_year['Year'] >= 1964) & (average_wind_per_year['Year'] <= 1989)]
period_2 = average_wind_per_year[(average_wind_per_year['Year'] >= 1990) & (average_wind_per_year['Year'] <= 2015)]

# Calculate the average wind speed for each period
avg_wind_period_1 = period_1['Maximum Wind'].mean()
avg_wind_period_2 = period_2['Maximum Wind'].mean()

# Prepare data for the bar chart
avg_wind_data = pd.DataFrame({
    'Period': ['1964-1989', '1990-2015'],
    'Average Wind Speed (knots)': [avg_wind_period_1, avg_wind_period_2]
})

# Step 8: Create a bar chart to compare average wind speeds
fig = px.bar(avg_wind_data, x='Period', y='Average Wind Speed (knots)',
             title='Pacific Comparison of Average Hurricane Wind Speed (1964-1989 vs. 1990-2015)',
             labels={'Average Wind Speed (knots)': 'Average Wind Speed (knots)', 'Period': 'Time Period'},
             color='Period')

# Remove the labels by not adding them to the traces
fig.update_traces(text=None)  # No text labels

# Update the layout for the graph
fig.update_layout(
    xaxis_title="Time Period",
    yaxis_title="Average Wind Speed (Knots)",
    title_x=0.5,  # Center the title
    template="plotly"  # Use Plotly's default template for styling
)

# Show the plot
fig.show()

```

#### PACIFIC HURRICANES MAX WIND SPEEDS BOX PLOT PLOT DISTRIBUTION OF THE LAST 50 YEARS FROM 2025

<img src="https://github.com/vnwal001/MyTestFolder/blob/main/pcavwsbox.png" alt="Pacific Max Wind Speed Box " width="910" height="525">

```
import numpy as np
import pandas as pd
import plotly.express as px
import seaborn as sns
import warnings

# Suppress warnings
warnings.simplefilter("ignore")

# Set Seaborn style
sns.set(style="whitegrid")

# Step 2: Read the dataset
data = pd.read_csv("https://raw.githubusercontent.com/vnwal001/MyTestFolder/refs/heads/main/pacific.csv")

# Step 3: Clean the dataset
# Remove leading and trailing spaces from all string columns
data = data.applymap(lambda x: x.strip() if isinstance(x, str) else x)

# Remove duplicates
data.drop_duplicates(inplace=True)

# Ensure the 'Date' column is in datetime format
data['Date'] = pd.to_datetime(data['Date'], format='%Y%m%d')

# Extract the year from the 'Date' column
data['Year'] = data['Date'].dt.year

# Step 4: Filter data for Status = 'HU' (Hurricane)
hu_data = data[data['Status'] == 'HU']
unique_ids_hurr = set(hu_data['ID'])
hu_data = data[data['ID'].isin(unique_ids_hurr)]

# Step 5: Handle Missing or Invalid Maximum Wind Values
# Drop rows where 'Maximum Wind' is missing or invalid
hu_data = hu_data.dropna(subset=['Maximum Wind'])

# Step 6: Create Periods for Comparison
# Define the two periods
period_1 = hu_data[(hu_data['Year'] >= 1964) & (hu_data['Year'] <= 1989)]
period_2 = hu_data[(hu_data['Year'] >= 1990) & (hu_data['Year'] <= 2015)]

# Add a column for the period (for boxplot comparison)
period_1['Period'] = '1964-1989'
period_2['Period'] = '1990-2015'

# Combine the two periods into a single dataframe
combined_data = pd.concat([period_1, period_2])

# Step 7: Create a boxplot to compare wind speed distributions
fig = px.box(combined_data, x='Period', y='Maximum Wind',
             title='Pacific Distribution of Hurricane Wind Speeds (1965-1989 vs. 1990-2015)',
             labels={'Maximum Wind': 'Wind Speed (knots)', 'Period': 'Time Period'},
             color='Period')

fig.update_layout(
    xaxis_title="Time Period",
    yaxis_title="Max Wind Speed (Knots)",
    title_x=0.5,  # Center the title
    template="plotly"  # Use Plotly's default template for styling
)

# Show the plot
fig.show()

```

#### *THE HURRICANE MAX WIND SPEED STORY OF THE LAST 50 YEARS FROM 2015 OVER THE PACIFIC OCEAN*

```
I wanted to make sense of the peaks and valleys in frequency trend plot so I decided to break my data in two 25 year time period.
By doing this I can clearly compare both time periods to verify what's actually happening . In doing this I observed the following:

- The hurriance average frequency has relatively been the same in periods 1964-1989 to 1990-2015, 1990-2015 has very small or negligible increase. 
- The boxplot distribution also clearly shows almost the same for both time periods with outliers  (relatively higher than normal) existing more in time period 1990-2015

```


#### SECTION 2 C ANSWERS 

#### ATLANTIC VS PACIFIC HURRICANES

<img src="https://github.com/vnwal001/MyTestFolder/blob/main/HF1.png" alt="Pacific Hurricane Frequency" width="910" height="525">
  
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
  1) I observed that about 25 states have populations clustered around the first bin which is between 4 million and 5 million people. This means they have a population of between 4 and 5 million people or less.
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
```
  1) I observed that about 90% of the states in both 1995 and 2009 are have a population of 15 million or less people. 

 ```    


## PART 2 : FURTHER ANALYSIS

### The first Boxplot showed some outliers I wanted to answer the following question:
### 1) What states population were the outliers in 1970, 1985, 1995 and 2009.
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
 - I was amazed to find that 1970 had six states that were outliers because visually I could only count 5 in the first boxplot. It appears 2 points overlapped

 #### 2)  Were the same states consistently outliers in each of those years and why were they outliers in those years or not
 - California, New York and Texas were consistent outliers in 1970, 1985, 1995 and 2009.
 - Ohio and Pennsylvania were outliers in 1970 but by 1985, they were no longer outliers while in 1995 AND 2009 Florida was an outlier.
 
In 1970, the populations of California, New York, Texas, Illinois, Pennsylvania, and Ohio were indeed significantly higher than those of many other states. These states represented key economic centers and had large urban populations, which contributed to their higher numbers. However, demographic trends were starting to shift, with some states like New York beginning to lose population to more rapidly growing states like California and Texas.

By 1985, the combination of economic decline, outmigration, demographic shifts, and the rapid growth of other states contributed to Pennsylvania and Ohio no longer having significantly high populations compared to states like California and Texas. These trends reflected broader changes in the U.S. economy and population movements during that period.



 ### Other Observations 
 ##### I wanted to see the average population growth or decline of the states that were consistenly outliers this will help me determine the state with fastest population growth rate among them.
 ##### The states I compared were California, New York and Texas

 | <img src="https://github.com/vnwal001/MyTestFolder/blob/main/h5.png" alt="Average Population Growth from 1970 to 2009" width="500"/> | <img src="https://github.com/vnwal001/MyTestFolder/blob/main/h6.png" alt="Average Population over those Years" width="500"/> |
|------------------------------------------------------|------------------------------------------------------|
```
I OBSERVED THAT THOUGH CALIFORNIA HAS THE HIGHER POPULATION, TEXAS HAS THE HIGHEST GROWTH RATE
Average Percentage Growth from the years 1970, 1985, 1995 and 2009:
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




## INTRIGUING OBSERVATION
### This observation was just something I found that awakened my curiosity about these charts, and I tried to find out why.
### I found that the shape of the of the eCDF and Horizontal Bar Chart (sorted in descending order) looked the same. I observed this for 2009. I could assumed it could be applied to other years
### With this mystery and knowing the number of outliers from the 2009 boxplot, visually helped me to locate the outlier points in both graph. This was purely out of curiosity. 

 | <img src="https://github.com/vnwal001/MyTestFolder/blob/main/h7.png" alt="2009 Population Histogram" width="500"/> | <img src="https://github.com/vnwal001/MyTestFolder/blob/main/h8.png" alt="2009 Population eCDF" width="500"/> |
|------------------------------------------------------|------------------------------------------------------|

- Explanation

```
Although the Horzontal Bar chart and eCDF convey different types of information (individual values vs. cumulative distribution), the overall shape of the two visualizations reflects the same underlying data, leading to their visual similarity when the bar chart is sorted in descending order.
```
**Python Code**

```
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

# Step 1: Load the data
data = pd.read_csv('https://raw.githubusercontent.com/vnwal001/MyTestFolder/refs/heads/main/population4.csv')

# Step 2: Extract relevant columns for the year 2009
pop_2009 = pd.to_numeric(data['2009'], errors='coerce').dropna()
states = data['State'][pop_2009.index]  # Get the corresponding state names

# Step 3: Create a DataFrame for 2009 populations
pop_2009_df = pd.DataFrame({'State': states, 'Population': pop_2009})

# Step 4: Exclude Washington, D.C.
pop_2009_df = pop_2009_df[pop_2009_df['State'] != 'District of Columbia']

# Step 5: Sort by population
pop_2009_df = pop_2009_df.sort_values(by='Population', ascending=False)

# Step 6: Create the bar chart using Seaborn
plt.figure(figsize=(12, 8))
sns.barplot(x='Population', y='State', data=pop_2009_df, palette='viridis')

# Add titles and labels
plt.title('State Populations in 2009 (Excluding D.C.)')
plt.xlabel('Population')
plt.ylabel('State')
plt.grid(axis='x')

# Show the plot
plt.tight_layout()
plt.show()
```

```
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt

# Step 1: Load the data
data = pd.read_csv('https://raw.githubusercontent.com/vnwal001/MyTestFolder/refs/heads/main/population4.csv')

# Step 2: Exclude Washington D.C.
data = data[data['State'] != 'District of Columbia']

# Step 3: Extract population data for the year 2009
population_2009 = pd.to_numeric(data['2009'], errors='coerce').dropna()

# Step 4: Create the eCDF plot for 2009
plt.figure(figsize=(10, 6))

# Plot eCDF for 2009
sns.ecdfplot(population_2009, label='2009', color='salmon', linewidth=2)

# Add titles and labels
plt.title('Empirical Cumulative Distribution Function (eCDF) of State Populations in 2009 (Excluding D.C.)')
plt.xlabel('Population (in thousands)')
plt.ylabel('ECDF')
plt.grid(True)

# Show the plot
plt.tight_layout()
plt.show()
```



**References:**
- https://chatgpt.com/
- https://www.census.gov/library/publications/2010/compendia/statab/130ed/population.html
