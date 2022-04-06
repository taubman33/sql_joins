[![General Assembly Logo](https://camo.githubusercontent.com/1a91b05b8f4d44b5bbfb83abac2b0996d8e26c92/687474703a2f2f692e696d6775722e636f6d2f6b6538555354712e706e67)](https://generalassemb.ly/education/web-development-immersive)


# SQL JOINs

![](https://365datascience.com/resources/blog/2019-11-what-are-joins.jpg)

## Overview

In this lesson, we'll learn how to interact with and define associated data.
Utilizing the power of `joins`, we can join related data together utilizing `foreign key` references.

## Getting Started

- Fork and Clone

## Learning Objectives

- Create tables with foreign key references.
- Create join tables to represent many-to-many relationships.
- Insert rows in join tables to create many-to-many relationships.
- Select data about many-to-many relationships using join tables.

## Introduction

While it is conceivable to store all of the data that is needed for a particular domain model object or resource in a single table, there are numerous downsides to such an approach. For example, in sql, if we want to update the name `America` or `Ireland` to `United States of America` or `Republic Of Ireland`, we would have to update every single row in the table that referred to either of these places of origin. Thus, `redundancy` of common data points can make altering or updating these fields difficult.

Further, there are weak guarantees for the consistency and correctness of hard-coded fields in a single column; what prevents a developer who is working on a different feature from using `french` rather than `France` when inserting new rows into the a table? Leveraging table relations can improve `data integrity` and provide stronger guarantees regarding the consistency and correctness of what we store and retrieve from a database.

One of the key features of relational databases is that they can represent
relationships between rows in different tables.

Consider spotify, we could start out with two tables, `artist` and `track`.
Our goal now is to somehow indicate the relationship between an artist and a track.
In this case, that relationship indicates who performed the track.

You can imagine that we'd like to use this information in a number of ways, such as...

- Getting the artist information for a given track
- Getting all tracks performed by a given artist
- Searching for tracks based on attributes of the artist (e.g., all tracks
  performed by artists at Interscope)

## Setting Up Our Database

Let's build out a todo database, starting with todos and users.
Note how id's are PRIMARY KEYs, and relationships are established when these ids are referenced by other tables.

`seed.sql`

```sql
-- Your Code Here
CREATE TABLE authors(
    id VARCHAR(255) PRIMARY KEY,
    name VARCHAR(255),
    nationality VARCHAR(255)
);

CREATE TABLE books(
    id VARCHAR(255) PRIMARY KEY,
    title VARCHAR(255),
    description VARCHAR(255),
    completed BOOLEAN NOT NULL,
    author_id VARCHAR(255) REFERENCES authors(id)
);
-- Your Code Here
```



### Writing SQL JOINS


Relationships
Like most databases, relationships are defined with Foreign Keys or References. Where we place these Foreign Keys dictates the type of relationship. Let's take a look at the seed.sql file. If you notice in the todos Create Table statement, we're using References to state that this user_id column references a user in the users table we created in the statement above. This reference is dictating a one-many relationship between the user and todos like so: one user - has many - todos. The Foreign Key or Reference allows us to traverse our database to find related records. We perform this by using a SQL statement called a JOIN.


To `SELECT` information on two or more tables at ones, we can use a `JOIN`
clause. This will produce rows that contain information from both tables. When
joining two or more tables, we have to tell the database how to match up the
rows. (e.g. to make sure the author information is correct for each book).

This is done using the `ON` clause, which specifies which properties to match.

Joins
Joins are statements that allow us to traverse and join data together by using some kind of Foreign Key or Reference, this is the only way we can perform joins.


This is an example of an `INNER JOIN`. Inner joins are the default type of join in `SQL`. When we used the `JOIN` keyword, `SQL` interprets this as `INNER JOIN`. Here's a visual on what happens during an inner join:

<div>
  <img src="https://dataschool.com/assets/images/how-to-teach-people-sql/innerJoin/innerJoin_3.gif" alt="inner_join_animated"/>
</div>

Take 5 minutes to read the following: **[Inner Joins Animated](https://dataschool.com/how-to-teach-people-sql/inner-join-animated/)**

As you can see, an `INNER JOIN` creates a new table with the data we speficed during the `SELECT` statement and organizes it based on some kind of `REFERENCE`.

There are a few types of joins such as:

- Left Join
- Right Join
- Outer Joins
- Union
- Cross Join

<div>
  <img src="https://i.redd.it/dyqnzpuddxk21.png"
  </div


```sql
SELECT id FROM authors where name = 'J.K. Rowling';
SELECT * FROM books where author_id = 2;

SELECT * FROM books JOIN authors ON books.author_id = authors.id;
SELECT * FROM books JOIN authors ON books.author_id = authors.id WHERE authors.nationality = 'United States of America';
```




We can use some premade Seed data to work with these commands

```bash
$ createdb library
```

Note that this is a command-line utility that ships with Postgres, as an alternate to using the SQL command `CREATE DATABASE library;` inside `psql`.

That means you should run this command from your Bash prompt -- not from inside `psql`.

## Inspecting The Schema

Look critically at each line of the provided `schema.sql` file. Here's how one row breaks down...

**`id SERIAL PRIMARY KEY`**

- `id`: column name, how we will refer to this column
- `SERIAL`: the data type (similar to integer or string). It's a special datatype for unique identifier columns, which the db auto-increments.
- `PRIMARY KEY`: a special constraint which indicates a unique identifier for each row

Take a few minutes to research the other rows.

## Load The Schema

There are two ways to execute a sql file using psql. 

* Using your normal shell (be sure you have changed directory into this repo):

```bash
$ psql -d library < schema.sql
```

* Or from inside the psql cli:

```sql
-- first cd to the directory where the file is
-- then launch the psql cli and connect to the right db
$ psql -d library
-- execute using \i and the filename
\i schema.sql
```

## Loading A Seed File

We've provided a sql file that adds sample data into our `library` database.

Load that in so we can practice interacting with our data. Make sure to also look at its contents and see how authors and books are related.

```bash
$ psql -d library < seed.sql
```

Or:

```sql
$ psql -d library
\i seed.sql
```


## Exercises

--- Find all fields (book and author related) for all books written by George R.R. Martin.

-- Find all fields (book and author related) for all books written by Milan Kundera.

-- Find all books written by an author from China or the UK.

-- Find out how many books Albert Camus wrote.

-- Find all books written after 1930 by authors from Argentina.

-- Find all books written before 1980 by authors not from the US.




**Hint: This <u>[resource](https://dataschool.com/how-to-teach-people-sql/left-right-join-animated/)</u> may be useful.**

## Recap

In this lesson, we learned how `foreign keys`/`references` are an important part of designing a database. Not only do they ensure our data is properly structured, they also allow us to perform some awesome queries called `joins`. These joins allow us to aggregate data accross multiple tables!

# Resources

- [Data School: SQL Joins Explained](https://dataschool.com/how-to-teach-people-sql/sql-join-types-explained-visually/)
- [Code School Try SQL](https://www.codeschool.com/courses/try-sql)
- [W3 Schools SQL tutorial](https://www.w3schools.com/sql/)
- [Postgres Guide](http://postgresguide.com/)

