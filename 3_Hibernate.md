0)






A)
### **Summary of Hibernate and ORM Explanation**  

Hibernate is an **ORM (Object Relational Mapping) framework** that helps in interacting with databases in Java without writing SQL queries manually.

---

### **What is ORM?**
ORM stands for **Object Relational Mapping**. It allows developers to **map Java objects** to **database tables** so that data handling becomes seamless.

---

### **Why Do We Need ORM?**
1. **Java is Object-Oriented**  
   - Everything in Java is based on objects (data + behavior).  
   - We naturally think in terms of objects while building applications.  

2. **Traditional Database Interaction with JDBC is Tedious**  
   - Java objects hold data, but databases store data in **tables**.  
   - JDBC requires **writing SQL queries manually** to store/retrieve objects.  
   - Developers need to **convert objects into SQL** and vice versa manually.  

3. **Challenges with JDBC**  
   - Manually writing SQL for CRUD operations.  
   - Mapping objects to tables manually.  
   - Managing connections, caching, and performance tuning.

---

### **How Hibernate Solves These Problems?**
1. **Direct Object Persistence**  
   - Instead of manually writing SQL, Hibernate allows saving an object with a simple method:  
     ```java
     session.save(studentObject);
     ```
   - Hibernate takes care of **converting objects into SQL queries and executing them**.

2. **Automatic Mapping of Objects to Tables**  
   - If we have a **Student** class:
     ```java
     class Student {
         int rollNumber;
         String name;
         int age;
     }
     ```
   - Hibernate will **automatically create a table** like:
     ```sql
     CREATE TABLE student (
         roll_number INT,
         name VARCHAR(255),
         age INT
     );
     ```

3. **Simplifies Database Switching**  
   - Easily switch from MySQL to PostgreSQL or Oracle without changing code.  
   - No need to rewrite queries—just change the database configuration.  

4. **Performance Optimization**  
   - **Caching**: Reduces database queries, improving performance.  
   - **Lazy Loading**: Loads data **only when needed**, saving resources.  

---

### **Key Benefits of Hibernate**
✅ **No need to write SQL manually**  
✅ **Reduces boilerplate code**  
✅ **Easier database management**  
✅ **Optimized performance with caching**  
✅ **Supports multiple databases without rewriting code**  

---

### **What’s Next?**
The upcoming lessons will cover practical implementations of Hibernate, including:  
✔ Setting up Hibernate  
✔ Creating entity classes  
✔ Performing CRUD operations without writing SQL manually  

Hibernate **automates** the process of interacting with databases, making development much easier. 🚀❤


B)### **Summary: Setting Up a Hibernate Project in IntelliJ IDEA**  

#### **Step 1: Create a New Project**
- Open **IntelliJ IDEA (Community Edition)**.  
- Click on **New Project** and choose **Maven** as the project type.  
- Name the project **HibProject**.  
- Select **JDK 21 or higher**.  
- Click **Create** to set up a basic Maven project.  

---

#### **Step 2: Add Hibernate and Database Dependencies**
Since Hibernate is not included by default, we need to add it manually using **Maven dependencies**.  

1. **Go to `pom.xml`** and inside the `<dependencies>` tag, add:
   
   - **PostgreSQL Driver Dependency** (since we are using PostgreSQL):  
     ```xml
     <dependency>
         <groupId>org.postgresql</groupId>
         <artifactId>postgresql</artifactId>
         <version>42.6.0</version>
     </dependency>
     ```

   - **Hibernate Core Dependency** (for ORM functionality):  
     ```xml
     <dependency>
         <groupId>org.hibernate.orm</groupId>
         <artifactId>hibernate-core</artifactId>
         <version>6.6.3.Final</version>
     </dependency>
     ```

2. **Reload Maven Dependencies**  
   - IntelliJ IDEA will detect changes and suggest reloading Maven. Click on **Reload Maven Changes** to download the required libraries.  
   - After reloading, Hibernate and Jakarta Persistence dependencies should appear in **External Libraries**.

---

#### **Step 3: Verify the Setup**
- Ensure **PostgreSQL** is installed on the machine.  
- Open **pgAdmin** to manage the database.  
- The project now has Hibernate and PostgreSQL ready for configuration.  

---

### **Next Steps**
In the next session, we will:  
✅ Configure Hibernate using **hibernate.cfg.xml**.  
✅ Define an **entity class** (Java class mapped to a database table).  
✅ Store and retrieve objects from the database.  

This was just the initial setup—configuration and implementation come next. 🚀

C)### Steps to Implement Hibernate in the Project

#### 1. **Create a New Project**
   - Use **IntelliJ IDEA Community** version.
   - Create a **Maven project** (you can also use Gradle).
   - Ensure JDK version **21 or above** is selected.

#### 2. **Add Hibernate & PostgreSQL Dependencies**
   - Go to **Maven Repository** and search for:
     1. **PostgreSQL Driver** (Choose the second latest version).
     2. **Hibernate Core** (Choose the second latest version).
   - Add these dependencies to `pom.xml`.
   - Reload Maven to fetch dependencies.

#### 3. **Create the Student Entity**
   - Create a `Student` class with fields:
     ```java
     private int rollNo;
     private String sName;
     private int sAge;
     ```
   - Generate **getters, setters, and `toString()`**.

#### 4. **Create a Session and SessionFactory**
   - **SessionFactory**: Used to create sessions.
   - **Session**: Used to perform database operations.
   - Code:
     ```java
     Configuration cfg = new Configuration();
     SessionFactory sf = cfg.buildSessionFactory();
     Session session = sf.openSession();
     ```
   - **Issue:** No database configuration yet.

#### 5. **Why Configuration is Needed?**
   - Hibernate does not yet know:
     - **Which database to use?** (PostgreSQL)
     - **Database connection details** (username, password, URL)
   - Configuration is required before running.

#### 6. **Next Step**
   - Configure Hibernate to connect with PostgreSQL.
   - Define **hibernate.cfg.xml** file.

In the next step, we'll set up the **Hibernate configuration file** to establish the database connection.


D)
## **Hibernate Configuration using XML**

### **1. Setting Up Configuration File**
- Hibernate configuration is usually stored in the `resources` folder as `hibernate.cfg.xml`.
- The configuration file should be named `hibernate.cfg.xml` by default. Otherwise, it must be explicitly specified.

### **2. Creating `hibernate.cfg.xml`**
- Start with `<hibernate-configuration>` as the root tag.
- Inside, use `<session-factory>` to define properties.

### **3. Defining Hibernate Properties**
- **Database Driver:**  
  ```xml
  <property name="hibernate.connection.driver_class">org.postgresql.Driver</property>
  ```
- **Database URL:**  
  ```xml
  <property name="hibernate.connection.url">jdbc:postgresql://localhost:5432/telusko</property>
  ```
- **Username and Password:**  
  ```xml
  <property name="hibernate.connection.username">postgres</property>
  <property name="hibernate.connection.password">0000</property>
  ```

### **4. Running Without Mapping**
- If `hibernate.cfg.xml` is missing, an error occurs:  
  `"Could not locate configuration XML"`
- Running without entity mapping gives an error:  
  `"Unable to locate persister: com.telusko.Student"`

---

