---
layout: default
title: Databases
---

# Databases

## What are databases?

Databases are collections of stored data. In addition to the data stored, a database will have definitions of the data and how different data are related to each other. Typically data will be arranged in tables, which have rows and columns: a row corresponds to one record in the database while a column will define an attribute on the records of a table. For instance, a `users` table may have a row for each user, where each row has multiple columns: one for the first name, last name, email, phone number, etc. Databases allow records to be created, altered, read, and destroyed.

## Databases' role in web development

Some websites are static, meaning that they have predefined content that does not change. A user may click links to bring them to different pages with different content, and they may even interact with the page in a way to cause the content on the page to change, but any user visiting them same site will see the same content. Content can only be changed by someone who maintains the site. Think of a restaurants website, or a website about another business. Typically they will have content about their hours, services they provide, contact information, events, and maybe some photos. It is not too hard for the maintainer of the site to hard-code this information directly into the HTML for the pages of the website and simply update the data whenever necessary.

Compare this to a dynamic website, such as a social media website. Users need to be able to submit their own content and see the content of others in realtime. This content cannot be hard-coded because it is infeasbile for any maintainer to continue hard-coding content as users submit changes to the content; even then, the information about the changes would need to be stored somewhere. This is where databases come in. They allow us to store content as well as changes to that content. There are many different, creative ways to store data, but the basic idea is that any data relating to content in a site that is expected to be created, altered, or removed by users of a dynamic website or a web application should live in a database. When a user goes to a site in their browser, the application running the site knows how to communicate with the database, read in the data, render it on the page, and make changes to it as the user interacts with the site.

## Types of databases

Databases are just a way of storing data. A very basic approach would be to simply write all of the data to a file and make changes to the file any time the data is changed. So why not just do that all the time? [Here's a solid answer from Stack Exchange to that very question](https://softwareengineering.stackexchange.com/a/190483). The short version is that databases provide structure, allow us to read specifically data very quickly, and allow multiple users to access and change data simultaneously.

### Relational

The earlier description of databases as tables of records (rows) with attributes (columns) can be described as tabular databases. Relational databases, an extention of tabular databases, provides us with the ability to define relationships between records. Expanding on the `users` example from before, if we had an additional table `companies` that had data about companies that users belong to, then we could add an attribute (column) to the `users` table called `company` that stored the ID of the `companies` record for the company that the user worked for. You might wonder why we couldn't just put the company name in that column. For one thing, a company name might not be unique; we could have two or more records (one for each company) where the company name is the same. We would not be able to determine which of these companies the user actually works for. Instead, we add an ID column to companies and make sure the IDs for each record are unique. We will find these relations to be very useful.

#### Foreign Keys

If we include a company ID in a user record to indicate the user's company, we probably want to ensure that the ID we specify actually belongs to a company record in the database. Foreign key constraints ensure that data integrity, specifically that the ID we provide must exist. Foreign keys also allow us to use cascading deletes; in our example, if we delete a company from the database, we probably would also want to delete all users who belong to that company. That said, it's usually a good idea to use soft deletes when deleting a record; these mark records as deleted without actually throwing away the data.

#### SQL

A great feature of relational databases is the ability to query for data in very rich ways, exploiting relations to do so. SQL, Structured Query Language, is a language that can be used to read and write data in databases, and it enables you to ask for data in interesting ways. Consider the scenario in which you want to get all users that belong to a specific company. You may just ask for all companies, find the ID for the company you care about (let's say you identified the company by name), find all users, and filter out the ones with a company ID matching the one you found. This is pretty tedious, especially if you have a lot of companies and users. What if instead you could just ask for one company record by company name? What if you could ask for all users with a company ID matching the one from the record that was returned by the previous question? And what if you could combine both of these queries into a single query? With SQL, this is possible. You can even specify a limit for the number of users returned, count how many records match the query, and much more.

There are several implementations of SQL databases. The three we will focus on are MySQL, PostgreSQL, and SQLite. A special note about these database management systems is that they have slightly different data types, meaning the constraints on the values of different attributes can vary between these systems.

##### MySQL

MySQL is the most commonly used relation database management system. This means that there is a wealth of resources available for working with MySQL, including how to set it up, tools to use with it, answers to common questions, and solutions to common problems. MySQL does not implement the entire SQL standard, which allows it to do some clever things to perform faster than the standard would allow. There is not a lot of development on MySQL, so it should be fairly stable but may not receive many of the latest features of the SQL standard.

##### PostgreSQL

Like MySQL, PostgreSQL has a large community behind it, meaning that a lot of resources are avaible. PostgreSQL is also very advanced and compliant with the SQL standard. It has reliable transactions because it implements atomicity, consistency, isolation, and durability (ACID). An atomic transaction is one where either all changes happen or none happen. You may want to perform a transaction in which many records are updated, but if there is an issue with updating one of the records, you want to make sure that none of the other updates were saved, otherwise you might not know which records were updated. Consistency means that changes in a transaction are legal; for instance, changes commited in the transaction cannot violate any constraints. Isolation indicates that the result of performing multiple concurrent transactions is the same as the result that would come from performing each transaction one at a time. Durability means that a commited transaction must be stored properly; if the database goes down and comes back up, the result of the transaction should still be present.

Because PostgreSQL is so heavily featured, it can be a bit slow, especially if most calls to the database are reads, not writes. But if data integrity is important, or the database is not read-heavy, then PostgreSQL can be a good choice.

##### SQLite

SQLite uses a single file to store data for the database but supports querying the data in the file with SQL. It does not include the user management features common to other systems, which allow access to the database by different users with possibly different permissions. SQLite only supports one write at a time, so it is a bad choice for applications that are write-heavy. [The SQLite website provides some good guidelines of when to use or not use SQLite](https://www.sqlite.org/whentouse.html).

### Non-Relational

To be continued...
