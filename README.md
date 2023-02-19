# SQL-Projects
1.	Display the Department id, name and id and the name of its manager.

SELECT Dname, Dnum, MGRSSN, CONCAT (Fname, ' ', Lname) AS FullName 
FROM Employee E, Departments D
WHERE D.MGRSSN = E.SSN

2.	Display the name of the departments and the name of the projects under its control.

SELECT Pname, Dname
FROM Project P, Departments D
WHERE P.DNUM = D.DNUM

3.	Display the full data about all the dependence associated with the name of the employee they depend on him/her.

SELECT DEPENDENT_NAME, CONCAT (Fname, ' ', Lname) 
FROM Dependent D, Employee E
WHERE D.ESSN = E.SSN

4.	Display (Using Union Function)
a.	 The name and the gender of the dependence that's gender is Female and depending on Female Employee.
b.	 And the male dependence that depends on Male Employee.

SELECT D.Dependent_name, D.sex, E.Fname FROM Dependent D INNER JOIN Employee E
ON D.essn = E.ssn
WHERE D.sex = 'F' and E.sex= 'F'
UNION
SELECT D.Dependent_name, D.sex, E.Fname FROM Dependent D INNER JOIN Employee E
ON D.essn = E.ssn
WHERE D.sex = 'M' and E.sex= 'M'

5.	Display the Id, name and location of the projects in Cairo or Alex city.

SELECT Pname, Pnumber, Plocation FROM Project
WHERE City = 'Cairo' OR City = 'Alex'

6.	Display the Projects full data of the projects with a name starts with "a" letter.

SELECT * FROM Project
WHERE Pname LIKE 'A%';

7.	display all the employees in department 30 whose salary from 1000 to 2000 LE monthly

SELECT Fname FROM Employee
WHERE Dno = 30 AND 2000>Salary AND Salary>1000

8.	Retrieve the names of all employees in department 10 who works more than or equal 10 hours per week on "AL Rabwah" project.

SELECT Fname
FROM Employee
JOIN Project ON (Project.Dnum = Employee.Dno)
JOIN Works_for ON (Works_for.Essn = Employee.SSN)
WHERE Employee.Dno = 10 AND Works_for.Hours >=10 AND Pname = 'Al Rabwah'

9.	Find the names of the employees who directly supervised with Kamel Mohamed.

SELECT CONCAT(E.Fname,' ', E.LNAME) Employee_Name , CONCAT(S.Fname,' ', S.Lname) Supervisor_Name 
FROM Employee E, Employee S 
WHERE E.SuperSSN = S.SSN AND S.Fname = 'Kamel'

10.	For each project, list the project name and the total hours per week (for all employees) spent on that project.

SELECT Pname, SUM(Hours)
FROM Employee
JOIN Project ON (Project.Dnum = Employee.Dno)
JOIN Works_for ON (Works_for.Essn = Employee.SSN)
GROUP BY Project.Pname

11.	Retrieve the names of all employees and the names of the projects they are working on, sorted by the project name.

SELECT Fname, Pname
FROM Employee
JOIN Project ON (Project.Dnum = Employee.Dno)
order BY Project.Pname

12.	Display the data of the department which has the smallest employee ID over all employees' ID.

SELECT Departments.* FROM Departments INNER JOIN Employee ON Departments.Dnum = Employee.Dno
AND Employee.SSN <=ALL(SELECT MIN(SSN) FROM Employee);

13.	For each department, retrieve the department name and the maximum, minimum and average salary of its employees.

SELECT D.Dname, MIN(E.Salary), MAX(Salary), AVG(Salary)
FROM Departments D INNER JOIN Employee E
ON D.Dnum = E.Dno
GROUP BY D.Dname, D.Dnum

14.	List the last name of all managers who have no dependents.

SELECT E.Fname from Employee E 
where E.SSN in 
(SELECT E.Superssn
from Dependent D FULL JOIN Employee E
on D.Essn = E.Superssn
WHERE D.Essn IS NULL)

15.	For each department-- if its average salary is less than the average salary of all employees-- display its number, name and number of its employees.

SELECT Departments.Dname, Departments.Dnum, COUNT(E.SSN) AS Employee_Count 
FROM Departments INNER JOIN Employee E
ON Departments.Dnum = E.Dno
GROUP BY Departments.Dname, Departments.Dnum
HAVING AVG(E.Salary)< SELECT AVG(salary) FROM Employee

16.	Retrieve a list of employees and the projects they are working on ordered by department and within each department, ordered alphabetically by last name, first name.

SELECT E.Dno ,E.Fname,E.Lname, P.Pname 
FROM Employee e INNER JOIN Project P
ON E.Dno = P.Dnum 
ORDER BY E.Dno, E.Fname, E.Lname;

17.	For each project located in Cairo City , find the project number, the controlling department name ,the department manager last name ,address and birthdate.

SELECT P.Pnumber, D.Dname, E.Lname, E.Address, E.Bdate 
FROM Project P JOIN Departments D ON P.Dnum = D.Dnum 
JOIN Employee E ON P.Dnum = E.Dno
WHERE P.City = 'Cairo'
