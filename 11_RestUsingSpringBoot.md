A)
### **Introduction to REST API with Spring Boot**  

#### **Current State of the Job Portal Application:**  
- The project has **both frontend and backend** in one Spring Boot application.  
- The backend handles requests, processes data, and returns **JSP pages** as responses.  
- The repository layer currently **mocks database data** using an in-memory list.  

#### **Why Move to REST APIs?**  
1. **Separation of Concerns:**  
   - Currently, the server returns **HTML pages** (JSP), which is not ideal for modern applications.  
   - Instead, we want a **dedicated backend** that serves only **data (JSON/XML)**.  

2. **Multi-Platform Support:**  
   - A **browser** expects **HTML pages**.  
   - A **mobile app** (Android/iOS) or another **server** expects **JSON/XML data**.  
   - Instead of maintaining separate backend servers, we can build **one REST API** that serves all clients.  

3. **Decoupling Frontend & Backend:**  
   - The frontend (React/Angular) will fetch data via API calls.  
   - The backend will **only handle business logic** and return data in **JSON/XML format**.  

#### **How Do We Achieve This?**  
- **Remove JSP views** and modify controllers to **return JSON data** instead of pages.  
- Use **Spring Boot’s RESTful API features** to expose endpoints.  
- Work with **Postman** as a client to test API responses.  

#### **Next Steps:**  
1. Understand **REST APIs** and how they work.  
2. Modify controllers to return **JSON** instead of **JSP views**.  
3. Use **Postman** to test API endpoints.  
4. Integrate a **React frontend** to consume the REST API.


B)
### **Understanding REST (Representational State Transfer) in Spring Boot**  

#### **What is REST?**  
REST stands for **Representational State Transfer** and is an **architectural style** for building web services. It allows clients (browsers, mobile apps, or other servers) to communicate with a server using **stateless** HTTP requests.  

#### **Key Concepts in REST API**  

1. **CRUD Operations (Create, Read, Update, Delete):**  
   - REST APIs allow **creating, reading, updating, and deleting** resources.  
   - Example: In a job portal, resources can be **jobs, employees, employers, and companies**.  

2. **Resource-Based Approach:**  
   - In REST, **everything is considered a resource**.  
   - Example resources:  
     - **Job:** `/jobs`  
     - **Employee:** `/employees`  
     - **Employer:** `/employers`  
   - Resources are identified using **nouns**, not **verbs**.  

3. **State Transfer:**  
   - The **server holds the current state** of a resource.  
   - Example:  
     - An employee’s **current employer** might be “Microsoft” today.  
     - Later, it might change to “Google.”  
     - When a client requests data, the server returns the **current state** in JSON/XML format.  

4. **Stateless Communication:**  
   - Each client request **must include all necessary details** (authentication, parameters, etc.).  
   - The **server does not remember previous interactions** between requests.  

---

### **RESTful URL Structure**  
Instead of using **action-based URLs** like:  
✅ **Old way (Not RESTful):**  
- `/viewAllJobs`  
- `/addJob`  

✅ **RESTful way (Using nouns):**  
- `GET /jobs` → Fetch all jobs  
- `POST /jobs` → Add a new job  
- `GET /jobs/{id}` → Fetch a specific job  
- `PUT /jobs/{id}` → Update a job  
- `DELETE /jobs/{id}` → Delete a job  

---

### **HTTP Methods in REST API**  
REST APIs use different **HTTP methods** to perform actions on resources:  

| **Method** | **Action** | **Example URL** | **Description** |
|------------|-----------|-----------------|-----------------|
| **GET**    | Read      | `/jobs`          | Get all jobs |
| **GET**    | Read      | `/jobs/{id}`     | Get job by ID |
| **POST**   | Create    | `/jobs`          | Add a new job |
| **PUT**    | Update    | `/jobs/{id}`     | Update a job |
| **DELETE** | Delete    | `/jobs/{id}`     | Delete a job |

