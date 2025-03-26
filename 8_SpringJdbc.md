A)

### **🔹 Introduction to Spring JDBC & H2 Database**  

---

## **1️⃣ What is Spring JDBC?**
Spring JDBC is a framework that **simplifies database interactions** in Java applications.  

🔹 **Problems with Traditional JDBC:**  
- Requires **manual connection management** (open/close connections).  
- You must **explicitly create SQL statements**.  
- Leads to **boilerplate code**.  

🔹 **Spring JDBC Benefits:**  
- Uses **JDBC Template** to **simplify SQL execution**.  
- **Manages connections automatically**.  
- Supports **Data Source pooling** for better efficiency.  

---

## **2️⃣ What is JDBC Template?**
- `JdbcTemplate` is a class in Spring JDBC that makes database interactions easier.  
- It helps **execute SQL queries** without worrying about managing connections.  

---

## **3️⃣ What is H2 Database?**
- H2 is an **in-memory database**, meaning data is **lost when the app stops**.  
- It is **lightweight** and great for **testing purposes**.  
- Comes with an **embedded JDBC driver**, so you don’t need extra setup.  

---

## **4️⃣ Setting Up Spring JDBC with H2**
### **📌 Step 1: Add Dependencies (`pom.xml`)**
```xml
<dependencies>
    <!-- Spring JDBC -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-jdbc</artifactId>
    </dependency>

    <!-- H2 Database -->
    <dependency>
        <groupId>com.h2database</groupId>
        <artifactId>h2</artifactId>
        <scope>runtime</scope>
    </dependency>
</dependencies>
```

---

### **📌 Step 2: Configure H2 in `application.properties`**
```properties
# H2 Database Configuration
spring.datasource.url=jdbc:h2:mem:testdb
spring.datasource.driverClassName=org.h2.Driver
spring.datasource.username=sa
spring.datasource.password=
spring.h2.console.enabled=true  # Enables H2 web console
spring.h2.console.path=/h2-console  # Access console at http://localhost:8080/h2-console
```

---

### **📌 Step 3: Create a Repository Using `JdbcTemplate`**
```java
package com.example.repository;

import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.stereotype.Repository;

@Repository
public class UserRepository {

    private final JdbcTemplate jdbcTemplate;

    public UserRepository(JdbcTemplate jdbcTemplate) {
        this.jdbcTemplate = jdbcTemplate;
    }

    public void createTable() {
        String sql = "CREATE TABLE users (id INT PRIMARY KEY, name VARCHAR(255))";
        jdbcTemplate.execute(sql);
        System.out.println("Users table created.");
    }

    public void insertUser(int id, String name) {
        String sql = "INSERT INTO users (id, name) VALUES (?, ?)";
        jdbcTemplate.update(sql, id, name);
        System.out.println("User inserted: " + name);
    }
}
```
---

### **📌 Step 4: Use `UserRepository` in Service Layer**
```java
package com.example.service;

import com.example.repository.UserRepository;
import org.springframework.stereotype.Service;

@Service
public class UserService {

    private final UserRepository userRepository;

    public UserService(UserRepository userRepository) {
        this.userRepository = userRepository;
    }

    public void setupDatabase() {
        userRepository.createTable();
        userRepository.insertUser(1, "Alice");
        userRepository.insertUser(2, "Bob");
    }
}
```
---

### **📌 Step 5: Run the Application**
```java
package com.example;

import com.example.service.UserService;
import org.springframework.boot.CommandLineRunner;
import org.springframework.stereotype.Component;

@Component
public class AppRunner implements CommandLineRunner {

    private final UserService userService;

    public AppRunner(UserService userService) {
        this.userService = userService;
    }

    @Override
    public void run(String... args) {
        userService.setupDatabase();
    }
}
```
✅ **Expected Output in Console:**
```
Users table created.
User inserted: Alice
User inserted: Bob
```

---

## **5️⃣ Key Takeaways**
✔ **Spring JDBC simplifies database operations using `JdbcTemplate`**.  
✔ **H2 is an in-memory database**, useful for **testing**.  
✔ **Spring automatically manages database connections**.  
✔ **Use `JdbcTemplate` for SQL execution instead of writing boilerplate JDBC code**.  

🚀 **Next Steps:**  
✔ Retrieve data from H2 using `JdbcTemplate`.  
✔ Explore **Spring Data JPA** for even more simplification.

B)
### **🔹 Setting Up a Spring JDBC Project with H2**  

---

