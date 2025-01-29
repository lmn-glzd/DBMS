# Database Management Systems/ Assignment 2 - Solution


## Table of Contents
1. [Setting up Tables](#setting-up-tables)
2. [Running Queries](#running-queries)

## Setting up Tables

1. Creating Department table
~~~~sql
 CREATE TABLE Department (

    DeptID INT PRIMARY KEY,

    DeptName VARCHAR(50),

    ParentDeptID INT

);
~~~~
 Result of query: Table created.

2. Creating Employee table
~~~~sql
CREATE TABLE Employee (

    EmpID INT PRIMARY KEY,

    EmpName VARCHAR(50),

    DeptID INT,

    ManagerID INT,

    Salary DECIMAL(10, 2),

    HireDate DATE,

    FOREIGN KEY (DeptID) REFERENCES Department(DeptID),

    FOREIGN KEY (ManagerID) REFERENCES Employee(EmpID)

);

~~~~
Result of query: Table created.



3. Inserting into Department table
~~~~sql
INSERT INTO Department (DeptID, DeptName, ParentDeptID) VALUES

(1, 'Engineering', NULL),

(2, 'Marketing', 17),

(3, 'Development', 1),

(4, 'Quality Assurance', 1),

(5, 'Sales', 2),

(6, 'Customer Support', 2),

(7, 'Software Development', 3),

(8, 'Hardware Development', 3),

(9, 'Internal QA', 4),

(10, 'External QA', 4),

(11, 'North Region Sales', 5),

(12, 'South Region Sales', 5),

(13, 'East Region Sales', 5),

(14, 'West Region Sales', 5),

(15, 'Product Support', 6),

(16, 'Service Support', 6);
~~~~
Result of query: 16 row(s) inserted.

Inserting additional department with id 17 to prevent "Integrity constraint violated" error so that employee with department id 17, William Martinez, will have department assigned.

~~~~sql
INSERT INTO Department (DeptID, DeptName, ParentDeptID) 
VALUES (17, 'Content Strategy', 2); 
~~~~
Result: 1 row(s) inserted.


4. Inserting into Employee table
~~~~sql
INSERT INTO Employee (EmpID, EmpName, DeptID, ManagerID, Salary, HireDate) VALUES

(1, 'John Doe', 7, 20, 80000.00, TO_DATE('2020-01-15', 'YYYY-MM-DD')),

(2, 'Jane Smith', 7, 1, 75000.00, TO_DATE('2020-02-20', 'YYYY-MM-DD')),

(3, 'David Johnson', 9, 1, 60000.00, TO_DATE('2020-03-10', 'YYYY-MM-DD')),

(4, 'Mary Brown', 11, 2, 70000.00, TO_DATE('2020-04-05', 'YYYY-MM-DD')),

(5, 'Michael Lee', 15, 2, 65000.00, TO_DATE('2020-05-15', 'YYYY-MM-DD')),

(6, 'Jennifer Wilson', 15, 1, 70000.00, TO_DATE('2020-06-20', 'YYYY-MM-DD')),

(7, 'Robert Taylor', 11, 3, 72000.00, TO_DATE('2020-07-10', 'YYYY-MM-DD')),

(8, 'Lisa Anderson', 13, 3, 68000.00, TO_DATE('2020-08-05', 'YYYY-MM-DD')),

(9, 'William Martinez', 17, 4, 75000.00, TO_DATE('2020-09-15', 'YYYY-MM-DD')),

(10, 'Karen Garcia', 12, 4, 70000.00, TO_DATE('2020-10-20', 'YYYY-MM-DD')),

(11, 'Daniel Rodriguez', 16, 5, 60000.00, TO_DATE('2020-11-10', 'YYYY-MM-DD')),

(12, 'Emily Hernandez', 16, 6, 65000.00, TO_DATE('2020-12-05', 'YYYY-MM-DD')),
-- additional inserts
(13, 'Sophia Martinez', 9, 3, 62000.00, TO_DATE('2021-01-10', 'YYYY-MM-DD')),
(14, 'James Anderson', 12, 4, 71000.00, TO_DATE('2021-02-15', 'YYYY-MM-DD')),
(15, 'Olivia Thompson', 13, 8, 69000.00, TO_DATE('2021-03-20', 'YYYY-MM-DD')),
(16, 'Benjamin Clark', 15, 5, 67000.00, TO_DATE('2021-04-25', 'YYYY-MM-DD')),
(17, 'Charlotte Lewis', 16, 12, 64000.00, TO_DATE('2021-05-30', 'YYYY-MM-DD')),
(18, 'Liam Walker', 17, 9, 76000.00, TO_DATE('2021-06-10', 'YYYY-MM-DD')),
(19, 'Ethan Ramirez', 11, 7, 73000.00, TO_DATE('2021-07-15', 'YYYY-MM-DD')),
(20, 'Emily Williams', 17, NULL, 100000.00, TO_DATE('2020-12-15', 'YYYY-MM-DD'));
~~~~
TO_DATE function has been used to format hiring dates to avoid "invalid date inserted" error. A new employee with id 20 is added to handle "integrity constraint violation" error. It helps to make sure  manager with id 20 exists and can be referenced by employee who has manager with id 20. Missing employees (with ids 13, 14,15,16,17,18,19) added to ensure data integrity and completeness.This adjustment is made to guarantee that relationships between employees and managers remain consistent, and that queries depending on hierarchical structures return accurate results.

Result of query: 20 row(s) inserted.



## Running queries

1. List all employees with their salary and hire date, sorted by salary highest to lowest.

~~~~sql
SELECT EmpID, EmpName, Salary, HireDate
FROM Employee
ORDER BY Salary DESC;
~~~~

Result of query: 
![alt text](<images/image1.png>)
![alt text](<images/image2.png>)
20 rows returned.

2. List employees hired in 2020 during summer months (June, July, August).

~~~~sql
SELECT EmpID, EmpName, HireDate
FROM Employee
WHERE EXTRACT(YEAR FROM HireDate) = 2020
  AND EXTRACT(MONTH FROM HireDate) IN (6, 7, 8);
~~~~
Result of query:
![alt text](<images/image3.png>)
3 rows returned.

3. Show all departments under Engineering (including sub-departments).

~~~~sql
SELECT DeptID, DeptName, ParentDeptID
FROM Department
START WITH DeptName = 'Engineering'
CONNECT BY PRIOR DeptID = ParentDeptID
ORDER BY DeptID;
~~~~
Result of query: 
![alt text](<images/image4.png>)
7 rows returned.

4. Find employees who earn more than their managers.

~~~~sql
SELECT e.EmpID, e.EmpName, e.Salary AS EmployeeSalary, m.EmpName AS ManagerName, m.Salary AS ManagerSalary
FROM Employee e
JOIN Employee m ON e.ManagerID = m.EmpID
WHERE e.Salary > m.Salary;
~~~~
Result of query: 
![alt text](<images/image5.png>)
9 rows returned.


5. Find departments with more than 2 employees.

~~~~sql
SELECT d.DeptID, d.DeptName, COUNT(e.EmpID) AS EmployeeCount
FROM Employee e
JOIN Department d ON e.DeptID = d.DeptID
GROUP BY d.DeptID, d.DeptName
HAVING COUNT(e.EmpID) > 2;
~~~~
Result of query: 
![alt text](<images/image6.png>)
4 rows returned

6. List all employees in the Sales hierarchy (main Sales department and all regional sales departments).

~~~~sql
SELECT e.EmpID, e.EmpName, e.Salary, e.HireDate, d.DeptName
FROM Employee e
JOIN Department d ON e.DeptID = d.DeptID
WHERE d.DeptID = 5  -- Sales department
   OR d.ParentDeptID = 5  -- Sub-departments under Sales
ORDER BY e.EmpID;
~~~~
Result of query: 
![alt text](<images/image7.png>)
7 rows returned.

7. List employees and their department's parent department name.

~~~~sql
SELECT e.EmpID, e.EmpName, e.Salary, e.HireDate, d.DeptName AS DepartmentName, 
       p.DeptName AS ParentDeptName
FROM Employee e
JOIN Department d ON e.DeptID = d.DeptID
LEFT JOIN Department p ON d.ParentDeptID = p.DeptID
ORDER BY e.EmpID;
~~~~
Result of query: 
![alt text](<images/image8.png>)
![alt text](<images/image9.png>)
20 rows returned.

8. Show the highest paid employee in each department.

~~~~sql
SELECT  d.DeptName, e.EmpID, e.EmpName, e.Salary
FROM Employee e
JOIN Department d ON e.DeptID = d.DeptID
WHERE e.Salary = (
    SELECT MAX(Salary)
    FROM Employee
    WHERE DeptID = e.DeptID
)
ORDER BY d.DeptID;
~~~~
Result of query: 
![alt text](<images/image10.png>)
8 rows returned.

9. Find the most recently hired employee in each department.

~~~~sql
SELECT  d.DeptName, e.EmpID, e.EmpName, e.HireDate
FROM Employee e
JOIN Department d ON e.DeptID = d.DeptID
WHERE e.HireDate = (
    SELECT MAX(HireDate)
    FROM Employee
    WHERE DeptID = e.DeptID
)
ORDER BY d.DeptID;
~~~~
Result of query: 
![alt text](<images/image11.png>)
8 rows returned.

10. List employees who manage more people than their own manager does.

~~~~sql
SELECT e.EmpID, e.EmpName, e.Salary, e.HireDate, e.ManagerID
FROM Employee e
JOIN (
    SELECT ManagerID, COUNT(*) AS NumReports
    FROM Employee
    GROUP BY ManagerID
) m ON e.ManagerID = m.ManagerID
WHERE (
    SELECT COUNT(*) 
    FROM Employee sub
    WHERE sub.ManagerID = e.EmpID
) > m.NumReports
ORDER BY e.EmpID;
~~~~
Result of query: 
![alt text](<images/image12.png>)
2 rows returned.

11. Calculate the salary percentiles within each department.

~~~~sql
SELECT  e.DeptID, d.DeptName, e.EmpID, e.EmpName, e.Salary,
       PERCENT_RANK() OVER (PARTITION BY e.DeptID ORDER BY e.Salary) AS SalaryPercentile
FROM Employee e
JOIN Department d ON e.DeptID = d.DeptID
ORDER BY e.DeptID, SalaryPercentile DESC;
~~~~
Result of query: 
![alt text](<images/image13.png>)
![alt text](<images/image14.png>)
20 rows returned.

12. Find salary distribution by department quartiles.

~~~~sql
SELECT  e.DeptID, d.DeptName, e.EmpID, e.EmpName, e.Salary,
       NTILE(4) OVER (PARTITION BY e.DeptID ORDER BY e.Salary) AS SalaryQuartile
FROM Employee e
JOIN Department d ON e.DeptID = d.DeptID
ORDER BY e.DeptID, SalaryQuartile, e.Salary;
~~~~
Result of query: 
![alt text](<images/image15.png>)
![alt text](<images/image16.png>)
20 rows returned.


13. Show the management chain for each employee.

~~~~sql
WITH ManagementChain (EmpID, EmpName, ManagerID, DeptID, Salary, HireDate, ChainLevel) AS (
    SELECT e.EmpID, e.EmpName, e.ManagerID, e.DeptID, e.Salary, e.HireDate, 1 AS ChainLevel
    FROM Employee e
    WHERE e.ManagerID IS NULL

    UNION ALL

    SELECT e.EmpID, e.EmpName, e.ManagerID, e.DeptID, e.Salary, e.HireDate, mc.ChainLevel + 1
    FROM Employee e
    JOIN ManagementChain mc ON e.ManagerID = mc.EmpID
)

SELECT mc.EmpID, mc.EmpName, mc.Salary, mc.DeptID, mc.HireDate, mc.ChainLevel, e.EmpName AS ManagerName
FROM ManagementChain mc
LEFT JOIN Employee e ON mc.ManagerID = e.EmpID
ORDER BY mc.EmpID, mc.ChainLevel;
~~~~
Result of query: 
![alt text](<images/image17.png>)
![alt text](<images/image18.png>)
20 rows returned 


14. Find employees who earn more than all employees in Marketing.

~~~~sql
SELECT e.EmpID, e.EmpName, e.Salary, e.DeptID, e.HireDate
FROM Employee e
WHERE e.Salary > (
    SELECT MAX(e1.Salary)
    FROM Employee e1
    JOIN Department d ON e1.DeptID = d.DeptID
    WHERE d.DeptName = 'Marketing'
)
AND e.DeptID != (
    SELECT DeptID
    FROM Department
    WHERE DeptName = 'Marketing'
);

~~~~
Result of query: 
![alt text](<images/image19.png>)
No employee who earns more than all employees in Marketing department.

 15. Show the percentage each employee's salary contributes to their department's total salary.

~~~~sql
SELECT e.EmpID,
       e.EmpName,
       e.Salary,
       d.DeptName,
       total_dept_salary.total_salary AS dept_total_salary,
       ROUND((e.Salary / total_dept_salary.total_salary) ,3) * 100 AS salary_percentage
FROM Employee e
JOIN Department d ON e.DeptID = d.DeptID
JOIN (
    SELECT DeptID, SUM(Salary) AS total_salary
    FROM Employee
    GROUP BY DeptID
) total_dept_salary ON e.DeptID = total_dept_salary.DeptID
ORDER BY d.DeptName, e.Salary DESC;

~~~~
Result of query: 
![alt text](<images/image20.png>)
![alt text](<images/image21.png>)
20 rows returned.