---

### **Response Formats: JSON vs XML**  
REST APIs **do not return HTML pages**. Instead, they return **data** in structured formats like **JSON** or **XML**.  

✅ **JSON (Preferred format, lightweight & readable)**  
```json
{
  "id": 1,
  "title": "Software Developer",
  "company": "Google",
  "location": "Remote"
}
```  

✅ **XML (Older format, more verbose)**  
```xml
<job>
  <id>1</id>
  <title>Software Developer</title>
  <company>Google</company>
  <location>Remote</location>
</job>
```  

JSON is **easier to read, takes less memory**, and is widely used in modern APIs.  

---

### **Next Steps**  
- Implement RESTful APIs using **Spring Boot**.  
- Learn about **HTTP methods** and how they map to REST operations.  
- Use **Postman** to test API endpoints.

C)
### **Understanding HTTP Methods in REST API**  

#### **What is HTTP?**  
HTTP (**Hypertext Transfer Protocol**) is the foundation of communication on the web. It allows clients (browsers, mobile apps, API clients) to communicate with servers using **requests and responses**.  

---

### **Key HTTP Methods in REST API**  

| **Method**  | **CRUD Operation** | **Purpose** |
|-------------|-------------------|-------------|
| **GET**     | Read               | Retrieve data from the server |
| **POST**    | Create             | Send data to create a new resource on the server |
| **PUT**     | Update             | Update an existing resource |
| **DELETE**  | Delete             | Remove a resource from the server |

---

### **Mapping HTTP Methods in Spring Boot**  
Spring Boot provides annotations to map these methods to specific URLs:  

✅ **GET Request (Read Data)**  
```java
@GetMapping("/jobs")
public List<Job> getAllJobs() {
    return jobService.getAllJobs();
}
```
- Retrieves a list of jobs.  
- Can be tested by entering `http://localhost:8080/jobs` in a browser.  

✅ **POST Request (Create Data)**  
```java
@PostMapping("/jobs")
public Job createJob(@RequestBody Job job) {
    return jobService.createJob(job);
}
```
- Adds a new job to the database.  
- Requires **Postman** or another API client to send data.  

✅ **PUT Request (Update Data)**  
```java
@PutMapping("/jobs/{id}")
public Job updateJob(@PathVariable Long id, @RequestBody Job job) {
    return jobService.updateJob(id, job);
}
```
- Updates an existing job using `PUT` request.  

✅ **DELETE Request (Remove Data)**  
```java
@DeleteMapping("/jobs/{id}")
public void deleteJob(@PathVariable Long id) {
    jobService.deleteJob(id);
}
```
- Deletes a job with the given ID.  

---

### **How to Test API Requests?**  
1. **GET Requests** → Can be tested directly in a browser (`http://localhost:8080/jobs`).  
2. **POST, PUT, DELETE Requests** → Browsers do not support them directly, so we use **Postman** or a frontend client (React, Angular, etc.).  
3. **Postman** allows sending different types of requests by specifying HTTP methods, URLs, and request bodies.  

---

### **Next Steps**  
- Learn how to use **Postman** to test API requests.  
- Implement **service and repository layers** in Spring Boot to connect with a database.


D)

This video introduces the frontend setup for the Job Portal project and explains how to test the backend using a fake JSON server before integrating it with Spring Boot.

### Key Takeaways:
1. **Frontend & Backend Structure:**
   - The frontend is built using **React**, while the backend will be developed using **Spring Boot**.
   - Initially, data is fetched from a **fake backend** using `json-server`.

2. **Setting Up the Fake Backend:**
   - Install `json-server` globally using:
     ```
     npm install -g json-server
     ```
   - Run the fake backend with:
     ```
     json-server --watch db.json --port 8000
     ```
   - The data is stored in `db.json` and can be accessed via:
     ```
     http://localhost:8000/posts
     ```

