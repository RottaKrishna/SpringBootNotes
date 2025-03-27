A)
### **Spring Data REST: Simplifying API Development**
#### **1. Understanding the Traditional Multi-Layer Architecture**
A typical Spring Boot application follows a layered approach:
- **Controller Layer:** Handles HTTP requests and responses.
- **Service Layer:** Contains business logic and interacts with repositories.
- **Repository Layer:** Connects to the database and fetches data.

Spring Data JPA already reduces code in the repository layer by providing built-in methods. However, in some applications, the service layer does nothing but call repository methods. If there's no business logic, maintaining a service layer and even a controller becomes redundant.

#### **2. What is Spring Data REST?**
Spring Data REST is a Spring project that **automatically exposes repositories as RESTful APIs**, removing the need to write explicit controller and service layers.

**Key Features:**
- Exposes **CRUD endpoints** automatically.
- Supports **HATEOAS** (Hypermedia as the Engine of Application State).
- Reduces boilerplate code significantly.

#### **3. How Does It Work?**
With Spring Data REST:
- You **only need a repository interface**, and Spring will automatically generate the API.
- No need for a controller to handle API requests.
- No need for a service layer (if there's no complex logic).

#### **4. Enabling Spring Data REST**
To use Spring Data REST, add the dependency:

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-rest</artifactId>
</dependency>
```

#### **5. Exposing a Repository as a REST API**
Simply annotate the repository with `@RepositoryRestResource`, and Spring will generate endpoints.

**Example Repository:**
```java
@RepositoryRestResource
public interface ProductRepository extends JpaRepository<Product, Long> {
}
```

Now, Spring automatically generates REST endpoints like:
- `GET /products` → Fetch all products.
- `GET /products/{id}` → Fetch a specific product.
- `POST /products` → Add a new product.
- `PUT /products/{id}` → Update a product.
- `DELETE /products/{id}` → Delete a product.

#### **6. Customizing the API Endpoints**
You can modify the endpoint path using:
```java
@RepositoryRestResource(path = "items")
public interface ProductRepository extends JpaRepository<Product, Long> {
}
```
Now, the API endpoint will be:
```
GET /items
```

#### **7. When to Use Spring Data REST?**
✅ **Use when:**
- You need a quick API without writing controllers.
- Your service layer only forwards calls to repositories.
- You want automatic HATEOAS support.

❌ **Avoid when:**
- You need custom business logic in controllers.
- You require fine-grained control over API responses.
- You need complex request handling, validation, or authentication.

---

Spring Data REST eliminates unnecessary layers and speeds up development. In the next hands-on session, we'll see it in action! 🚀

B)
### **Setting Up a Spring Data REST Project**
#### **1. Creating a New Project Using Spring Initializr**
Since we don't want to modify our existing project, we will create a new one while reusing some existing code.

- **Go to** [Spring Initializr](https://start.spring.io/)
- **Project Type:** Maven
- **Spring Boot Version:** Latest stable version (Spring Boot 3.x)
- **Group ID:** `com.telusko`
- **Artifact ID:** `spring-data-rest-demo`
- **Java Version:** `21`

#### **2. Adding Dependencies**
We need the following dependencies:
- **Spring Boot Starter JPA** (for database interaction)
- **Spring Boot Starter Data REST** (to expose repositories as RESTful APIs)
- **PostgreSQL Driver** (since we’re using PostgreSQL)
- **Lombok** (to reduce boilerplate code)

```xml
<dependencies>
    <!-- Spring Data JPA -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-jpa</artifactId>
    </dependency>

    <!-- Spring Data REST -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-rest</artifactId>
    </dependency>

    <!-- PostgreSQL Driver -->
    <dependency>
        <groupId>org.postgresql</groupId>
        <artifactId>postgresql</artifactId>
        <scope>runtime</scope>
    </dependency>

    <!-- Lombok -->
    <dependency>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
        <scope>provided</scope>
    </dependency>
</dependencies>
```

#### **3. Setting Up the Project in IntelliJ**
- Unzip the project and open it in **IntelliJ IDEA**.
- Ensure the **Maven dependencies are loaded** (re-import if necessary).

#### **4. Creating the Model and Repository**
Since we already have an existing `JobPost` model and repository, we will copy them into our new project.

- **Create a package:** `model`
- **Create another package:** `repo`
- **Copy** the `JobPost` entity into the `model` package.
- **Copy** the repository interface into the `repo` package.

**Example `JobPost` Entity:**
```java
package com.telusko.model;

import jakarta.persistence.*;
import lombok.*;

@Entity
@Data
@NoArgsConstructor
@AllArgsConstructor
public class JobPost {
    
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String title;
    private String description;
    private String company;
}
```

**Example Repository Interface:**
```java
package com.telusko.repo;

import com.telusko.model.JobPost;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.data.rest.core.annotation.RepositoryRestResource;

@RepositoryRestResource
public interface JobPostRepository extends JpaRepository<JobPost, Long> {
}
```
✅ **With `@RepositoryRestResource`, Spring will expose RESTful APIs automatically!**

#### **5. Configuring Database in `application.properties`**
We also need to set up the **database configuration**.

**Copy the following from the old project:**
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
### **Next Steps: Will It Work?**
- Since we **haven't written a controller**, the repository should automatically handle API requests.
- In the next session, we'll **run the application** and see if Spring Data REST works as expected! 🚀


C)
### **Testing Spring Data REST API**
Now that we have set up our Spring Data REST project, let's see if it works.

#### **1. Running the Application**
- Run the **Spring Boot** application.
- Since we haven't created any controllers, Spring Data REST will expose the APIs automatically.

#### **2. Checking API Endpoints**
##### **a) Fetching All Job Posts**
Once the application starts, we can test it using **Postman** or a browser.
- Open **Postman** and send a `GET` request to:

  ```
  http://localhost:8080/jobPosts
  ```

- Expected **JSON Response:**
  ```json
  {
      "_embedded": {
          "jobPosts": [
              {
                  "id": 1,
                  "title": "Java Developer",
                  "description": "Experience in Spring Boot",
                  "company": "Tech Corp",
                  "_links": {
                      "self": {
                          "href": "http://localhost:8080/jobPosts/1"
                      },
                      "jobPost": {
                          "href": "http://localhost:8080/jobPosts/1"
                      }
                  }
              },
              {
                  "id": 2,
                  "title": "Python Developer",
                  "description": "Expert in Django",
                  "company": "Data Inc.",
                  "_links": {
                      "self": {
                          "href": "http://localhost:8080/jobPosts/2"
                      },
                      "jobPost": {
                          "href": "http://localhost:8080/jobPosts/2"
                      }
                  }
              }
          ]
      }
  }
  ```

##### **b) Fetching a Single Job Post**
To get a specific job post, use the `self` link from the response:
```
http://localhost:8080/jobPosts/1
```

This is an example of **HATEOAS** (**Hypermedia As The Engine Of Application State**), which helps clients discover API endpoints dynamically.

#### **3. Understanding HATEOAS**
- **Each response includes links to related resources.**
- You don’t need to hardcode endpoints; Spring Data REST generates them dynamically.

---
### **Next Steps: Handling Additions & Deletions**
- Now that we can **fetch** data, let's move on to **creating, updating, and deleting** job posts in the next section. 🚀

D)
### **Updating and Deleting Job Posts with Spring Data REST**  
Now that we have retrieved data from our REST API, let's explore how to **update and delete** records using Spring Data REST.

---

## **1. Updating a Job Post (PUT Request)**  

### **Steps to Update a Record**
1. First, fetch the **existing job post** you want to update.  
   Example:  
   ```
   GET http://localhost:8080/jobPosts/3
   ```
   Response:
   ```json
   {
       "id": 3,
       "title": "React Developer",
       "description": "Experience in React.js",
       "company": "Web Tech",
       "experience": 3
   }
   ```

2. Copy the JSON response and **modify** the necessary fields.  
   - Let's change the **title** to `"Front-end Developer"`  
   - Update the **experience** from `3` to `1`  

3. Send a `PUT` request to update the record:  
   ```
   PUT http://localhost:8080/jobPosts/3
   ```
   - **Request Body (JSON, Raw Format)**  
   ```json
   {
       "id": 3,
       "title": "Front-end Developer",
       "description": "Experience in React.js",
       "company": "Web Tech",
       "experience": 1
   }
   ```
4. Click **Send**, and if successful, you will receive an updated response.

5. **Verify the update** in your database by fetching all records again:
   ```
   GET http://localhost:8080/jobPosts
   ```

---

## **2. Deleting a Job Post (DELETE Request)**  

### **Steps to Delete a Record**
1. Identify the **ID** of the record you want to delete.  
   Let's delete **Job Post with ID = 3**  

2. Send a `DELETE` request:  
   ```
   DELETE http://localhost:8080/jobPosts/3
   ```
3. If successful, the response will be:
   ```
   HTTP 200 OK
   ```
4. **Verify deletion** by fetching all records:
   ```
   GET http://localhost:8080/jobPosts
   ```
   - The deleted job post **will not appear** in the list.

---

## **Key Takeaways**
✅ **Spring Data REST automatically generates API endpoints** for CRUD operations.  
✅ **No need to write controller methods**—it works directly with JPA repositories.  
✅ **Built-in support for HATEOAS** (provides links for navigation).  
✅ **Reduces boilerplate code**, making development faster.  

With just a **repository layer and model layer**, we have a fully functional REST API! 🚀