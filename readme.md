# Metrodata Academy SIBKM DevSecOps - Task 1: SQL Queries

## Objectives
Create a database, import data tables into the database, and perform various queries from these three objective sections below:

### 1. Create Queries for:
- Displaying all data in the employee table. <br/>
- Displaying all data in the employee table with a salary greater than or equal to 7000.<br/>
- Displaying data in the employee table where the name (first name / last name) contains the character 'a', sorted by first_name.<br/>
- Displaying all data in the employee table for employees who started working before the year 2005, sorted by hire_date from the most recent.<br/>
- Displaying data for first_name as "Employee" and salary as "Monthly Salary" in the employee table for those with salaries between 5000 - 12000 and department_id 20 and 50.

### 2. Create Queries (Group Functions):
- Displaying the Maximum salary, Minimum salary, total salary, average employee salary (rounded to 2 decimal places), and group them by job_id.<br/>
- Displaying the highest and lowest salaries of employees working in the "Shipping" department.<br/>
- Displaying manager ID, manager name, and minimum salary for managers with a minimum employee salary greater than or equal to $6,000, ordered by the minimum employee salary.<br/>
- Displaying all employees who have a salary above the average employee salary.

### 3. Create Queries (Join) for:
- Displaying country.id, country.name, and region.name.<br/>
- Displaying employee.first_name, employee.last_name, job.title, and department.name.<br/>
- Displaying employee.first_name, employee.last_name (subordinate), and manager.first_name as "Manager" for each manager.

## Resolutions

### 1. Creating the Environment
**1.1 Create a Codespace Virtual Machine**
1. Visit the [Codespace webpage](https://github.com/codespaces).
2. Click "Use this template" to set up a project.

   <img src="_resources/Screenshot%202023-08-28%20at%2009.48.44.png" width="75%"/>

**1.2 Preparing the Database**
1. Install MySQL
   ```bash
   sudo apt update
   sudo apt install mysql-server
   sudo service mysql start
   ```
2. Upload the provided `.sql` file to the Codespace VM.

   <img src="_resources/Screenshot%202023-08-28%20at%2009.58.35.png" width="75%"/>

3. Connect to MySQL server
   ```bash
   sudo mysql -u root
   ```

4. Create a new database
   ```sql
   CREATE DATABASE HR;
   ```

5. Switch to the new database
   ```sql
   USE HR;
   ```

6. Import data using the uploaded `.sql` file
   ```sql
   source /path/to/data.sql;
   ```

### 2. Solving the Problems
**2.1 Solving the First Section of Objectives**
1. Display all data in the employee table
   ```sql
   SELECT * FROM employee;
   ```
   
   <img src="_resources/Screenshot%202023-08-28%20at%2010.40.26.png" width="75%"/>

2. Display data in the employee table with a salary greater than or equal to 7000
   ```sql
   SELECT * FROM employee 
   WHERE salary >= 7000;
   ```

   <img src="_resources/Screenshot%202023-08-28%20at%2010.41.48.png" width="75%"/>

3. Display data in the employee table where the name contains 'a', sorted by first_name
   ```sql
   SELECT * FROM employee 
   WHERE first_name LIKE '%a%' 
      OR last_name LIKE '%a%'
   ORDER BY first_name;
   ```
   
   <img src="_resources/Screenshot%202023-08-28%20at%2010.44.32.png" width="75%"/>

4. Display all data for employees who started before 2005, sorted by hire_date
   ```sql
   SELECT * FROM employee 
   WHERE YEAR(hire_date) < 2005
   ORDER BY hire_date DESC;
   ```

   <img src="_resources/Screenshot%202023-08-28%20at%2010.45.45.png" width="75%"/>

5. Display data with aliases for first_name as "Employee" and salary as "Monthly Salary" for specific conditions
   ```sql
   SELECT first_name AS 'Employee', 
      salary AS 'Monthly Salary'
   FROM employee
   WHERE salary BETWEEN 5000 AND 12000 
      AND department IN (20, 50);
   ```

   <img src="_resources/Screenshot%202023-08-28%20at%2010.57.59.png" width="75%"/>

**2.2 Solving the Second Section of Objectives**
1. Display aggregated salary information grouped by job_id
   ```sql
   SELECT job,
       ROUND(MAX(salary), 2) AS Maximum_Salary,
       ROUND(MIN(salary), 2) AS Minimum_Salary,
       ROUND(SUM(salary), 2) AS Total_Salary,
       ROUND(AVG(salary), 2) AS Average_Salary
   FROM employee
   GROUP BY job;
   ```

   <img src="_resources/Screenshot%202023-08-28%20at%2010.57.00.png" width="75%"/>

2. Display the highest and lowest salaries of "Shipping" department employees
   ```sql
   SELECT MAX(salary) AS Highest_Salary, MIN(salary) AS Lowest_Salary
   FROM employee
   WHERE department = (
       SELECT id
       FROM department
       WHERE name = 'Shipping'
   );
   ```

   <img src="_resources/Screenshot%202023-08-28%20at%2011.00.26.png" width="75%"/>

3. Display manager information with minimum employee salary constraints
   ```sql
   SELECT e.manager AS Manager_Id,
       CONCAT(m.first_name, ' ', m.last_name) AS Manager_Name,
       MIN(e.salary) AS Minimum_Salary
   FROM employee AS e
   JOIN employee AS m ON e.manager = m.id
   WHERE e.manager IS NOT NULL
   GROUP BY e.manager
   HAVING Minimum_Salary >= 6000
   ORDER BY Minimum_Salary;
   ```

   <img src="_resources/Screenshot%202023-08-28%20at%2011.05.18.png" width="75%"/>

4. Display employees with salary above average employee salary
   ```sql
   SELECT *
   FROM employee
   WHERE salary > (
 	SELECT AVG(salary)
       FROM employee
   );
   ```

   <img src="_resources/Screenshot%202023-08-28%20at%2011.44.21.png" width="75%"/>

**2.3 Solving the Third Section of Objectives**
1. Display country, country name, and region name
   ```sql
   SELECT c.id AS Country_Id, 
      c.name AS Country_Name, 
      r.name AS Region_Name
   FROM country AS c
   JOIN region AS r ON c.region = r.id;
   ```

   <img src="_resources/Screenshot%202023-08-28%20at%2011.50.49.png" width="75%"/>

2. Display employee information with job and department details
   ```sql
   SELECT e.first_name AS First_Name, 
      e.last_name AS Last_Name, 
      j.title Job_Title, 
      d.name AS Department
   FROM employee AS e
   JOIN job AS j ON e.job = j.id
   JOIN department AS d ON e.department = d.id;
   ```

   <img src="_resources/Screenshot%202023-08-28%20at%2011.57.21.png" width="75%"/>

3. Display employee names and their respective managers' names
   ```sql
   SELECT e.first_name AS First_Name,
       e.last_name AS Last_Name,
       m.first_name AS Manager
   FROM employee AS e
   LEFT JOIN employee AS m ON e.manager = m.id;
   ```

   <img src="_resources/Screenshot%202023-08-28%20at%2012.00.57.png" width="75%"/>
