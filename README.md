
# Filtering and Ordering - Lab


## Introduction
In this lab, you will write more `SELECT` statements to solidify your ability to query a SQL database. You will also write more specific queries using the tools you learned in the previous lesson.

## Objectives
You will be able to:
* Write SQL queries to filter and order results
* Order the results of your queries by using `ORDER BY` (`ASC` & `DESC`)
* Limit the number of records returned by a query using `LIMIT`
* Filter results using `BETWEEN` and `IS NULL`

### Famous Dogs

Here's a database full of famous dogs!  The `dogs` table is populated with the following data:

|name      |age    |gender |breed           |temperament|hungry |
|----------|-------|-------|----------------|-----------|-------|
|Snoopy    |3      |M      |beagle          |friendly   |1      |
|McGruff   |10     |M      |bloodhound      |aware      |0      |
|Scooby    |6      |M      |great dane      |hungry     |1      |
|Little Ann|5      |F      |coonhound       |loyal      |0      |
|Pickles   |13     |F      |black lab       |mischievous|1      |
|Clifford  |4      |M      |big red         |smiley     |1      |
|Lassie    |7      |F      |collie          |loving     |1      |
|Snowy     |8      |F      |fox terrier     |adventurous|0      |
|NULL      |4      |M      |golden retriever|playful    |1      |

## Connecting to the Database

First, import sqlite3 and establish a connection to the database **dogs.db**. Then, create a cursor object so that you can pass SQL queries to the database.


```python
#Your code here; import sqlite, create a connection and then a cursor object.
import sqlite3
```

 


```python
conn = sqlite3.connect("dogs.db")
```


```python
conn.execute("select * from dogs;").fetchall()
```




    [(1, 'Snoopy', 3, 'M', 'beagle', 'friendly', 1),
     (2, 'McGruff', 10, 'M', 'bloodhound', 'aware', 0),
     (3, 'Scooby', 6, 'M', 'great dane', 'hungry', 1),
     (4, 'Little Ann', 5, 'F', 'coonhound', 'loyal', 0),
     (5, 'Pickles', 13, 'F', 'black lab', 'mischievous', 1),
     (6, 'Clifford', 4, 'M', 'big red', 'smiley', 1),
     (7, 'Lassie', 7, 'F', 'collie', 'loving', 1),
     (8, 'Snowy', 8, 'F', 'fox terrier', 'adventurous', 0),
     (9, None, 4, 'M', 'golden retriever', 'playful', 1)]



## Queries

Display the outputs for each of the following query descriptions.

### Select the name and breed for all female dogs


```python
#Your code here
conn.execute("select * from dogs where gender like '%F';").fetchall()
```




    [(4, 'Little Ann', 5, 'F', 'coonhound', 'loyal', 0),
     (5, 'Pickles', 13, 'F', 'black lab', 'mischievous', 1),
     (7, 'Lassie', 7, 'F', 'collie', 'loving', 1),
     (8, 'Snowy', 8, 'F', 'fox terrier', 'adventurous', 0)]




```python
# or 
conn.execute("select * from dogs where gender='F';").fetchall()
```




    [(4, 'Little Ann', 5, 'F', 'coonhound', 'loyal', 0),
     (5, 'Pickles', 13, 'F', 'black lab', 'mischievous', 1),
     (7, 'Lassie', 7, 'F', 'collie', 'loving', 1),
     (8, 'Snowy', 8, 'F', 'fox terrier', 'adventurous', 0)]



### Select the names of all dogs listed in alphabetical order.  Notice that SQL lists the nameless dog first.


```python
#Your code here
conn.execute('select * from dogs order by name asc;').fetchall()
```




    [(9, None, 4, 'M', 'golden retriever', 'playful', 1),
     (6, 'Clifford', 4, 'M', 'big red', 'smiley', 1),
     (7, 'Lassie', 7, 'F', 'collie', 'loving', 1),
     (4, 'Little Ann', 5, 'F', 'coonhound', 'loyal', 0),
     (2, 'McGruff', 10, 'M', 'bloodhound', 'aware', 0),
     (5, 'Pickles', 13, 'F', 'black lab', 'mischievous', 1),
     (3, 'Scooby', 6, 'M', 'great dane', 'hungry', 1),
     (1, 'Snoopy', 3, 'M', 'beagle', 'friendly', 1),
     (8, 'Snowy', 8, 'F', 'fox terrier', 'adventurous', 0)]



### Select any dog that doesn't have a name


```python
#Your code here
conn.execute('select * from dogs where name is null;').fetchall()
```




    [(9, None, 4, 'M', 'golden retriever', 'playful', 1)]



### Select the name and breed of only the hungry dogs and list them from youngest to oldest


```python
conn.execute('select * from dogs where hungry order by age asc').fetchall()
```




    [(1, 'Snoopy', 3, 'M', 'beagle', 'friendly', 1),
     (6, 'Clifford', 4, 'M', 'big red', 'smiley', 1),
     (9, None, 4, 'M', 'golden retriever', 'playful', 1),
     (3, 'Scooby', 6, 'M', 'great dane', 'hungry', 1),
     (7, 'Lassie', 7, 'F', 'collie', 'loving', 1),
     (5, 'Pickles', 13, 'F', 'black lab', 'mischievous', 1)]



### Select the oldest dog's name, age, and temperament


```python
#Your code here
conn.execute('select name from dogs order by age desc limit 1').fetchall()
```




    [('Pickles',)]



### Select the three youngest dogs


```python
#Your code here
conn.execute('select name from dogs order by age asc limit 3').fetchall()
```




    [('Snoopy',), ('Clifford',), (None,)]



### Select the name and breed of the dogs who are between five and ten years old, ordered from oldest to youngest


```python
#Your code here
conn.execute('select name, breed from dogs where age <10 and age > 5 order by age desc').fetchall()
```




    [('Snowy', 'fox terrier'), ('Lassie', 'collie'), ('Scooby', 'great dane')]



### Select the name, age, and hungry columns for hungry dogs between the ages of two and seven.  This query should also list these dogs in alphabetical order.


```python
#Your code here
conn.execute('select name, age, hungry from dogs where hungry and age > 2 and age < 7 order by name').fetchall()
```




    [(None, 4, 1), ('Clifford', 4, 1), ('Scooby', 6, 1), ('Snoopy', 3, 1)]



## Summary

Great work! In this lab you practiced writing more complex SQL statements to not only query specific information but also define the quantity and order of your results. 