## **Mapping Entity with Hibernate**
### **5. Registering Entity in Code**
Before calling `configure()`, add:
```java
cfg.addAnnotatedClass(com.telusko.Student.class);
```
However, this alone is not enough.

### **6. Using Annotations for Mapping**
- Mark the class with `@Entity` to make it managed by Hibernate.
- Use `@Id` to specify the primary key.

Example:
```java
@Entity
public class Student {
    @Id
    private int rollNo;
    private String name;
}
```
- `@Entity` comes from **Jakarta Persistence API (JPA)**, which standardizes ORM tools.

---

## **Handling Transactions**
### **7. Understanding Transactions**
- In databases, saving data requires transactions.
- Hibernate requires explicit commit after saving data.

### **8. Implementing Transaction Management**
```java
Transaction tx = session.beginTransaction();
session.save(student);
tx.commit();
```
Without `commit()`, data will not be saved.

---

## **Auto Table Creation**
### **9. Handling Missing Tables**
If the table doesn’t exist, Hibernate can create it automatically using:
```xml
<property name="hibernate.hbm2ddl.auto">update</property>
```
### **10. Available Options**
- `none` → No changes in schema.
- `create` → Always creates a new table (deletes existing).
- `create-drop` → Creates a table and drops it after the session.
- `update` → Creates table if missing, updates schema without removing columns.

Best practice:
- **Use `update` in development**.
- **Avoid it in production**.

---

## **Final Output**
- No explicit SQL queries were written.
- Hibernate automatically generated and executed SQL.
- Successfully stored data in the PostgreSQL database.

---

### **Next Steps**
- Handling deprecated `save()` method.
- Viewing SQL queries executed by Hibernate.

E)

## **Viewing and Debugging SQL Queries in Hibernate**

### **1. Handling Duplicate Primary Key Issue**
- Running the same program twice without changing the primary key (`rollNo`) results in an error:
  ```
  duplicate key value violates unique constraint
  ```
- The primary key (`rollNo`) must be unique. If we try to insert a record with an existing primary key, an error occurs.
- To avoid this, use a different primary key value each time.

### **2. Confirming Data Insertion**
- After inserting new data, we check the database using SQL queries to confirm.
- Example:
  ```sql
  SELECT * FROM student;
  ```
- The output shows multiple records successfully inserted.

---

## **3. Viewing Hibernate-Generated SQL Queries**
- Hibernate internally uses JDBC, meaning SQL queries are executed behind the scenes.
- To see these queries in the console, enable the `hibernate.show_sql` property in `hibernate.cfg.xml`:

  ```xml
  <property name="hibernate.show_sql">true</property>
  ```

- Example query output:
  ```sql
  INSERT INTO Student (name, age, rollNo) VALUES ('Harsh', 21, 103);
  ```

### **4. Formatting SQL Queries for Better Readability**
- By default, SQL queries are displayed in a single line.
- To format them properly, enable the `hibernate.format_sql` property:

  ```xml
  <property name="hibernate.format_sql">true</property>
  ```

- The formatted output makes complex queries easier to read.

---

## **5. Setting Hibernate Dialect**
- Different databases use slightly different SQL syntax.
- Hibernate provides "dialects" to handle these differences.
- The dialect should be explicitly specified in `hibernate.cfg.xml`:

  ```xml
  <property name="hibernate.dialect">org.hibernate.dialect.PostgreSQLDialect</property>
  ```

- This property is **optional** but recommended to avoid compatibility issues.

---

## **Final Steps**
- Successfully inserted and viewed data.
- Enabled query logging and formatting for better debugging.
- Specified the correct database dialect.

### **Next Video Preview**
- Code optimization in Hibernate.
- Understanding unnecessary lines of code and removing redundancy.

F)

## **Optimizing Hibernate Code**

### **1. Replacing `save()` with `persist()`**
- The `save()` method is **deprecated in Hibernate 6.0+**.
- Hibernate is aligning with **JPA** standards, which use `persist()`.
- Change:
  ```java
  session.save(student); // Deprecated
  ```
  To:
  ```java
  session.persist(student); // Recommended
  ```

---

### **2. Managing `SessionFactory` Properly**
- **Issue:** `SessionFactory` is a heavyweight object that consumes resources.
- **Solution 1:** Use **try-with-resources** to automatically close resources.
- **Solution 2:** Manually close `Session` and `SessionFactory`:
  ```java
  session.close();
  sessionFactory.close();
  ```
- Always close resources when they are no longer needed.

---

### **3. Refactoring Configuration Code**
- **Issue:** The code for `Configuration`, `addAnnotatedClass()`, and `configure()` was spread over multiple lines.
- **Optimization:** Merge into a **single statement**:
  ```java
  SessionFactory sessionFactory = new Configuration()
      .addAnnotatedClass(com.telusko.Student.class)
      .configure()
      .buildSessionFactory();
  ```
- This makes the code cleaner and easier to manage.

---

### **4. Testing the Optimized Code**
- Inserted a new record:
  ```java
  Student student = new Student(106, "Avni", 21);
  session.persist(student);
  ```
- **Output:** Query executed successfully with no warnings.

---

## **Next Steps**
- Now that data is stored efficiently, the next step is **fetching data** from the database.
- The next video will cover different ways to retrieve records.



G)
## **Fetching Data in Hibernate**

### **1. Goal: Retrieve Data from the Database**
- Previously, we stored data in the database using `persist()`.
- Now, instead of saving, we need to **fetch** an existing record.

---

### **2. Setting Up for Fetching**
- First, we create a **Student object reference** (`s2`) but don’t initialize it yet:
  ```java
  Student s2 = null;
  ```
- Printing `s2` at this point will return `null`.

---

### **3. Is a Transaction Needed?**
- **Fetching data does not require a transaction**, so we can remove:
  ```java
  session.beginTransaction();
  session.getTransaction().commit();
  ```
- Transactions are only required for **insert, update, or delete operations**.

---

### **4. Fetching Data Using `get()`**
- Hibernate provides a `get()` method to retrieve records by **primary key**:
  ```java
  s2 = session.get(Student.class, 102);
  ```
  - **First parameter:** Class type (`Student.class`).
  - **Second parameter:** Primary key value (`102`).

- **Hibernate executes the SQL query automatically:**
  ```sql
  SELECT * FROM student WHERE id = 102;
  ```
- It then converts the result **into a Student object** automatically.

- **Printing `s2` displays the fetched record:**
  ```java
  System.out.println(s2);
  ```

---

### **5. Handling Missing Records**
- If the record **does not exist**, `get()` returns `null`:
  ```java
  s2 = session.get(Student.class, 109);
  ```
- If you call `s2.getName()` on a `null` object, it **throws a NullPointerException**.
- **Solution:** Always check before accessing attributes:
  ```java
  if (s2 != null) {
      System.out.println(s2.getName());
  } else {
      System.out.println("Student not found.");
  }
  ```

---

### **6. `load()` vs `get()`**
- **Tried `load()` instead of `get()`**:
  ```java
  s2 = session.load(Student.class, 102);
  ```
- **Issues with `load()`:**
  - It throws an error: *"Could not initialize proxy – no Session."*
  - `load()` **returns a proxy object**, not the actual entity.
  - If the requested ID **does not exist**, `load()` throws an **exception** instead of returning `null`.

