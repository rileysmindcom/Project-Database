# Project Database

## 1. My Lab Objective
My objective of this lab is to utilize Java to automatically import vehicle fuel-efficiency statistic into a MySQL database creating a graphical user interface that allows the user to search, filter, and view stored data. Such as the SQL, java file processes, and GUI programming are all linked in this project.

## 2. Dataset Description
The Auto MPG dataset includes an actual vehicle specs of miles-per-galloon, number of cylinders, engine displacement, vehicle weight, horsepower, acceleration, model year, and automobile name, which was used in this study. Each line in the separated values of the TSV file is what contains the data that introduces a distinct car due to its characteristics.

## 3. SQL + Java Data Insertion
The program reads the auto MPG TSV file through each line, and inserts each record into the MySQL table named AutoMPG using the InsertData.java file. The file is processed through the Java's BufferedReader, and each line is prepared to subject an SQL insert statement. Which, guarantees that the data is secure, and can be imported into the database to enable the GUI to later access, and display data.

## 4. GUI Description
This allows the user to enter text input, and can search for the AutoMPG database using graphical user interface. This allows the application to execute an SQL select query, and shows all of the matching results for a text field when the user inputs the name of an automobile.

### Search by name
When we search by name the user inputs a complete automobile name, and the program retrieves these entries to whose car_name contains the input text using such as SQL query, and shows these results in the window of the application.

### Search “ALL”
The user can view each vehicle inside of the dataset by typing "ALL", causes the program to execute SELECT*FROM with an AutoMPG displaying the whole table.

### Query runs under the hood
Using the connection supplied by DBConnection.java, each search reveals an SQL query to MySQL. Which, the query executes in the background to display the GUI with results.

### JSliders for MPG and Horsepower
The interface enables users to select a minimum MPG, and minimum horsepower figure using sliders if JSliders are added. The slider values and application then runs a filtered SQL WHERE query, which displays all the cars that match in the results section.
