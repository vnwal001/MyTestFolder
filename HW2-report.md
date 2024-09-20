# Homework 2: Data Cleaning
## Name : Victor Nwala
## Course : CS 625

**Due:** September 22, 2024 by 11:59pm  




### Report



### Part 1. Data Cleaning




1.	Remove rows/columns:

    i.	Remove blank rows/row contain misleading values/columns that has no values (more than one column of the same row for example). Remove the column "Gross".

  	Answer: 

  	A) I clicked on the Gross Column--------> Edit Column--------->Remove Column

  	B) I created a new column (MissingValues) to count number of missing values in every rows, I used the GREL expression


 `if(isNull(cells['MOVIES'].value), 1, 0)+
 if(isNull(cells['GENRE'].value), 1, 0)+ 
 if(isNull(cells['RATING'].value), 1, 0) + 
 if(isNull(cells['ONE-LINE'].value), 1, 0) +
 if(isNull(cells['STARS'].value), 1, 0)+ 
 if(isNull(cells['VOTES'].value), 1, 0)+ 
 if(isNull(cells['RunTime'].value), 1, 0) `
  
   C) I performed a numeric facet on the MissingValues Column, and I sorted for all matching rows with MissingValue > 1, I deleted the matching rows and removed the MissingValues column
  	       
   ii Remove rows that contain misleading info. You must explain in your report the criteria you defined to remove those selected row(s)/column(s).

Answer:

I merged all the clusters by the STAR column, using the Key collison, fingerprint method. I did this believe if the cells in that column have exactly the same stars and director, it is highly like to be the same movie. I found 38 clusters. 



2.	Refilling the values in the column(s):

   Answer:
   

Refill the blank cells for the columns "Rating", "Votes", and "Run Time" to 0 and change their data type to numeric. Similarly check values of all other columns and update the values accordingly (free to decide). 

I performed the following transformation GREL expressions on the respective columns: 

 
- `if(isNull(value), 0, value)`  
  If rating is null, change to zero.

- `if(isNull(value), 0, value)`  
  If rating votes is null, change to zero.

- `if(isNull(value), 0, value)`  
  If rating runtime is null, change to zero.

- `value.toNumber()`  
  Convert votes to numbers.

- `value.toNumber()`  
  Convert runtime to numbers.

- `value.toNumber()`  
  Convert ratings to numbers.

- `value.toString()`  
  Convert votes back to string.

- `value.replace(",", "")`  
  Remove all commas in votes.

- `value.toNumber()`  
  Convert all votes back to numbers.

- `if(isNull(value), "N/A", value)`
  if movies column is empty replace with N/A

- `if(isNull(value), "N/A", value)`
  if year column is empty replace with N/A
   
- `if(isNull(value), "N/A", value)`
  if year one-line is empty replace with N/A
  
- `if(isNull(value), "N/A", value)`
  if year one-line is empty replace with N/A






