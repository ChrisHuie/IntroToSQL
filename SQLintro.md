## What is a relational database?

There are two major components in a database:

- Data(Information Stored) and Schema(How the data is organized)
- Sections. Also called tables. This how the data get structured. 


##### Tables
You can think of tables as spreadsheets:


| Column ↓  | Column ↓   |  Column ↓  |
|---|---|---|
| Row →   |   |   | 
| Row →   |   |   |
| Row →   |   |   | 

##### Table Organization

Usually separate tables contain data for a specific type of thing:

If we look at [w3schools](https://www.w3schools.com/sql/trysql.asp?filename=trysql_select_all) again you can see all the different tables in the database


Orders table:

| OrderID  |  	CustomerID | 	EmployeeID  | OrderDate  |  ShipperID |
|---|---|---|---|---|
| 10248  | 90  |  5 | 1996-07-04  | 	3  |
| 10249  |  81 |  6 |  1996-07-05 |  1 |
|  10250 | 34  | 4  |  1996-07-08 |  2 |


Products Tables:

| ProductID  | ProductName  | SupplierID  | CategoryID  |  Unit | Price|
|---|---|---|---|---|---|
| 1  |  Chais |  1 |  1 |  10 boxes x 20 bags | 18  |
|  2 |  Chang |  1 |  1 | 24 - 12 oz bottles  | 19 |
|  3 | Aniseed Syrup  | 1  | 2  |  12 - 550 ml bottles | 10  |

<!--Filter data table example:

Rearrange table example:-->

Having multiple tables storing data can seem complicated at first, why not have everything in one giant table?

Each table can have relationships with each other, instead of repeating the same product how ever many times someone buys it, we can instead reference back to that product in a separate table.

Again, don't worry if this is a bit confusing! It will make more sense as we go!


The same database can be used in different ways across applications at the same time.

- Website could be pulling data to show your purchase history
- An analyst could be pulling the data to learn what the most popular item is

---

### Data types

data types for table columns are defined in the schema

The most common data types are:

- Text (Names, Descriptions)
- Numeric (cost, age, weight, quantity)
- Dates (Dates and time)

Its important to have the correct data type for your data. This ensures that sorting and calculations work properly. We won't be setting up a database in this workshop, but it something to keep in mind when you do!

*Note:* Data types may change depending on which database you're using.


We could spend a whole workshop discussing databases, but we're going to focus on querying language SQL for the remainder of this workshop. 

Learn more about databases on your own [here](https://www.w3schools.com/sql/sql_create_db.asp)!

If you would like to see a workshop more focused on Databases and how create them let me know!

---

### Querying

Important first step is to understand your data.

- How is it structured?
- What does it represent?


Lets do some different types querying to figure this out!

Lets take a look at all the categories of products we have

`SELECT * FROM Categories;`

Lets see how many customers we have?

`SELECT COUNT(*) FROM Customers;`

Lets take a look at our orders

`SELECT * FROM Orders;`

Lets rearrange the orders by date

*Note:* When we query this isn't actually changing the database. You're just choosing what and how to look at the data.

```
SELECT * FROM Orders
ORDER BY OrderDate DESC;
```
ASC = Ascending
DESC = Descending

Neat! Lets see which customer made that top order (most recent)! We will take the value from the CustomerID column.

```
SELECT * FROM customers
WHERE CustomerID = 66;
```

Cool! We can also query for matches in multiple columns

```
SELECT * FROM OrderDetails
WHERE ProductID = 14 AND Quantity < 10;
```

We can also filter columns by more than one value.

```
SELECT * FROM OrderDetails
WHERE ProductID = 14 OR ProductID = 42;
```


So far we have been returning all the columns of a record that match our query. What if we want to only look at a few of them? 

Instead of using `*` to select all we can put in the specific columns we want returned. We can also put them in different order!

```
SELECT Country, city, CustomerName FROM Customers;
```
Lets order by Ascending order (A-Z) of Country Name

```
SELECT Country, city, CustomerName FROM Customers
ORDER BY Country ASC;
```

Lets See just a list of countries 
Using `DISTINCT`. Which will just return one instance.

Run the query below with and without `DISTINCT` to see the difference.

```
SELECT DISTINCT Country FROM Customers
ORDER BY Country ASC;

```


Sometimes we want to filter by records that contain values or characters that are not an exact match.

What customers have a name starting with `A`?

```
SELECT * FROM Customers
WHERE CustomerName LIKE 'A%';
```

- 'A%' Starts with an A
- '%A' Ends with an A
- '%A%' Contains an A

Read more types of way to filter [here](https://www.w3schools.com/sql/sql_like.asp).


## Functions

If you've used excel you'll probably be familiar with some of these functions. 


- SUM() `SELECT SUM(Quantity) FROM OrderDetails;`
- MAX() `SELECT ProductID, ProductName, MAX(Price) FROM Products;`
- MIN() `SELECT ProductID, ProductName, MIN(Price) FROM Products;`
- AVG() `SELECT AVG(Price) FROM Products;`
- COUNT() `SELECT COUNT(*) FROM Customers;`


### Changing Data to the database

So far we've only been looking at our data in different ways, which is super useful for answering questions, but lets actually make some changes to the database!


#### Insert into

```
SELECT * FROM Customers;
```

```
INSERT INTO Customers (CustomerName, City, Country)
VALUES ('Sage', 'Seattle', 'USA');
```

```
SELECT * FROM Customers;
```

##### What's Null?
You should notice that some of the sections on our new row contain the word `null`. 
In SQL you can think of `null` means that the cell contains no value. 


#### Update

```
SELECT * FROM Customers;
```

```
UPDATE Customers
SET ContactName='Sage', City='Seattle'
WHERE CustomerID=1;
```

```
SELECT * FROM Customers;
```

Its common to select and update more than one single row by ID. 

```
SELECT * FROM Customers
WHERE CustomerName LIKE 'A%'
```

```
UPDATE Customers
SET ContactName='I started with an A', City='A town'
WHERE CustomerName LIKE 'A%'
```

```
SELECT * FROM Customers
WHERE CustomerName LIKE 'A%'
```

#### Delete

```
SELECT * FROM Customers;
```

```
DELETE FROM Customers
WHERE CustomerName='Sage';
```

```
SELECT * FROM Customers;
```


### Joins

We're not going to go super deep into joins in this workshop and they can be a bit tricky to understand, so don't worry if you don't quite get it yet! Read about and practice them further [here](https://www.w3schools.com/sql/sql_join.asp).

This of it as essentially joining tables together. We're then returning combined table for use to use. 

```
SELECT Orders.OrderID, Customers.CustomerName, Orders.OrderDate,
OrderDetails.ProductID, Products.productName, Products.Price
FROM Orders
JOIN Customers ON Orders.CustomerID=Customers.CustomerID
JOIN OrderDetails ON Orders.OrderID=OrderDetails.OrderID
JOIN Products ON OrderDetails.ProductID=Products.ProductID;
```

TODO: Make simpler for first join intro and add graph

By default when you just use 'join' we are creating a inner join. Read more about different joins [here](https://www.w3schools.com/sql/sql_join.asp)
.

Find a helpful graph on joins [here](https://stackoverflow.com/questions/565620/difference-between-join-and-inner-join).
