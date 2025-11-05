0. Create Entity Diagram for table1 and table2 and share screenshot
uploded the screenshot 

#1. Prepare script to create table1 and table2 with primary key
CREATE TABLE departments (
  dept_id SERIAL PRIMARY KEY,
  dept_name VARCHAR(100) NOT NULL,
  location VARCHAR(100)
);


CREATE TABLE employees (
  emp_id SERIAL PRIMARY KEY,
  emp_name VARCHAR(100) NOT NULL,
  email VARCHAR(100) UNIQUE,
  salary DECIMAL(10,2),
  dept_id INT NOT NULL,
  hire_date DATE,
  FOREIGN KEY (dept_id) REFERENCES departments(dept_id)
);


#2. Prepare script to add foreign key constraint on any one table
ALTER TABLE employees ADD COLUMN manager_id INT;


ALTER TABLE employees 
ADD CONSTRAINT fk_manager 
FOREIGN KEY (manager_id) REFERENCES employees(emp_id);


#3. Prepare script to add unique constraint to any one column
ALTER TABLE employees 
ADD CONSTRAINT uk_email UNIQUE (email);


ALTER TABLE departments 
ADD CONSTRAINT uk_dept_name UNIQUE (dept_name);

#4. Prepare script to add index to any column
CREATE INDEX idx_emp_name ON employees(emp_name);

CREATE INDEX idx_emp_email ON employees(email);

CREATE INDEX idx_emp_salary ON employees(salary);

CREATE INDEX idx_emp_dept_id ON employees(dept_id);

CREATE INDEX idx_emp_hire_date_salary ON employees(hire_date, salary);

#5. Create insert queries to add around 4 to 8 rows in both the tables
INSERT INTO departments (dept_name, location) VALUES ('HR', 'New York');
INSERT INTO departments (dept_name, location) VALUES ('IT', 'San Francisco');
INSERT INTO departments (dept_name, location) VALUES ('Sales', 'Chicago');
INSERT INTO departments (dept_name, location) VALUES ('Finance', 'Boston');

INSERT INTO employees (emp_name, email, salary, dept_id, hire_date) VALUES ('Kumar Reddy', 'kumar@example.com', 50000, 2, '2023-01-15');
INSERT INTO employees (emp_name, email, salary, dept_id, hire_date) VALUES ('Priya Singh', 'priya@example.com', 55000, 2, '2023-03-20');
INSERT INTO employees (emp_name, email, salary, dept_id, hire_date) VALUES ('Amit Patel', 'amit@example.com', 45000, 3, '2022-06-10');
INSERT INTO employees (emp_name, email, salary, dept_id, hire_date) VALUES ('Neha Sharma', 'neha@example.com', 60000, 1, '2021-11-05');
INSERT INTO employees (emp_name, email, salary, dept_id, hire_date) VALUES ('Rajesh Kumar', 'rajesh@example.com', 52000, 4, '2023-02-14');
INSERT INTO employees (emp_name, email, salary, dept_id, hire_date) VALUES ('Ananya Gupta', 'ananya@example.com', 58000, 2, '2022-09-08');
INSERT INTO employees (emp_name, email, salary, dept_id, hire_date) VALUES ('Vikram Verma', 'vikram@example.com', 48000, 3, '2023-04-22');
INSERT INTO employees (emp_name, email, salary, dept_id, hire_date) VALUES ('Divya Iyer', 'divya@example.com', 62000, 1, '2021-08-30');

#6. Prepare a select query using WHERE, 'NOT IN', LIKE and ORDER BY clause
SELECT * FROM employees WHERE salary > 50000 ORDER BY salary DESC;

SELECT * FROM employees WHERE dept_id NOT IN (1, 3) ORDER BY emp_name;

SELECT * FROM employees WHERE emp_name LIKE '%Kumar%' ORDER BY hire_date;

SELECT emp_name, email, salary FROM employees WHERE salary BETWEEN 48000 AND 55000 ORDER BY salary DESC;

#7. Prepare a select query using GROUP BY and HAVING clause, with COUNT, SUM
SELECT dept_id, COUNT(*) as employee_count, SUM(salary) as total_salary, AVG(salary) as avg_salary FROM employees GROUP BY dept_id;

SELECT dept_id, COUNT(*) as employee_count FROM employees GROUP BY dept_id HAVING COUNT(*) > 1;

SELECT dept_id, COUNT(*) as employee_count, SUM(salary) as total_salary FROM employees GROUP BY dept_id HAVING SUM(salary) > 100000 ORDER BY total_salary DESC;

SELECT dept_id, AVG(salary) as avg_salary FROM employees GROUP BY dept_id HAVING AVG(salary) > 50000;

#8. Prepare a INNER JOIN query between table1 and table2
SELECT e.emp_id, e.emp_name, e.email, e.salary, d.dept_name, d.location FROM employees e INNER JOIN departments d ON e.dept_id = d.dept_id ORDER BY e.emp_name;

SELECT d.dept_name, COUNT(e.emp_id) as total_employees, SUM(e.salary) as total_salary FROM employees e INNER JOIN departments d ON e.dept_id = d.dept_id GROUP BY d.dept_name;

#9. Prepare LEFT JOIN query between table1 and table2
SELECT d.dept_id, d.dept_name, d.location, e.emp_name, e.salary FROM departments d LEFT JOIN employees e ON d.dept_id = e.dept_id ORDER BY d.dept_name;

SELECT d.dept_name, COUNT(e.emp_id) as employee_count FROM departments d LEFT JOIN employees e ON d.dept_id = e.dept_id GROUP BY d.dept_name ORDER BY employee_count DESC;

#10. Prepare 5 insert and update statements on table 1, with COMMIT and ROLLBACK in between queries.
BEGIN;

INSERT INTO employees (emp_name, email, salary, dept_id, hire_date) VALUES ('Mohit Singh', 'mohit@example.com', 51000, 2, '2024-01-10');

COMMIT;

BEGIN;

UPDATE employees SET salary = 52000 WHERE emp_id = 1;

ROLLBACK;

BEGIN;

UPDATE employees SET salary = 53000 WHERE emp_id = 2;

COMMIT;

BEGIN;

INSERT INTO employees (emp_name, email, salary, dept_id, hire_date) VALUES ('Sonal Sharma', 'sonal@example.com', 59000, 1, '2024-02-15');

UPDATE employees SET salary = 51500 WHERE emp_id = 3;

COMMIT;

BEGIN;

UPDATE employees SET salary = 63000 WHERE emp_id = 4;

ROLLBACK;

