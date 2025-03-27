 A)

---

## **Spring Data JPA - Introduction**  

### **1. Understanding the Layers in a Spring Boot Application**  
- **Controller Layer**: Handles **HTTP requests** and returns responses.  
- **Service Layer**: Contains **business logic**.  
- **Repository Layer**: Communicates with the **database** (but we haven't connected to a database yet).  

---

### **2. Why Do We Need a Database?**  
- So far, we’ve built the **job portal** without storing data.  
- Now, we need to **integrate a database** to persist and retrieve data.  

---

### **3. Different Ways to Work with Databases in Spring**  
1. **JDBC**: Write raw SQL queries using `JdbcTemplate`.  
2. **Spring JDBC**: A simpler way to use JDBC with Spring.  
3. **Spring ORM**: Uses Hibernate but requires **manual configurations**.  
4. **Spring Data JPA**: The **simplest approach**, reducing boilerplate code.  

---

### **4. Why Use Spring Data JPA?**  
- **Eliminates writing SQL queries** (uses ORM).  
- **Works seamlessly with Hibernate**, making it easier to work with relational databases.  
- **Automatically generates database operations** like `save()`, `findAll()`, `deleteById()`, etc.  

---

### **5. Example - Moving from JDBC to JPA**  
- Previously, we used **Spring JDBC** for a **student project**:  
  - Stored **Student (id, name, marks)** in **H2/PostgreSQL**.  
  - Used `JdbcTemplate` to write **manual SQL queries**.  
- Now, with **Spring Data JPA**, we will **avoid writing SQL manually**.  

---

### **6. What is ORM (Object-Relational Mapping)?**  
- Instead of writing **SQL**, we use **Java objects** to interact with the database.  
- ORM **automatically converts** Java objects into SQL operations.  
- **Hibernate** is the most popular ORM framework.  

---

### **7. Next Step: Understanding ORM and JPA**  
- In the next section, we’ll first discuss **ORM concepts**.  
- Then, we’ll move to **Spring Data JPA** and integrate it into the **job portal project**.  

That’s the introduction to **Spring Data JPA**. Next, we’ll cover **ORM in detail**!


B)
### **ORM (Object-Relational Mapping) - Explanation**  

---

## **1. What is ORM?**  
**ORM stands for Object-Relational Mapping.**  
- Java is an **Object-Oriented** language, where everything is represented as objects.  
- A **database stores data in a tabular format (rows & columns)**.  
- **ORM bridges this gap** by mapping Java objects to database tables automatically.  

---

## **2. How Java and Databases Work Together**  
### **In Java (Object-Oriented World)**  
- Everything is modeled as **objects**.  
- Example: A `Student` class with:  
  ```java
  class Student {
      int rollNo;
      String name;
      int marks;
  }
  ```
- We create multiple objects (students), each holding different data.  

### **In Databases (Relational World)**  
- Data is stored in **tables**.  
- Example: A `student` table with columns:  
  ```
  +--------+--------+-------+
  | RollNo | Name   | Marks |
  +--------+--------+-------+
  | 1      | Alice  | 90    |
  | 2      | Bob    | 85    |
  +--------+--------+-------+
  ```
- Data is stored in **rows** (each row represents a student).  

---

## **3. How ORM Helps?**  
- ORM **automatically maps** a Java class to a database table.  
- Example:  
  - **Class Name → Table Name** (`Student` → `student`)  
  - **Class Properties → Table Columns** (`rollNo`, `name`, `marks`)  
  - **Object → Row in Table**  
- ORM eliminates **manual SQL queries**, making development easier.  

---

## **4. Popular ORM Tools**  
There are multiple ORM frameworks available:  
1. **Hibernate** (most popular)  
2. **TopLink**  
3. **EclipseLink**  

---

## **5. Hibernate - The Most Popular ORM Tool**  
- Hibernate **automates table creation and object persistence**.  
- Example: Storing a `Student` object in the database:  
  ```java
  Student student = new Student(1, "Alice", 90);
  session.save(student);
  ```
  - Hibernate **creates a new row** in the table.  
