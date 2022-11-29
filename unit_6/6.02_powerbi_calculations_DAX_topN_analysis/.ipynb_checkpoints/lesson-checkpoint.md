# Lesson 6.2: Calculations - Power Query - Top N analysis - DAX

### Lesson Duration: 3 hours

> Purpose: The purpose of this lesson is to introduce some advanced techniques in PowerBI including creating new columns from calculations and DAX, conducting _Top N_ analysis.
---

### Learning Objectives

After this lesson, students will be able to:

- Merge queries into combined tables
- Apply calculations in PowerBI 
- Know what DAX is and how to develop the syntax
- Use basic DAX expressions 
- Conduct _Top N_ analysis

---

### Lesson 1 key concepts

> :clock10: 20 min

- Review the Sales information for the Bookshop
- Attempt to build a report of net sales
- Merge tables into one 
- Create a net sales summary analysis from the merged table 

For this lesson we will use the file `lesson_resources/Bookshop.xls` as in the last lesson. Our intention is to review which books have earned the most income, taking into account of discounts given. 
There is a lesson lesson_starter pbix workbook in this folder too which contains the starting data from the previous lesson and activities - ie tables from the Bookshop excel plus a combined data table of Q1-Q4 sales tabs
Ask students to use the PowerBI workbook they created in the last activities and save it under a new name.  Or provide them the starter PowerBI workbook

<details>
  <summary> Reviewing available sales data and identify a method to combine the data</summary>

- Open the starter workbook 
- Talk through or rebuild the reports 1) Book checkouts (Book, Checkouts) 2) Books_editions (Authors, Book, Edition, Info, Ratings) and 3) Sales_price_discount (Book, Edition, Sales Q1-Q4) 
- review the data model and draw out (whiteboard) which tables are connected directly to which in the model, and which tables are more granular than others 
- talk through what the calculation of net sales price should look like (price * (1-discount- per order/item))
- show how to create a new calculation within any table and the drop down pick list which helps find table-field references to include in calculations
- note that the syntax will be familiar to excel and SQL users - and using the new calculation is just a matter of checking it or dragging the field into the report editor
- ask the students to try building a simple table report which calculates the sales prices of each order id or net sales by book title- there is an example of each in the lesson workbook
- discuss why the logic of the calculation is not straightforward (because the data is in separate tables) and recall the SQL syntax when querying multiple tables in a DB

:exclamation: Note for instructor: there are many ways that this information could be brought together but it is not as simple as a calculation along the row of the report, which would be the case if the data were in the same table in an excel workbook.

- there are ways to bring tables together in PowerBI - appending and merging 
	- When you have one or more columns that you’d like to add to another query, you **merge** the queries.
	- When you have additional rows of data that you’d like to add to an existing query, you **append** the query.
- since each table is the result of a query already, we will refer to an existing table as a query 
- to merge the query Sales_Q1-Q4 which contains the order and discount, with the query Edition which contains Price (giving us a row for each order and isbn combination) select any table and choose **Edit query** to go into the Power Query editor 
- you may need to browse to the raw file Bookshop.xlsx at this point
- in Power Query Editor ignore the transform file and helper queries at the top, 
- select Sales_Q1-Q4 to highlight the sales table query and from the Home tab, use **Merge Queries as New** to select both the Q1-Q4 and Edition tables, with matching ISBN as the join
- at this point it will be necessary to pick a join method - review what the students learnt about join types in SQL - suggest : INNER
- point out that in the case a merge results in a duplicate primary key (in this case *orderid*) then aggregating in the merge operation might be required. 
- in this case there appears not be any duplicates issue - select the column Price that is desired in this new query before naming the new query **Sales_and_Prices**, closing and applying the transformation

</details>

---

#### :pencil2: Check for Understanding - Class activity/quick quiz

> :clock10: 10 min (+ 10 min Review)

<details>
  <summary> Click for Instructions: Activity 1 </summary>
 
