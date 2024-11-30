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
#### PART 1 Hurricane Intensity and Frequency Analysis
- A) Is the frequency of hurricanes increasing over time?
- B) Are hurricane wind speeds becoming stronger over time?
- C) What are the potential factors contributing to any observed changes in hurricane frequency and intensity?

#### PART 2 Comparative Analysis of Atlantic vs. Pacific Hurricanes
- How do trends in hurricane frequency and intensity differ between the Atlantic and Pacific Oceans over years?

#### PART 3 Impact of Category 5 Hurricanes on Population Migration
- For hurricanes that make landfall in the U.S., is there a noticeable migration response in the aftermath of Category 5 hurricanes? Specifically, does the population of selected cities decrease significantly a year  after the hurricane, and can any patterns be identified?



##### To prepare my data for use I did the following:
- The original file was in xls format, I created a new csv file to place my data
- I copied over the values of the only data I needed for my analysis, I copied the state population data for just 1970, 1985, 1995 and 2009.
- For the specified years, if they had both and April and July data, I picked the July data for that year because most years data were collected in July. I only took the April data if there was not a July date available
- I renamed the Years column to just the Year and removed the month.
  

#### SECTION 2 QUESTION A ANSWERS 
#### ANALYZING HURRICANE FREQUENCIES
#### ATLANTIC HURRICANES FREQUENCIES
<img src="https://github.com/vnwal001/MyTestFolder/blob/main/HF2.png" alt="Atlantic Hurricane Frequency" width="910" height="525">

Idiom: Bar Chart / Mark: Line

| Data: Attribute | Data: Attribute Type  | Encode: Channel | 
| --- | --- | --- |
| Year | key, categorical | horizontal spatial region (x-axis) |
| Frequency | value, quantitative | vertical spatial region (y-axis) |


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

Idiom: Bar Chart / Mark: Bar

| Data: Attribute       | Data: Attribute Type  | Encode: Channel                             |
|-----------------------|-----------------------|---------------------------------------------|
| Period                | Key, Categorical      | Horizontal spatial region (x-axis)          |
| Average Frequency     | Value, Quantitative   | Vertical spatial region (y-axis)            |


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

Idiom: Boxplot / Mark: Box

| Data: Attribute       | Data: Attribute Type  | Encode: Channel                             |
|-----------------------|-----------------------|---------------------------------------------|
| Period                | Key, Categorical      | Horizontal spatial region (x-axis)          |
| Frequency             | Value, Quantitative   | Vertical spatial region (y-axis)            |


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

Idiom: Line Chart / Mark: Line

| Data: Attribute       | Data: Attribute Type  | Encode: Channel                             |
|-----------------------|-----------------------|---------------------------------------------|
| Year                  | Key, Categorical      | Horizontal position (x-axis)                |
| Frequency             | Value, Quantitative   | Vertical position (y-axis)                  |


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

Idiom: Bar Chart / Mark: Bar

| Data: Attribute       | Data: Attribute Type  | Encode: Channel                             |
|-----------------------|-----------------------|---------------------------------------------|
| Period                | Key, Categorical      | Horizontal position (x-axis)                |
| Average Frequency     | Value, Quantitative   | Vertical position (y-axis)                  |


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

Idiom: Box Plot / Mark: Box

| Data: Attribute       | Data: Attribute Type  | Encode: Channel                             |
|-----------------------|-----------------------|---------------------------------------------|
| Period                | Key, Categorical      | Horizontal position (x-axis)                |
| Frequency             | Value, Quantitative   | Vertical position (y-axis)                  |


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

Idiom: Line Plot / Mark: Line

| Data: Attribute       | Data: Attribute Type  | Encode: Channel                             |
|-----------------------|-----------------------|---------------------------------------------|
| Year                  | Temporal, Quantitative| Horizontal position (x-axis)                |
| Maximum Wind          | Quantitative          | Vertical position (y-axis)                  |


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

Idiom: Bar Chart / Mark: Bar