- Retrieving data as Java objects:  
  ```java
  Student student = session.get(Student.class, 1);
  ```
  - Hibernate fetches the **database row** and **converts it into a Java object**.  

---

## **6. Where Does JPA Fit?**  
- **JPA (Java Persistence API) is a standard specification** for ORM in Java.  
- Hibernate is an **implementation** of JPA.  
- JPA provides a **common interface**, so even if we switch from Hibernate to another ORM tool, we don’t need to change much code.  

---

## **7. How Spring Data JPA Simplifies This?**  
- **Spring ORM** integrates Hibernate with the Spring framework.  
- **Spring Data JPA** further **simplifies** database operations:  
  - **No need to write boilerplate code** for Hibernate.  
  - **CRUD operations** are provided out-of-the-box.  
  - Uses **JPA Repository** to interact with the database easily.  

---

## **Next Step: Using Spring Data JPA**  
Now that we understand ORM and JPA, let's see how **Spring Data JPA** makes database operations even easier!

C)


---

## **Spring Data JPA Introduction**

### **1. Difference Between JDBC and Spring Data JPA**
- In JDBC, we manually write SQL queries and create tables (e.g., `schema.sql`).
- In Spring Data JPA, the framework automatically manages table creation and query execution using **Hibernate** (an implementation of JPA).

### **2. Creating a New Spring Boot Project**
- **Using Spring Initializr**
  - **Group ID:** `com.tolusko`
  - **Artifact ID:** `spring-data-jpa-example`
  - **Java Version:** 21
  - **Packaging:** Jar
  - **Dependencies:**
    - `Spring Data JPA` (for persistence using Hibernate)
    - `PostgreSQL Driver` (for database connectivity)

- The project will be a **console application**, so no web dependencies are included.

### **3. Setting Up the Project**
1. **Copy Model Class (`Student`)**
   - Create a `model` package and add the `Student` class.
   - Fields: `rollNumber`, `name`, `marks`.

2. **Database Configuration (`application.properties`)**
   - Configure **PostgreSQL connection**.
   - Ensure there is no existing `student` table in the database.

3. **Creating the JPA Repository**
   - Instead of writing SQL queries, we create an interface that extends `JpaRepository`.
   - Example:
     ```java
     @Repository
     public interface StudentRepo extends JpaRepository<Student, Integer> {}
     ```
   - `JpaRepository<Student, Integer>`:
     - `Student`: Entity class to be managed.
     - `Integer`: Type of primary key.

4. **Marking `Student` as an Entity**
   - Add `@Entity` to `Student` to let Hibernate manage it.
   - Mark `rollNumber` as `@Id` (Primary Key).
   - Example:
     ```java
     @Entity
     public class Student {
         @Id
         private int rollNumber;
         private String name;
         private int marks;
     }
     ```

5. **Initializing and Saving Data**
   - Fetch `StudentRepo` from the Spring context.
   - Create student objects and save them using `save()`.
   - Example:
     ```java
     ApplicationContext context = SpringApplication.run(Main.class, args);
     StudentRepo repo = context.getBean(StudentRepo.class);

     Student s1 = new Student(101, "John", 85);
     repo.save(s1);
     ```

### **4. Automatic Table Creation**
- Spring Data JPA **creates the table automatically** when the application starts.
- The `application.properties` settings:
  ```properties
  spring.jpa.hibernate.ddl-auto=update
  spring.jpa.show-sql=true
  ```
  - `ddl-auto=update`: Creates the table if not present, otherwise updates it.
  - `show-sql=true`: Logs the SQL queries executed by Hibernate.

### **5. Execution & Results**
- Running the application:
  - Hibernate generates and executes SQL:
    ```sql
    CREATE TABLE student (
      roll_number INTEGER PRIMARY KEY,
      name VARCHAR(255),
      marks INTEGER
    );
    INSERT INTO student (roll_number, name, marks) VALUES (101, 'John', 85);
    ```
  - Verified in **PGAdmin** (PostgreSQL UI).

### **Conclusion**
- **No need to write SQL queries manually**.
- **Spring Data JPA automates table creation and query execution**.
- **Next topic**: Fetching data from the database.

---

D)
 

---

