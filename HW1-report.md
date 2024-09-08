# HW 1 - CS 625, Fall 2024

VICTOR NWALA 

Due: September 08, 2024

## Git, GitHub

*What is the URL of the GitHub repo that you created in your personal account?*

The URL of my Report is [Link](https://github.com/odu-cs625-datavis/Fall24-aveerasa-vnwal001)
   
*In which direction does the 'push' command work (send local changes to remote OR send remote changes to local or something else)?*

The push command sends local copy to remote
   
*You have committed a change on your local machine/remote. However, you want to undo the changes committed. How would you do that?*

Find the commit hash of the commit you want to undo using git log. 

Run git revert <commit_hash>. This creates a new commit that undoes the changes. 

Push the changes to the remote repository using git push origin <branch_name>.

## Markdown

*Create a bulleted list with at least 3 items*

- Orange
- Mango
- Grape



*Write a single paragraph that demonstrates the use of italics, bold, bold italics, code, and includes a link. The paragraph must explain your favorite Olympic sport/game, the country that won the most number of olympic medals (Summer) in your favorite sport in 2020 (Japan) and 2024 (France). You are free to include more information.*

My favorite olympic sport is *soccer*, it is also known as *__football__* outside of the U.S. In the men's catergory of this 
sport you can only win one medal. Brazil won the gold medal in 2020 (Japan), while Spain won the gold in 2024 (France). `For
more details you can refer to the official olympic` [website](https://olympics.com/en/news/olympic-football-winners-list-men-women-gold-medals-champions) 

*Create a level 3 heading*
### This In My Level 3 Heading
*Insert a image of your favorite Olympics sport/game, sized appropriately*

![\label{fig:Spanish Team}](https://github.com/vnwal001/CS625-2ND/blob/main/olympic%20image.jpg)

## Tableau  
*Insert the image of your horizontal bar chart here. Reminder, this should show countries that won most number of medals in Paris2024 Summer Olynpics by continent.*

![\label{fig:Olympic Medal Count}](https://github.com/vnwal001/CS625-2ND/blob/main/Paris%202024%20Olympics.jpg)

## Google Colab

*What is the URL of your Google Colab notebook?*

This is the link to my Google Colab [Notebook](https://colab.research.google.com/drive/1_mXwqMkIBWL-y3ICkcqrNPPDWoSydE7I#scrollTo=gh2gkteR8ish)

## Python/Seaborn

*Insert the first penguin chart here*
![\label{fig:Penguin 1}](https://github.com/vnwal001/CS625-2ND/blob/main/panda1.png)


*Describe what the figure is showing.*

The figure above shows a scatter plot

*Insert the second penguin chart here*
![\label{fig:Penguin 2}](https://github.com/vnwal001/CS625-2ND/blob/main/panda2.png)

*Describe what the figure is showing.*
The image above shows a bar chart of the body mass of penguin species 

*What happened when you removed the outer parentheses from the code? Why?*

When I remove the parenthesis from the code, it breaks with an error. It breaks because without the parenthesis you cannot write the code in multiple
line. 

## Observable and Vega-Lite

*What happens when you replace `markCircle()` with `markSquare()`?*

It replaces the circles on the scatter plot with squares  

*What happens when you replace `markCircle()` with `markPoint()`?*

It replaces the circles on the scatter plots with points

*What change do you need to make to swap the x and y axes on the scatterplot?*

To swap the x and y axes on a scatterplot in Vega-Lite, you can simply swap the x and y fields in the encoding block of your Vega-Lite specification.

*Insert the bar chart image here*
![\label{fig:Bar Chart}](https://github.com/vnwal001/CS625-2ND/blob/main/barChart.png)

*Why do you think this chart is the result of this code change?*

 In this chart y came before x in the encoding order. The encoding parameter order has the independent variable “Origin” before the dependent variable count.

## References

*Every report must list the references (including the URL) that you consulted while completing the assignment. Replace the items below with the references you consulted*

* Reference 1, <https://observablehq.com/@observablehq/vega-lite/>
* Reference 2, <https://markdownguide.offshoot.io/basic-syntax/>
* Reference 3, <https://chatgpt.com/>
* Reference 4, <https://www.meta.ai/>
