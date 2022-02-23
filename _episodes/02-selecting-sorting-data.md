---
title: "Sorting and selecting data"
teaching: 0
exercises: 0
questions:
- "What is a query?"
- "How do you query a database using SQL?"
- "How do you retrieve unique values in SQL?"
- "How do you sort results in SQL?"
objectives:
- "Understand how SQL can be used to query databases"
- "Understand how to build queries - using SQL keywords like DISTINCT and ORDER BY"
keypoints:
- "FIXME"
---

## What is a query?

A query is a request, or question, for data. As an example "HV-SPØRGSMÅL BASERET
PÅ VORES EKSEMPELDATA". When we send a query to a database, we do so by using a 
common language called Structured Query Language, SQL for short. The standard, 
official way to pronounce SQL is by spelling out the letters. Many people
pronounce it "sequel". The latter tends to be used by database pros. Use whatever
you feel most comfortable with.

## My very first query

Begin by opening DB Browser for SQLite, and our example database (see Setup).
Choose "Browse Data", and the TABLENAME DER MATCHER EKSEMPEL DATA table. This
table contains columns or fields. In this case FELTNAVNE DER MATCHER EKSEMPELDATA.

An SQL query that selects only the FELTNAVN column from the TABELNAVN table:

    SELECT feltnavn
    FROM tablenavn;

Note that we end our query with a semicolon, ;.

## Capitalization and good style

In the query above, we capitalized the words SELECT and FROM. We did taht 
because they are SQL keywords. The SQL interpreter does not care, the query
would return the exact same results if we had written everything in lower case
letters.

However, it is considered good style to write out the SQL keywords in upper case
letters. It makes it easier to read and pick out exactly what is happening in
the query. Understanding SQL is difficult enough as it is, so if we can make life
a little easier for ourself and others by capitalizing the keywords, that is a
good idea.

If we want more information from our database than just the FELTNAVN, we
can add a new column to the list of fields after SELECT, like this:

    SELECT feltnavn, feltnavn2, feltnavn3
    FROM tablenavn;

If we want everything, the wildcard * can be used:

    SELECT *
    FROM tablenavn;

## Unique values

Sometimes we only want unique records returned. Even if we only have unique
records in our database, we will probably get duplicated records if we only 
select some fields.

We get the unique records if we add the SQL keyword DISTINCT after the 
SELECT. That will eliminate duplicated records, and only return unique results:

    SELECT DISTINCT feltnavn
    FROM tablenavn;
    
If we select more than one column, we will get the unique or distinct combinations
of the values in those columns:

    SELECT DISTINCT feltnavn, feltnavn2
    FROM tabelnavn;

## Sorting

If we want to sort the results of our query, we use the keyword ORDER BY.
This is a query that sorts the TABELNAVN table in ascencing order by FELTNAVN 
using the ASC keyword together with ORDER BY:

    SELECT *
    FROM tabelnavn 
    ORDER BY feltnavn ASC;
    
The keyword ASC specifies that the results should be sorted in ascending order. 
If we want to sort in descending order, we could use the keyword DESC:

    SELECT *
    FROM tabelnavn
    ORDER BY feltnavn DESC;
    
It is possible to sort by more than one column at once, even in different directions:

    SELECT *
    FROM tablenavn
    ORDER BY feltnavn DESC feltnavn2 ASC;

Here we sort first by FELTNAVN in descending order, and then by FELTNAVN2 in
ascending order.


## En challenge!

REWRITE TO MATCH EXAMPLE DATA!
> ## Challenge
>
> Write a `CREATE VIEW` query that `JOINS` the `articles` table with the 
> `journals` table on `ISSNs` and returns the `COUNT` of article records 
> grouped by the `Journal_Title` in `DESC` order. 
>
> > ## Solution
> > ~~~
> > CREATE VIEW journal_counts AS
> > SELECT journals.Journal_Title, COUNT(*)
> > FROM articles
> > JOIN journals
> > ON articles.ISSNs = journals.ISSNs
> > GROUP BY Journal_Title
> > ORDER BY COUNT(*) DESC
> > ~~~
> > {: .sql}
> {: .solution}
{: .challenge}


{% include links.md %}
