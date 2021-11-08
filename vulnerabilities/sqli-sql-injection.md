# SQLi (SQL Injection)

## What is SQL injection (SQLi)?

SQL injection is a web security vulnerability that allows an attacker to interfere with the queries that an application makes to its database. It generally allows an attacker to view data that they are not normally able to retrieve. This might include data belonging to other users, or any other data that the application itself is able to access. In many cases, an attacker can modify or delete this data, causing persistent changes to the application's content or behavior.

In some situations, an attacker can escalate an SQL injection attack to compromise the underlying server or other back-end infrastructure, or perform a denial-of-service attack.

## SQL injection examples

There are a wide variety of SQL injection vulnerabilities, attacks, and techniques, which arise in different situations. Some common SQL injection examples include:

* Retrieving hidden data, where you can modify an SQL query to return additional results.
* Subverting application logic, where you can change a query to interfere with the application's logic.
* UNION attacks, where you can retrieve data from different database tables.
* Examining the database, where you can extract information about the version and structure of the database.
* Blind SQL injection, where the results of a query you control are not returned in the application's responses.

## Retrieving hidden data

Consider a shopping application that displays products in different categories. When the user clicks on the Gifts category, their browser requests the URL:

`https://insecure-website.com/products?category=Gifts`

This causes the application to make an SQL query to retrieve details of the relevant products from the database:

`SELECT * FROM products WHERE category = 'Gifts' AND released = 1`

The application doesn't implement any defenses against SQL injection attacks, so an attacker can construct an attack like:

`https://insecure-website.com/products?category=Gifts'-- `

This results in the SQL query:

`SELECT * FROM products WHERE category = 'Gifts'--' AND released = 1`

The key thing here is that the double-dash sequence -- is a comment indicator in SQL, and means that the rest of the query is interpreted as a comment. This effectively removes the remainder of the query, so it no longer includes AND released = 1. This means that all products are displayed, including unreleased products.

Going further, an attacker can cause the application to display all the products in any category, including categories that they don't know about:

`https://insecure-website.com/products?category=Gifts'+OR+1=1--`

This results in the SQL query:

`SELECT * FROM products WHERE category = 'Gifts' OR 1=1--' AND released = 1`

The modified query will return all items where either the category is Gifts, or 1 is equal to 1. Since 1=1 is always true, the query will return all items.

## Subverting application logic

Consider an application that lets users log in with a username and password. If a user submits the username wiener and the password bluecheese, the application checks the credentials by performing the following SQL query:

`SELECT * FROM users WHERE username = 'wiener' AND password = 'bluecheese'`

If the query returns the details of a user, then the login is successful. Otherwise, it is rejected.

Here, an attacker can log in as any user without a password simply by using the SQL comment sequence -- to remove the password check from the WHERE clause of the query. For example, submitting the username administrator'-- and a blank password results in the following query:

`SELECT * FROM users WHERE username = 'administrator'--' AND password = ''`

This query returns the user whose username is administrator and successfully logs the attacker in as that user.

## Retrieving data from other database tables

## PortSwigger Labs

#### SQL injection vulnerability in WHERE clause allowing retrieval of hidden data

> This lab contains an SQL injection vulnerability in the product category filter. When the user selects a category, the application carries out an SQL query like the following:
>
> `SELECT * FROM products WHERE category = 'Gifts' AND released = 1`
>
> To solve the lab, perform an SQL injection attack that causes the application to display details of all products in any category, both released and unreleased.

When navigating to the webpage and selecting a category we get the below URL

`https://acbe1f741ffa1ec1c0ad7146005d0034.web-security-academy.net/filter?category=Pets`

We can add a little SQL to get the additional data

`https://acbe1f741ffa1ec1c0ad7146005d0034.web-security-academy.net/filter?category=Pets'OR%201=1--`

#### SQL injection vulnerability allowing login bypass

> This lab contains an SQL injection vulnerability in the login function.
>
> To solve the lab, perform an SQL injection attack that logs in to the application as the administrator user.

When navigating to the webpage and selecting My Account we get as simple login form

In the Username field we can place in `administrator'--` and in the Password field we can put junk text and we successfully log in as the administrator user.