3. **Running the React Frontend:**
   - Clone or download the provided frontend project.
   - Navigate to the project folder and install dependencies:
     ```
     npm install
     ```
   - Start the React app:
     ```
     npm start
     ```
   - The app runs on **port 3000**, and it fetches job posts from the fake backend.

4. **Understanding React Project Structure:**
   - **`index.html`** (public folder) is the base HTML file.
   - **`index.js`** (src folder) injects React components into the HTML page.
   - **`App.js`** loads the main component (`AllPosts`).
   - **`AllPosts.js`** fetches data from `json-server` using **Axios** and dynamically renders job posts.

5. **Replacing the Fake Backend with Spring Boot:**
   - The current setup fetches data from `json-server`, but we will later switch to Spring Boot.
   - Instead of using `localhost:8000/posts`, the new API endpoint will be from the Spring Boot backend.

6. **Alternative to React for Testing:**
   - Instead of setting up a frontend, we can test the backend using **Postman**.

### Next Steps:
- In the next video, the focus will be on using **Postman** to test the API instead of the frontend.


E)
### **Notes on Setting Up the Frontend and Testing with Postman**  

#### **1. Understanding the Project Structure**  
- The project consists of two parts: **Frontend (React)** and **Backend (Spring Boot)**.  
- A **pre-built React frontend** is provided. The initial focus is on **fetching job posts**.  

#### **2. Setting Up a Fake Backend**  
- Since the Spring Boot backend is not yet implemented, a **fake backend** is used.  
- The **JSON Server** package is used to simulate a backend.  
- Steps to install and run JSON Server:  
  1. Install JSON Server globally:  
     ```sh
     npm install -g json-server
     ```
  2. Run JSON Server with a local JSON file (`db.json`):  
     ```sh
     json-server --watch db.json --port 8000
     ```
  3. Data is now available at `http://localhost:8000/posts`.  

#### **3. Running the React Frontend**  
- The React app fetches job posts from the backend (currently JSON Server).  
- Steps to run the frontend:  
  1. Install dependencies:  
     ```sh
     npm install
     ```
  2. Start the React app:  
     ```sh
     npm start
     ```
- React fetches job posts from `http://localhost:8000/posts` using Axios.  
- If the JSON server is not running, the frontend will throw an error.  

#### **4. Using Postman to Test APIs**  
- **Postman** is a REST API client for testing backend APIs.  
- Download Postman from [https://www.postman.com/downloads/](https://www.postman.com/downloads/).  
- Basic operations in Postman:  
  - **GET request**: Fetch all job posts.  
    - URL: `http://localhost:8000/posts`  
    - Click **Send** to retrieve JSON data.  
  - **POST request**: Add a new job post.  
    - Select **POST** method.  
    - Set URL: `http://localhost:8000/posts`.  
    - Go to **Body > Raw > JSON** and enter sample data:  
      ```json
      {
        "id": 3,
        "profile": "Software Developer",
        "description": "Develops applications",
        "experience": "2 years",
        "technologies": ["Java", "Spring Boot"]
      }
      ```
    - Click **Send** to add a new job.  
  - **PUT, DELETE requests** are used to update and remove posts.  

#### **5. Comparing Postman vs. Browser for API Testing**  
- Browsers can only test **GET requests**.  
- Postman allows testing **POST, PUT, DELETE**, making it essential for backend development.  

#### **6. Next Steps: Building the Backend in Spring Boot**  
- The fake backend (JSON Server) will be replaced by a real Spring Boot backend.  
- The backend will return **JSON responses** instead of JSP pages.

F)
creating a rest controller Related

### **Migrating from JSP to a RESTful Spring Boot Backend**  

Since we previously worked with JSP views, we are now shifting to a **pure backend** implementation using REST APIs. We will reuse the **Model, Repository, and Service** layers while replacing the **Controller** to return JSON responses instead of views.

---

