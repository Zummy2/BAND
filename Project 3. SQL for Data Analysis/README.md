# Chinook Database

## Introduction

In this project, you will query the Chinook Database. The Chinook Database holds information about a music store. For this project, you will be assisting the Chinook team with understanding the media in their store, their customers and employees, and their invoice information.

## Instructions

You will need to follow the instructions on the next three concepts to get the Chinook database up and running on your own machine, and check that it is set up correctly. There will be two parts to this project.

1. The first part is a series of questions that will assure you have mastered the main concepts taught throughout the SQL lessons. Though these questions will not be "graded" by a reviewer, they will help you self assess.

2. The second part is a presentation. Similar to the first project, there isn't a 'right answer' for the second portion of the project. You have the ability to be creative in the questions you ask. You will then write a SQL query to pull the data needed to successfully answer your question. Use the pulled data to build a visual (bar chart, histogram, or another plot) that answers your question. The essentials of your project submission are discussed in the next sections. In order to review your presentation, you will need to save your slides as a PDF.

## Set Up the Database

Here are the steps:

1. Open up DB Browser to SQLite
2. Click on Open Database
3. Navigate to the Chinook.db file (probably in your downloads)
4. Click on the Execute SQL
5. Start querying your data

**Start Querying Your Data**

The database Entity Relationship Diagram was provided in the previous concept, but you can also find it on the [Chinook database homepage](https://chinookdatabase.codeplex.com/).

#### Quiz

1. Who is the best customer?
The customer who has spent the most money will be declared the best customer. Build a query that returns the person who has spent the most money. I found the solution by linking the following three: Invoice, InvoiceLine, and Customer tables to retrieve this information, but you can probably do it with fewer!

My solution

```
  SELECT C.CUSTOMERID,
  	SUM(I.TOTAL)
  FROM CUSTOMER C
  JOIN INVOICE I ON C.CUSTOMERID = I.CUSTOMERID
  GROUP BY 1
  ORDER BY 2 DESC
```

2. Use your query to return the email, first name, last name, and Genre of all Rock Music listeners. Return your list ordered alphabetically by email address starting with A.

I chose to link information from the Customer, Invoice, InvoiceLine, Track, and Genre tables, but you may be able to find another way to get at the information.

My solution


```
  SELECT DISTINCT C.EMAIL,
  	C.FIRSTNAME,
  	C.LASTNAME,
  	G.NAME
  FROM CUSTOMER C
  JOIN INVOICE I ON C.CUSTOMERID = I.CUSTOMERID
  JOIN INVOICELINE IL ON IL.INVOICELINEID = I.INVOICEID
  JOIN TRACK T ON IL.TRACKID = T.TRACKID
  JOIN GENRE G ON T.GENREID = G.GENREID
  WHERE G.NAME = 'Rock'
  ORDER BY 1;
```

3. Who is writing the rock music?
Now that we know that our customers love rock music, we can decide which musicians to invite to play at the concert.

Let's invite the artists who have written the most rock music in our dataset. Write a query that returns the Artist name and total track count of the top 10 rock bands.

You will need to use the Genre, Track , Album, and Artist tables.

My solution

```
  SELECT AR.NAME,
  	COUNT(T.NAME)
  FROM TRACK T
  JOIN GENRE G ON T.GENREID = G.GENREID
  JOIN ALBUM AL ON AL.ALBUMID = T.ALBUMID
  JOIN ARTIST AR ON AR.ARTISTID = AL.ARTISTID
  WHERE G.NAME = 'Rock'
  GROUP BY 1
  ORDER BY 2 DESC

  ### Practice:

1. how many songs based on genre does customer 12 buy?
```
  SELECT DISTINCT G.NAME GENRE,
  	COUNT(T.NAME)
  FROM CUSTOMER C
  JOIN INVOICE I ON I.CUSTOMERID = C.CUSTOMERID
  JOIN INVOICELINE IL ON IL.INVOICEID = I.INVOICEID
  JOIN TRACK T ON T.TRACKID = IL.TRACKID
  JOIN GENRE G ON G.GENREID = T.GENREID
  WHERE C.CUSTOMERID = 12
  GROUP BY 1
  ORDER BY 1
```

2. How much did customer 13 spend across genres?

```
  SELECT C.LASTNAME,
  C.FIRSTNAME,
  G.NAME GENRE,
  SUM(IL.UNITPRICE)
  FROM CUSTOMER C
  JOIN INVOICE I ON I.CUSTOMERID = C.CUSTOMERID
  JOIN INVOICELINE IL ON IL.INVOICEID = I.INVOICEID
  JOIN TRACK T ON T.TRACKID = IL.TRACKID
  JOIN GENRE G ON G.GENREID = T.GENREID
  WHERE C.CUSTOMERID = 13
  GROUP BY 1,2,3
  ORDER BY 1
```

3. How much did each customers spend per genre?

```
  SELECT C.CUSTOMERID,
  C.LASTNAME,
  C.FIRSTNAME,
  G.NAME GENRE,
  SUM(IL.UNITPRICE)
  FROM CUSTOMER C
  JOIN INVOICE I ON I.CUSTOMERID = C.CUSTOMERID
  JOIN INVOICELINE IL ON IL.INVOICEID = I.INVOICEID
  JOIN TRACK T ON T.TRACKID = IL.TRACKID
  JOIN GENRE G ON G.GENREID = T.GENREID
  GROUP BY 1,2,3,4
  ORDER BY 1
```

4. How much was spent overall on each genre?

```
  SELECT G.NAME GENRE,
  	SUM(IL.UNITPRICE)
  FROM CUSTOMER C
  JOIN INVOICE I ON I.CUSTOMERID = C.CUSTOMERID
  JOIN INVOICELINE IL ON IL.INVOICEID = I.INVOICEID
  JOIN TRACK T ON T.TRACKID = IL.TRACKID
  JOIN GENRE G ON G.GENREID = T.GENREID
  GROUP BY 1
  ORDER BY 2 DESC
```

5. How much did Americans spend?

```
  SELECT *
  FROM
  				(SELECT C.CUSTOMERID,
  						C.FIRSTNAME || ' ' || C.LASTNAME AS CUSTOMER,
  						SUM(IL.UNITPRICE) AS TOTAL
  					FROM CUSTOMER C
  					JOIN INVOICE I ON I.CUSTOMERID = C.CUSTOMERID
  					JOIN INVOICELINE IL ON IL.INVOICEID = I.INVOICEID
  					JOIN TRACK T ON T.TRACKID = IL.TRACKID
  					WHERE COUNTRY = 'USA'
  					GROUP BY 1,2
  					ORDER BY 3 DESC) AS X
```



### Project submission - Important

**Presentations**

You are now on the portion of the project you will need to submit to a reviewer. To pass this project follow the below instructions to create a presentation.

**Your presentation should include:**

1. Four slides
2. One visualization per slide
3. A 1-2 sentence explanation of each slide
4. The SQL query used to create the data used in the visualization.

> Note: you may choose to use queries that were motivated by the questions on the previous concepts, or you may choose four entirely new questions. However, if you use any of the previous queries, they must be those that had a JOIN as stated in the Rubric.

The submission template is a Google Slides file. Make a copy of the submission template to complete your project. We suggest you use the layout provided, though it is not a requirement.

**Queries**
Please include a text file that includes each of the queries used to create the visualizations. You should **format** your queries for readability, use this tool to help http://www.sql-format.com/. In a plain text file (use notepad, notepad++, or atom).

**Visualizations**
We suggest you use a spreadsheet application, such as Excel or Google Sheets to create your visualizations. However, you’re welcome to use whatever tool you’d like. Your visualizations could be any that you learned about in the previous lesson. Below is one example, and a link has been provided to an example slide.

![image](./Misc/002.png)

You should have four slides that are similar to the below submission, but the questions you ask are up to you, and all four of your final submitted queries should contain a JOIN and AGGREGATION.

**Additional Guidelines**

* There shouldn’t be any additional data prep (sorting, filtering, renaming, etc.) between the query output and the visualization.
* All your four queries must include at least one join and an aggregation.
* Review your project against the project Rubric. Reviewers will use this to evaluate your work.
* The first part of this project is aimed at helping you understand the database, so you can ask interesting questions in the second part. Feel free to use and expand upon the queries you wrote in the first part.
* Once you've finished your project, submit the presentation as a PDF and the queries as a .txt file.
* Don't be afraid to challenge yourself! Try to combine the SQL concepts you know!