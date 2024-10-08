# Course: CS 625
## Name: Victor Nwala
## Homework 2

**Due:** September 22, 2024 by 11:59 PM

### Report

### Part 1: Data Cleaning

#### 1. Remove Rows/Columns

i. Remove blank rows/rows containing misleading values/columns with no values (more than one column of the same row, for example). Remove the column "Gross".

**Answer:**
- A) I clicked on the Gross Column → Edit Column → Remove Column.
- B) I created a new column (MissingValues) to count the number of missing values in each row using the GREL expression:

    ```grel
    if(isNull(cells['MOVIES'].value), 1, 0) +
    if(isNull(cells['GENRE'].value), 1, 0) + 
    if(isNull(cells['RATING'].value), 1, 0) + 
    if(isNull(cells['ONE-LINE'].value), 1, 0) +
    if(isNull(cells['STARS'].value), 1, 0) + 
    if(isNull(cells['VOTES'].value), 1, 0) + 
    if(isNull(cells['RunTime'].value), 1, 0)
    ```

- C) I performed a numeric facet on the MissingValues column, sorted for all matching rows with MissingValue > 1, deleted those rows, and removed the MissingValues column.

ii. Remove rows that contain misleading info. Explain the criteria you defined for the selected row(s)/column(s).

**Answer:**
I merged all the clusters by the STAR column using the Key collision and fingerprint method. I concluded that if the cells in that column had exactly the same stars and director, they were highly likely to be the same movie. I found 38 clusters.

#### 2. Refilling Values in Columns

**Answer:**
I refilled the blank cells for the columns "Rating", "Votes", and "Run Time" with 0 and changed their data types to numeric. I also checked values of all other columns and updated them accordingly. I performed the following transformations using GREL expressions:

- `if(isNull(value), 0, value)`  (For Rating)
- `if(isNull(value), 0, value)`  (For Votes)
- `if(isNull(value), 0, value)`  (For Run Time)
- `value.toNumber()`  (Convert Votes to numbers)
- `value.toNumber()`  (Convert Run Time to numbers)
- `value.toNumber()`  (Convert Ratings to numbers)
- `value.toString()`  (Convert Votes back to string)
- `value.replace(",", "")`  (Remove all commas in Votes)
- `value.toNumber()` Convert all votes back to numbers.
- `if(isNull(value), "N/A", value)`  (Replace empty Movies with "N/A")
- `if(isNull(value), "N/A", value)`  (Replace empty Year with "N/A")
- `if(isNull(value), "N/A", value)`  (Replace empty One-Line with "N/A")
- if(isNull(value), "N/A", value)`  (Replace empty STARS with "N/A")

#### 3. Handling Ambiguous Values in the "Year" Column

i. Remove rows with cell values as Roman numerals/strings only.

ii. Replace values of the year enclosed in parentheses with a single year only.

**Answer:**
I performed the following GREL transformations on the Year column:

- `value.trim()`  (Remove all spaces)
- `if(length(value) < 9, value.replace("(", "").replace(")", ""), value)`  (Remove parentheses for single year values)
- `if(length(value) < 7, value.replace(/–/,""), value)`  (Remove the symbol "–" for single year values)
- `value.replace(/[a-zA-Z ]+/, '')`  (Remove all letters and spaces)
- `value.replace("()", "")`  (Remove empty parentheses)
- `if(length(value) < 9, value.replace("(", "").replace(")", ""), value)`  (Again remove perenthesis for columns with single year value, this was done to catch the new values after alphabets have been removed, their index now falls with range) 
- `if(length(value) < 7, value.replace(/–/,""),value)`  (Again remove the symbol (–) for single year values, this was done to catch the new values after alphabets have been removed, their index now falls with range) 
- I also performed a custom facet by blank (null or empty string), I found 4 blank rows and deleted matching rows on the YEAR Column 


iii. Create new columns "startYear" and "endYear".

**Answer:**
I transformed the Year column to split it into startYear and endYear:

- `if(length(value) < 7, value.replace(value,"(" + value + /–/ + value + ")"),value)`
  This to transform all the single year values to this format, (xxxx-xxxx)
- I split the Year column in 2 by index lenght of 5 each, the result was 2 columns, Year 1 with format (xxxx and Year 2 with format, –xxxx
- I Renamed column YEAR 1 to startYear
- I  Renamed column YEAR 2 to endYear
- `value.replace("(", "")` To replace the open parenthesis symbol in startYear, 8,171 rows were affected
- `grel:value.replace(/–/, "")`
  To replace the "–" symbol in endYear, 8,166 rows were affected.
- *I noticed 8,177 - 8,166 = 5, so I have 5 problematic rows* 
- I removed the Initial Year Column

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
