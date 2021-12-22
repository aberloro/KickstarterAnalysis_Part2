# Kickstarting with Excel

## Overview of Project

### Purpose

The purpose of this analysis is to provide our client, Louise, with an analysis of the relationship of campaign outcome vs funding goal and outcome vs launch date for Kickstarter campaigns in the Theater category around the globe.

**Background** 

Louise has already Kickstarted her American play, *Fever*, and nearly collected her goal of $10,000.  Previous analysis with this client has shown us that: 
- Theater is a strong category of Kickstarters, with the subcategory of Plays being both popular and generally successful.  
- A look at *all* parent categories showed May to be a good launch date, and December to be an unfavorable one. There is a downward trend of success rate as the year progresses.
- The mean and median pledge amount of successful campaigns closely matched the goal for successful campaigns, where it is divergent for failed campaigns.  
- Louise’s budget is about double the average successful campaign.  

See https://github.com/aberloro/Module01_CanvasAssignment_KickstarterAnalysis/blob/main/README.mdfor the full initial report to Louise.    


## Analysis and Challenges

This analysis was performed using Excel functions, Excel computations, and visualizations including Pivot Tables and Line Charts.  


### Analysis of Outcomes Based on Launch Date

**The first step to analyze Outcomes vs Launch Date is to get the needed Date information from our current data.  The data was given in a unix timestamp format, but we need a standard MM/DD/YYYY. There were 2 steps:**
- Convert unix timestamp of campaign’s launch time into a date we can easily read

        - MM/DD/YYYY = (((x/60)/60)/24) + DATE(1870,1,1)
        - where x= the reference cell in the Launched_At column.  
- Isolate just the year out of MM/DD/YYY format from above conversion

        - YYYY=YEAR(x) 
        - where x is the appropriate reference cell in the Date Created Conversion column

**The next step is use a Pivot Table to filter only the relevant data out of the larger data set:**
- Filters set to years and parent category to allow isolation of Theater projects in time
- Fields are set as shown

     <img width="524" alt="pivot set up outcome vs date" src="https://user-images.githubusercontent.com/93740725/147001663-15dbde84-0286-4b58-9107-a07e5edf302c.png">
 - Extra input from the Row Field is deleted so only “date created conversion” is listed in the PivotTable Fields box and so “months” show up under Row Labels in the Pivot Table

**The last step is to convert the Pivot Table into a Line Chart that easily shows outcome trends over time:**
- Y axis holds the counts of each outcome
- X axis holds the month of the launch date

    <img width="240" alt="Theater_Outcomes_vs_Launch" src="https://user-images.githubusercontent.com/93740725/147001801-78a36f87-4b94-4dce-82c5-06e3bf722b01.png">


### Analysis of Outcomes Based on Goals

To look at trends between outcomes and goal size, we need to filter the relevant data from our complete data set.  This requires identifying goal ranges and creating a new summary table to hold the filtered data by outcome and subcategory in each goal range. The rows in this new table are the goal ranges, and the columns are the number of each outcome, total outcomes, and percentage of each outcome:

<img width="323" alt="summary table 3" src="https://user-images.githubusercontent.com/93740725/147003030-98a8c8d8-7f6f-4d45-af58-33f6c15bbccb.png">


**Use COUNTIFS to count the number of occurrences of each outcome of plays successful, failed, cancelled) in each of a certain goal range:**

- Sample formula to identify successful plays with a goal range of $1000 to $4999

        - =COUNTIFS(Kickstarter!$F:$F,"=successful", Kickstarter!$R:$R, "=plays", Kickstarter!$D:$D, ">=1000", Kickstarter!$D:$D, "<=4999")
         
- Sample formula to identify failed plays with a goal range over $50,000

        - =COUNTIFS(Kickstarter!$F:$F,"=failed", Kickstarter!$R:$R, "=plays",  Kickstarter!$D:$D, ">=50000")
- Sample formula to identify cancelled plays with a goal of less than $1000

        - =COUNTIFS(Kickstarter!$F:$F,"=canceled", Kickstarter!$R:$R, "=plays", Kickstarter!$D:$D, "<1000")


**After filtering in the needed data, calculate the total and percentage of each outcome.**	
- Find the sum of the total projects in each goal range 

        - =SUM(Bx:Dx) 
        - where x is the appropriate row and we are adding values from columns B, C, and D

-  Calculate percentage of each outcome 

        - Sample percentage calculation of successful projects 
        - =Bx/Ex
        - Where x is the appropriate row or goal range and
        - Column E holds the sum of projects in that goal range and
        - Column B holds the number of successful projects in the specific goal range
        - Set cell format to percentage


**The last step is to create a line chart showing Outcomes of Plays vs Ranges of Goals:**

<img width="251" alt="Outcomes_vs_Goals" src="https://user-images.githubusercontent.com/93740725/147002556-c3780865-15d8-48d2-ad95-f06bec91118c.png">


### Challenges and Difficulties Encountered

The biggest challenge I had was getting my =COUNTIFS function to work.  I did *not* initially notice that there needed to be a format conversion of goal data. The goal data was formatted to general, and it needed to be in accounting for greater-than and less-than queries to work. 