### **Spring Data JPA – Fetching Data from the Database**  

#### **1. Saving Multiple Records**  
- Instead of saving just `S1`, we save multiple student objects (`S1`, `S2`, `S3`).  
- Restarting the application doesn’t create the table again since it already exists.  
- Hibernate logs confirm that `INSERT` queries are executed for each student object.  
- PGAdmin verifies that all records are stored successfully.  

#### **2. Fetching All Records**  
- The `findAll()` method of `JpaRepository` retrieves all records.  
- Simply calling `repo.findAll()` prints all stored objects.  
- Running the application confirms that all student records are fetched and displayed.  

#### **3. Fetching Specific Records**  
- The next step is to fetch records based on specific parameters (e.g., roll number or name).  

---

E)
### **Spring Data JPA – Fetching a Specific Record Using `findById()`**  

#### **1. Fetching a Single Record by ID**  
- `findAll()` retrieves all records, but sometimes we need only one.  
- `findById(id)` fetches a record based on the **primary key**.  
- Example: Fetching the student with roll number `103`.  

#### **2. SQL Query Behind `findById()`**  
- Executes:  
  ```sql
  SELECT roll_number, marks, name FROM student WHERE roll_number = 103;
  ```
- The query filters results using the primary key.  

#### **3. Handling Null Values with `Optional`**  
- `findById(id)` returns an **Optional<Student>`, not just a `Student`.  
- This prevents `NullPointerException` if the record doesn’t exist.  
- Example: Searching for a non-existing `104` returns an **empty Optional**.  

#### **4. Using `orElse()` to Handle Missing Data**  
- If data exists, it returns the student object.  
- If not, `orElse(new Student())` returns a blank student object instead of `null`.  

#### **5. Java 8 `Optional` Concept**  
- Helps prevent `NullPointerException`.  
- Ensures safe handling of missing data.  

#### **Next Steps**  
- Learn how to **fetch records by non-primary key fields** like name or marks.  

---

F)
### **Spring Data JPA – Fetching Records by Non-Primary Key Fields**  

#### **1. Finding a Record by Name**  
- `findById(id)` works for primary keys, but what about other fields like `name`?  
- **Default JPA Repository** does not have a `findByName()` method.  

#### **2. Writing a Custom Query with JPQL**  
- We can define a custom method in the repository:  
  ```java
  @Query("SELECT s FROM Student s WHERE s.name = ?1")
  List<Student> findByName(String name);
  ```
- **JPQL (Java Persistence Query Language)** differs from SQL:  
  - Uses **entity class names** instead of table names.  
  - Uses **property names** instead of column names.  

#### **3. Magic of Spring Data JPA – Query DSL**  
- Without writing a query, **JPA can generate methods automatically**.  
- Just define the method in the repository:  
  ```java
  List<Student> findByName(String name);
  ```
- **Spring Data JPA automatically implements it!**  

#### **4. Finding by Marks**  
- Works similarly:  
  ```java
  List<Student> findByMarks(int marks);
  ```
- **Spring understands the method name and generates the query.**  

#### **5. Finding by Marks Greater Than a Value**  
- Use method naming conventions:  
  ```java
  List<Student> findByMarksGreaterThan(int marks);
  ```
- **Other options:**  
  - `findByMarksLessThan(int marks)`  
  - `findByMarksBetween(int min, int max)`  

#### **6. Summary – JPA Query DSL**  
- **Find by a field:** `findByFieldName(value)`  
- **Find by comparison:** `findByFieldNameGreaterThan(value)`  
- **Find by range:** `findByFieldNameBetween(min, max)`  

#### **Next Steps**  
- Learn **update and delete operations** in the next session.  

G)
### **Spring Data JPA – Updating and Deleting Records**  

#### **1. Updating a Record**  
- **No explicit `update` method in JPA.**  
- Use `save(entity)` method to update an existing record.  

**Example:** Updating student `S3` (Kiran’s marks from 80 to 65).  
```java
Student student = repo.findById(3).orElseThrow();
student.setMarks(65);
repo.save(student);
```
**How it works:**  
- First, **JPA fetches the record** (`SELECT query`).  
- If the record exists, **it updates the values** (`UPDATE query`).  
- If the record **does not exist**, it creates a new one (`INSERT query`).  

#### **2. Deleting a Record**  
- Use `delete(entity)` or `deleteById(id)`.  

**Example:** Deleting student `S2`.  
```java
Student student = repo.findById(2).orElseThrow();
repo.delete(student);
```
OR  
```java
repo.deleteById(2);
```

**How it works:**  
- **First, JPA checks if the record exists** (`SELECT query`).  
- If found, **it deletes the record** (`DELETE query`).  

#### **3. Summary – CRUD Operations in JPA**  

| Operation  | Method |
|------------|--------------------------------|
| Create     | `save(entity)` |
| Read       | `findById(id)`, `findAll()` |
| Update     | `save(entity)` (if record exists) |
| Delete     | `delete(entity)`, `deleteById(id)` |

#### **Next Steps**  
- Integrate Update and Delete functionality into the **Job Portal Application**.  

H)
### **Converting Job Application to Use JPA**  

#### **1. Adding Dependencies**  
- **Spring Data JPA** and **PostgreSQL** dependencies are added in `pom.xml`.  
- Ensure that Maven is reloaded properly after adding dependencies.

#### **2. Refactoring `JobRepo`**  
- Convert `JobRepo` from a **class with a list** to a **Spring Data JPA interface**.  
- Extend `JpaRepository<JobPost, Integer>`.  
- No need to write methods manually; built-in methods like `save()`, `findAll()`, `findById()`, and `deleteById()` handle operations.

```java
public interface JobRepo extends JpaRepository<JobPost, Integer> {
}
```

#### **3. Updating `JobService`**  
- Replace manual list operations with JPA repository methods.

```java
@Service
public class JobService {
    