## **1️⃣ Creating the Project Using Spring Initializr**
1. **Go to Spring Initializr**: [https://start.spring.io](https://start.spring.io)
2. **Project Type**: Maven  
3. **Language**: Java  
4. **Spring Boot Version**: 3.2 (or latest available)  
5. **Group ID**: `com.telusko`  
6. **Artifact ID**: `spring-jdbc-example`  
7. **Packaging**: JAR  
8. **Java Version**: Java 17 or 21  

### **📌 Add Dependencies:**
- **Spring JDBC** → Provides JDBC template for database interactions.  
- **H2 Database** → In-memory database for testing.  

Click **Generate**, download, unzip, and open the project in **IntelliJ IDEA**.

---

## **2️⃣ Verify Project Setup**
- Open `pom.xml` to check dependencies:
```xml
<dependencies>
    <!-- Spring JDBC -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-jdbc</artifactId>
    </dependency>

    <!-- H2 Database -->
    <dependency>
        <groupId>com.h2database</groupId>
        <artifactId>h2</artifactId>
        <scope>runtime</scope>
    </dependency>
</dependencies>
```
- Run the project to ensure there are **no errors**.  
- Optionally, print `"Hello World"` in `main()` to test.

---

## **3️⃣ Creating the `Student` Model**
- A **Student** has:
  - `rollNumber` (int)
  - `name` (String)
  - `marks` (int)

### **📌 Create `Student` Class (`model/Student.java`)**
```java
package com.telusko.model;

import org.springframework.stereotype.Component;

@Component
public class Student {
    private int rollNumber;
    private String name;
    private int marks;

    // Getters and Setters
    public int getRollNumber() { return rollNumber; }
    public void setRollNumber(int rollNumber) { this.rollNumber = rollNumber; }

    public String getName() { return name; }
    public void setName(String name) { this.name = name; }

    public int getMarks() { return marks; }
    public void setMarks(int marks) { this.marks = marks; }

    // toString() method
    @Override
    public String toString() {
        return "Student{rollNumber=" + rollNumber + ", name='" + name + "', marks=" + marks + '}';
    }
}
```
✅ **Why `@Component`?**  
- Makes `Student` a **Spring-managed bean**.  
- Can be **auto-wired** in other components.

✅ **Why `toString()`?**  
- Helps in **logging/debugging**.

---

## **4️⃣ Understanding Database & Object Relationship**
| **Table**       | **Java Class**  |
|----------------|---------------|
| `student` (Table) | `Student` (Class) |
| Columns: `rollNumber`, `name`, `marks` | Fields: `rollNumber`, `name`, `marks` |
| Rows (Data) | Objects (Instances) |

💡 **Each table row corresponds to a `Student` object.**  

---

## **5️⃣ Setting Up Spring Application Context**
### **📌 Modify `MainApplication` (`SpringJdbcExampleApplication.java`)**
```java
package com.telusko;

import com.telusko.model.Student;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.ApplicationContext;

@SpringBootApplication
public class SpringJdbcExampleApplication {
    public static void main(String[] args) {
        ApplicationContext context = SpringApplication.run(SpringJdbcExampleApplication.class, args);

        // Retrieve Student Bean
        Student s = context.getBean(Student.class);
        s.setRollNumber(101);
        s.setName("Navin");
        s.setMarks(78);

        System.out.println(s);
    }
}
```
✅ **Why `ApplicationContext`?**  
- Retrieves Spring-managed `Student` bean.  

✅ **Why `context.getBean(Student.class)`?**  
- Ensures **Spring handles object creation** instead of using `new Student()`.

---

## **6️⃣ Next Steps**
1. **Configure H2 database** (Already included with default settings).  
2. **Create tables & layers** (`Service` & `Repository`).  
3. **Perform CRUD operations** on `Student`.  

🚀 **Next Video:** Creating `Repository` and `Service` Layers.

C)
Here’s a structured summary of your notes:  

---

# **Spring JDBC with H2 – Service and Repository Layers**  

## **1. Creating the Service Layer**  
- Created a `StudentService` class inside a `service` package.  
- Marked it with `@Service` to make it a Spring-managed bean.  
- Used **Dependency Injection** (`@Autowired`) to inject the repository dependency.  
- Implemented an `addStudent(Student s)` method to handle student additions.  
- Called `repo.save(s)` to delegate the actual database operation to the repository layer.  
- Implemented a `getStudents()` method to fetch all student records from the database.  

## **2. Creating the Repository Layer**  
- Created a `StudentRepo` class inside a `repo` package (or `dao` if preferred).  
- Marked it with `@Repository` annotation.  
- Implemented the `save(Student s)` method, responsible for storing student data in the database.  
- Implemented a `findAll()` method returning a **dummy list** of students (to be replaced with actual database queries later).  

