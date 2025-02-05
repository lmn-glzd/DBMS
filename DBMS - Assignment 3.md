# Database Management Systems - Assignment 3


## Table of Contents
* [Running Queries](#running-queries)



## Running queries
1. Got aware of typo in my initial table creation. Previously, I have inserted "Content Strategy" department under "Marketing" department, while not being aware of parent id of Marketing department was dept id of "Content Strategy", which is not possible in reality. Therefore, updated the parent of "Marketing" department to "Business" which can include "Marketing", "Sales" and "Customer Support" as its subdepartments.
~~~sql
UPDATE department
SET DEPTNAME = 'Business' , PARENTDEPTID = NULL
WHERE DEPTID = 17;
~~~
Result of query: 1 row(s) updated.

"Marketing" department record has not been changed and still has the parent id 17 to maintain provided data by instructor. It now moves under "Business Strategy" department which is the top department in its hierarcy and therefore, parent id is set to NULL.

2. Find all employees who earn more than $70,000.

~~~~sql
SELECT EmpID, EmpName, Salary
FROM Employee
WHERE Salary > 70000;
~~~~

Result of query: 
![alt text](<images 2/2.png>)
8 rows returned.

4. List all departments and their parent department names.

~~~~sql
SELECT d.DeptID, d.DeptName, p.DeptName AS ParentDeptName
FROM Department d
LEFT JOIN Department p ON d.ParentDeptID = p.DeptID;
~~~~
Result of query:
![alt text](<images 2/4.1.png>)
![alt text](<images 2/4.2.png>)
17 rows returned.

6. List each employee and their manager's name.

~~~~sql
SELECT e.EmpID, e.EmpName, e.Salary, m.EmpName AS ManagerName
FROM Employee e
LEFT JOIN Employee m ON e.ManagerID = m.EmpID;
~~~~
Result of query: 
![alt text](<images 2/6.1.png>)
![alt text](<images 2/6.2.png>)
20 rows returned.

8. Show the average salary by department.

~~~~sql
SELECT d.DeptID, d.DeptName, ROUND(AVG(e.Salary), 2) AS AvgSalary
FROM Employee e
JOIN Department d ON e.DeptID = d.DeptID
GROUP BY d.DeptID, d.DeptName
ORDER BY AvgSalary DESC;

~~~~
Result of query: 
![alt text](<images 2/8.png>)
8 rows returned.


10. Calculate the salary range (difference between highest and lowest salary) for each department.

~~~~sql
SELECT d.DeptID, d.DeptName,
       ROUND(MAX(e.Salary) - MIN(e.Salary), 2) AS SalaryRange
FROM Department d
LEFT JOIN Employee e ON d.DeptID = e.DeptID
GROUP BY d.DeptID, d.DeptName
ORDER BY SalaryRange DESC;
~~~~
Result of query: 
![alt text](<images 2/10.png>)
8 rows returned

12. Show departments that have no employees.

~~~~sql
SELECT d.DeptID, d.DeptName
FROM Department d
LEFT JOIN Employee e ON d.DeptID = e.DeptID
WHERE e.EmpID IS NULL;
~~~~
Result of query: 
![alt text](<images 2/12.png>)
9 rows returned.

14.Find employees who earn more than the average salary in their department.

~~~~sql
SELECT e.EmpID, e.EmpName, e.Salary, e.DeptID
FROM Employee e
JOIN (
    SELECT DeptID, AVG(Salary) AS AvgSalary
    FROM Employee
    GROUP BY DeptID
) avg_salary ON e.DeptID = avg_salary.DeptID
WHERE e.Salary > avg_salary.AvgSalary;
~~~~
Result of query: 
![alt text](<images 2/14.png>)
10 rows returned.

16. List employees hired in each quarter of 2020, with their department.

~~~~sql
SELECT e.EmpID, e.EmpName, e.HireDate, e.DeptID, d.DeptName,
       CASE 
           WHEN EXTRACT(MONTH FROM e.HireDate) BETWEEN 1 AND 3 THEN 'Q1'
           WHEN EXTRACT(MONTH FROM e.HireDate) BETWEEN 4 AND 6 THEN 'Q2'
           WHEN EXTRACT(MONTH FROM e.HireDate) BETWEEN 7 AND 9 THEN 'Q3'
           WHEN EXTRACT(MONTH FROM e.HireDate) BETWEEN 10 AND 12 THEN 'Q4'
       END AS Quarter
FROM Employee e
JOIN Department d ON e.DeptID = d.DeptID
WHERE EXTRACT(YEAR FROM e.HireDate) = 2020
ORDER BY Quarter, e.HireDate;
~~~~
Result of query: 
![alt text](<images 2/16.png>)
13 rows returned.

18.Show departments with their average employee tenure in months.

Note: Records with empty tenure months correspond to departments with no direct employee. Since the question statement asks to "display departments with their average employee tenure", all departments are intentionally displayed first to match with possible tenure period. The ones who don't have employee shown with no tenure period. If intention was to show the tenure duration for each department who has at least one employee, we would simply replace "left join" with inner "join" to simply get only non null values for departments.

~~~~sql
SELECT d.DeptID, d.DeptName,
       AVG(MONTHS_BETWEEN(CURRENT_DATE, e.HireDate)) AS AvgTenureMonths
FROM Department d
LEFT JOIN Employee e ON d.DeptID = e.DeptID
GROUP BY d.DeptID, d.DeptName;

~~~~
Result of query: 
![alt text](<images 2/18.1.png>)
![alt text](<images 2/18.2.png>)
17 rows returned.

20. Show the department hierarchy as a tree structure.

~~~~sql
SELECT DeptID, 
       DeptName, 
       LEVEL AS DeptLevel, 
       SYS_CONNECT_BY_PATH(DeptName, ' -> ') AS Hierarchy
FROM Department
START WITH ParentDeptID IS NULL 
CONNECT BY PRIOR DeptID = ParentDeptID
ORDER SIBLINGS BY DeptName;
~~~~
Result of query: 
![alt text](<images 2/20.1.png>)
![alt text](<images 2/20.2.png>)
17 rows returned.
