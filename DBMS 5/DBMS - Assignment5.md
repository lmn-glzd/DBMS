# Database Management Systems/ Assignment 5


## Table of Contents
1. [ER Diagram](#er_diagram)
2. [Running ERD generated queries](#running_erd_generated_queries)
*   Visualizing Tables
3. [Type Tables Added](#type_tables_added)
4. [Primary Key in Type Table](#primary_key_in_type_table)
5. [ERD for Students](#erd_for_students)
* Visualizing Tables


## ER Diagram
![alt text](<images/image.png>)

*Note*: Due to nonavailiability of some of the features on ERD plus, the type of attributes and constraints have not been described in diagram, which is reflected on sql statements and tables' creation.

## Running ERD generated queries

1. 
~~~~sql
CREATE TABLE Customers
(
  customer_id INT NOT NULL,
  customer_name INT NOT NULL,
  customer_surname INT NOT NULL,
  delivery_location INT NOT NULL,
  PRIMARY KEY (customer_id)
);
~~~~
 Result of query:
 ![alt text](<images/customers.png>)

2. 
~~~~sql
CREATE TABLE Orders
(
  total_price INT NOT NULL,
  shipping_cost INT NOT NULL,
  delivery_date_requested INT NOT NULL,
  delivery_date_actual INT NOT NULL,
  order_id INT NOT NULL,
  customer_id INT NOT NULL,
  PRIMARY KEY (order_id),
  FOREIGN KEY (customer_id) REFERENCES Customers(customer_id)
);

~~~~
Result of query: 
 ![alt text](<images/orders.png>)

3.
~~~~sql
CREATE TABLE Products
(
  price INT NOT NULL,
  quantity_available INT NOT NULL,
  product_id INT NOT NULL,
  PRIMARY KEY (product_id)
);
~~~~
Result of query:  
![alt text](<images/products.png>)

4. 
~~~~sql
CREATE TABLE Order_Details
(
  order_id INT NOT NULL,
  product_id INT NOT NULL,
  product_quantity INT NOT NULL,
  sub_total_price INT NOT NULL,
  unit_price INT NOT NULL,
  customer_id INT NOT NULL,
  PRIMARY KEY (order_id, product_id),
  FOREIGN KEY (order_id) REFERENCES Orders(order_id),
  FOREIGN KEY (product_id) REFERENCES Products(product_id),
  UNIQUE (product_id)
);
~~~~
Result: 
![alt text](<images/order_details_table.png>)


## Visualizing Tables 

Customer Table:
![alt text](<images/customers_table.png>)
Orders Table:
![alt text](<images/orders_table.png>)
Order_Details Table:
![alt text](<images/order_details_table.png>)
Products Table:
![alt text](<images/products_table.png>)



## Type Tables Added
Type tables for possible categorial attributes of "order_type", "delivery_status", and "product_type" added to diagram:
![alt text](<images/image 2.png>)



## Primary Key in Type Table
In a type table, the primary key is used to uniquely identify each record. It makes sure that each type (like a category, brand, or status) is distinct and can be easily referenced in other parts of the database. For example, in a Product_Category table, the primary key could be Category_ID, which ensures each category (e.g., "Electronics", "Clothing") is unique.
The reason for using a primary key in a type table is to maintain data integrity and make sure we can link it properly with other tables. If we have products in the Products table, we can use the Category_ID from the type table as a foreign key, keeping everything organized and consistent.
So, the primary key helps with unique identification and establishing relationships between tables.

## ERD for Students
![alt text](<images/students.png>)

Converting to SQL statement:
```sql
CREATE TABLE Student
(
  Student_ID INT NOT NULL,
  First_Name INT NOT NULL,
  Last_Name INT NOT NULL,
  PRIMARY KEY (Student_ID)
);
```
Result: 
![alt text](<images/stduent.png>)

```sql
CREATE TABLE Undergraduate
(
  Major INT NOT NULL,
  Grad_Year INT NOT NULL,
  Admission_Year INT NOT NULL,
  Student_ID INT NOT NULL,
  PRIMARY KEY (Student_ID),
  FOREIGN KEY (Student_ID) REFERENCES Student(Student_ID)
);
```
Result:
![alt text](<images/undergraduate.png>)

```sql
CREATE TABLE Graduate
(
  Degree INT NOT NULL,
  Thesis_Topic INT NOT NULL,
  Student_ID INT NOT NULL,
  PRIMARY KEY (Student_ID),
  FOREIGN KEY (Student_ID) REFERENCES Student(Student_ID)
);
```
Result:
![alt text](<images/graduate.png>)

```sql
CREATE TABLE Research_Assistant
(
  Research_Project INT NOT NULL,
  Supervisor INT NOT NULL,
  Student_ID INT
);
```
Result:
![alt text](<images/research_assistant.png>)


## Visualizing physical tables

![alt text](<images/student_table.png>)
![alt text](<images/undergraduate_table.png>)
![alt text](<images/graduate_table.png>)
![alt text](<images/ra_table.png>)