    @Autowired
    private JobRepo repo;

    public JobPost addJob(JobPost job) {
        return repo.save(job); // Save for both create & update
    }

    public List<JobPost> getAllJobs() {
        return repo.findAll(); // Fetch all jobs
    }

    public JobPost getJobById(int postId) {
        return repo.findById(postId).orElse(new JobPost()); // Handle optional
    }

    public void deleteJob(int postId) {
        repo.deleteById(postId);
    }

    public String loadData() {
        List<JobPost> jobs = List.of(
            new JobPost(1, "Developer", "Backend Developer", 3, List.of("Java", "Spring")),
            new JobPost(2, "QA Engineer", "Testing apps", 2, List.of("Selenium", "JUnit"))
        );
        repo.saveAll(jobs);
        return "Success";
    }
}
```

#### **4. Adding Data Load API (`JobController`)**  
- Add a new endpoint to load initial job data.

```java
@RestController
@RequestMapping("/jobs")
public class JobController {

    @Autowired
    private JobService service;

    @GetMapping("/load")
    public String loadData() {
        return service.loadData(); // Loads default jobs into DB
    }
}
```

#### **5. Key Changes & Next Steps**  
- **Convert `JobPost` into a proper JPA Entity.**  
- **Add `@Entity` and `@Id` annotations.**  
- **Configure `application.properties` to connect to PostgreSQL.**  

I)
### **Finalizing JPA Integration in Job Application**  

#### **1. Convert `JobPost` to a JPA Entity**  
- Mark `JobPost` as a **JPA Entity** using `@Entity`.  
- Mark `postId` as the **Primary Key** using `@Id`.  

```java
import jakarta.persistence.*;

@Entity
public class JobPost {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY) // Auto-incremented ID
    private int postId;
    
    private String profile;
    private String description;
    private int experience;
    
    @ElementCollection // Used for storing a list in the DB
    private List<String> techStack;

    // Constructors, Getters & Setters
}
```

---

#### **2. Add Database Configuration (`application.properties`)**  
- Configure **PostgreSQL** in `application.properties`.  
- Enable **auto table creation** (`spring.jpa.hibernate.ddl-auto`).  
- Enable **SQL query logging** (`spring.jpa.show-sql`).  

```properties
spring.datasource.url=jdbc:postgresql://localhost:5432/jobportal
spring.datasource.username=postgres
spring.datasource.password=yourpassword
spring.datasource.driver-class-name=org.postgresql.Driver
spring.jpa.database-platform=org.hibernate.dialect.PostgreSQLDialect
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
```

---

#### **3. Verify Table Creation in PostgreSQL**  
- **Run the application** → `Tomcat started` ✅  
- **Check the logs** → `CREATE TABLE job_post ...` ✅  
- **Open pgAdmin** → Verify `job_post` table created ✅  

---

#### **4. Load Initial Data via API (`JobService`)**  
- Implement **`loadData()`** method to insert predefined jobs.

```java
@Service
public class JobService {
    
