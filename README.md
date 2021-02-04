# Employee Database with SQL

A database built in SQL using PostgreSQL. 


---


## Getting Started

These instructions will get your PostgreSQL database up and running on your local machine.


### Prerequisites

Before the installations there are some important concepts you need to know:  

**PostgreSQL**, typically referred to as just "Postgres," is a relational database system. This type of database consists of tables and their predefined relationships. 

**pgAdmin** is the window into our database: it's where queries are written and executed and where results are viewed. While Postgres holds the files, pgAdmin provides the access. All SQL actions take place within these two programs, so let's install them.

**Visual Studio Code** is a free source-code editor made by Microsoft for Windows, Linux and macOS. 



### Installing

**First**, visit the **PostgresSQL** [download website](https://www.enterprisedb.com/downloads/postgres-postgresql-downloads) - The web framework used to initiate your download. Be sure to choose the correct download option for your operating system. Both Postgres and pgAdmin are downloaded together as a package. Be sure to Not select the latest Postgres version, we're installing a previous version to the latest because it is a more stable release.  During installation, you'll need to create a password. Be sure to record it, as you'll use it to access your SQL database.  

An InstallBuilder window will show the components selected for installation. Be sure to uncheck Stack Builder's box. Stack Builder is used to install Postgres add-ons, but we won't need it for our project.

<img src=""width="250" height="250" />

<img src=""width="250" height="250" />

To confirm your installation, start pgAdmin (a new browser window will launch) and double-click to connect to the default server and enter your password.


%%%%%confirm installation



**Second**, download [Visual Studio Code](https://code.visualstudio.com/) for your operating system.


### Download files

Download the folders and files contained in this repository on your local machine except for the png_images folder


---


## Data Modeling


### Identifying Data Relationships

In the database_creation_csv_files folder there are 6 csv files with employee data such as salaries, employees, managers by department, employees by department, titles, and departments. These files will be our tables for our database.  Feel free to open and examine the tables to get familiar with the data.


### Entity Relationship Diagrams (ERD)

In the ERD_and_queries folder you can find the Entity_Relationship_Diagram.sql file. You will use Visual Studio Code to open the file and see the table relationships and data types of each column in our 6 tables.  

If you are a more of a visual person and would like to visualize the table relationships graphically before we create our database, go to [Quick DBD](https://www.quickdatabasediagrams.com/) website, click the "Try the App" button shown in the following image to get started. You don't need to create an account to make your first diagram.

try app image


The next screen is the text editor. Because it's your first visit, the text editor will have a sample ERD already in place, plus an option to tour the app and accept the website's cookies. Click the "Got it!" button to dismiss the message about cookies and go ahead and skip the tour.

skip tour image


Instead of altering the sample, we'll go ahead and delete the text in the text editor to clear the canvas for our own use.

%%%% empty tex editor


Copy the content of the Entity_Relationship_Diagram.sql file and paste it on the text editor. The flow chart will update revealing the connections between the tables.

flow chart image

---


## Data Engineering


### Create a Database

**First**, launch pgAdminReturn to the pgAdmin window we opened earlier.  If you have closed your pgAdmin window, or shut down the program completely, you can open a new one by locating the pgAdmin icon and clicking it to start the software again.


**Second**, connect to the Server

If you've been disconnected from your server, locate it in the menu to the left, then single-click the PostgreSQL (your version) server to initiate a connection. At this point, you will be prompted to enter the password you created during installation.  After connecting to the server, you should see that there is already a database named "postgres."


image conect toserver


This is the default database that is created when the pgAdmin and Postgres package was installed. Instead of using this database, you will create another one for this project.


**Third**, create a New Database


Right-click on "PostgreSQL your version." and create the database.


create database image


Name the database PH-EmployeeDB and click "Save".


save database image


A red X beside the new database's name indicates we aren't yet connected to it, but it is there and ready for use. Click on the new database to connect. 


connect to DB image



### Create Tables in SQL


Looking back at the pgAdmin window, right-click on the database PH-EmployeeDB.  Then, from the dropdown menu, scroll down to the Query Tool and click to select.


Query tool image


To create our 6 new tables, open the schema.sql file using Visual Studio Code, copy all the information from the file and paste it on the Query Tool in pgAdmin. In the next block of code you can see an example of the first part of it.

```
-- Creating tables for PH-EmployeeDB
CREATE TABLE departments (
     dept_no VARCHAR(4) NOT NULL,
     dept_name VARCHAR(40) NOT NULL,
     PRIMARY KEY (dept_no),
     UNIQUE (dept_name));
```

Then execute code.  In the toolbar of the pgAdmin webpage, hover over the different icons and find the icon for execute/refresh and click it. This button runs the code and saves our work to the database.


### Import Data


In the pgAdmin window, select the dropdown menu for our PH-EmployeeDB database. To import data into the tables, first confirm all of our tables are listed.  If you are unable to see all the tables, right click on "Tables" and then click "Refresh".


check tables image

To import a CSV into Postgres with pgAdmin, follow these steps. 

1. Right-click the first table, departments.
2. From the menu that pops up, scroll to Import/Export. 
3. Toggle the button to show "Import." 
4. Click the ellipsis on the Filename field to search for your project folder.
5. Select departments.csv. Make sure Format is set to "csv" and Encoding is blank. 
6. Leave the OID field as is, but toggle the Header field to "Yes" and select the comma as the Delimiter. 
7. Click OK to begin importing the data. 
8. If the import is successful, a pop-up window will appear at the bottom of your pgAdmin page 




---

## Query Business Questions


Retrieve the employees who are retiring with their name, last name, title, dates of employment and order them by employee number.

```
--CREATE RETIREMENT TITLES TABLE

SELECT e.emp_no,
e.first_name,
e.last_name,
ti.title,
ti.from_date,
ti.to_date
INTO retirement_titles
FROM employees AS e
INNER JOIN titles AS ti
ON (e.emp_no = ti.emp_no)
WHERE (e.birth_date BETWEEN '1952-01-01' AND '1955-12-31')
ORDER BY 
e.emp_no ASC;
```


In the last table we can see there are employees who appear more than once.  That is because some employees have been promoted and have changed titles.  We need to retrieve the most recent titles and make sure employees only appear once.


```
-- Use Dictinct with Orderby to remove duplicate rows
SELECT DISTINCT ON (emp_no) emp_no,
first_name,
last_name,
title
INTO unique_titles
FROM retirement_titles
ORDER BY emp_no ASC, to_date DESC;
```

How many employees are about to retire by title?

```
SELECT * FROM unique_titles;

--Number of employees by title who are about to retire
SELECT COUNT(emp_no),
title
INTO retiring_titles
FROM unique_titles
GROUP BY title
ORDER BY COUNT(emp_no) DESC;
```

Retrieve first name, last name, birth date, employed since date and title of employees currently employed at the company born between 01/01/1965 and 12/31/1965.


```
-- Employees elegible for mentorship program

SELECT DISTINCT ON (emp_no) e.emp_no,
e.first_name,
e.last_name,
e.birth_date,
de.from_date,
de.to_date,
ti.title
INTO mentorship_elegibilty
FROM employees AS e
INNER JOIN dept_emp AS de
ON (e.emp_no = de.emp_no)
INNER JOIN titles AS ti
ON (e.emp_no = ti.emp_no)
WHERE (de.to_date = '9999-01-01') AND (e.birth_date BETWEEN '1965-01-01' AND '1965-12-31')
ORDER BY e.emp_no;
```


Retrieve first name, last name, birth date, employed since date and title of employees currently employed at the company born between 01/01/1965 and 12/31/1965 grouped by their title.


```
-- mentorship eligibility per tittle

SELECT COUNT(emp_no),
title
INTO mentorship_elegibilty_per_title
FROM mentorship_elegibilty
GROUP BY title
ORDER BY COUNT(emp_no) DESC;
```

---

## Contributing

Please read [CONTRIBUTING.md](https://gist.github.com/PurpleBooth/b24679402957c63ec426) for details on our code of conduct, and the process for submitting pull requests to us.

---

## Acknowledgments

* Hat tip to anyone whose code was used
* Inspiration
* etc

---

## Author

Contact:
Email:
Twitter

---
## ANALYSIS AND RESULTS
**1. Roles to be filled:**

<img src="https://github.com/nativelasquez-austin/Pewlett_Hackard/blob/main/Resources/Retiring_titles.png" width="250" height="250" />

**2. Mentorship program:**

<img src="https://github.com/nativelasquez-austin/Pewlett_Hackard/blob/main/Resources/mentorship_eligibility_per_title.png" width="250" height="250" />



<table>
  <tr>
    <td>Mentors Needed</td>
     <td>Mentors Available</td>
  </tr>
  <tr>
    <td><img src="https://github.com/nativelasquez-austin/Pewlett_Hackard/blob/main/Resources/Retiring_titles.png" width=350 height=350></td>
    <td><img src="https://github.com/nativelasquez-austin/Pewlett_Hackard/blob/main/Resources/mentorship_eligibility_per_title.png" width=350 height=350></td>
  </tr>
 </table>
 