| Data: Attribute             | Data: Attribute Type | Encode: Channel                             |
|-----------------------------|----------------------|---------------------------------------------|
| Period                      | Temporal, Ordinal    | Horizontal position (x-axis)                |
| Average Wind Speed (knots)  | Quantitative         | Vertical position (y-axis)                  |


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

Idiom: Box Plot / Mark: Box

| Data: Attribute             | Data: Attribute Type | Encode: Channel                             |
|-----------------------------|----------------------|---------------------------------------------|
| Period                      | Temporal, Ordinal    | Horizontal position (x-axis)                |
| Maximum Wind                | Quantitative         | Vertical position (y-axis)                  |



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

- The hurriance wind speeds have increased from 55.4 to 56.4 from 1964-1989 to 1990-2015. 
- The boxplot distribution also clearly shows almost the same for both time periods with outliers  (relatively higher than normal) existing in both time periods 

```


#### ANALYZING PACIFIC HURRICANES MAX WIND SPEEDS

#### PACIFIC HURRICANES MAX WIND SPEEDS

<img src="https://github.com/vnwal001/MyTestFolder/blob/main/pcws.png" alt="Pacific Max Wind Speed" width="910" height="525">

Idiom: Line Plot / Mark: Line

| Data: Attribute             | Data: Attribute Type | Encode: Channel                             |
|-----------------------------|----------------------|---------------------------------------------|
| Year                        | Temporal             | Horizontal position (x-axis)                |
| Maximum Wind                | Quantitative         | Vertical position (y-axis)                  |


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

Idiom: Bar Chart / Mark: Bar

| Data: Attribute               | Data: Attribute Type | Encode: Channel                            |
|--------------------------------|----------------------|--------------------------------------------|
| Period                         | Ordinal              | Horizontal position (x-axis)               |
| Average Wind Speed (knots)     | Quantitative         | Vertical position (y-axis)                 |
| Period (for coloring)          | Nominal              | Color (to distinguish between periods)     |


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

Idiom: Boxplot / Mark: Box

| Data: Attribute               | Data: Attribute Type | Encode: Channel                            |
|--------------------------------|----------------------|--------------------------------------------|
| Period                         | Ordinal              | Horizontal position (x-axis)               |
| Maximum Wind                   | Quantitative         | Vertical position (y-axis)                 |
| Period (for coloring)          | Nominal              | Color (to differentiate between periods)   |


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

- The hurriance wind speeds have increased from 55.5 to 56.2 from 1964-1989 to 1990-2015. 
- The boxplot distribution also clearly shows almost the same for both time periods with outliers  (relatively higher than normal) existing more in time period 1990-2015

```



#### SECTION 2 C:  What are the potential factors contributing to any observed changes in hurricane frequency and intensity?

#### SECTION 2 C ANSWERS 

```
These are the contributors to observed increase in hurricane frequency over the atlantic and pacific over the last 50 years from 2015:

- Rising sea surface temperatures (due to global warming),
- Reduced vertical wind shear (favorable atmospheric conditions),
- Climate oscillations, El Niño-Southern Oscillation (ENSO), the Atlantic Multidecadal Oscillation (AMO), and the Pacific Decadal Oscillation (PDO)
- Changes in atmospheric circulation patterns, and
- Improved monitoring and detection of storms.

The general hurricane windspeed average has relatively been the same from 1965 to 2015 but there has certainly been notable extraordinarily high wind speeds (outliers) :

- Climate Change (Rising Sea Surface Temperatures)
- Low Vertical Wind Shear (Favorable Atmospheric Conditions)
- Increased Ocean Heat Content (Deep Ocean Energy)
- Weakening Jet Stream and Atmospheric Circulation Changes
- Increased Atmospheric Moisture Content
- Ocean-Atmosphere Interactions (El Niño/La Niña)
- Rapid Intensification Events
- Natural Climate Oscillations (AMO, PDO)
- Improved Monitoring and Detection

```