    @Autowired
    private JobRepo repo;

    public String loadData() {
        List<JobPost> jobs = List.of(
            new JobPost(1, "Developer", "Backend Developer", 3, List.of("Java", "Spring")),
            new JobPost(2, "QA Engineer", "Testing apps", 2, List.of("Selenium", "JUnit"))
        );
        repo.saveAll(jobs);
        return "Success";
    }
}
```

- Call **`GET /jobs/load`** in **Postman** → `Success` ✅  
- Verify data in **pgAdmin** → **Data is stored in DB** ✅  

---

#### **5. Fetch Data from Database**  
- **Call `GET /jobs`** → Fetch all jobs ✅  
- **Call `GET /jobs/2`** → Fetch job by ID ✅  
- **Update/Delete jobs using JPA methods** ✅  

---

### **Next Steps**  
Now that the basic JPA integration is complete, you can:  
- **Implement custom queries** (e.g., find jobs by profile, experience).  
- **Improve database design** (store `techStack` in a separate table).  
- **Add exception handling** (handle missing job entries).  

J)
### **Implementing Search Functionality in Job Portal using Spring Data JPA**  

We will now enhance our **Job Portal** backend by adding **search functionality** based on job profile and description.

---

### **1. Update Repository (`JobRepo`)**
- **Spring Data JPA** allows you to define methods with naming conventions instead of writing raw SQL queries.
- Here, we will **search for jobs** where either `profile` or `description` **contains** a given keyword.

#### **Implementation:**
```java
import org.springframework.data.jpa.repository.JpaRepository;
import java.util.List;

public interface JobRepo extends JpaRepository<JobPost, Integer> {
    List<JobPost> findByProfileContainingOrDescriptionContaining(String profile, String description);
}
```

- `findByProfileContainingOrDescriptionContaining(String profile, String description)`  
  - Searches for jobs where the `profile` **contains** the keyword OR the `description` **contains** the keyword.

---

### **2. Implement Service Method (`JobService`)**
- We will call the repository method from the service layer.

#### **Implementation:**
```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import java.util.List;

@Service
public class JobService {

    @Autowired
    private JobRepo repo;

    public List<JobPost> searchJobs(String keyword) {
        return repo.findByProfileContainingOrDescriptionContaining(keyword, keyword);
    }
}
```

- Calls `findByProfileContainingOrDescriptionContaining()` and passes the keyword for both fields.

---

### **3. Add Controller Method (`JobController`)**
- We will expose a **GET API** to search jobs by a keyword.

#### **Implementation:**
```java
import org.springframework.web.bind.annotation.*;

import java.util.List;

@RestController
@RequestMapping("/jobpost")
public class JobController {

    @Autowired
    private JobService service;

    @GetMapping("/keyword/{keyword}")
    public List<JobPost> searchJobs(@PathVariable String keyword) {
        return service.searchJobs(keyword);
    }
}
```

- **API Endpoint:** `GET /jobpost/keyword/{keyword}`  
- **Example Requests:**
  - `/jobpost/keyword/Java` → Returns jobs with **"Java"** in profile or description.
  - `/jobpost/keyword/API` → Returns jobs containing **"API"**.

---

### **4. Testing with Postman**
✅ **Search for "Java"**
- `GET http://localhost:8080/jobpost/keyword/Java`
- **Response:**
```json
[
  {
    "postId": 1,
    "profile": "Java Developer",
    "description": "Spring Boot Expert",
    "experience": 3,
    "techStack": ["Java", "Spring"]
  }
]
```