## **3. Connecting Service & Repository Layers**  
- The `StudentService` class calls `repo.save(s)` to store data.  
- The `getStudents()` method calls `repo.findAll()` to fetch all students.  
- Added a dummy `findAll()` implementation to verify flow.  

## **4. Running & Verifying**  
- The application runs successfully.  
- "Added" is printed when a student is added.  
- Fetching students currently returns an empty list (since no database connection is set up yet).  

## **Next Step: JDBC Template**  
- The repository layer will use **JdbcTemplate** to perform actual database operations.  
- Data will be stored and retrieved from the **H2 database**.  

D)
Here’s a structured summary of your notes:  

---

# **Spring JDBC with H2 – Using JdbcTemplate**  

## **1. Setting Up JdbcTemplate**
- H2 dependency includes an embedded database and driver by default.
- No extra configuration is required to use it.
- JdbcTemplate is used to interact with the database.

## **2. Adding JdbcTemplate to Repository Layer**
- Declared `private JdbcTemplate jdbc;`
- Used `@Autowired` to let Spring inject an instance automatically.
- JdbcTemplate relies on DataSource behind the scenes.

## **3. Implementing Data Insertion with JdbcTemplate**
- Used `jdbc.update(String sql, Object... args)` to execute insert queries.
- Created an SQL query using placeholders (`?`) for dynamic values:

  ```java
  String sql = "INSERT INTO student (rollNo, name, marks) VALUES (?, ?, ?)";
  ```
  
- Passed values from the `Student` object:

  ```java
  jdbc.update(sql, s.getRollNo(), s.getName(), s.getMarks());
  ```

- Printed the number of rows affected to verify success.

## **4. Running & Debugging**
- Code execution failed with:  
  `"Table STUDENT not found"`
- H2 database exists, but no **table** was created.
- Need to define the **schema** and **prepopulate data**.

## **Next Step: Creating Schema & Populating Data**
- Define table structure in H2.
- Preload sample data before operations.

E)


---

# **Spring Boot – Creating Schema & Preloading Data in H2**  

## **1. Creating Schema in H2**  
- Normally, in databases like PostgreSQL or MySQL, you manually create schemas using SQL queries.  
- In H2 (embedded DB), create schema and data files in the `resources` folder:  
  - **`schema.sql`** → Defines table structure  
  - **`data.sql`** → Preloads data  

### **Steps to Create `schema.sql`**  
1. Right-click `resources` → New File → Name it `schema.sql`.  
2. Define the table:  

   ```sql
   CREATE TABLE student (
       rollno INT PRIMARY KEY,
       name VARCHAR(50),
       marks INT
   );
   ```

---

## **2. Preloading Data in H2**
- Create another file: **`data.sql`**
- Use `INSERT` statements to add sample data:

   ```sql
   INSERT INTO student (rollno, name, marks) VALUES (101, 'Kiran', 79);
   INSERT INTO student (rollno, name, marks) VALUES (102, 'Harsh', 68);
   INSERT INTO student (rollno, name, marks) VALUES (103, 'Sonal', 82);
   ```

---

## **3. Running & Debugging**  
- Spring Boot automatically loads `schema.sql` and `data.sql` at startup.  
- Running the code initially caused an error:  
  - **Error: "Column given not found"**  
  - **Fix:** Use **single quotes** (`' '`) in SQL instead of double quotes (`" "`).  

- After fixing, the output showed:
  - **"1 affected"** → Data insertion was successful.  

---

## **4. Next Step: Retrieving Data with `findAll()`**  
- To verify if data is correctly stored, implement a method to fetch all records.  
- This will be covered in the next step.  

F)
Here’s a structured summary of your notes:  

---

# **Spring Boot – Fetching Data using `findAll()` in JDBC Template**  

## **1. Writing the Query to Fetch Data**  
- To retrieve all student records from the database, we use:  

   ```java
   String sql = "SELECT * FROM student";
   ```

- In JDBC, normally, we use **`executeQuery()`**, but with Spring **JDBC Template**, we use **`query()`** instead.

---

## **2. Using `RowMapper` to Map Database Rows to Java Objects**  
- `query()` method requires:  
  1. SQL query (`String sql`)  
  2. `RowMapper<Student>` to map result set rows to Java objects  

