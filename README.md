# Employee Database with SQL

Building a PostgreSQL database from scratch and extracting data for analysis. Data Modeling, Data Engineering, 


---

## Business Problem


* Currently using VBA and want to replace it.
* Need a more roboust Database Management System.


---

## Executive Summary

* 90,398 employees will be retiring in the near future. The most representative positions are: 32% are Senior Engineers, 31% are Senior StaffEngineers, and 16% are Engineers.

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

<img src="https://github.com/NataliaVelasquez18/Employee-database/blob/main/png_images/uncheck_stack_builder.png" width="350" height="250" />


To confirm your installation, start pgAdmin (a new browser window will launch) and double-click to connect to the default server and enter your password.


<img src="https://github.com/NataliaVelasquez18/Employee-database/blob/main/png_images/confirm_pgadmin_installation.png" width="390" height="250" />




**Second**, download [Visual Studio Code](https://code.visualstudio.com/) for your operating system.


### Download files

Download the folders and files contained in this repository on your local machine except for the png_images folder


---


## Data Modeling


### Identifying Data Relationships

In the [database_creation_csv_files folder](https://github.com/NataliaVelasquez18/Employee-database/tree/main/database_creation_csv_files) there are 6 csv files with employee data such as [salaries](https://github.com/NataliaVelasquez18/Employee-database/blob/main/database_creation_csv_files/salaries.csv), [employees](https://github.com/NataliaVelasquez18/Employee-database/blob/main/database_creation_csv_files/employees.csv), [managers by department](https://github.com/NataliaVelasquez18/Employee-database/blob/main/database_creation_csv_files/dept_manager.csv), [employees by department](https://github.com/NataliaVelasquez18/Employee-database/blob/main/database_creation_csv_files/dept_emp.csv), [titles](https://github.com/NataliaVelasquez18/Employee-database/blob/main/database_creation_csv_files/titles.csv), and [departments](https://github.com/NataliaVelasquez18/Employee-database/blob/main/database_creation_csv_files/departments.csv). These files are the tables for our database.  


### Entity Relationship Diagrams (ERD)

In the [ERD_and_queries](https://github.com/NataliaVelasquez18/Employee-database/tree/main/ERD_and_queries) folder you can find the [Entity_Relationship_Diagram.sql](https://github.com/NataliaVelasquez18/Employee-database/blob/main/ERD_and_queries/Entity_Relationship_Diagram.sql) file. You will use Visual Studio Code to open the file and see the table relationships and data types of each column in our 6 tables.  

If you are a more of a visual person and would like to visualize the table relationships graphically before we create our database, go to [Quick DBD](https://www.quickdatabasediagrams.com/) website, click the "Try the App". You don't need to create an account to make your first diagram.  The next screen is the text editor which will have a sample ERD already in place.  Instead of altering the sample, we'll go ahead and delete the text in the text editor to clear the canvas for our own use.


<img src="https://github.com/NataliaVelasquez18/Employee-database/blob/main/png_images/empty_text_editor.png" width="250" height="250" />


Copy the content from the [Entity_Relationship_Diagram.sql](https://github.com/NataliaVelasquez18/Employee-database/blob/main/ERD_and_queries/Entity_Relationship_Diagram.sql) file and paste it on the text editor. The flow chart will update revealing the connections between the tables.

<img src="https://github.com/NataliaVelasquez18/Employee-database/blob/main/png_images/ERD.png" width="250" height="250" />


---


## Data Engineering


### Create a Database

**First**, launch pgAdminReturn to the pgAdmin window we opened earlier.  If you have closed your pgAdmin window, or shut down the program completely, you can open a new one by locating the pgAdmin icon and clicking it to start the software again.


**Second**, connect to the Server.  If you've been disconnected from your server, locate it in the menu to the left, then single-click the PostgreSQL (your version) server to initiate a connection. At this point, you will be prompted to enter the password you created during installation.  After connecting to the server, you should see that there is already a database named "postgres."


<img src="https://github.com/NataliaVelasquez18/Employee-database/blob/main/png_images/connect_to_server.png" width="350" height="150" />



This is the default database that is created when the pgAdmin and Postgres package was installed. Instead of using this database, you will create another one for this project.


**Third**, create a New Database


Right-click on "PostgreSQL your version." and create the database.


<img src="https://github.com/NataliaVelasquez18/Employee-database/blob/main/png_images/create_database.png" width="390" height="150" />


Name the database PH-EmployeeDB and click "Save".  A red X beside the new database's name indicates we aren't yet connected to it, but it is there and ready for use. Click on the new database to connect. 


<img src="https://github.com/NataliaVelasquez18/Employee-database/blob/main/png_images/connect_to_DB.png" width="300" height="140" />



### Create Tables in SQL


Looking back at the pgAdmin window, right-click on the database PH-EmployeeDB.  Then, from the dropdown menu, scroll down to the Query Tool and click to select.


<img src="https://github.com/NataliaVelasquez18/Employee-database/blob/main/png_images/query_tool.png" width="350" height="250" />



To create our 6 new tables, open the [schema.sql](https://github.com/NataliaVelasquez18/Employee-database/blob/main/ERD_and_queries/schema.sql) file using Visual Studio Code, copy all the information from the file and paste it on the Query Tool in pgAdmin. In the next block of code you can see an example of the first part of it.

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

<img src="https://github.com/NataliaVelasquez18/Employee-database/blob/main/png_images/check_tables.png" width="550" height="350" />


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

## Business Analysis


### Query Business Questions


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

<img src="https://github.com/NataliaVelasquez18/Employee-database/blob/main/png_images/Retiring_titles.png" width="250" height="250" />