- **Conclusion:** Use `get()` for **simple retrieval**.

---

### **7. Summary**
- `get()` is **used for fetching data by primary key**.
- It **returns `null` if no record is found** (avoids exceptions).
- Transactions are **not needed for fetching**.
- Always **check for `null` before accessing object properties**.

---

## **Next: Updating Data**
- Now that we know how to fetch records, the next step is **modifying existing data**.
- This will involve **update operations**, which **do require a transaction**.

H)
## **Updating and Deleting Data in Hibernate**

### **1. CRUD Recap**
- **C**reate → `persist()`
- **R**ead → `get()`
- **U**pdate → `merge()`
- **D**elete → `remove()`
- We have covered **Create and Read**. Now, let’s move to **Update and Delete**.

---

## **Updating Data**
### **1. Goal: Update Age of an Existing Student**
- Example: Change **Harsh's age** from **21 to 23** (Roll No: `103`).

### **2. Steps to Update**
1. **Create an object with new values**:
   ```java
   Student s1 = new Student();
   s1.setRollNo(103);
   s1.setName("Harsh");
   s1.setAge(23);
   ```
2. **Use `merge()` method** (instead of `update()` which is deprecated):
   ```java
   session.merge(s1);
   ```
3. **Transaction is required** for update operations:
   ```java
   Transaction transaction = session.beginTransaction();
   session.merge(s1);
   transaction.commit();
   ```
4. **Why is a transaction needed?**
   - Without `commit()`, the changes won't reflect in the database.

5. **SQL Queries Hibernate Generates**:
   ```sql
   SELECT * FROM student WHERE rollno = 103;
   UPDATE student SET age = 23 WHERE rollno = 103;
   ```
6. **Verifying the Update**
   - Query the database manually or print `s1` to check if age is updated.

---

### **3. Using `merge()` for Insert (if record doesn’t exist)**
- `merge()` behaves like `saveOrUpdate()`, meaning:
  - **If record exists** → It updates.
  - **If record doesn’t exist** → It inserts.
- Example:
  ```java
  Student s2 = new Student();
  s2.setRollNo(109);
  s2.setName("Anvit");
  s2.setAge(29);
  session.merge(s2);
  transaction.commit();
  ```
- Hibernate will check if **rollno = 109** exists.
  - If **not found**, it inserts a new row.

---

## **Deleting Data**
### **1. Goal: Remove a Student Record**
- Example: Delete student **Anvit** (Roll No: `109`).

### **2. Steps to Delete**
1. **Fetch the object to be deleted**:
   ```java
   Student s3 = session.get(Student.class, 109);
   ```
2. **Use `remove()` method** (instead of `delete()` which is deprecated):
   ```java
   session.remove(s3);
   ```
3. **Transaction is required**:
   ```java
   Transaction transaction = session.beginTransaction();
   session.remove(s3);
   transaction.commit();
   ```
4. **SQL Queries Hibernate Generates**:
   ```sql
   SELECT * FROM student WHERE rollno = 109;
   DELETE FROM student WHERE rollno = 109;
   ```
5. **Verifying the Deletion**
   - Check the database manually to ensure **rollno = 109** is removed.

---

## **Summary**
| Operation | Hibernate Method | Requires Transaction? |
|-----------|----------------|----------------------|
| Create    | `persist(s1)`   | ✅ Yes |
| Read      | `get(Student.class, id)` | ❌ No |
| Update    | `merge(s1)`     | ✅ Yes |
| Delete    | `remove(s1)`    | ✅ Yes |

Now we have covered **all CRUD operations** in Hibernate. 

I)
## **Customizing Table and Column Names in Hibernate**

### **1. Default Table and Column Naming**
- By default, Hibernate creates a table with the same name as the **Entity class**.
- Column names are taken from **variable names**.
- Example: `class Student` → table `student`, columns `rollNo, sName, Age`.

### **2. Creating a New Entity: Alien**
1. Define a new class `Alien`:
   ```java
   @Entity
   public class Alien {
       @Id
       private int aid;
       private String aname;
       private String tech;

       // Getters, Setters, and toString()
   }
   ```
2. Insert data into `Alien` table:
   ```java
   Alien a1 = new Alien();
   a1.setAid(101);
   a1.setAname("Navin");
   a1.setTech("Java");
   session.persist(a1);
   ```

### **3. Understanding Hibernate Configuration Modes**
- **`update` mode**: Modifies an existing table if required.
- **`create` mode**: Drops and recreates the table each time.
  ```xml
  <property name="hibernate.hbm2ddl.auto">create</property>
  ```
- Running in `create` mode ensures a fresh table is always created.

### **4. Changing Table Name**
- By default, Hibernate takes the **Entity class name** as the table name.
- We can override it using `@Table(name="custom_table_name")`.
  ```java
  @Entity
  @Table(name = "alien_table")
  public class Alien {
      @Id
      private int aid;
      private String aname;
      private String tech;
  }
  ```
- Running this will create a table named `alien_table` instead of `Alien`.

### **5. Changing Column Names**
- By default, column names are taken from field names.
- We can change them using `@Column(name="custom_column_name")`.
  ```java
  @Column(name = "alien_name")
  private String aname;
  ```
- This will rename the column `aname` to `alien_name` in the database.

### **6. Skipping a Field in Database Using `@Transient`**
- If we don’t want a field to be stored in the database, use `@Transient`.
  ```java
  @Transient
  private String tech; // This won't be stored in the database
  ```
- This allows storing additional information in objects without saving it in the DB.

### **7. Summary of Annotations**
| Annotation  | Purpose |
|------------|---------|
| `@Entity` | Marks a class as a Hibernate entity (table) |
| `@Table(name = "table_name")` | Customizes table name |
| `@Id` | Marks the primary key column |
| `@Column(name = "column_name")` | Customizes column name |
| `@Transient` | Excludes a field from being stored in DB |

This helps in customizing the structure of database tables as needed!

J)
### Summary: Hibernate @Embeddable Annotation

So far, we've been storing simple data types like `int`, `String`, etc., which Hibernate easily maps to database types. But what if we need to store complex types like a `Laptop` object containing fields like `brand`, 
`model`, and `ram`?

#### Problem:
- We initially stored laptop details as a single `String`, but this isn't ideal for querying individual fields like `brand` or `model`.
- If we directly create a `Laptop` class and use it in our `Alien` entity, Hibernate throws an error:  
  _"Could not determine the recommended JDBC type for Laptop"_
- Hibernate doesn't know how to map a custom class unless explicitly told.

#### Solution: @Embeddable
- Instead of making `Laptop` a separate table (`@Entity`), we want its fields stored within the `alien_table`.
- Mark `Laptop` with `@Embeddable`, allowing it to be embedded inside `Alien`.

#### Implementation:
```java
@Entity
class Alien {
    @Id
    private int aid;
    private String aname;
    private String tech;

    @Embedded
    private Laptop laptop;  // Complex type
    
    // Getters, Setters, toString
}

@Embeddable
class Laptop {
    private String brand;
    private String model;
    private int ram;
    
    // Getters, Setters, toString
}
```