### **Implementation Using an Anonymous Class**
```java
List<Student> students = jdbcTemplate.query(sql, new RowMapper<Student>() {
    @Override
    public Student mapRow(ResultSet rs, int rowNum) throws SQLException {
        Student s = new Student();
        s.setRollNo(rs.getInt("rollno"));
        s.setName(rs.getString("name"));
        s.setMarks(rs.getInt("marks"));
        return s;
    }
});
```

- **How `mapRow()` Works:**  
  - It processes **one row at a time** from `ResultSet`.  
  - Retrieves values using `rs.getInt()` or `rs.getString()`.  
  - Populates a `Student` object and returns it.  

---

## **3. Optimizing with Lambda Expression**
- Since `RowMapper` is a **functional interface**, we can simplify it using **Lambda expressions**:  

```java
List<Student> students = jdbcTemplate.query(sql, (rs, rowNum) -> {
    return new Student(
        rs.getInt("rollno"),
        rs.getString("name"),
        rs.getInt("marks")
    );
});
```

- Even shorter:
```java
List<Student> students = jdbcTemplate.query(sql, (rs, rowNum) -> 
    new Student(rs.getInt("rollno"), rs.getString("name"), rs.getInt("marks"))
);
```

---

## **4. Running & Output**  
- Running the application successfully retrieves the data.  
- The output confirms:  
  - Previously inserted records from `data.sql`  
  - Newly added records from `insert` statements  
- The optimized code reduces **lines of code** while maintaining functionality.  

---

## **Next Steps**
- Understanding how Lambda functions work in Java  
- Further optimizing data retrieval in Spring JDBC  

G)
Here are your simplified notes based on the video:  

---

# **Fetching Data with Spring JDBC**  
### **1. Fetching Data Using Spring JDBC Template**
- **SQL Query:** `"SELECT * FROM student"`
- **Execution:** Use `jdbc.query(sql, RowMapper)`  
  - The first parameter is the query (`sql`).  
  - The second parameter is `RowMapper`, which maps each row of the `ResultSet` to a Java object.

### **2. Understanding RowMapper**
- **Role of RowMapper:** Extracts data from `ResultSet` and converts it into Java objects.
- **Implementation:**
  ```java
  RowMapper<Student> mapper = new RowMapper<Student>() {
      public Student mapRow(ResultSet rs, int rowNum) throws SQLException {
          Student s = new Student();
          s.setRollNo(rs.getInt("rollno"));
          s.setName(rs.getString("name"));
          s.setMarks(rs.getInt("marks"));
          return s;
      }
  };
  ```
- **Lambda Alternative:**  
  ```java
  RowMapper<Student> mapper = (rs, rowNum) -> {
      Student s = new Student();
      s.setRollNo(rs.getInt("rollno"));
      s.setName(rs.getString("name"));
      s.setMarks(rs.getInt("marks"));
      return s;
  };
  ```
- **Fetching Data:**  
  ```java
  return jdbc.query(sql, mapper);
  ```
- Returns a `List<Student>` containing all records.

---

# **Switching to External Databases (PostgreSQL)**
### **1. Why Change from H2?**
- H2 is an in-memory database, but real-world applications need persistent storage.
- PostgreSQL, MySQL, or Oracle can be used instead.

### **2. Steps to Use PostgreSQL**
#### **(A) Create a Database and Table**
1. Open **pgAdmin** → Create a database (`telusko`).
2. Run SQL scripts from `schema.sql` to create the **student** table.
3. Insert initial records.

#### **(B) Update `pom.xml` for PostgreSQL**
1. **Remove H2 Dependency** (Comment it out).  
2. **Add PostgreSQL Dependency** from [Maven Repository](https://mvnrepository.com/):  
   ```xml
   <dependency>
       <groupId>org.postgresql</groupId>
       <artifactId>postgresql</artifactId>
       <version>42.2.27</version> <!-- Choose a stable version -->
   </dependency>
   ```
3. **Reload Maven Changes** to fetch dependencies.

#### **(C) Configure `application.properties`**
Add PostgreSQL connection settings:
```properties
spring.datasource.url=jdbc:postgresql://localhost:5432/telusko
spring.datasource.username=postgres
spring.datasource.password=0000  # Use your actual password
spring.datasource.driver-class-name=org.postgresql.Driver
```

### **3. Running the Application**
- Start the Spring Boot app.
- Check logs; if **data from PostgreSQL** appears, setup is successful.

---

### **Key Takeaways**
✔ `RowMapper` simplifies **mapping SQL rows to Java objects**.  
✔ **Lambda expressions** make RowMapper code concise.  
✔ Switching databases requires **only two file changes** (`pom.xml` and `application.properties`).  
✔ **PostgreSQL/MySQL support** is enabled via proper JDBC configurations.  