#### SECTION 2 PART 2 



#### ATLANTIC VS PACIFIC HURRICANES


## Atlantic vs Pacific Hurricane Frequency

| Atlantic Hurricane Frequency | Pacific Hurricane Frequency |
|-------------------------------|-----------------------------|
| ![Atlantic Hurricane Frequency](https://github.com/vnwal001/MyTestFolder/blob/main/HF2.png) | ![Pacific Hurricane Frequency](https://github.com/vnwal001/MyTestFolder/blob/main/HF3.png) |

## Atlantic vs Pacific Maximum Wind Speed

| Atlantic Max Wind Speed | Pacific Max Wind Speed |
|--------------------------|------------------------|
| ![Atlantic Hurricane Max Wind](https://github.com/vnwal001/MyTestFolder/blob/main/atws.png) | ![Pacific Max Wind Speed](https://github.com/vnwal001/MyTestFolder/blob/main/pcws.png) |






# Hurricane Data Boxplot Distribution 

## Comparison of Pacific vs. Atlantic Hurricanes


| Atlantic vs Pacific Frequency | Atlantic vs Pacific Max Wind Speed |
|------------------------------|------------------------------------|
| ![Atlantic vs Pacific Frequency](https://github.com/vnwal001/MyTestFolder/blob/main/combined_frequency.png) | ![Atlantic vs Pacific Max Wind Speed](https://github.com/vnwal001/MyTestFolder/blob/main/combined_windSpeed.png) |




#### *ATLANTIC VS PACIFIC HURRICANE MAX WIND SPEED AND FREQUENCY STORY*

```
Comparing the charts over the past 50 years from 2015, this is what I discovered concerning hurricane frequencies and wind speeds across the pacific vs the atlantic:
- Hurricane frequencies have generally increased as well as hurricane wind speed.
- The increase in box frequency and Wind Speed might not be dramatic, but there is an increase.

```
#### *HIDDEN STORY*

```
Examining the hurricane wind speed across the pacific shows a gradual increase. The first gradual increase was in 1970, the next was in 1990. I believe this trend continues in the 2000s
(just a hypothesis) . The increase in the atlantic is not as methodical as this.
In short, the more methodical trend of increasing hurricane wind speeds in the Pacific could be driven by consistent climate factors like sea surface temperature increases linked to El Niño/La Niña cycles and global warming, while the Atlantic Ocean experiences more complex, variable climate dynamics influenced by multiple oceanic and atmospheric factors. These include the AMO, vertical wind shear, and shifting oceanic currents. Additionally, discrepancies in data collection and the interplay of multiple factors in the Atlantic make it less straightforward to discern a similar long-term trend.

```

#### SECTION 2: PART 3 Impact of Category 5 Hurricanes on Population Migration
#### QUESTION: For hurricanes that make landfall in the U.S., is there a noticeable migration response in the aftermath of Category 5 hurricanes? Specifically, does the population of selected cities decrease significantly a year  after the hurricane, and can any patterns be identified?

#### ANSWERS 

```
- To answer this question, I focussed on the Hurricanes over the atlantic, these are typically the hurricanes that make landfall in the U.S.
- I also identified the hurricanes with highest Maximum speed that made landfall. I should note that hurricanes typically do not make landfall at their maximum speed.
- I plotted the path of these selected hurricanes, I focussed on the hurricane path as it got closer to the U.S mainland and into the U.S. mainland.  

```
##### TOP 5 HURRICANES PATHS AROUND AND INTO US MAINLAND
<img src="https://github.com/vnwal001/MyTestFolder/blob/main/hu_path.png" alt="Average Pacific Max Wind Speed" width="1350" height="819">

**Idiom: Map / Mark: CircleMarker, PolyLine**

| **Data: Attribute**            | **Data: Attribute Type** | **Encode: Channel**                              | **Mark**              |
|---------------------------------|--------------------------|-------------------------------------------------|-----------------------|
| Name                           | Nominal                  | Popup and Tooltip (to show the hurricane name)   | CircleMarker, PolyLine |
| Maximum Wind                   | Quantitative             | Radius (size of the circle marker based on wind speed) | CircleMarker        |
| Latitude, Longitude            | Spatial                  | Position (location of the hurricane on the map) | CircleMarker         |
| Name (for coloring)            | Nominal                  | Color (to differentiate between hurricanes)      | CircleMarker, PolyLine |
| Path (Coordinates)             | Spatial                  | Path (line connecting the hurricane locations)   | PolyLine             |


```
import numpy as np
import pandas as pd
import folium
import warnings
import re
from geopy.geocoders import Nominatim
from geopy.exc import GeocoderTimedOut

# Suppress warnings
warnings.simplefilter("ignore")

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

# Step 4: Filter data for Status = 'HU' (Hurricane) and Name not equal to 'UNNAMED'
hu_data = data[(data['Status'] == 'HU') & (data['Name'] != 'UNNAMED')]
unique_ids_hurr = set(hu_data['ID'])
hu_data = data[data['ID'].isin(unique_ids_hurr)]

# Step 5: Handle Missing or Invalid Maximum Wind Values
# Drop rows where 'Maximum Wind' is missing or invalid
hu_data = hu_data.dropna(subset=['Maximum Wind'])

# Step 6: Define the US Mainland Coastal Boundaries
lat_min = 24.396308  # Southern border of the US (latitude)
lat_max = 49.384358  # Northern border of the US (latitude)
lon_min = -125.0     # Western border of the US (longitude)
lon_max = -66.93457  # Eastern border of the US (longitude)

# Coastal zone buffer region (you can adjust the buffer distance)
coastal_buffer = 0.5  # Allow a buffer of 0.5 degrees around the coastline

# Step 7: Filter hurricanes based on the coastal region
hu_coastal_data = hu_data[
    (hu_data['Latitude'].apply(lambda x: float(x[:-1]) if x[-1] == 'N' else -float(x[:-1]))
     .between(lat_min + coastal_buffer, lat_max - coastal_buffer)) &
    (hu_data['Longitude'].apply(lambda x: float(x[:-1]) if x[-1] == 'E' else -float(x[:-1]))
     .between(lon_min + coastal_buffer, lon_max - coastal_buffer))
]

# Step 8: Find the top 5 hurricanes by maximum wind speed
top_5_hurricanes = hu_coastal_data.groupby('Name')['Maximum Wind'].max().nlargest(5).index

# Filter the data for the top 5 hurricanes
top_5_data = hu_coastal_data[hu_coastal_data['Name'].isin(top_5_hurricanes)]

# Step 9: Create a base map centered on the US region
m = folium.Map(location=[37.5, -95], zoom_start=5)  # Center map around the US

# Step 10: Define colors for each hurricane
colors = ['red', 'blue', 'green', 'purple', 'orange']
hurricane_colors = dict(zip(top_5_hurricanes, colors))

# Initialize geolocator for getting city names from coordinates
geolocator = Nominatim(user_agent="hurricane_map")

# Function to get the nearest location name based on latitude and longitude
def get_location_name(lat, lon):
    try:
        # Try to get the location name (city, town, or county) from coordinates
        location = geolocator.reverse((lat, lon), language="en", exactly_one=True)
        if location:
            address = location.raw.get('address', {})
            # Try to fetch different levels of location information (city, town, or county)
            city_name = address.get('city', None)
            town_name = address.get('town', None)
            village_name = address.get('village', None)
            county_name = address.get('county', None)

            # If city, town, or village are not available, return the county if available
            if city_name:
                return city_name
            elif town_name:
                return town_name
            elif village_name:
                return village_name
            elif county_name:
                return county_name
            else:
                return "Unknown"
        else:
            return "Unknown"
    except GeocoderTimedOut:
        return "Unknown"  # In case of geocoding service timeout
    except Exception as e:
        return "Unknown"  # Catch any other exceptions

# Step 11: Plot the full path of each of the top 5 hurricanes on the map
for hurricane in top_5_hurricanes:
    # Filter data for the current hurricane
    hurricane_data = top_5_data[top_5_data['Name'] == hurricane]

    # Create a list of coordinates for the path of the hurricane
    path = []
    for idx, row in hurricane_data.iterrows():
        lat = row['Latitude']
        lon = row['Longitude']

        # Convert latitude and longitude from string to float
        if lat[-1] == 'N':
            lat = float(lat[:-1])  # Positive for Northern Hemisphere
        elif lat[-1] == 'S':
            lat = -float(lat[:-1])  # Negative for Southern Hemisphere
        else:
            lat = float(lat)  # If no direction, just convert to float (assuming no S/N)

        if lon[-1] == 'E':
            lon = float(lon[:-1])  # Positive for Eastern Hemisphere
        elif lon[-1] == 'W':
            lon = -float(lon[:-1])  # Negative for Western Hemisphere
        else:
            lon = float(lon)  # If no direction, just convert to float (assuming no E/W)

        path.append([lat, lon])

        # Get the nearest location name from the coordinates
        location_name = get_location_name(lat, lon)

        # Add a marker for the current position of the hurricane with a tooltip
        folium.CircleMarker(
            location=[lat, lon],
            radius=5 + (row['Maximum Wind'] / 20),  # Adjust the radius based on wind speed
            color=hurricane_colors[hurricane],
            fill=True,
            fill_color=hurricane_colors[hurricane],
            fill_opacity=0.6,
            popup=f"Name: {hurricane}<br>Year: {row['Year']}<br>Wind: {row['Maximum Wind']} knots",
            tooltip=f"{hurricane} | Year: {row['Year']} | Wind: {row['Maximum Wind']} knots | Location: {location_name}"
        ).add_to(m)

    # Plot the full path as a line on the map
    folium.PolyLine(path, color=hurricane_colors[hurricane], weight=3, opacity=0.7).add_to(m)

# Step 12: Display the map
# Save the map as an HTML file
m.save('/content/full_path_top_5_hurricanes_with_nearest_location.html')

# If you're in Jupyter Notebook or Colab, use:
m

```
| **Hurricane**        | **Max Wind Speed (Life Cycle)** | **Max Wind Speed at Landfall** | **City Most Affected** | **Population (Year of Hurricane)** | **Population (One Year After)** | **Fatalities** | **Economic Damage (2024 USD)** |
|----------------------|---------------------------------|-------------------------------|------------------------|------------------------------------|----------------------------------|----------------|-------------------------------|
| **Hurricane Carla (1961)** | 175 mph (280 km/h)             | 145 mph (233 km/h)             | Galveston, Texas       | ~46,000 (1961)                    | ~46,000 (1962)                   | 43             | $20 billion                    |
| **Hurricane Camille (1969)** | 190 mph (305 km/h)             | 190 mph (305 km/h)             | Gulfport, Mississippi  | ~70,000 (1969)                    | ~75,000 (1970)                   | 256            | $21 billion                    |
| **Hurricane Andrew (1992)** | 165 mph (266 km/h)             | 165 mph (266 km/h)             | Homestead, Florida     | ~35,000 (1992)                    | ~36,000 (1993)                   | 65             | $75 billion                    |
| **Hurricane Katrina (2005)** | 175 mph (280 km/h)             | 125 mph (201 km/h)             | New Orleans, Louisiana | ~450,000 (2005)                   | ~230,000 (2006)                  | 1,836          | $186 billion                   |

##### Population Comparism Before And After the Hurricane
<img src="https://github.com/vnwal001/MyTestFolder/blob/main/citypop.png" alt="Comparism Before And After the Hurricane" width="1378" height="784">

**Idiom: Grouped Bar Plot / Mark: Bar**

| **Data: Attribute**               | **Data: Attribute Type** | **Encode: Channel**                              | **Mark**              |
|------------------------------------|--------------------------|-------------------------------------------------|-----------------------|
| Hurricane/City                    | Nominal                  | Horizontal position (x-axis)                    | Bar                   |
| Population Before Hurricane       | Quantitative             | Vertical position (y-axis)                      | Bar                   |
| Population 1 Yr After             | Quantitative             | Vertical position (y-axis)                      | Bar                   |
| Number of Fatalities              | Quantitative             | Vertical position (y-axis)                      | Bar                   |
| Speed at Landfall (mph)           | Quantitative             | Vertical position (y-axis)                      | Bar                   |
| Metric (for grouping)             | Nominal                  | Color (to differentiate between metrics)        | Bar                   |
| Value                             | Quantitative             | Value (height of the bars)                      | Bar                   |


```
# Import necessary libraries
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

# Load the dataset
url = "https://raw.githubusercontent.com/vnwal001/MyTestFolder/refs/heads/main/hurricaneData.csv"  # Replace this with your actual CSV file path or URL
data = pd.read_csv(url)

# Step 1: Clean the Data
# Remove leading/trailing spaces in all column names
data.columns = data.columns.str.strip()

# Remove commas from the numeric columns before converting to numeric
data['Population Before Hurricane'] = data['Population Before Hurricane'].replace({',': ''}, regex=True)
data['Population 1 Yr After'] = data['Population 1 Yr After'].replace({',': ''}, regex=True)
data['Number of Fatalities'] = data['Number of Fatalities'].replace({',': ''}, regex=True)
data['Speed at Landfall (mph)'] = data['Speed at Landfall (mph)'].replace({',': ''}, regex=True)

# Convert all columns to the correct data type (numeric)
data['Population Before Hurricane'] = pd.to_numeric(data['Population Before Hurricane'], errors='coerce')
data['Population 1 Yr After'] = pd.to_numeric(data['Population 1 Yr After'], errors='coerce')
data['Number of Fatalities'] = pd.to_numeric(data['Number of Fatalities'], errors='coerce')
data['Speed at Landfall (mph)'] = pd.to_numeric(data['Speed at Landfall (mph)'], errors='coerce')

# Handle missing values by replacing with the mean or median, or dropping rows
data = data.dropna(subset=['Population Before Hurricane', 'Population 1 Yr After', 'Number of Fatalities', 'Speed at Landfall (mph)'])

# Step 2: Reshape the data for grouped bar plot (melt the data)
data_melted = data.melt(id_vars=['Hurricane/City'], 
                        value_vars=['Population Before Hurricane', 'Population 1 Yr After', 'Number of Fatalities', 'Speed at Landfall (mph)'],
                        var_name='Metric', value_name='Value')

# Step 3: Create a grouped bar plot
plt.figure(figsize=(14, 8))
ax = sns.barplot(x='Hurricane/City', y='Value', hue='Metric', data=data_melted, palette='Set2')

# Step 4: Set logarithmic scale for y-axis
plt.yscale('log')

# Step 5: Add bar labels to the bars
for p in ax.patches:
    ax.annotate(f'{p.get_height():,.0f}',  # Display the height of each bar
                (p.get_x() + p.get_width() / 2., p.get_height()),  # Position label at the top of each bar
                ha='center', va='center',  # Align the label
                fontsize=10, color='black',  # Label styling
                xytext=(0, 5), textcoords='offset points')  # Position the label slightly above the bar

# Step 6: Customize the plot
plt.title('Comparison of Hurricane Impacts on Cities (Log Scale)', fontsize=16)
plt.xlabel('Hurricane/City', fontsize=12)
plt.ylabel('Log(Value)', fontsize=12)
plt.xticks(rotation=90)  # Rotate x-axis labels to make them readable
plt.legend(title='Metric', bbox_to_anchor=(1.05, 1), loc='upper left')  # Move the legend to the side
plt.tight_layout()  # Adjust layout to prevent clipping

# Show the plot
plt.show()

```

```
DEDUCTIONS
Hurricane Katrina had the most fatalties and economic devastation, hence a rapid drop in the popultion of the city affected the most, a lot of people migrated out of New Orleans. I should note that by this, having the highest wind speed does not translate to being
the most devastation hurricane.
While wind speed is an important factor in determining the strength of a hurricane, other elements like storm surge, flooding, storm size, infrastructure weaknesses, and response efforts play critical roles in determining the overall devastation. Hurricane Katrina's impact was driven largely by the storm surge and flooding, particularly in New Orleans, and the failure of critical infrastructure, making it one of the deadliest and most costly hurricanes in history despite its lower wind speed at landfall compared to other storms on the list.
Yes, hurricane devastion causing population migration but it has to be a very severe devastation. People will normally stay back and rebuld after a hurricane. 

```
### FINAL THOUGHT 
##### I will like to put the entire Atlantic and Pacific Hurricane in Context, with these visualizations. 
![Total Frequency](https://github.com/vnwal001/MyTestFolder/blob/main/total_frequency.png?raw=true)
**Idiom**: Boxplot / **Mark**: Box

| Data: Attribute               | Data: Attribute Type | Encode: Channel                            |
|--------------------------------|----------------------|--------------------------------------------|
| Region                         | Nominal              | Horizontal position (x-axis)               |
| Frequency                      | Quantitative         | Vertical position (y-axis)                 |
| Region (for coloring)          | Nominal              | Color (to differentiate between regions)   |


![Total Max Wind](https://github.com/vnwal001/MyTestFolder/blob/main/total_maxwind.png?raw=true)
**Idiom**: Boxplot / **Mark**: Box

| Data: Attribute               | Data: Attribute Type | Encode: Channel                            |
|--------------------------------|----------------------|--------------------------------------------|
| Period                         | Ordinal              | Horizontal position (x-axis)               |
| Maximum Wind                   | Quantitative         | Vertical position (y-axis)                 |
| Period (for coloring)          | Nominal              | Color (to differentiate between periods)   |


![Total Landfall](https://github.com/vnwal001/MyTestFolder/blob/main/total_landfall_2.png?raw=true)
**Idiom**: Bar Chart / **Mark**: Bar

| Data: Attribute                | Data: Attribute Type | Encode: Channel                                |
|---------------------------------|----------------------|------------------------------------------------|
| Basin                           | Nominal              | Horizontal position (x-axis)                   |
| Total Landfall Hurricanes       | Quantitative         | Vertical position (y-axis)                     |
| Total Landfall Hurricanes       | Quantitative         | Text label on bars (for display)               |
| Basin (for coloring)            | Nominal              | Color (to differentiate between Atlantic and Pacific) |


```
As can be shown by the visualization, the frequency of the hurricanes and max wind speed are higher overall on pacific ocean than the atlantic ocean. Nevetheless, the amount of hurricanes that make Landfall are much
more higher on the atlantic ocean. Hence hurricanes over the atlantic have more overall impact than those of the atlantic ocean.

The reason hurricanes over the Atlantic cause more overall impact than those over the Pacific is largely due to landfall and the population density of the areas that are affected. Even though Pacific hurricanes may be stronger and more frequent, they often do not hit densely populated areas as much as those in the Atlantic. Therefore, hurricanes in the Atlantic not only cause direct damage due to their winds but also trigger a series
of secondary impacts such as flooding, power outages, infrastructure damage, and long-term economic consequences in highly populated coastal areas.

Atlantic Ocean hurricanes tend to follow tracks that bring them near or directly into the path of populated areas in the Caribbean, Gulf of Mexico, and the Eastern United States. These areas are more likely to experience the direct effects of landfall.
In contrast, the Pacific Ocean hurricanes often have paths that either curve away from land or affect less populated regions, such as parts of the Pacific islands, or dissipate over the ocean.

```




**References:**
- https://chatgpt.com/
- https://www.census.gov/library/publications/2010/compendia/statab/130ed/population.html