My COUNTIFS calculations were all returning a ZERO result, which is a red flag. I though initially I had entered part of the function incorrectly, so I ran trouble shooting by first isolating just one component of the COUNTIFS formula at a time. 


- Was my formula counting successful outcomes?

    - =COUNTIF(Kickstarter!F:F,"=successful")
    - I got numbers so YES, this part of my formula was correct!


- Was my formula counting the Play subcategory?
    - =COUNTIF (Kickstarter!R:R, "=play") 
    - ALL ZEROS, oh no!  The subcategory is listed as PLAYS plural, not singular as I had initially typed. My typo prevented excel from seeking out the right results. There were zero “play”s but a number of “plays”
    - The correct formula reads =COUNTIF (Kickstarter!R:R, "=plays") 


- Was my formula counting the number of campaigns in each goal range?


    - =COUNTIF(Kickstarter!D:D, "<1000") 
    - ALL ZEROS AGAIN! So NO the formula was not counting any campaigns, and a visual check of the data shows that there *are* in-fact plays in this goal range.  
    - This was corrected by changing the *format* of column D from *general* to *accounting*. 

After I was able to get results in each goal range for successful plays and for failed plays, I had a problem getting results for cancelled plays: all zeros again.  I started trouble-shooting using the same method as above and found out that I spelled “cancelled” (British spelling) instead of “canceled” (American spelling.) I got all zeros again after correcting my spelling.  A visual check of the data after filtering for cancelled plays shows there were none. 

A second challenge, for me, was in the visual interpretation of the Outcomes vs Goals data.  The two plot lines of the Outcomes Vs Goals chart are exact mirror images to each other.  For me, the two lines are redundant and make the chart hard to read.  I personally find it easier to read if only either Successful Plays or Failed plays is shown, like below for example. 

<img width="251" alt="Successful Outcomes vs Goal" src="https://user-images.githubusercontent.com/93740725/147003321-77a9f081-4352-4d4e-af0a-f13d78213787.png">

## Results

### Conclusions for Theater Outcomes vs Launch Date

The Outcomes based on Launch Date Chart Shows that the plot line for the number of failed Theater Campaigns lies below and closely follows the shape of the plot line of Successful Theater Campaigns.  So *in general*, campaigns are more likely to succeed than fail regardless of the launch month.  There are 2 notable exceptions with actionable applications. 

 - **One**, there is a much higher distance between the successful vs failed campaign plot lines in May, with that gap closing as the year progresses.  Even though there were more over-all campaign launches in May, there was a higher percentage of successful vs failed.  This means May is a great month to launch.

 - **Two**, the gap between successful and failed Theater campaign launches is essentially closed by December.  This means as the year progresses, there are a higher percentage of failures compared to the optimal launch date of May.  Avoid launches in the last quarter, especially December. 


### Conclusions and Limitations for Play Outcomes vs Goals

The outcomes of Plays vs Goal ranges shows that Plays with a goal range of Less than $4,999 have the highest success rate at 73% to 76%.  The success rate decreases as the budget increases to $29,999. Success rates jumped around (up and down) as the goal amount increased again, but this is based on just a handful of campaigns. Based on this chart, I recommend keeping the goal range in the less than $4,999 range.  

### Limitations 

One major limitation with this data set is the length of the time period we have access to.  Kickstarter launched in 2009. This data set only has 14 inputs from that year, and none are from the Theater category.  That’s an incredibly small sample size for 2009 and makes the data less reliable. In contrast, there are 950 inputs from 2016 with nearly 400 from Theater.  This data is more reliable than the earlier data. 

Early data in this set may also be impacted by reluctance of investors and campaigners to use Kickstarter then vs now when folks are more confident in it.  It would be interesting to look at investor behavior / outcomes of non-Kickstarted campaigns during the 5 year time period immediately before Kickstarter, and comparing that to Kickstarter’s first five years, and most recent 5 years. A longer picture of Kickstarter outcomes would result in more robust data. 

A second limitation of this set is in reference to the Play Outcomes Vs Goals table: most (85%) of the sample lives in the first three goal ranges, or for campaigns below $9,999.  The data here can be considered more accurate than the data in the subsequent 9 goal ranges (campaigns between $10,000 and >$50,000), which all together only represent 15% of the sample size. We don’t have a lot of data with which to analyze projects with budgets over $10,000.  It would be helpful to identify any outliers by creating a Box and Whisker Plot to see if any goal data is impacting our trend line on this graph. 

A final limitation of this data is that it provides no context into the rest of the economy of investor spending.  Adding data from the GDP, and how much spending went into each parent category overall (not just through Kickstarter) would be helpful. For example, how would a recession, a boom, or pandemic impact investor behavior or impact the number of launches? 


### Additional Tables / Graphs 

Louise might benefit from insight into how the duration of a campaign impacts its outcome.  We could determine this by subtracting the launch date from the deadline, and looking at the outcomes (successful, failed, and cancelled) for projects within in a set range of durations (similar to how we did a set range of goals.) That data would be put in a summary table and line graph.  

Another useful insight would be to dive into the number of backers per campaign.  What is the average number of backers for successful vs failed vs cancelled campaigns?  Would charting the number of outcomes against different ranges of backers reveal a trend?