3.	The column "Year" has numerous ambiguous values. Follow the steps given below to proceed further.

    i.	Remove the rows if the cell value is Roman numerals/string only.


    ii.	Replace the value of the year that enclosed by (xxxx) single year only. 
    
        Example: (2024) --> 2024, (2021-) --> 2021, 1965 TV Special --> 1965, (ii) (2012-) --> 2012 [Apply GREL/Python commands to arrive at the solution wherever is possible]


    Answer
    I performed the following transformation GREL expressions on the Year columns:
  	
    - `value.trim()`  
       Remove all spaces in the year column
      
    - `if(length(value) < 9, value.replace("(", "").replace(")", ""), value)`  
       Remove perenthesis for columns with single year value
      
    - `if(length(value) < 7, value.replace(/–/,""),value)`  
       Remove the symbol (–) for single year values
      
    - `value.replace(/[a-zA-Z ]+/, '')`  
      Removes all letters (both uppercase and lowercase) and spaces from the value
      
    - `value.replace("()", "")`  
      Removes all empty parenthesis
      
    - `if(length(value) < 9, value.replace("(", "").replace(")", ""), value)`  
       Again remove perenthesis for columns with single year value, this was done to catch the new values after alphabets have been removed, their index now falls with range 
      
    - `if(length(value) < 7, value.replace(/–/,""),value)`  
       Again remove the symbol (–) for single year values, this was done to catch the new values after alphabets have been removed, their index now falls with range 
      
  	 - I also performed a custom facet by blank (null or empty string), I found 4 blank rows and deleted matching rows on the YEAR Column 





    iii. After successful execution of (i) to (iii) the "Year" column may have values in the format (xxxx-xxxx) or (<roman letter> xxxx-xxxx).  
    
    Create new columns namely "startYear" and "endYear". Then fill the their cell by extracting value(s) from "Year" column. Refer the example given below.
    
    Year --> 1966:  startYear --> 1966; endYear --> 1966
    Yea --> (1966-1969): startYear --> 1966; endYear --> 1969
    Year --> (I/II/?) (1966): startYear  1966; endYear --> 1966

    Remove the column "Year" after successful execution of steps 3.(i) - 3(iii).


   Answer: 

   I perform the follow transformations on the year column to split it into startYear and endYear
   
   -`if(length(value) < 7, value.replace(value,"(" + value + /–/ + value + ")"),value)`
     This to transform all the single year values to this format, (xxxx-xxxx)
     
   - Split 8171 cell(s) in column YEAR into several columns by field lengths
     
   - I split the Year column in 2 by index lenght of 5 each, the result was 2 columns, Year 1 with format (xxxx and Year 2 with format, –xxxx
     
   - I Renamed column YEAR 1 to startYear
    
   - I  Renamed column YEAR 2 to endYear
    
   -`grel:value.replace("(", "")`
     To replace the open parenthesis symbol in startYear, 8, 171 rows were affected 

    -`grel:value.replace(/–/, "")`
     To replace the "–" symbol in endYear
     
    - I removed the Initial Year Column


5.	Create a new column called "Verdict" and fill its values based on the criteria given below:

|   Rating       |  Verdict     |
|----------------|--------------|
| 0              |  Not known   |
|>0 and <=4.5    |    Flop      |
|>4.5 and <=6.5  |   Average    |
|>6.5 and <=8.0  |     Hit      |
| Above>8.0      |   Super Hit  |
|----------------|--------------|

*Caution:* This is medium sized, messy dataset.  Clean the data as well as you can, with an eye towards being able to answer the questions in Part 2. However, you are not expected to clean the entire dataset fully.

In your report, explain the steps you took to clean the data. Include screenshots, GREL statements, etc. as needed to clearly document what you did. If you did any manual cleaning, note that and explain why you did this manually. Include enough detail so that I am convinced that you understand how to use OpenRefine.

You will likely not have learned everything in class that you need to know to complete the assignment. I expect that you will watch the tutorials and read documentation, including documentation on the [GREL regex language](https://openrefine.org/docs/manual/grel).

When you are done cleaning the file:

* Export the file as a new CSV and save it in your repo as `HW2-Movies.csv`.
Extract JSON scripts containing all of the operations you performed on the file and save it in your repo as HW2-Movies.json. (Select Extract at the top of the Undo/Redo tab. Then copy and paste the JSON script into a new file.)

* Include links to these files in your report.

### Part 2. Analyze Cleaned Data

Use the cleaned data to answer the following questions in your report (and explain how you arrived at the answers):

1.	How many movies were listed as “Super Hit” in the year 2021?
2.	Which movie got lowest rating in the years 2018 to 2020 (excluding 0)?
3.	List the top 3 genres (no duplicates) that got highest number of votes (excluding 0)
4.	Name the director who directed the 10 minutes run time movie in the year 2020 that received lowest number of votes. The output must list name of the movie, number of votes, and genre. 
5.	List the top 5 movies that received highest number of votes, but verdict is “Flop” 


*I do not expect everyone to have the exact same answers. Some of these will depend upon decisions you make while cleaning the data. Note in your report any cleaning decisions you made that could impact your answers.*

## Submission

You should be working in the private GitHub repo that I created for you in the [odu-cs625-datavis organization](https://github.com/odu-cs625-datavis/).  If you are working locally, make sure that you have committed and pushed your local repo, including any images you reference, to GitHub. 

For this assignment, your GitHub repository should include the following files:

* `HW2-report.md` - your report
* `HW2-Movies.csv` - cleaned CSV
* `HW2-Movies.json` - operations used to clean the data in JSON format
*  any images that you reference in your report

Submit the URL of your report (*not the URL of your repo*) in Canvas under HW2. This should be something like  
https<nolink>://github.com/odu-cs625-datavis/Fall24-asv-*username*/blob/main/HW2-report.md

*If you make changes to your report after submitting in Canvas, I will use the last commit time in your repo as your assignment submission time.*
Reference:
Dataset: 
https://medium.com/@FranciscoHinojosaLuna/5-datasets-to-practice-data-cleaning-27378f422e1c 

Movies : https://www.kaggle.com/datasets/bharatnatrayn/movies-dataset-for-feature-extracion-prediction?resource=download 