#### Key Takeaways:
- `@Embeddable` makes `Laptop` part of the `Alien` table instead of creating a separate table.
- `@Embedded` in `Alien` tells Hibernate to use `Laptop` as an embedded field.
- Now, Hibernate successfully maps `Laptop` fields (`brand`, `model`, `ram`) as columns in `alien_table`.

#### Next Topic:
- Fetching the embedded data: The `session.get(Alien.class, 101)` retrieves data but doesn't trigger a `SELECT` query (due to caching). This will be explored in upcoming videos.

K)


## **Embeddables and Entity Relationships in Hibernate**  

### **1. Storing Simple and Complex Types**  
- Simple types like `int`, `String` directly map to JDBC types.  
- Complex types (like `Laptop` object in `Alien`) cannot be directly mapped.  

### **2. Using `@Embeddable` for Complex Types**  
- If `Laptop` is embedded within `Alien`, its fields become part of `alien_table`.  
- Use `@Embeddable` annotation in `Laptop` class.  
- No separate table is created; instead, its fields are stored within `alien_table`.  

**Issue:**  
- If `Laptop` should have an identity or be used by multiple `Alien` objects, embedding is not ideal.  

---

### **3. Entity Relationships (Mappings in Hibernate)**  

#### **3.1 One-to-One (`@OneToOne`)**  
- `Alien` has one `Laptop`.  
- `Laptop` should have its own table.  
- Use **foreign key** in `Alien` or `Laptop` to link them.  

#### **3.2 One-to-Many (`@OneToMany`)**  
- One `Alien` has multiple `Laptops`.  
- Foreign key (`AID`) is stored in the `Laptop` table.  

#### **3.3 Many-to-One (`@ManyToOne`)**  
- Multiple `Laptops` belong to one `Alien`.  
- The `Laptop` table will have `AID` as a foreign key.  

#### **3.4 Many-to-Many (`@ManyToMany`)**  
- Example: Students using laptops in a lab.  
- A third table (`alien_laptop`) is needed to store relations.  
- This table contains `AID` and `LID` columns.  

---

### **4. Choosing the Right Mapping Approach**  
| Relationship | Foreign Key Placement | Annotation to Use |
|-------------|----------------------|------------------|
| One-to-One | Either table | `@OneToOne` |
| One-to-Many | In child table | `@OneToMany` (Parent), `@ManyToOne` (Child) |
| Many-to-One | In child table | `@ManyToOne` |
| Many-to-Many | Separate third table | `@ManyToMany` |

**Note:** Many-to-Many needs additional handling beyond just using `@ManyToMany`.  

---

This covers the key concepts of embedding and mapping entities in Hibernate.

L)### **Implementing One-to-One Relationship in Hibernate**  

#### **1. Converting `Laptop` from Embeddable to Entity**  
- Previously, `Laptop` was embedded inside `Alien`, but now it needs its own table.  
- To make `Laptop` a separate entity:  
  ```java
  @Entity
  public class Laptop {
      @Id
      private int LID;  
      private String brand;  
      private String model;

      // Getters, Setters, and toString() method
  }
  ```  
- **Why?** `Laptop` has its own identity and may be referenced elsewhere.  

---

#### **2. Setting Up One-to-One Mapping in `Alien`**  
- In `Alien`, we establish a `OneToOne` relationship with `Laptop`:  
  ```java
  @Entity
  public class Alien {
      @Id
      private int AID;
      private String name;
      
      @OneToOne
      private Laptop laptop;
  }
  ```  
- This ensures each `Alien` is linked to exactly one `Laptop`.  

---

#### **3. Persisting Data in the Correct Order**  
- Since `Laptop` is an independent entity, **insert Laptop first, then Alien**:  
  ```java
  Laptop l1 = new Laptop();
  l1.setLID(1);
  l1.setBrand("Dell");

  Alien a1 = new Alien();
  a1.setAID(101);
  a1.setName("John");
  a1.setLaptop(l1);

  EntityManager em = factory.createEntityManager();
  em.getTransaction().begin();
  
  em.persist(l1);  // Save Laptop first  
  em.persist(a1);  // Then save Alien  

  em.getTransaction().commit();
  ```  
- **Why?** `Alien` references `Laptop`, so `Laptop` must exist before `Alien` is persisted.  

---

#### **4. Database Structure After Execution**  
- Hibernate generates the tables automatically.  
- `alien_table` contains a **foreign key** column (`laptop_id`) referencing `laptop_table`.  
- The database now has:  
  - **`laptop_table`**: Stores laptop details (`LID`, `brand`, `model`).  
  - **`alien_table`**: Stores alien details (`AID`, `name`) with a `laptop_id` foreign key.  

---

### **Key Takeaways**  
✅ **`@OneToOne` ensures each Alien has exactly one Laptop.**  
✅ **Foreign key is stored in `alien_table`, referencing `laptop_table`.**  
✅ **Persist `Laptop` before `Alien` to avoid foreign key errors.**  

Next, we explore **One-to-Many relationships**, where one Alien owns multiple Laptops.

M)
### **One-to-Many and Many-to-One Relationship in Hibernate**  

#### **1. Changing from One-to-One to One-to-Many**  
- **Problem:** One `Alien` can now have **multiple** `Laptops`, not just one.  
- **Solution:** Replace `Laptop` reference with a **list** of `Laptop` objects.  

**Updated `Alien` class:**  
```java
@Entity
public class Alien {
    @Id
    private int AID;
    private String name;

    @OneToMany
    private List<Laptop> laptops;

    // Getters, Setters, toString()
}
```
- **Effect:** This will create a **third table** (`alien_laptop`) to handle the **relationship**.

---

#### **2. Persisting Multiple Laptops**  
```java
EntityManager em = factory.createEntityManager();
em.getTransaction().begin();

Laptop l1 = new Laptop();
l1.setLID(1);
l1.setBrand("Dell");
l1.setModel("XPS");

Laptop l2 = new Laptop();
l2.setLID(2);
l2.setBrand("HP");
l2.setModel("EliteBook");

Alien a1 = new Alien();
a1.setAID(101);
a1.setName("John");

// Assign multiple laptops
a1.setLaptops(List.of(l1, l2));

em.persist(l1);
em.persist(l2);
em.persist(a1);

em.getTransaction().commit();
```
- **Why persist `Laptop` first?**  
  - If we persist `Alien` first, Hibernate might not know how to handle `laptops`.  

---

#### **3. Database Structure**  
**Hibernate creates three tables:**  
1. **alien_table** (`AID`, `name`)  
2. **laptop_table** (`LID`, `brand`, `model`)  
3. **alien_laptop (Join Table)** (`AID`, `LID`)  

- **Why a third table?**  
  - One `Alien` can have **multiple** `Laptops`, so a direct foreign key **won’t work** inside `Alien`.  
  - Instead, a **many-to-many-style** **join table** is created to store associations.  

**alien_laptop (Join Table) example:**  
```
+-----+-----+
| AID | LID |
+-----+-----+
| 101 |  1  |
| 101 |  2  |
+-----+-----+
```

---

### **Avoiding the Third Table**
- **Issue:** Hibernate **creates a third table** even though a `Laptop` can only belong to one `Alien`.  
- **Solution:** Instead of storing the relationship in `Alien`, store it in `Laptop`.  

#### **4. Adding Alien Reference in Laptop**
- Modify `Laptop` to reference `Alien`:  
  ```java
  @Entity
  public class Laptop {
      @Id
      private int LID;
      private String brand;
      private String model;

      @ManyToOne
      private Alien alien;

      // Getters, Setters
  }
  ```
- Now, each `Laptop` **knows** which `Alien` it belongs to.

---

#### **5. Updating the Alien Class**
- Remove `@OneToMany` direct mapping. Instead, **refer to it inside `Laptop`**:  
  ```java
  @Entity
  public class Alien {
      @Id
      private int AID;
      private String name;

      @OneToMany(mappedBy = "alien")
      private List<Laptop> laptops;
  }
  ```
- **What does `mappedBy` do?**  
  - **Prevents** Hibernate from creating a third table.  
  - `Laptop` is **responsible** for managing the relationship.  
  - `Alien` just **knows** that multiple laptops exist but **doesn’t store the relationship itself**.

---

#### **6. Persisting Data Again**
```java
l1.setAlien(a1);
l2.setAlien(a1);

em.persist(a1);
em.persist(l1);
em.persist(l2);
```
- **Effect:**  
  - **No third table.**  
  - `Laptop` table stores **foreign key reference** to `Alien`.  

---

#### **7. Final Database Structure**
1. **alien_table** (`AID`, `name`)  
2. **laptop_table** (`LID`, `brand`, `model`, `alien_id`)  

**laptop_table (Foreign Key Reference):**  
```
+-----+-------+----------+---------+
| LID | Brand |  Model   | AlienID |
+-----+-------+----------+---------+
|  1  | Dell  | XPS      |   101   |
|  2  | HP    | EliteBook|   101   |
+-----+-------+----------+---------+
```
- **Why is `alien_laptop` table gone?**  
  - `mappedBy = "alien"` tells Hibernate **not to create a separate table**.  
  - The relationship is stored **inside the `laptop_table`**.  

---

### **Key Takeaways**  
✅ **One-to-Many (`@OneToMany`)** creates a **third table** unless mapped in the child entity.  
✅ **Many-to-One (`@ManyToOne`)** makes the relationship **simpler**, avoiding unnecessary join tables.  
✅ **`mappedBy` prevents redundant tables**, keeping the relationship inside the child entity (`Laptop`).  

🔹 **Next: Many-to-Many Relationship!**

N)### **Hibernate Relationships: One-to-Many, Many-to-One, and Many-to-Many**

#### **1. One-to-Many & Many-to-One Relationship**
- **One `Alien` can have multiple `Laptops`** (One-to-Many)
- **Each `Laptop` belongs to one `Alien`** (Many-to-One)
- By default, Hibernate creates a **third table** for mapping
- To avoid the third table, we use `mappedBy` in the **parent** entity

#### **Code Implementation:**
##### **Alien.java** (Parent)
```java
@Entity
public class Alien {
    @Id
    private int AID;
    private String name;

    @OneToMany(mappedBy = "alien")  // Prevent extra join table
    private List<Laptop> laptops;
    
    // Getters and Setters
}
```
##### **Laptop.java** (Child)
```java
@Entity
public class Laptop {
    @Id
    private int LID;
    private String brand;
    private String model;

    @ManyToOne
    private Alien alien;
    
    // Getters and Setters
}
```
##### **Data Insertion:**
```java
Alien a1 = new Alien();
a1.setAID(101);
a1.setName("John");

Laptop l1 = new Laptop();
l1.setLID(1);
l1.setBrand("Dell");
l1.setModel("XPS");
l1.setAlien(a1);

Laptop l2 = new Laptop();
l2.setLID(2);
l2.setBrand("HP");
l2.setModel("EliteBook");
l2.setAlien(a1);

em.persist(a1);
em.persist(l1);
em.persist(l2);
```
##### **Database Structure:**
- `alien_table` (AID, name)
- `laptop_table` (LID, brand, model, alien_id)

---

### **2. Many-to-Many Relationship**
- **One `Alien` can have multiple `Laptops`**
- **One `Laptop` can belong to multiple `Aliens`**
- Without `mappedBy`, Hibernate creates **two mapping tables** (`alien_laptop` and `laptop_alien`)
- To avoid duplicate mapping, use `mappedBy` in **one** entity

#### **Code Implementation:**
##### **Alien.java**
```java
@Entity
public class Alien {
    @Id
    private int AID;
    private String name;

    @ManyToMany(mappedBy = "aliens")  // Avoid duplicate mapping table
    private List<Laptop> laptops;
    
    // Getters and Setters
}
```
##### **Laptop.java**
```java
@Entity
public class Laptop {
    @Id
    private int LID;
    private String brand;
    private String model;

    @ManyToMany
    private List<Alien> aliens;
    
    // Getters and Setters
}
```
##### **Data Insertion:**
```java
Alien a1 = new Alien();
a1.setAID(101);
a1.setName("John");

Alien a2 = new Alien();
a2.setAID(102);
a2.setName("Harsh");

Laptop l1 = new Laptop();
l1.setLID(1);
l1.setBrand("Dell");
l1.setModel("XPS");

Laptop l2 = new Laptop();
l2.setLID(2);
l2.setBrand("HP");
l2.setModel("EliteBook");

// Assign relationships
a1.setLaptops(List.of(l1, l2));
a2.setLaptops(List.of(l2));

l1.setAliens(List.of(a1));
l2.setAliens(List.of(a1, a2));

em.persist(a1);
em.persist(a2);
em.persist(l1);
em.persist(l2);
```
##### **Database Structure:**
1. `alien_table` (AID, name)
2. `laptop_table` (LID, brand, model)
3. `alien_laptop` (AID, LID) → Mapping table

##### **Why remove `laptop_alien` table?**
- Both entities tried to create a mapping table.
- Using `mappedBy = "aliens"` in `Alien` prevents duplication.

---

### **Key Takeaways**
✅ **One-to-Many (`@OneToMany`)** creates a join table unless `mappedBy` is used.
✅ **Many-to-One (`@ManyToOne`)** is stored as a foreign key in the child table.
✅ **Many-to-Many (`@ManyToMany`)** creates a mapping table.
✅ **Using `mappedBy` prevents duplicate mapping tables.**

O)Here’s a structured summary of the  **Eager vs. Lazy Fetching in Hibernate**:

---

## **1. Code Changes Before Discussing Fetching**
- **Reverted ManyToMany to OneToMany:**
  - One alien can have multiple laptops.
  - A laptop belongs to only one alien.
  - Removed the `@ManyToMany` mapping from the `Laptop` entity.
- **Updated Data Structure:**
  - Alien 1 → Laptops 1, 2
  - Alien 2 → Laptop 3
  - Removed Alien 3.

---

## **2. Understanding Hibernate Caching**
- Hibernate provides **Level 1 Cache** (L1 Cache) by default:
  - Data is cached within the same session.
  - If an entity is retrieved multiple times in the same session, Hibernate won’t query the database again.
- **To test cache behavior:**
  - Created a **new session** (`session1`).
  - Retrieved the same alien entity.
  - This time, Hibernate fired a SQL query because L1 Cache works only within the same session.
- **Level 2 Cache** (L2 Cache) allows sharing data across multiple sessions.
  - Needs external libraries like **EHCache** or **Caffeine** (not covered in this video).

---

## **3. Lazy Fetching (Default Behavior)**
- By default, Hibernate uses **Lazy Fetching** for collections.
- Example:
  - Fetching an Alien only retrieves **Alien details** (`id`, `name`, `tech`).
  - **Laptop details are not fetched** immediately.
- The laptop data is fetched **only when explicitly accessed** (e.g., printing the list of laptops).
- Hibernate dynamically fires a **separate SQL query** when the laptop list is accessed.

### **Why Lazy Fetching?**
- **Performance Optimization**: Prevents unnecessary data retrieval.
- **Avoids Expensive Queries**: Only fetches related data when needed.

---

## **4. Eager Fetching**
- **Forces Hibernate to load related data immediately.**
- To enable:
  ```java
  @OneToMany(mappedBy="alien", fetch=FetchType.EAGER)
  private List<Laptop> laptops;
  ```
- Now, even if the laptop list isn’t accessed, Hibernate fetches **Alien + Laptop** data in one SQL query.

### **Why Eager Fetching?**
- Useful when related data is always required.
- Avoids additional SQL queries when accessing related entities.

### **Is Eager Fetching a Good Idea?**
❌ **Not recommended for large collections** (e.g., thousands of records).
✅ **Best for small, essential data** that is always needed.

---

### **Final Takeaways**
1. **Lazy Fetching (Default for Collections):** Loads data only when accessed.
2. **Eager Fetching:** Forces immediate data loading.
3. **Use Lazy Fetching for large collections** to avoid performance issues.
4. **Level 1 Cache (default):** Works within a session.
5. **Level 2 Cache (optional):** Shares cache across sessions using external libraries.

P)
Here’s a **structured summary** of the caching concepts explained in the video:

---

## **1. Overview of How Data Fetching Works**
- Consider a **3-layer architecture**:
  - **Client (React)**
  - **Server (Spring Boot + Hibernate)**
  - **Database**
- When a user sends a request (e.g., fetching **Alien with ID 101**):
  1. The **request** goes from the client to the **server**.
  2. The server opens a **session** and checks if the data is available.
  3. If not found, the **query is sent to the database**.
  4. Database returns the **data**, and Hibernate stores it in the session.
  5. The **response** is sent back to the client.

---

## **2. Level 1 Cache (L1 Cache)**
### **How It Works**
- **Session-scoped cache**: Works **only within a single session**.
- Example:
  - Fetch **Alien 101** → Query fired to the **database**.
  - Fetch **Alien 102** → Query fired to the **database**.
  - Fetch **Alien 101 again in the same session** → No query fired, Hibernate **returns cached data**.

### **Advantages of L1 Cache**
✅ Reduces **duplicate queries** in the same session.  
✅ Improves **performance** by avoiding unnecessary database hits.  

### **Potential Issue**
- **Stale Data Problem**:  
  - If **Alien 101 is modified** by another transaction **before the second request**, the **cache may return outdated data**.

### **Key Takeaways**
- **L1 Cache is enabled by default** in Hibernate.
- **It works only within the same session**.
- **When the session closes, the cache is lost**.

---

## **3. Level 2 Cache (L2 Cache)**
### **What It Solves**
- L1 Cache is session-specific. If a **new session (S2)** is created, it won’t reuse data from **session S1**.
- **L2 Cache allows sharing cached data across multiple sessions.**

### **How It Works**
- Uses an **external caching provider**.
- Hibernate supports **JCache** (Java Caching API) as a standard.
- Common L2 Cache implementations:
  - **EHCache**
  - **Caffeine**
  - **Hazelcast**
  - **Redis**

### **When to Use L2 Cache?**
✅ If the same data is frequently **requested across multiple sessions**.  
✅ If reducing database **load** is a priority.  

### **When Not to Use L2 Cache?**
❌ If **data changes frequently**, caching might serve stale data.  
❌ If **real-time consistency** is required (e.g., financial transactions).  

---

## **4. Final Takeaways**
1. **L1 Cache**: Enabled by default, session-scoped.
2. **L2 Cache**: Requires an external provider, allows cross-session caching.
3. **Caching improves performance** but can lead to stale data.
4. **Use caching wisely**—evaluate whether your data changes frequently.

Q)
### **Hibernate Query Language (HQL) - Summary**

#### **1. What is HQL?**
- HQL stands for **Hibernate Query Language**.
- It is **similar to SQL** but works with **Java objects instead of database tables**.
- HQL queries are independent of the database, meaning they can run on different DBMSs (MySQL, PostgreSQL, Oracle, etc.).

---

#### **2. Why HQL Instead of SQL?**
- **SQL (Structured Query Language)** works with:
  - Tables
  - Columns
  - Raw database records

- **HQL (Hibernate Query Language)** works with:
  - Entities (Java objects mapped to tables)
  - Properties (Java class fields instead of column names)

**Example:**
- SQL:  
  ```sql
  SELECT * FROM student;
  ```
- HQL:  
  ```hql
  FROM Student
  ```
  - **No need to write `SELECT *`**; Hibernate understands you want all fields.
  - `Student` is the **entity name**, not the table name.

---

#### **3. CRUD Operations in Hibernate**
- Hibernate provides simple methods for basic operations:
  - **Read:** `session.get(Student.class, primaryKey)`
  - **Delete:** `session.delete(studentObject)`
  - **Create:** `session.save(studentObject)`
  - **Update:** `session.update(studentObject)`

✅ These methods are easy but **work mostly with primary keys**.

❓ **What if you need complex queries?**
- Example: **Search students by name or country**
- Here, **HQL** helps by allowing **custom queries**.

---

#### **4. HQL vs Native Queries**
| Feature  | HQL  | Native SQL |
|----------|------|------------|
| Works with | Entities & properties | Tables & columns |
| Database-independent | ✅ Yes | ❌ No |
| Syntax | Similar to SQL | Standard SQL |
| Flexibility | High | Full SQL power |

- **If you need database-specific features**, use **Native SQL**.
- **For cross-database compatibility, use HQL**.

---

#### **5. Key Differences Between SQL and HQL**
| SQL Example | HQL Equivalent |
|------------|----------------|
| `SELECT * FROM student;` | `FROM Student` |
| `SELECT name FROM student;` | `SELECT s.name FROM Student s` |
| `SELECT * FROM student WHERE country='India';` | `FROM Student s WHERE s.country='India'` |

---

#### **6. Next Steps**
- The upcoming videos will **demonstrate how to write and execute HQL queries** in Hibernate.
- You'll see how **HQL can simplify database operations while keeping Java objects in focus**.

R)### **Implementing HQL in Hibernate - Step-by-Step**

#### **1. Preparing the Project**
- We are using the **Laptop** entity which has:
  - `lid` (Primary Key)
  - `brand`
  - `model`
  - `ram`
- The goal is to **fetch laptops from the database** using HQL.

---

#### **2. Insert Sample Data into Database**
- Before querying, let's insert a sample **Laptop** record.
- Modify the Hibernate configuration to update instead of creating the schema:
  ```xml
  <property name="hibernate.hbm2ddl.auto">update</property>
  ```
- **Adding a laptop entry:**
  ```java
  SessionFactory factory = new Configuration().configure().buildSessionFactory();
  Session session = factory.openSession();
  Transaction tx = session.beginTransaction();

  Laptop l1 = new Laptop();
  l1.setLid(4);
  l1.setBrand("Asus");
  l1.setModel("Strix");
  l1.setRam(32);

  session.persist(l1);
  tx.commit();

  session.close();
  factory.close();
  ```

✅ Now, we have sample data in the database.

---

#### **3. Fetching Data Using `session.get()`**
- If you **only need one record**, you can fetch by **Primary Key** using `session.get()`.
  ```java
  Session session = factory.openSession();

  Laptop l1 = session.get(Laptop.class, 3);
  System.out.println(l1);

  session.close();
  ```
✅ This works **only with primary keys**.

---

#### **4. Fetching Data Using HQL**
- To **fetch records based on other attributes** (e.g., `ram = 32`), use **HQL**.
- **SQL vs HQL Comparison:**
  - SQL:
    ```sql
    SELECT * FROM laptop WHERE ram = 32;
    ```
  - HQL:
    ```hql
    FROM Laptop WHERE ram = 32
    ```

- **Implementation in Java:**
  ```java
  Session session = factory.openSession();

  String hql = "FROM Laptop WHERE ram = 32";
  Query<Laptop> query = session.createQuery(hql, Laptop.class);
  List<Laptop> laptops = query.getResultList();

  for (Laptop laptop : laptops) {
      System.out.println(laptop);
  }

  session.close();
  ```

✅ **HQL automatically maps results to Java objects** (no manual conversion needed like in JDBC).

---

#### **5. Filtering with User Input**
- Instead of hardcoding `ram = 32`, we can pass values dynamically using **parameter binding**.
- **Example:**
  ```java
  int ramSize = 32; // This value can come from user input

  String hql = "FROM Laptop WHERE ram = :ramValue";
  Query<Laptop> query = session.createQuery(hql, Laptop.class);
  query.setParameter("ramValue", ramSize);

  List<Laptop> laptops = query.getResultList();

  for (Laptop laptop : laptops) {
      System.out.println(laptop);
  }
  ```

✅ **Advantages of Parameter Binding:**
- Prevents **SQL injection**.
- Allows **dynamic values** from user input or external sources.

---

#### **6. Fetching Specific Columns Instead of Entire Object**
- The above queries return **full Laptop objects**.
- What if we **only want `brand` and `ram`** instead of all columns?

**SQL Equivalent:**
```sql
SELECT brand, ram FROM laptop WHERE ram = 32;
```

**HQL Implementation:**
```java
String hql = "SELECT l.brand, l.ram FROM Laptop l WHERE l.ram = :ramValue";
Query<Object[]> query = session.createQuery(hql, Object[].class);
query.setParameter("ramValue", 32);

List<Object[]> results = query.getResultList();
for (Object[] row : results) {
    System.out.println("Brand: " + row[0] + ", RAM: " + row[1]);
}
```

✅ **HQL allows selecting specific properties** instead of fetching entire entities.

---

### **Summary**
| Feature | SQL | HQL |
|---------|-----|-----|
| Works with | Tables & columns | Entities & properties |
| Fetch all rows | `SELECT * FROM laptop` | `FROM Laptop` |
| Fetch with filter | `SELECT * FROM laptop WHERE ram=32` | `FROM Laptop WHERE ram=32` |
| Fetch specific columns | `SELECT brand, ram FROM laptop WHERE ram=32` | `SELECT l.brand, l.ram FROM Laptop l WHERE l.ram=32` |

---

### **Next Steps**
1. **Fetching only a few columns** instead of full objects.
2. **Passing user inputs dynamically** in HQL queries.
3. **Using aggregate functions (COUNT, AVG, etc.)** in HQL.

S)### **Advanced HQL Queries in Hibernate**

#### **1. Fetching Data Using `brand` Instead of `ram`**
Instead of filtering by `ram`, we can filter by `brand`.

**HQL Query:**
```hql
FROM Laptop WHERE brand = 'Asus'
```

**Implementation in Java:**
```java
String hql = "FROM Laptop WHERE brand = 'Asus'";
Query<Laptop> query = session.createQuery(hql, Laptop.class);
List<Laptop> laptops = query.getResultList();

for (Laptop laptop : laptops) {
    System.out.println(laptop);
}
```
✅ This fetches all laptops with brand `Asus`.

---

#### **2. Using a Variable Instead of Hardcoded Value**
Instead of hardcoding `brand = 'Asus'`, we can use a variable.

```java
String brand = "Asus";
String hql = "FROM Laptop WHERE brand = :brandValue";
Query<Laptop> query = session.createQuery(hql, Laptop.class);
query.setParameter("brandValue", brand);

List<Laptop> laptops = query.getResultList();
for (Laptop laptop : laptops) {
    System.out.println(laptop);
}
```
✅ This allows dynamic brand filtering.

---

#### **3. Using `LIKE` Instead of `=` for String Matching**
To find all brands containing "Asus", we use `LIKE`.

```java
String brandPattern = "%Asus%";
String hql = "FROM Laptop WHERE brand LIKE :brandPattern";
Query<Laptop> query = session.createQuery(hql, Laptop.class);
query.setParameter("brandPattern", brandPattern);

List<Laptop> laptops = query.getResultList();
for (Laptop laptop : laptops) {
    System.out.println(laptop);
}
```
✅ `%Asus%` fetches brands containing "Asus" anywhere in the name.

---

#### **4. Using Ordinal Parameters (Numbered Placeholders)**
Instead of named parameters, HQL allows **numbered placeholders (`?1`, `?2`)**.

```java
String brand = "Asus";
int ramSize = 32;
String hql = "FROM Laptop WHERE brand = ?1 AND ram = ?2";
Query<Laptop> query = session.createQuery(hql, Laptop.class);
query.setParameter(1, brand);
query.setParameter(2, ramSize);

List<Laptop> laptops = query.getResultList();
for (Laptop laptop : laptops) {
    System.out.println(laptop);
}
```
✅ Numbered placeholders (`?1`, `?2`) help avoid confusion when using multiple parameters.

---

#### **5. Fetching a Single Field Instead of Full Objects**
To fetch only the `model` column instead of full `Laptop` objects:

```java
String hql = "SELECT l.model FROM Laptop l WHERE l.brand = :brandValue";
Query<String> query = session.createQuery(hql, String.class);
query.setParameter("brandValue", "Asus");

List<String> models = query.getResultList();
for (String model : models) {
    System.out.println(model);
}
```
✅ This returns a **list of model names** instead of `Laptop` objects.

---

#### **6. Fetching Multiple Fields (Brand & Model)**
To fetch both `brand` and `model`:

```java
String hql = "SELECT l.brand, l.model FROM Laptop l WHERE l.brand = :brandValue";
Query<Object[]> query = session.createQuery(hql, Object[].class);
query.setParameter("brandValue", "Asus");

List<Object[]> results = query.getResultList();
for (Object[] row : results) {
    System.out.println("Brand: " + row[0] + ", Model: " + row[1]);
}
```
✅ This returns a **list of object arrays** (each array contains `brand` and `model`).