- link to activity [activity 1](https://github.com/ironhack-edu/data_6.02_activities_powerbi/blob/master/6.02_activity_1.md)

</details>

<details>
  <summary>Click for Solution: Activity 1 solutions</summary>

- image [activity 1_attempted report](https://education-team-2020.s3.eu-west-1.amazonaws.com/data-analytics/6.2-images/6.2-6_2_1_attempt.png)
- image [activity 1_merged queries](https://education-team-2020.s3.eu-west-1.amazonaws.com/data-analytics/6.2-images/6.2-6_2_1_merge.png)

</details>

---

:coffee: **BREAK**

---

### Lesson 2 key concepts

> :clock10: 20 min

this lesson continues where the last one left off - at this point you will demonstrate how to use the now merged PowerBI query to create a calculation and report to answer the question - what was the net sales(taking into account discount) per book?
continue using the workbook from the last lesson 

In the activity 1 students were asked to research or think of other ways to solve this problem - so this is a good time to discuss those methods. Doing a DAX lookup formula for example - similar to a Vlookup in excel (review Vlookup syntax if students are not familiar with this) 
you could whiteboard out this solution : [Lookup function DAX](https://docs.microsoft.com/en-us/dax/lookupvalue-function-dax)

<details>
  <summary> create calculation in Power Query and build report </summary>

- selecting the **Sales_and_Prices** query, go into Power Query editor and work on the query - ensuring the Price field from Edition has been included in the table 
- explain that the new column could also be created in the report editor but it is easier to see the impact here and pick up any errors 
- create a new column **sale_price** in the custom column editor as [Edition.Price]*(1-[Discount]) - this editor is easy to use because you can pick the fields from the list on the right  
- review the resulting column - notice there are many nulls (this is because 1-null is null) 
- in order to use the Discount field in our calculation we will replace all null values with 0 using the **replace values** option available when you select the column Discount 
- this wont yet have updated the custom column - you will need to reorder the transformation steps in the transformation pane on the right 
- check the data type is decimal or set it as decimal (it would have been a text field when it contained nulls)
- ensure **sale_price** looks correct before closing and applying the transformations 
- in the report view, create a stacked bar chart with BookID on the Y axis, sales price (summed) on the X axis
- add format to the legend or filter to enhance the report - answering more questions than were initially posed! 
- edit the title using the formatting options to "net sales by BOOK and format"

</details>

---

#### :pencil2: Check for Understanding - Class activity/quick quiz

> :clock10: 10 min (+ 10 min Review)

<details>
  <summary> Click for Instructions: Activity 2 </summary>

- Link to [activity 2](https://github.com/ironhack-edu/data_6.02_activities_powerbi/blob/master/6.02_activity_2.md).

</details>

<details>
  <summary>Click for Solution: Activity 2 solutions</summary>

- image [activity 2_calculation](https://education-team-2020.s3.eu-west-1.amazonaws.com/data-analytics/6.2-images/6.2-6_2_2_calc.png)
- image [activity 2_successful report](https://education-team-2020.s3.eu-west-1.amazonaws.com/data-analytics/6.2-images/6.2-6_2_2_success.png)
- image [activity 2_scatter plot](https://education-team-2020.s3.eu-west-1.amazonaws.com/data-analytics/6.2-images/6.2-6_2_2_scatter.png)

</details>
---

:coffee: **BREAK**

---

### Lesson 3 key concepts

> :clock10: 20 min

- What is DAX (introduction)
- How to use DAX to solve the same problem of the past lesson
- one other DAX example
- where to go to find more DAX methods 

<details>
  <summary> Click for Description: DAX intro </summary>

- The DAX language was created specifically for the handling of data models, through the use of formulas and expressions. 
- DAX is used in several Microsoft Products such as Microsoft Power BI, Microsoft Analysis Services and Microsoft Power Pivot for Excel. 
- These products all share the same internal engine, called Tabular.
- DAX = data analysis expressions 
- DAX resembles excel but has some important differences : the formula will always start with the name of the column or measure = eg new_col=... 
- another difference is that excel uses cell references but DAX using the context of a row 
- there are many operations that can be done with DAX - we will look at just a few in this lesson 
- for more on DAX - theres a [DAX guide](https://dax.guide/) and [reference material from microsoft](https://docs.microsoft.com/en-us/dax/)
- to solve the problem seen in the earlier part of the lesson we will use a DAX LOOKUP formula 
- from the table **Sales_Q1-Q4** create a new column - this can be done either in report view or power query editor 
- define the formula as **dax_price = LOOKUPVALUE('Edition'[Price],'Edition'[ISBN],[ISBN])**
- recreate the same report stacked bar from the previous lesson using the newly defined DAX field 
- to show another DAX example - DAX can be used to create a filtered measure 
- this will calculate the average discount accurately for all paperback books sold using the DAX [averagex function](https://docs.microsoft.com/en-us/dax/averagex-function-dax)
- **disc_paperback = averagex(FILTER('Sales_Q1-Q4',Edition[Format]="paperback"),'Sales_Q1-Q4'[Discount])**
- optional - create more DAX examples or DAX lookup columns to capture values across data tables 

</details>

---

#### :pencil2: Check for Understanding - Class activity/quick quiz

> :clock10: 10 min (+ 10 min Review)

<details>
  <summary> Click for Instructions: Activity 3 </summary>

- Link to [activity 3](https://github.com/ironhack-edu/data_6.02_activities_powerbi/blob/master/6.02_activity_3.md).

</details>

<details>
  <summary>Click for Solution: Activity 3 solutions</summary>

- there is no solution for this activity as the students are asked to explore independently and find any way to do the previous activities which may be the LOOKUP method very similar to what was shown in class.

</details>

---

:coffee: **BREAK**

---

### Lesson 4 key concepts

> :clock10: 20 min

- `Top N` analysis
- In this analysis, we want to find the `N` number of books that were checked out the most, alongside some other information about the book - genre. 
- We will work through a method by filtering to reveal the top N and via DAX, resulting in two reporting outputs. 
- you can continue using the lesson workbook for this final part of the lesson but before starting, spend some time asking the students for their DAX discoveries from the last activity. 

<details>
  <summary> Filter Top N and DAX Top N </summary>

- we will start by creating a report with [top n filtering](https://powerbidocs.com/2020/01/21/power-bi-top-n-filters/)
- create a one page report which has two charts. the first is a duplicate of the bookid, format and dax price (sum) stacked bar from the end of the previous lesson
- the second chart on this page - create a treemap which shows net sales by genre (genre on category, price on values) 
- suggestion : to make this part clearer, change the colors of the treemap which currently are too similar to the colors of the bar chart by default. This is possible using the formatting menu > colors > advanced controls  (see lesson workbook for example of contrasting color scheme)
- this report could also be created from the earlier lesson merged table called **Sales_and_Prices**
- select the first chart and look at the **filters on this visual** section 
- change the BookID filter which is currently set to ALL to Top N - configure the filter as Top 5 by value DAX sales price 
- point out that there is a limitation of this top N filtering approach which is easily displayed when you select on the tree map - only the two biggest genres are picking up the 5 top books.
- PowerBI only allows measure based filtering at the visual level - lets look at the DAX way of doing the same thing
- more about [filter levels](https://support.ti.davidson.edu/hc/en-us/articles/360018121174-Understanding-Filters-in-Power-BI)
- with DAX we can create a new table with the [TOPN function](https://databear.com/power-bi-dax-topn-function/)
- first we will create a summary table to aggregate the net sales per bookid, book title and genre , next we create a top n from that summary 
- from the Modeling menu - select New Table
- define table as : **summary_book_genre_net_sales = SUMMARIZE('Sales_Q1-Q4',Book[BookID],Book[Title], Info[Genre],"net_sales",sum('Sales_Q1-Q4'[dax_price]))**
- suggestion : it is a good idea to visually display this summary table as a new report (table visual) so the students can see it 
- final stage - create a top N table [create top N as a new table using DAX](https://docs.microsoft.com/en-us/dax/topn-function-dax)
- define table as **Top5booksbysales = TOPN(5,'summary_book_genre_net_sales','summary_book_genre_net_sales'[net_sales],DESC)**
- display the results on a new or accompanying report
- there are other methods such as [RANK](https://www.sqlbi.com/articles/filtering-the-top-3-products-for-each-category-in-power-bi/) which could be used as alternatives for a top N type analysis - this is just a taster of one method

</details>

---

### :pencil2: Practice on key concepts - Lab

> :clock10: 30 min

<details>
  <summary> Click for Instructions: Lab </summary>

- Link to the lab: [Lab | Calculations and Dax](https://github.com/ironhack-labs/lab-calculations-dax)

</details>

<details>
  <summary>Click for Solution: Lab solutions</summary>

There is no lab solution provided - the business scenario of the lab involves creative thinking and there many ways to arrive at a solution.

</details>

---

:sandwich: **LUNCH BREAK**

---