✅ **Search for "API"**
- `GET http://localhost:8080/jobpost/keyword/API`
- **Response:**
```json
[
  {
    "postId": 2,
    "profile": "Backend Engineer",
    "description": "REST API development",
    "experience": 4,
    "techStack": ["Node.js", "Spring Boot"]
  },
  {
    "postId": 3,
    "profile": "Full Stack Developer",
    "description": "Developing APIs and Frontend",
    "experience": 5,
    "techStack": ["React", "Spring Boot"]
  }
]
```

---

### **5. Summary of Features Implemented**
✔ **Find jobs based on text search in `profile` and `description`**  
✔ **Implemented search using Spring Data JPA method naming conventions**  
✔ **Exposed REST API for search functionality**  
✔ **Tested using Postman for different keywords**  

---

### **Next Steps**
- We can also implement **pagination** for better results if the dataset grows large.  


K)
### **Integrating Search Functionality in React Frontend with Spring Boot Backend**  

Now that we have implemented the **search feature** in Spring Boot, let's integrate it into a **React frontend**.

---

## **1. Backend Recap**  
We have created a **REST API** in Spring Boot:
- **Endpoint:** `GET /jobpost/keyword/{keyword}`
- **Functionality:** Searches for job posts where **profile** or **description** contains the given keyword.

---

## **2. React Frontend Setup**
- Install required dependencies:
  ```bash
  npm install
  ```

- Start the frontend:
  ```bash
  npm start
  ```

Ensure the backend is running on **localhost:8080** and frontend on **localhost:3000**.

---

## **3. Implementing Search in React**
We'll use:
- `useState` for managing the search query.
- `useEffect` for triggering the API call on each keystroke.

### **Search Component (`SearchBar.js`)**
```jsx
import React, { useState, useEffect } from "react";

const SearchBar = ({ onSearch }) => {
  const [query, setQuery] = useState("");

  useEffect(() => {
    if (query.trim() !== "") {
      onSearch(query);
    }
  }, [query, onSearch]);

  return (
    <input
      type="text"
      placeholder="Search job posts..."
      value={query}
      onChange={(e) => setQuery(e.target.value)}
      style={{ padding: "10px", width: "300px", fontSize: "16px" }}
    />
  );
};

export default SearchBar;
```
- Calls `onSearch(query)` every time the user types.

---

### **Fetching Data from Backend (`AllPosts.js`)**
```jsx
import React, { useState } from "react";
import SearchBar from "./SearchBar";

const AllPosts = () => {
  const [posts, setPosts] = useState([]);

  const fetchPosts = async (query) => {
    try {
      const response = await fetch(`http://localhost:8080/jobpost/keyword/${query}`);
      if (response.ok) {
        const data = await response.json();
        setPosts(data);
      }
    } catch (error) {
      console.error("Error fetching posts:", error);
    }
  };

  return (
    <div>
      <h2>Job Listings</h2>
      <SearchBar onSearch={fetchPosts} />
      <ul>
        {posts.map((post) => (
          <li key={post.postId}>
            <h3>{post.profile}</h3>
            <p>{post.description}</p>
          </li>
        ))}
      </ul>
    </div>
  );
};

export default AllPosts;
```

- Calls `fetchPosts(query)` every time the search input changes.
- Displays the fetched job posts in a list.

---

## **4. Running the Application**
1. **Start the Spring Boot backend:**
   ```bash
   mvn spring-boot:run
   ```
2. **Start the React frontend:**
   ```bash
   npm start
   ```
3. **Test the search:**
   - Type `Java` → See Java Developer jobs.
   - Type `API` → See Backend Engineer jobs.

---

## **5. Summary**
✔ **Integrated search functionality into React frontend**  
✔ **Used `fetch` API to call Spring Boot's search endpoint**  
✔ **Displayed job posts dynamically based on user input**  

L)
### **Implementing Update and Delete in React with Spring Boot Backend**  

Now that we have implemented **update and delete operations** in our Spring Boot backend, let’s integrate them into our **React frontend**.

---

## **1. Backend Recap**
### **Update API**
- **Endpoint:** `PUT /jobpost/{id}`
- **Functionality:** Updates an existing job post with new details.

### **Delete API**
- **Endpoint:** `DELETE /jobpost/{id}`
- **Functionality:** Deletes a job post by ID.

---

## **2. Update Feature in React**
- We will create an `EditJob` component.
- The component will **fetch job details**, allow users to **edit them**, and **send an update request**.

### **EditJob Component (`EditJob.js`)**
```jsx
import React, { useState, useEffect } from "react";
import { useParams, useNavigate } from "react-router-dom";

