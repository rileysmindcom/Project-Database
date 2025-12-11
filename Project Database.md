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

### My Flowchart:
![Project Database](https://github.com/user-attachments/assets/d0c09b52-8e07-4211-9e00-25ebb86962b7)

### My Project Code:
*DBConnection.java*
```
import java.sql.*;

public class DBConnection {
    public static Connection getConnection() {
        String url = "jdbc:mysql://localhost:3306/Auto";
        String user = "root";
        String pass = "MyLab7865";

        try {
            return DriverManager.getConnection(url, user, pass);
        } catch (Exception e) {
            e.printStackTrace();
            return null;
        }
    }
}
```
*AutoGUI.java*
```
import javax.swing.*;
import java.awt.*;
import java.awt.event.*;
import java.sql.*;

public class AutoGUI {
    public static void main(String[] args) {
        JFrame frame = new JFrame("Auto MPG Search");
        frame.setSize(500, 200);
        frame.setLayout(new FlowLayout());

        JTextField input = new JTextField(20);
        JButton searchButton = new JButton("Search");

        JTextArea output = new JTextArea(20, 40);
        JScrollPane scroll = new JScrollPane(output);

        frame.add(new JLabel("Enter car name or 'ALL':"));
        frame.add(input);
        frame.add(searchButton);
        frame.add(scroll);

        searchButton.addActionListener(e -> {
            String text = input.getText();
            output.setText("");

            String query = text.equalsIgnoreCase("ALL") ?
                    "SELECT * FROM AutoMPG" :
                    "SELECT * FROM AutoMPG WHERE car_name LIKE '%" + text + "%'";

            try (Connection conn = DBConnection.getConnection();
                 Statement stmt = conn.createStatement();
                 ResultSet rs = stmt.executeQuery(query)) {

                while (rs.next()) {
                    output.append(
                            rs.getInt("id") + " | " +
                                    rs.getDouble("mpg") + " | " +
                                    rs.getString("car_name") + "\n"
                    );
                }

            } catch (Exception ex) {
                output.setText("Error: " + ex.getMessage());
            }
        });

        frame.setVisible(true);
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
    }
}
```
*InsertData.java*
```
import java.io.*;
import java.sql.*;

public class InsertData {
    public static void main(String[] args) {
        String file = "src/auto-mpg.data.tsv";

        try (BufferedReader br = new BufferedReader(new FileReader(file));
             Connection conn = DBConnection.getConnection()) {

            br.readLine();

            String line;
            String sql = "INSERT INTO AutoMPG (mpg, cylinders, displacement, horsepower, weight, acceleration, model_year, origin, car_name) VALUES (?, ?, ?, ?, ?, ?, ?, ?, ?)";
            PreparedStatement ps = conn.prepareStatement(sql);

            while ((line = br.readLine()) != null) {
                String[] data = line.split("\t");

                ps.setDouble(1, Double.parseDouble(data[0]));
                ps.setInt(2, Integer.parseInt(data[1]));
                ps.setDouble(3, Double.parseDouble(data[2]));
                ps.setDouble(4, Double.parseDouble(data[3]));
                ps.setDouble(5, Double.parseDouble(data[4]));
                ps.setDouble(6, Double.parseDouble(data[5]));
                ps.setInt(7, Integer.parseInt(data[6]));

                ps.setString(8, "USA");

                ps.setString(9, data[7]);

                ps.executeUpdate();
            }

            System.out.println("Data inserted successfully!");

        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```
*auto-mpg.data.tsv*
```
mpg	cylinders	displacement	horsepower	weight	acceleration	model_year	car_name
18.0	8	307.0	130.0	3504.0	12.0	70	chevrolet chevelle malibu
15.0	8	350.0	165.0	3693.0	11.5	70	buick skylark 320
18.0	8	318.0	150.0	3436.0	11.0	70	plymouth satellite
16.0	8	304.0	150.0	3433.0	12.0	70	amc rebel sst
17.0	8	302.0	140.0	3449.0	10.5	70	ford torino
15.0	8	429.0	198.0	4341.0	10.0	70	ford galaxie 500
14.0	8	454.0	220.0	4354.0	9.0	70	chevrolet impala
14.0	8	440.0	215.0	4312.0	8.5	70	plymouth fury iii
14.0	8	455.0	225.0	4425.0	10.0	70	pontiac catalina
15.0	8	390.0	190.0	3850.0	8.5	70	amc ambassador dpl
```
### How does the program run?
We first load the auto MPG dataset into the MySQL Auto database to execute in the InertData.java program. Second we Run the AutoGUI.java code to begin the graphical user interface once the data has been successfully inserted. Third we conduct a search to see the results obtained based off the database as we type "ALL" or enter the name of an automobile in the GUI to get results from the TSV file.

### Challenges
The first challenge I've experienced was figuring out how to use NumberFormatException brought on through the TSV file's header line, which I was able to resolve by removing the first line before processing. Second was that Intelliji was missing the MySQL Connector J library, which I needed to find the correct jar driver to get the result of "Data Inserted Successfully!" inside of the program. Third was modifying, and reading the algorithm making sure that every column matched the data type into the database table, resolving the SQL insertion issues.

### My Assignment Video Link:

