# Lab 01 — SQL injection in WHERE clause, retrieve hidden data

**Difficulty:** Apprentice
**Lab link:** https://portswigger.net/web-security/sql-injection/lab-retrieve-hidden-data

## What the vulnerability is
What is the app doing wrong with the user input The app in the section of products; category = Gifts the Sql query is become SELECT * FROM products WHERE category = 'Gifts' AND released = 1
where released is restriction and SQL query ask database to the details(*) from the products table where the category is gifts and released is 1 which release only one and hide other which we can refer unreleased products to released=0.
This app doesnot implement the defence against SQL injection attack.

## How I found it
In burp suite i intercept or modify the category parameter from the repeter. 

## The payload
i change the parameter of category by adding the value'-- or '+OR+1=1--

## Why it works
it worked because the -- is a comment indicator which makes the rest of the query interpreted as comment which makes the original query no longer includes AND released=1  as the results products are displayed including those not released. and for another to dispaly all the products in any cateogry which returns all the items where either the category is gifts, or 1 is equal to 1. As 1=1 is always true.

## How to fix it
The developer should use Parameterized Queries means the database what the query is first, and then you hand it the user input separately as a piece of data. This separates the SQL code from the user data, so the database treats user input strictly as data and never as executable code.
query = "SELECT * FROM products WHERE category = ?" that means user input into ? which suggest even if anyone put '-- or 1=1-- the databse treat the single name or sting as cateogry name and never trate it as sql commands.
