# File_Handling_Java_Project
1. Main Class and Menu System
java
Copy
Edit
public class File_handling {
    private static final String FileName = "output.txt";
The main class is named File_handling, and it uses a constant FileName to specify the file where employee data will be stored.
2. Menu Display and User Input Handling
java
Copy
Edit
while (true) {
    System.out.println("1.ENTER THE DATA");
    System.out.println("2.READ THE DATA");
    System.out.println("3.DELETE THE DATA");
    System.out.println("4.SEARCH EMPLOYEE BY ID");
    System.out.println("5.EXIT");
    System.out.print("ENTER YOUR CHOICE : ");
    
    int choice = s.nextInt();
The program continuously displays a menu with the following options:
Enter the data
Read the data
Delete the data
Search employee by ID
Exit the program
User input is collected using a Scanner.
3. Switch Case for Menu Options
java
Copy
Edit
switch (choice) {
    case 1:
        enterData(s);
        break;
    case 2:
        viewData(s);
        break;
    case 3:
        searchData(s);
        break;
    case 4:
        deleteData(s);
        break;
    case 5:
        System.exit(0);
        break;
}
Based on the user's choice, the corresponding method is called:
enterData(s) for adding employee data.
viewData(s) for viewing all data.
searchData(s) for searching an employee by ID.
deleteData(s) for deleting data by ID.
System.exit(0) to terminate the program.
4. enterData() Method
java
Copy
Edit
public static void enterData(Scanner s) {
    try {
        FileWriter fw = new FileWriter(FileName, true);
        System.out.print("ENTER THE EMPLOYEE ID : ");
        String id = s.next();
        System.out.print("ENTER THE EMPLOYEE NAME : ");
        String name = s.next();
        System.out.print("ENTER THE EMPLOYEE SALARY : ");
        String salary = s.next();
        System.out.print("ENTER THE EMPLOYEE PLACE : ");
        String place = s.next();
        
        String employeedata = id + " | " + name + " | " + salary + " | " + place;
        fw.write(employeedata + System.lineSeparator());
        fw.close();
        System.out.println("DATAS ADDED SUCESSFULLY");
    } catch (Exception e) {
        System.out.println("ERROR IN FILE WRITER");
    }
}
This method collects the following details from the user:
Employee ID
Employee Name
Employee Salary
Employee Place
It then writes the data to output.txt in the format:
nginx
Copy
Edit
ID | Name | Salary | Place
FileWriter is used in append mode (true) to add new entries without overwriting existing data.
5. viewData() Method
java
Copy
Edit
public static void viewData(Scanner s) {
    try {
        File file = new File(FileName);
        if (!file.exists()) {
            System.out.println("THERE IS NO DATA IN THE DOCUMENT");
        }
        BufferedReader readed = new BufferedReader(new FileReader(file));
        String line = readed.readLine();
        while (line != null) {
            System.out.println(line);
            line = readed.readLine();
        }
        readed.close();
    } catch (Exception e) {
        System.out.println("PROBLEM IN VIEW DATA EXCEPTION");
    }
}
This method reads and displays all the employee records from output.txt.
It uses BufferedReader to read the file line by line.
If the file is empty or doesn't exist, it notifies the user.
6. searchData() Method
java
Copy
Edit
public static void searchData(Scanner s) {
    System.out.println("ENTER THE SEARCHING ID :");
    String searchid = s.next();

    try {
        BufferedReader readed = new BufferedReader(new FileReader(FileName));
        String line = readed.readLine();
        boolean found = false;
        while (line != null) {
            String data[] = line.split("\\|");
            if (data[0].trim().equals(searchid)) {
                System.out.println("DATA FOUND : " + line);
                found = true;
                break;
            }
            line = readed.readLine();
        }
        readed.close();
        if (!found) {
            System.out.println("YOUR DATA IS NOT IN THE DATABASE");
        }
    } catch (Exception e) {
        System.out.println("SEARCHING DATA EXCEPTION");
    }
}
This method searches for an employee by ID.
It reads each line from the file and splits the data using | as a separator.
If the ID matches the user input, the corresponding record is displayed.
If no match is found, a "not found" message is shown.
7. deleteData() Method
java
Copy
Edit
public static void deleteData(Scanner s) {
    try {
        System.out.println("ENTER THE VALUE THAT YOU WANT TO DELETE : ");
        String deleteid = s.next();
        File inputfile = new File(FileName);
        File tempfile = new File("tempfile.txt");
        BufferedReader readed = new BufferedReader(new FileReader(FileName));
        BufferedWriter writer = new BufferedWriter(new FileWriter(tempfile));
        String line;
        boolean deleted = false;
        while ((line = readed.readLine()) != null) {
            String data[] = line.split("\\|");
            if (data[0].trim().equals(deleteid)) {
                deleted = true;
                continue;
            }
            writer.write(line + System.lineSeparator());
        }
        readed.close();
        writer.close();
        if (deleted) {
            inputfile.delete();
            tempfile.renameTo(inputfile);
            System.out.println("ENTERED VALUE HAS BEEN DELETED");
        } else {
            tempfile.delete();
            System.out.println("YOU ENTERED VALUE ID NOT IN THE FILE");
        }
    } catch (Exception e) {
        System.out.print("ERROR IN THE DELETE DATA METHOD");
    }
}
This method deletes an employee record by ID.
It creates a temporary file and copies all records except the one with the matching ID.
After copying, the original file is deleted, and the temporary file is renamed as output.txt.
If no match is found, a message is displayed, and the temporary file is deleted.
8. Key Features and Functionalities
Persistent Storage: Uses output.txt to store and manage employee records.
CRUD Operations:
Create: Add new employee data.
Read: View all employee data.
Update: Not implemented in this version.
Delete: Remove employee data by ID.
Data Format: Each record is stored in a pipe-separated format:
nginx
Copy
Edit
ID | Name | Salary | Place
9. Possible Improvements
Data Validation: Validate user inputs (e.g., numeric ID and salary).
Error Handling: More descriptive error messages.
Search Enhancement: Add search functionality for name or place.
Update Feature: Implement a method to update existing records.
This program effectively demonstrates the use of File Handling in Java for managing employee records with simple CRUD operations.