const EditJob = () => {
  const { id } = useParams();
  const navigate = useNavigate();
  const [job, setJob] = useState({
    profile: "",
    description: "",
    experience: "",
  });

  useEffect(() => {
    fetch(`http://localhost:8080/jobpost/${id}`)
      .then((res) => res.json())
      .then((data) => setJob(data))
      .catch((error) => console.error("Error fetching job:", error));
  }, [id]);

  const handleChange = (e) => {
    setJob({ ...job, [e.target.name]: e.target.value });
  };

  const handleSubmit = async (e) => {
    e.preventDefault();
    try {
      const response = await fetch(`http://localhost:8080/jobpost/${id}`, {
        method: "PUT",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify(job),
      });

      if (response.ok) {
        navigate("/");
      } else {
        console.error("Failed to update job post");
      }
    } catch (error) {
      console.error("Error updating job post:", error);
    }
  };

  return (
    <div>
      <h2>Edit Job Post</h2>
      <form onSubmit={handleSubmit}>
        <input type="text" name="profile" value={job.profile} onChange={handleChange} />
        <textarea name="description" value={job.description} onChange={handleChange} />
        <input type="number" name="experience" value={job.experience} onChange={handleChange} />
        <button type="submit">Update</button>
      </form>
    </div>
  );
};

export default EditJob;
```
✔ Fetches job details based on ID  
✔ Allows users to modify the job post  
✔ Sends an `update` request to the backend  

---

## **3. Delete Feature in React**
- The delete function will be added in the `AllPosts` component.

### **Delete Functionality (`AllPosts.js`)**
```jsx
import React, { useState, useEffect } from "react";
import { useNavigate } from "react-router-dom";

const AllPosts = () => {
  const [posts, setPosts] = useState([]);
  const navigate = useNavigate();

  useEffect(() => {
    fetch("http://localhost:8080/jobpost")
      .then((res) => res.json())
      .then((data) => setPosts(data))
      .catch((error) => console.error("Error fetching posts:", error));
  }, []);

  const handleDelete = async (id) => {
    try {
      const response = await fetch(`http://localhost:8080/jobpost/${id}`, {
        method: "DELETE",
      });

      if (response.ok) {
        setPosts(posts.filter((post) => post.postId !== id));
      } else {
        console.error("Failed to delete job post");
      }
    } catch (error) {
      console.error("Error deleting job post:", error);
    }
  };

  return (
    <div>
      <h2>Job Listings</h2>
      <ul>
        {posts.map((post) => (
          <li key={post.postId}>
            <h3>{post.profile}</h3>
            <p>{post.description}</p>
            <button onClick={() => navigate(`/edit/${post.postId}`)}>Edit</button>
            <button onClick={() => handleDelete(post.postId)}>Delete</button>
          </li>
        ))}
      </ul>
    </div>
  );
};

export default AllPosts;
```
✔ Fetches all job posts  
✔ Provides **Edit** and **Delete** buttons  
✔ Updates the UI after deletion  

---

## **4. Running the Application**
1. **Start the backend:**
   ```bash
   mvn spring-boot:run
   ```
2. **Start the frontend:**
   ```bash
   npm start
   ```
3. **Test the update functionality:**
   - Click **Edit** on a job post.
   - Modify details.
   - Click **Submit** → Job updates.

4. **Test the delete functionality:**
   - Click **Delete** on a job post.
   - The post disappears.
   - Refresh → Post remains deleted.

---

## **5. Summary**
✔ **Implemented update functionality**  
✔ **Implemented delete functionality**  
✔ **Ensured smooth interaction between React and Spring Boot**  

