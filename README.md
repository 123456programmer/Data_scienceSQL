# Data_scienceSQL
This data_science project is about for a better understanding, managers have provided ratings for each employee which will help the HR department to finalize the employee performance mapping. As a DBA, one should find the maximum salary of the employees and ensure that all jobs are meeting the organization's profile standard. 

CREATE DATABASE employee;
USE employee;
SHOW DATABASES;
CREATE TABLE 'employee' . 'data_science_team'(
'EMP_ID' VARCHAR(45)NOT NULL,
'FIRST_NAME' VARCHAR(45)NOT NULL,
'LAST_NAME' VARCHAR(45)NOT NULL,
'GENDER' VARCHAR(45)NOT NULL,
'ROLE' VARCHAR(45)NOT NULL,
'DEPT' VARCHAR(45)NOT NULL,
'EXP' INT(45)NULL,
'COUNTRY' VARCHAR(45)NOT NULL,
'CONTINENT' VARCHAR(45)NOT NULL,
PRIMARY KEY ('EMP_ID'));
LOAD DATA INFILE
IMPORT CSV
'C:/ProgramData/MYSQL/MYSQL Server 8.0/Uploads/data_science_team.csv' 
ignore 1 lines
INTO TABLE data_science_team
FIELDS TERMINATED BY ','
SELECT * FROM data_science_team 
CREATE TABLE emp_record_table
CREATE TABLE 'employee'.'emp_record_table'(
'EMP_ID' VARCHAR(50)NOT NULL,
'FIRST_NAME' VARCHAR(45)NOT NULL,
'LAST_NAME' VARCHAR(45)NOT NULL,
'GENDER' VARCHAR(45)NOT NULL,
'ROLE' VARCHAR(45)NOT NULL,
'DEPT' VARCHAR(45)NOT NULL,
'CONTINENT' VARCHAR(45)NOT NULL,
'SALARY' INT(45)NOT NULL, 
'EMP_RATING' INT(45)NOT NULL,
'MANAGER_ID' VARCHAR(45)NOT NULL,
'PROJ_ID' VARCHAR(45)NOT NULL,
'PRIMARY KEY' ('EMP_ID'));
LOAD DATA INFILE 
'C:/ProgramData/MYSQL/MYSQL Server8.0/Uploads/emp_record_table.csv' 
INTO TABLE emp_record_table
FIELDS TERMINATED BY','
ignore 1 lines;
SELECT * FROM emp_record_table 
 
CREATE TABLE 'employee'.'proj_table' (
'PROJECT_ID' VARCHAR(45)NOT NULL, 
'PROJ_NAME' VARCHAR(45)NULL,
'DOMAIN' VARCHAR(45)NULL,
'START_DATE' DATE OF NULL,
'CLOSURE_DATE' DATE NOT NULL,
'DEV_QTR' VARCHAR(45)NULL, 
'STATUS' VARCHAR(45)NULL, 
PRIMARY KEY ('PROJ_ID')); 
SELECT * FROM PROJ_TABLE 
ALTER TABLE data_science_team ADD FOREIGN KEY (Emp_ID) References
emp_record_table(Emp_ID)  
SELECT emp_id,First_Name,Last_Name,Gender Dept emp_record_table;
SELECT emp_id,First_Name,Last_Name,Gender,Dept EMP_RATING FROM emp_record_table where EMP_RATING<2;
SELECT emp_id,First_Name,Last_Name, Gender, Dept, EMP_RATING FROM emp_record_table where EMP_RATING>4; 
SELECT emp_id,First_name,Last_name, Gender, Dept, EMP_RATING FROM emp_record_table where EMP_RATING between 2 And 4;
SELECT CONCAT(first_name,'',last_name)AS NAME 
FROM emp_record_table
WHERE Dept='FINANCE';
SELECT role, manager_ID, COUNT(*) AS employee_count
FROM emp_record_table
GROUP BY role, manager_ID;
SELECT first_name, last_name, dept
FROM emp_record_table 
WHERE Dept = 'HEALTHCARE'
UNION 
SELECT first_name, last_name, dept
FROM emp_record_table
WHERE Dept = 'FINANCE';
SELECT emp_id, first_name, last_name, role, dept, emp_rating 
FROM emp_record_table
WHERE (dept, emp_rating) IN (
SELECT Dept, MAX(emp_rating) 
FROM emp_record_table 
GROUP BY Dept
)
ORDER BY Dept ASC;
SELECT role, MIN(salary) AS min_Salary, MAX(salary) AS max_Salary
FROM emp_record_table
GROUP BY role;
SELECT first_name, last_name, exp as experience
DENSE_RANK OVER (ORDER BY exp DESC) exp_rank
FROM emp_record_table;
CREATE VIEW 6K_salary AS 
SELECT emp_table, first_name, last_name, country, salary
FROM emp_record_table
WHERE salary > 6000;
SELECT * FROM 6K_salary;
#Nested Query for Employees with Experience of more than 10 years 
SELECT emp_id, first_name, last_name, exp
FROM emp_record_table
WHERE exp IN (
SELECT exp
FROM emp_record_table 
WHERE exp > 10
);
# Query to create stored procedure to retrieve details of the employees whose experience is more than 3 years 
DELIMITER //
Create PROCEDURE Employee3()
BEGIN 
SELECT * FROM emp_record_table 
WHERE exp > 3;
END //
DELIMITER;
Call Employee3; 
DELIMETER //
CREATE PROCEDURE check_role6() 
BEGIN 
DECLARE exp_val INT;
DECLARE role_val VARCHAR(255);
DECLARE done INT DEFAULT FALSE;
DECLARE cur CURSOR FOR SELECT exp FROM data_science_team;
DECCLARE CONTINUE HANDLER FOR NOT FOUND SET done = True;
OPEN cur;
read_loop: LOOP
FETCH cur into exp_val;
IF done THEN 
LEAVE read_loop;
END IF;
CASE 
WHEN exp_val <=2 THEN SET role_val = 'JUNIOR DATA SCIENTIST';
WHEN exp_val BETWEEN 3 AND 5 THEN SET role_val = 'ASSOCIATE DATA SCIENTIST';
WHEN exp_val BETWEEN 6 AND 10 THEN SET role_val = 'SENIOR DATA SCIENTIST';
WHEN exp_val BETWEEN 11 AND 12 THEN SET role_val = 'LEAD DATA SCIENTIST';
WHEN exp_val BETWEEN 13 AND 16 THEN SET role_val = 'MANAGER';
ELSE SET role_val = 'all good';
END CASE;
__UPDATE emp_record_table SET role_column = role_val WHERE
exp_column = exp_val;
SELECT role_val;
END LOOP;
CLOSE cur;
END //
DELIMITER;
call check_role6();
#Creating an Index to improve cost and performance of query to find employee whose First_Name is 'Adam' in employee table
ALTER TABLE emp_record_table ADD INDEX name_index (first_name);
SELECT * FROM emp_record_table WHERE first_name = 'Adam';
#	Query for calculating Bonus for all the employees, based on rating and salaries (Using the formula: 5% of salary * employee rating) 
SELECT first_name, last_name, salary, ((salary * .05)*emp_rating) AS bonus 
FROM emp_record_table;
#Average salary Distribution based on continent and country 
SELECT continent, AVG(salary)
FROM emp_record_table
GROUP BY continent 
ORDER BY continent ASC;
#Average salary distribution based on the country 
SELECT country, AVG(salary)
FROM emp_record_table
GROUP BY country 
ORDER BY country ASC;