---

#### **7. Type Casting When Fetching Multiple Fields**
Since HQL returns `Object[]`, we need to **typecast before printing**.

```java
for (Object[] row : results) {
    String brand = (String) row[0];
    String model = (String) row[1];
    System.out.println("Brand: " + brand + ", Model: " + model);
}
```
✅ **Typecasting ensures correct data extraction**.

---

### **Summary**
| Task | SQL | HQL |
|------|-----|-----|
| Fetch by brand | `SELECT * FROM laptop WHERE brand='Asus'` | `FROM Laptop WHERE brand='Asus'` |
| Fetch with `LIKE` | `SELECT * FROM laptop WHERE brand LIKE '%Asus%'` | `FROM Laptop WHERE brand LIKE :brandPattern` |
| Fetch specific field | `SELECT model FROM laptop WHERE brand='Asus'` | `SELECT l.model FROM Laptop l WHERE l.brand=:brandValue` |
| Fetch multiple fields | `SELECT brand, model FROM laptop` | `SELECT l.brand, l.model FROM Laptop l` |
| Using placeholders | `WHERE brand = ? AND ram = ?` | `WHERE brand = ?1 AND ram = ?2` |

---

### **Key Takeaways**
✅ Use **named parameters (`:param`)** instead of hardcoding values.
✅ Use **LIKE** for pattern matching instead of strict `=` comparison.
✅ Use **`Object[]` for multiple fields** and typecast properly.
✅ Numbered placeholders (`?1`, `?2`) help when using multiple parameters.
✅ Use **list of objects when fetching multiple fields**, otherwise use **list of strings** when fetching a single column.


T)**Hibernate: get() vs load() (Eager vs Lazy Fetching)**

### 1. Introduction
- In Hibernate, two methods fetch entities: `get()` and `load()`.
- `get()` performs **eager loading**.
- `load()` performs **lazy loading**.
- `load()` is deprecated, but an alternative (`byId().getReference()`) is available.

---

### 2. Using `get()` (Eager Loading)

```java
Laptop laptop = session.get(Laptop.class, 2);
System.out.println(laptop);
```

- **Behavior:**
  - Fires a SQL query immediately.
  - Fetches the entity from the database.
  - Returns `null` if the entity is not found.

- **Example Execution:**
  - If `id = 2`, SQL query is executed immediately.
  - Even if we do not print or use `laptop`, query is still executed.

**Console Output:**
```sql
select * from Laptop where id = 2;
```

---

### 3. Using `load()` (Lazy Loading)

```java
Laptop laptop = session.load(Laptop.class, 2);
System.out.println(laptop);
```

- **Behavior:**
  - Does **not** fire the query immediately.
  - Returns a proxy object (a placeholder).
  - Fires the query **only when we access the entity's data**.
  - If the entity does not exist, it throws `ObjectNotFoundException`.

**Example Execution:**
  - If `id = 2`, Hibernate **does not fire a SQL query until** `System.out.println(laptop)` is executed.

**Console Output:**
- If **not printed:** No SQL query.
- If **printed:**
```sql
select * from Laptop where id = 2;
```

⚠️ `load()` is **deprecated**, so we should use `byId().getReference()` instead.

---

### 4. Alternative to `load()`: `byId().getReference()`

```java
Laptop laptop = session.byId(Laptop.class).getReference(2);
System.out.println(laptop);
```

- **Behavior:**
  - Works like `load()`, i.e., **lazy loading**.
  - Does not execute a query until the object is accessed.
  - Not deprecated.

**Execution Behavior:**
- If **not printed:** No SQL query.
- If **printed:** Fires the query.

**Console Output:**
```sql
select * from Laptop where id = 2;
```

---

### 5. Summary of Differences

| Feature        | `get()` (Eager) | `load()` (Lazy) | `byId().getReference()` (Lazy) |
|--------------|--------------|-------------|----------------------|
| Query Execution | Immediate | Delayed until accessed | Delayed until accessed |
| Proxy Object | No | Yes | Yes |
| Returns `null` if not found | Yes | No (Throws Exception) | No (Throws Exception) |
| Deprecated | No | Yes | No |

### 6. Best Practices
- Use `get()` when you **need** the entity immediately.
- Use `byId().getReference()` for **lazy loading** instead of `load()`.
- Be cautious when using lazy loading to avoid `LazyInitializationException` (when accessing a proxy object outside a transaction).


U)**Hibernate: get() vs load() (Eager vs Lazy Fetching)**

### 1. Introduction
- In Hibernate, two methods fetch entities: `get()` and `load()`.
- `get()` performs **eager loading**.
- `load()` performs **lazy loading**.
- `load()` is deprecated, but an alternative (`byId().getReference()`) is available.

---

### 2. Using `get()` (Eager Loading)

```java
Laptop laptop = session.get(Laptop.class, 2);
System.out.println(laptop);
```

- **Behavior:**
  - Fires a SQL query immediately.
  - Fetches the entity from the database.
  - Returns `null` if the entity is not found.

- **Example Execution:**
  - If `id = 2`, SQL query is executed immediately.
  - Even if we do not print or use `laptop`, query is still executed.

**Console Output:**
```sql
select * from Laptop where id = 2;
```

---

### 3. Using `load()` (Lazy Loading)

```java
Laptop laptop = session.load(Laptop.class, 2);
System.out.println(laptop);
```

- **Behavior:**
  - Does **not** fire the query immediately.
  - Returns a proxy object (a placeholder).
  - Fires the query **only when we access the entity's data**.
  - If the entity does not exist, it throws `ObjectNotFoundException`.

**Example Execution:**
  - If `id = 2`, Hibernate **does not fire a SQL query until** `System.out.println(laptop)` is executed.

**Console Output:**
- If **not printed:** No SQL query.
- If **printed:**
```sql
select * from Laptop where id = 2;
```

⚠️ `load()` is **deprecated**, so we should use `byId().getReference()` instead.

---

### 4. Alternative to `load()`: `byId().getReference()`

```java
Laptop laptop = session.byId(Laptop.class).getReference(2);
System.out.println(laptop);
```

- **Behavior:**
  - Works like `load()`, i.e., **lazy loading**.
  - Does not execute a query until the object is accessed.
  - Not deprecated.

**Execution Behavior:**
- If **not printed:** No SQL query.
- If **printed:** Fires the query.

**Console Output:**
```sql
select * from Laptop where id = 2;
```

---

### 5. Summary of Differences

| Feature        | `get()` (Eager) | `load()` (Lazy) | `byId().getReference()` (Lazy) |
|--------------|--------------|-------------|----------------------|
| Query Execution | Immediate | Delayed until accessed | Delayed until accessed |
| Proxy Object | No | Yes | Yes |
| Returns `null` if not found | Yes | No (Throws Exception) | No (Throws Exception) |
| Deprecated | No | Yes | No |

### 6. Best Practices
- Use `get()` when you **need** the entity immediately.
- Use `byId().getReference()` for **lazy loading** instead of `load()`.
- Be cautious when using lazy loading to avoid `LazyInitializationException` (when accessing a proxy object outside a transaction).