## **1. Creating a New Spring Boot Project**
### **Using Spring Initializr**
- **Project Type**: Maven  
- **Spring Boot Version**: Latest stable version  
- **Group**: `com.company`  
- **Artifact**: `spring-boot-backend`  
- **Packaging**: JAR  
- **Java Version**: 21  
- **Dependencies**:
  - **Spring Web** (for REST APIs)
  - **Lombok** (for reducing boilerplate code)

Download and extract the project, then open it in **IntelliJ IDEA**.

---

## **2. Reusing Existing Components**
We will reuse the following from the previous project:
- **Model** (`JobPost.java`)
- **Repository** (`JobPostRepository.java`)
- **Service** (`JobService.java`)

Copy these files into the new project.

We **do not** need:
- JSP views  
- Old `application.properties` (delete it if copied by mistake)

---

## **3. Implementing the REST Controller**
Since we are building a **RESTful API**, we must modify the controller to **return JSON** instead of views.

### **New `JobPostController`**
Create a new class `JobPostController.java` in the `controller` package.

```java
package com.company.controller;

import com.company.model.JobPost;
import com.company.service.JobService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.*;

import java.util.List;

@RestController  // This replaces @Controller
@RequestMapping("/api")  // Base path for all endpoints
public class JobPostController {

    @Autowired
    private JobService jobService;

    @GetMapping("/job-posts")
    public List<JobPost> getAllJobs() {
        return jobService.getAllJobs();
    }
}
```
### **Key Changes**
✅ **Replaced `@Controller` with `@RestController`** → This tells Spring that we are returning JSON, not views.  
✅ **Used `@RequestMapping("/api")`** → Organizes our endpoints under `/api` for better structuring.  
✅ **Changed the return type to `List<JobPost>`** instead of returning a view name.  

---

## **4. Running and Testing the API**
### **Step 1: Start the Spring Boot Application**
Run the application from the main class.

**Common Errors & Fixes:**
- **Port already in use** → Stop the previous project and restart.
- **Lombok not working** → Enable annotation processing in IntelliJ.

### **Step 2: Testing in Browser**
Open:
```
http://localhost:8080/api/job-posts
```
If you get an **HTTP 500 error**, check the logs.  
**Fix:** Ensure `@RestController` is used instead of `@Controller`.  

### **Step 3: Testing in Postman**
- **Method:** `GET`
- **URL:** `http://localhost:8080/api/job-posts`
- Click **Send** and verify that JSON data is returned.

---

## **Conclusion**
🚀 We successfully migrated from a JSP-based project to a **Spring Boot REST API**.  
🔹 **Removed JSP views**  
🔹 **Used `@RestController` for JSON responses**  
🔹 **Reused Model, Repository, and Service layers**  
🔹 **Tested API using Browser & Postman**  

Now, we can extend this further by adding **POST, PUT, and DELETE** endpoints for a complete CRUD API.

G)
### **Notes on Connecting React Frontend with Spring Boot Backend**  

#### **1. Current Setup**  
- The **React frontend** was initially fetching data from `db.json` (a mock backend).  
- The goal is to **replace the mock backend** with a real Spring Boot backend.  

#### **2. Updating API URL in React**  
- The React app previously fetched data from:  
  ```js
  axios.get("http://localhost:8000/posts") // Mock backend (db.json)
  ```
- To connect with the Spring Boot backend, change the URL:  
  ```js
  axios.get("http://localhost:8080/jobs") // Spring Boot backend
  ```
- **Problem:** React throws a **Network Error** due to **CORS (Cross-Origin Resource Sharing)** issues.  

#### **3. Fixing CORS Issue in Spring Boot**  
- By default, browsers block requests from different origins (different ports or domains).  
- To allow requests from the frontend (`http://localhost:3000`), add `@CrossOrigin` in the **Spring Boot Controller**:  

  ```java
  import org.springframework.web.bind.annotation.*;

  @RestController
  @RequestMapping("/jobs")
  @CrossOrigin(origins = "http://localhost:3000") // Allow frontend requests
  public class JobController {
      @GetMapping
      public List<Job> getAllJobs() {
          // Fetch jobs from database
          return jobService.getAllJobs();
      }
  }
  ```
