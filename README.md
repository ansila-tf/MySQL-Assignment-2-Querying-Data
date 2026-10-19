MySQL Assignment 2 – Employee Database Queries

Overview

This assignment practices SQL queries using an employee database. It
covers retrieving unique values, column aliases, filtering, sorting,
aggregate functions, grouping, having, and joins across related
tables.

Database Tables

The queries use these tables:

• employees – employee details such as name, gender, age, hire
date, designation, department ID, location ID, and salary.
• departments – department IDs and department names.
• location – location IDs and location names.

Topics and SQL Queries

1. Distinct Values

Task: Retrieve distinct salaries from the employees table.

select distinct salary
from employees;

2. Column Aliases

Task: Display age as employee_age and salary as
employee_salary.

select
    age as employee_age,
    salary as employee_salary
from employees;

3. Where Clause and Operators

Task 1: Retrieve employees with a salary greater than 50000 and
hired before 2016-01-01.

select *
from employees
where salary > 50000
  and hire_date < '2016-01-01';

Task 2: Find the employee whose designation is missing and fill it
with Data Scientist.

First, find rows with a missing designation:

select *
from employees
where designation is null;

Then update the employee. In this dataset, the employee ID is 5004:

update employees
set designation = 'Data Scientist'
where employee_id = 5004;

Verify the update:

select *
from employees
where employee_id = 5004;

> Run the `update` statement only if the designation is still missing.

4. Sorting and Limiting Results

Task 1: Sort employees by department ID in ascending order and
salary in descending order.

select *
from employees
order by department_id asc, salary desc;

Task 2: Display the first five employees hired in 2018, ordered by
hire date.

select *
from employees
where year(hire_date) = 2018
order by hire_date asc
limit 5;

5. Aggregate Functions

Task 1: Calculate the total salary of employees in the Finance
department.

select sum(employees.salary) as total_finance_salary
from employees
inner join departments
on employees.department_id = departments.department_id
where departments.department_name = 'Finance';

Task 2: Find the minimum age among all employees.

select min(age) as minimum_age
from employees;

6. Group By

Task 1: Find the maximum salary in each location.

select location.location, max(employees.salary) as maximum_salary
from employees
inner join location
on employees.location_id = location.location_id
group by location.location;

Task 2: Find the average salary for each designation containing
Analyst.

select designation, avg(salary) as average_salary
from employees
where designation like '%Analyst%'
group by designation;

7. Having

Task 1: Find departments with fewer than three employees.

select department_id, count(*) as employee_count
from employees
group by department_id
having count(*) < 3;

> This query returns departments that have at least one employee and
> fewer than three employees. To include departments with zero
> employees, use a `left join`.

Task 2: Find locations where the average age of female employees is
below 30.

select location_id, avg(age) as average_age
from employees
where gender = 'F'
group by location_id
having avg(age) < 30;

8. Joins

8.1 Inner Join

Task: List employee names, designations, and department names for
employees assigned to a department.

select employees.employee_name,
       employees.designation,
       departments.department_name
from employees
inner join departments
on employees.department_id = departments.department_id;

8.2 Left Join

Task: List all departments and the number of employees in each
department, including departments with no employees.

select departments.department_id,
       departments.department_name,
       count(employees.employee_id) as total_employees
from departments
left join employees
on departments.department_id = employees.department_id
group by departments.department_id, departments.department_name;

8.3 Right Join

Task: Display all locations with the names of employees assigned to
each location. Show NULL for employee names when a location has no
employees.

select location.location,
       employees.employee_name
from employees
right join location
on employees.location_id = location.location_id;

Key Concepts Learned

• distinct removes duplicate values from query results.
• as assigns a readable alias to a column.
• where filters rows before grouping.
• order by sorts results.
• limit restricts the number of rows returned.
• Aggregate functions such as sum(), min(), max(), avg(), and
count() summarize data.
• group by groups rows with matching values.
• having filters grouped results.
• inner join, left join, and right join combine related tables.