- This allows requests from the React frontend (`http://localhost:3000`).  

#### **4. Restart and Verify**  
1. Restart the Spring Boot backend (`mvn spring-boot:run`).  
2. Restart the React app (`npm start`).  
3. Refresh the browser; now, data should load from the backend instead of `db.json`.  

#### **5. Next Step: Adding a New Job Post**  
- The next step is implementing the **POST request** to allow users to add job postings.


H)
### **Notes on Fetching a Single Job Post by ID in Spring Boot**  

#### **1. Fetching All Job Posts**  
- The endpoint **`GET /jobposts`** returns all job posts.  
- Example:  
  ```
  GET http://localhost:8080/jobposts
  ```
  Response (JSON):  
  ```json
  [
    { "id": 1, "title": "Software Engineer", "company": "ABC Corp" },
    { "id": 2, "title": "Backend Developer", "company": "XYZ Ltd" }
  ]
  ```

#### **2. Fetching a Single Job Post by ID**  
- To get a specific job post by its **ID**, define a new endpoint:  
  ```
  GET http://localhost:8080/jobposts/3
  ```

#### **3. Creating the Controller Method**  
- In the **JobController**, add:  

  ```java
  @GetMapping("/jobposts/{postId}")  // Dynamic path variable
  public Job getJobById(@PathVariable int postId) {
      return jobService.getJobById(postId);
  }
  ```
- **`@PathVariable int postId`** extracts the `postId` from the URL.

#### **4. Implementing the Service Method**  
- The service layer should handle fetching the job by ID:  

  ```java
  public Job getJobById(int postId) {
      return jobRepository.findById(postId);
  }
  ```

#### **5. Implementing the Repository Method**  
- Since the data is in a list, manually search for the job post:  

  ```java
  public Job findById(int postId) {
      for (Job job : jobList) {
          if (job.getId() == postId) {
              return job;  // Return matching job
          }
      }
      return null;  // Return null if not found
  }
  ```
  
#### **6. Testing in Postman**  
- **Valid Request:**  
  ```
  GET http://localhost:8080/jobposts/2
  ```
  **Response:**  
  ```json
  { "id": 2, "title": "Backend Developer", "company": "XYZ Ltd" }
  ```
- **Invalid Request (ID not found):**  
  ```
  GET http://localhost:8080/jobposts/10
  ```
  **Response:**  
  ```json
  null
  ```

#### **7. Key Takeaways**  
- Use **`@PathVariable`** to extract URL parameters.  
- If fetching a resource by ID, return **null** if not found (or throw an exception).  
- Next step: **Sending data (POST request) to the backend.**


I)
### **Sending Data to the Server in Spring Boot**

#### **1. Using Postman for Testing**
- We have tested **GET requests** using Postman.
- Now, we will learn how to **send data (POST request)** to the server.
- Later, we will integrate this with a React application.

---

#### **2. Creating the `addJob` Method in Controller**
- In **JobController**, add a method to handle job post creation:
  
  ```java
  @PostMapping("/jobposts")
  public Job addJob(@RequestBody Job jobPost) {
      return jobService.addJob(jobPost);
  }
  ```
  
- **Key Points:**
  - **`@PostMapping("/jobposts")`** → Specifies that this method handles **POST requests** at `/jobposts`.
  - **`@RequestBody`** → Converts incoming JSON data into a `Job` object.
  - Calls `jobService.addJob(jobPost)` to store the job.

---

#### **3. Implementing `addJob` in Service Layer**
- In the **Service Class**, define:
  
  ```java
  public Job addJob(Job jobPost) {
      jobList.add(jobPost);
      return jobPost;
  }
  ```
  
  - This method **adds** the new job post to the list.
  - Returns the same object for confirmation.

---

#### **4. Testing the POST Request in Postman**
- **Endpoint:**
  ```
  POST http://localhost:8080/jobposts
  ```
- **Steps in Postman:**
  1. Select **POST** method.
  2. Go to **Body** → Select **raw** → Choose **JSON**.
  3. Enter the following JSON data:
     ```json
     {
         "id": 6,
         "title": "iOS Developer",
         "company": "Apple Inc.",
         "experience": 2
     }
     ```
  4. Click **Send**.
  5. **Response:** Should return the same JSON object if successful.

---

#### **5. Confirming the Data is Stored**
- To verify, send a **GET request**:
  ```
  GET http://localhost:8080/jobposts
  ```
- The response should include the new job:
  ```json
  [
      { "id": 1, "title": "Software Engineer", "company": "ABC Corp" },
      { "id": 6, "title": "iOS Developer", "company": "Apple Inc.", "experience": 2 }
  ]
  ```

---

#### **6. Enhancing the Response**
- Instead of returning the same object, fetch it from the list:
  
  ```java
  @PostMapping("/jobposts")
  public Job addJob(@RequestBody Job jobPost) {
      jobService.addJob(jobPost);
      return jobService.getJobById(jobPost.getId());
  }
  ```
- This ensures the object was actually saved before returning.

---

#### **7. XML Support (Optional)**
- JSON is used by default, but Spring Boot can support XML.
- Requires additional configuration (not covered here).

---

### **Key Takeaways**
✅ **`@RequestBody`** is used to receive JSON data in a request.
✅ **`@PostMapping`** is used for submitting data.
✅ Postman can be used to test API endpoints.
✅ Confirm data storage by fetching all job posts after submission.
✅ Can enhance responses by retrieving data after saving it.

---

**Next Step:** Connecting this to a **database** instead of storing in a list.

J)
Here’s a structured summary of the lesson on `PUT` and `DELETE` mappings in Spring Boot:  

---

### **PUT and DELETE Requests in Spring Boot (REST API)**
In this lesson, we explored how to handle `PUT` and `DELETE` HTTP requests using Spring Boot’s REST API.  

#### **1. PUT Request (Updating a Job Post)**
- **Objective:** Update an existing job post.
- **Steps:**
  1. Perform a `GET` request to retrieve the existing job post.
  2. Modify the required fields (e.g., update "Frontend Developer" to "React Developer" and experience from 3 to 2 years).
  3. Change the request type to `PUT` in Postman and provide the modified data.
  4. Implement the `PUT` mapping in the controller to handle update logic.

- **Spring Boot Implementation:**
  ```java
  @PutMapping("/jobpost")
  public JobPost updateJob(@RequestBody JobPost jobPost) {
      return service.updateJob(jobPost);
  }
  ```
  - Uses `@PutMapping` to indicate an update operation.
  - The request body (`JobPost`) contains the updated data.
  - Calls a `service.updateJob(jobPost)` method to handle the update.

- **Service Layer:**
  ```java
  public JobPost updateJob(JobPost jobPost) {
      for (JobPost jp : jobs) {
          if (jp.getPostId().equals(jobPost.getPostId())) {
              jp.setPostProfile(jobPost.getPostProfile());
              jp.setRequiredExperience(jobPost.getRequiredExperience());
              jp.setTechStack(jobPost.getTechStack());
          }
      }
      return jobPost;
  }
  ```
  - Loops through the list to find the matching `postId`.
  - Updates the fields with the new values.

---

#### **2. DELETE Request (Removing a Job Post)**
- **Objective:** Delete a job post by its ID.
- **Steps:**
  1. Perform a `DELETE` request in Postman with `/jobpost/{id}` (e.g., `/jobpost/3`).
  2. Implement the `DELETE` mapping in the controller.

- **Spring Boot Implementation:**
  ```java
  @DeleteMapping("/jobpost/{postId}")
  public String deleteJob(@PathVariable int postId) {
      service.deleteJob(postId);
      return "Deleted";
  }
  ```
  - Uses `@DeleteMapping` with a path variable for `postId`.
  - Calls `service.deleteJob(postId)` to process the deletion.

- **Service Layer:**
  ```java
  public void deleteJob(int postId) {
      jobs.removeIf(job -> job.getPostId() == postId);
  }
  ```
  - Uses `removeIf()` to filter and delete the job by `postId`.

---

### **Testing the API**
- **PUT Request Test:**  
  - Update a job and verify changes using a `GET` request.  
  - Confirm updated details are reflected in the repository.

- **DELETE Request Test:**  
  - Delete a job by ID and verify using a `GET` request.  
  - Ensure the job no longer exists in the repository.

---

### **Key Takeaways**
✔ `@PutMapping` is used to update an existing resource.  
✔ `@DeleteMapping` is used to delete a resource by ID.  
✔ The service layer ensures data consistency.  
✔ Use `removeIf()` for efficient deletion.  
✔ Always test API changes in Postman before integrating with frontend.

---

K)
Here’s a clear and structured summary of the video on **Content Negotiation**:  

---

## **Content Negotiation in Spring Boot**  

### **1. What is Content Negotiation?**  
- It allows the client to request data in different formats, such as **JSON** or **XML**.  
- By default, Spring Boot returns **JSON** data using the **Jackson library**.  

---

### **2. Requesting XML Instead of JSON**  
- In **Postman**, you can specify the desired format using the **Accept** header:  
  ```
  Accept: application/xml
  ```
- However, if XML support is not configured, you will receive a **406 Not Acceptable** error.  

---

### **3. Why is XML Not Working?**  
- Spring Boot **automatically converts** Java objects to **JSON** using the **Jackson library**.  
- **Jackson does not support XML by default**—it only works with JSON.  

---

### **4. Enabling XML Support**  
- To support XML, add the **Jackson Dataformat XML** dependency in `pom.xml`:  
  ```xml
  <dependency>
      <groupId>com.fasterxml.jackson.dataformat</groupId>
      <artifactId>jackson-dataformat-xml</artifactId>
      <version>2.15.3</version>
  </dependency>
  ```
- Reload Maven and restart the application.  
- Now, sending an `Accept: application/xml` request returns data in **XML format**.  

---

### **5. Restricting Response Format (Producing Specific Formats)**  
- If you want your API to return **only JSON** (even if XML is requested), use `produces`:  
  ```java
  @GetMapping(value = "/jobPosts", produces = "application/json")
  public List<JobPost> getJobPosts() { 
      return jobPostService.getAllPosts();
  }
  ```
- Now, if a client requests **XML**, they will receive **406 Not Acceptable**.  

---

### **6. Restricting Input Format (Consuming Specific Formats)**  
- If you want your API to **accept only XML** for `POST` requests, use `consumes`:  
  ```java
  @PostMapping(value = "/jobPosts", consumes = "application/xml")
  public JobPost createJobPost(@RequestBody JobPost jobPost) { 
      return jobPostService.save(jobPost);
  }
  ```
- If a client sends JSON, they will get a **415 Unsupported Media Type** error.  

---

### **7. Summary**  
| **Feature**        | **Default Behavior** | **Customization** |
|--------------------|--------------------|------------------|
| Response format   | JSON (via Jackson)  | Add `jackson-dataformat-xml` for XML support |
| Request format    | Accepts JSON        | Use `@PostMapping(consumes="...")` to restrict |
| Restrict Response | JSON by default     | Use `@GetMapping(produces="application/json")` |

- Spring Boot defaults to **JSON**.
- XML support must be **manually added**.
- Use **produces** and **consumes** to **control** what the API **returns and accepts**.

That's all about **content negotiation in Spring Boot**!
