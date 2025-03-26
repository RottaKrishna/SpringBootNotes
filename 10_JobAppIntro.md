A)
You're now starting to build the backend for your job portal application in Spring Boot. Since you're using JSP for views, your focus will be on setting up the necessary controllers, models, and database interactions first.

Here's a rough breakdown of what you need:

1. **Project Setup**: Create a Spring Boot project with dependencies like Spring Web, Spring Data JPA, and a database connector (PostgreSQL or MySQL).
   
2. **Model Layer**: Define entity classes like `Job` with attributes such as `id`, `title`, `description`, `experience`, `techStack`, and `companyName`.

3. **Repository Layer**: Create a `JobRepository` interface extending `JpaRepository` for database operations.

4. **Service Layer**: Implement `JobService` to handle business logic.

5. **Controller Layer**:
   - `GET /jobs`: Fetch and display all jobs.
   - `POST /jobs`: Add a new job (for employers).
   - `GET /jobs/{id}`: View a specific job.
   - `POST /jobs/apply/{id}`: Apply for a job.

6. **Database Setup**: Configure your database connection in `application.properties`.

7. **Integration with JSP**: Once backend is ready, integrate it with your JSP views.

B)
You've successfully set up your Spring Boot project for the job portal application and added the necessary dependencies for JSP-based views. Here's a summary of what you've done so far:

### **Project Setup**
1. **Created a Spring Boot project using Spring Initializr** with:
   - **Maven** as the build tool
   - **Java 21** as the language version
   - Dependencies:
     - `spring-boot-starter-web` (for web application development)
     - `Lombok` (to reduce boilerplate code)

2. **Added additional dependencies** in `pom.xml` for JSP support:
   - `tomcat-embed-jasper` (JSP compiler)
   - `jakarta.servlet.jsp.jstl` (JSTL support for JSP)
   - `jakarta.servlet-api` (to handle servlet requirements)

### **Setting Up Views**
3. **Created the required directory structure:**
   - `src/main/webapp/WEB-INF/views/` → Contains JSP pages
   - `src/main/webapp/css/` → Contains CSS files

4. **Included JSP views:**
   - `home.jsp` → Homepage
   - `addJob.jsp` → Page for employers to post a job
   - `viewAllJobs.jsp` → Displays a list of available jobs
   - `success.jsp` → Displays confirmation after successfully adding a job

### **Next Steps**
Now that the frontend (JSP views) is in place, the next step is to build the **backend** to handle job postings and retrieval. This includes:

1. **Defining the `Job` entity** (Model)
2. **Creating a `JobRepository`** (DAO Layer)
3. **Implementing `JobService`** (Business Logic Layer)
4. **Building `JobController`** to handle HTTP requests
5. **Configuring database properties** in `application.properties`
6. **Integrating with JSP views** to display and process job data

C)
Now that you've set up your JSP views and defined the necessary URLs, let's summarize the key points and the next steps.

---

### **JSP Views & URL Mapping**
You have set up the following JSP pages:
1. **Home (`home.jsp`)** – Displays navigation links to:
   - View All Jobs (`/viewAllJobs`)
   - Add Job (`/addJob`)
   - Contact (optional external link)

2. **View All Jobs (`viewAllJobs.jsp`)** – Displays a list of available job postings.
   - Uses a `forEach` loop to iterate through job listings.
   - Fetches details like **job title, description, experience, and tech stack**.

3. **Add Job (`addJob.jsp`)** – Provides a form to submit job postings.
   - Uses `POST` request to `/handleForm` for form submission.

4. **Success (`success.jsp`)** – Displays details of the job after successfully adding it.

---

### **URL Mappings Required**
1. `/home` → Show homepage
2. `/` → Redirect to homepage
3. `/viewAllJobs` → Display all jobs
4. `/addJob` → Show job submission form
5. `/handleForm` → Handle job submission (POST request)

---

### **Next Steps: Building the Controller**
Since none of the views work yet, the next step is to create the **Controller** to handle these URLs. The controller will:
- Map URLs to JSP views.
- Process form submissions and store job postings.
- Send job data to the views.

D)
Now that we've started working on the backend, let's summarize what we've done so far and what needs to be done next.

---

### **Backend Implementation (Step-by-Step)**
1. **Set up `application.properties`**  
   - Configured the **prefix** (`/views/`) and **suffix** (`.jsp`) for JSP files.

2. **Created the `JobController` Class**
   - Marked it with `@Controller` annotation.
   - Defined URL mappings using `@RequestMapping`.

3. **Implemented Home Page Mapping**
   - Handles both `/` and `/home` URLs.
   - Returns `"home"` to load `home.jsp`.

4. **Implemented Add Job Page Mapping**
   - Handles `/addJob` URL.
   - Returns `"addJob"` to load `addJob.jsp`.
   - Fixed a case-sensitive issue with `addJob.jsp`.

---

### **Next Steps: Handling Form Submission**
- The **Add Job form** submits data to `/handleForm` using a `POST` request.
- We need to create a method in `JobController` to handle this request.
- Since it's a `POST` request, we will use:
  ```java
  @PostMapping("/handleForm")
  ```
- We need to:
  - Capture form data using a **Model class** (Job entity).
  - Store the job (temporarily, since no database yet).
  - Redirect to the success page.

---

E)
### **Handling Form Submission in Spring Boot**

Now that we’ve built the **JobController**, let's summarize the key steps we took to handle form submission.

---

### **1. Creating the Controller Method**
- We created the `handleForm` method to process **POST** requests.
- Instead of `@RequestMapping`, we used `@PostMapping` (specific for POST requests).
- The method receives a **JobPost** object, which holds form data.
- The method returns `"success"` to display the `success.jsp` page.

```java
@PostMapping("/handleForm")
public String handleForm(JobPost jobPost, Model model) {
    model.addAttribute("jobPost", jobPost);
    return "success";
}
```

---

### **2. Creating the Job Model (`JobPost`)**
- The `JobPost` class contains fields like `id`, `profile`, `description`, `experience`, and `techStack[]`.
- To avoid writing **getters, setters, constructors, and `toString()`**, we used **Lombok annotations**:

```java
@Data
@AllArgsConstructor
@NoArgsConstructor
@Component
public class JobPost {
    private int id;
    private String profile;
    private String description;
    private int experience;
    private List<String> techStack;
}
```

---

### **3. Fixing Common Errors**
1. **Mapped the correct package** (`com.telusko.jobapp.model`).
2. **Ensured `@PostMapping` was used**, not default `@RequestMapping` (which handles GET by default).
3. **Passed `JobPost` object to JSP** via `model.addAttribute("jobPost", jobPost);`.

---

### **4. Displaying Data in `success.jsp`**
In `success.jsp`, we used:
```jsp
<p>Job Profile: ${jobPost.profile}</p>
<p>Description: ${jobPost.description}</p>
<p>Experience: ${jobPost.experience}</p>
<p>Technologies: ${jobPost.techStack}</p>
```

---

### **Next Steps**
- Right now, data is not being saved. We need to store job posts in a **repository** (even temporarily).
- Also, we need to implement the **View Jobs** functionality to display added jobs.

F)
Here’s a clean summary of the lesson with key takeaways:  

---

### **Implementing a Layered Architecture for Job Management**  

#### **Problem Statement**  
- The application currently allows users to submit job posts. However, the data is not stored anywhere.  
- Since a database is not being used, the data will be stored in an **ArrayList**.  

#### **Layered Structure**  
Spring Boot follows a layered architecture:  
1. **Controller Layer** → Handles user requests and responses.  
2. **Service Layer** → Handles business logic and interacts with the repository.  
3. **Repository Layer** → Manages data storage and retrieval.  

#### **Steps Taken**  

1. **Create the Service Layer** (`JobService.java`)  
   - Created a new class `JobService` inside the `service` package.  
   - Implemented two methods:  
     ```java
     public void addJob(JobPost job) // Adds a job to the repository
     public List<JobPost> getAllJobs() // Retrieves all jobs from the repository
     ```  
   - The service layer will **delegate data handling** to the repository layer.  

2. **Create the Repository Layer** (`JobRepo.java`)  
   - Created `JobRepo` inside the `repo` package.  
   - Used an **ArrayList** to store job posts, simulating a database.  
   - Implemented methods:  
     ```java
     public List<JobPost> getAllJobs() { return jobs; }
     public void addJob(JobPost job) { jobs.add(job); }
     ```  
   - This ensures **data persistence within the session**.  

3. **Modify the Controller** (`JobController.java`)  
   - Injected `JobService` using `@Autowired`.  
   - Modified the `addJob` method to pass data from the controller → service → repository.  
   - Ensured the newly added job is **printed to verify** the addition.  

#### **Dependency Injection & Annotations**  
- Used `@Service` for the `JobService` class.  
- Used `@Repository` for the `JobRepo` class.  
- Used `@Autowired` to inject dependencies.  

#### **Testing the Implementation**  
- Restarted the server and submitted a new job.  
- Verified the job was added by printing the updated job list.  
- The job was successfully stored in the repository.  

#### **Next Steps**  
- Display the stored job posts in the **view page** instead of printing them in the console.  

---

This completes the backend logic for **storing jobs in-memory**. 

G)
### **Summary of View Implementation in Job Portal**
Now that we have set up the backend layers (Controller, Service, and Repository) to store job data in memory, we need to display these job posts on the **view all jobs** page.

#### **Steps Implemented:**
1. **Create a Controller Method for "View All Jobs"**
   - A new method **viewJobs()** is added to the controller.
   - It is mapped to the URL **"/viewAllJobs"** using `@GetMapping`.
   - This method will fetch job data and pass it to the view.

2. **Fetch Data from the Service Layer**
   - Retrieve all jobs from the service layer by calling `service.getAllJobs()`.
   - Store the result in a `List<JobPost>`.

3. **Pass Data to the View**
   - Use Spring's `Model` object to send the job list to the JSP.
   - `model.addAttribute("jobPosts", jobs);` ensures the data is accessible in the view.

4. **Update the View (JSP)**
   - Ensure the JSP uses `${jobPosts}` to display job listings dynamically.

5. **Test the Feature**
   - Navigate to **"/viewAllJobs"** and verify job data is displayed.
   - Add a new job and check if it appears in the list.
   - Since we are using an in-memory list, data is lost when the server restarts.

### **Next Steps:**
- Implement a **database layer** so that jobs persist even after restarting the application.
- Explore **frontend integration**, where a React-based UI or a mobile app can consume this backend.

H)
### **Quick Summary of the Job Portal Project**  

#### **Overview:**  
- The project is a **job portal** where employers can **post jobs**, and job seekers can **view all jobs**.  
- It follows a **layered architecture**: Controller → Service → Repository.  
- Currently, job data is stored **in memory** (a list). Later, we will integrate a **database**.  

#### **Key Functionalities Implemented:**  
1. **View All Jobs**  
   - A `GET` request to **"/viewAllJobs"** fetches and displays job postings.  
   - Data is retrieved from the **Service Layer** and passed to the JSP using **Model**.  

2. **Add a Job**  
   - The form submits data via `POST` to **"/addJob"**.  
   - Job details are stored in an **in-memory list** inside the **Repository Layer**.  

3. **Views in the Project:**  
   - **Home Page:** Landing page.  
   - **Add Job Page:** Employers fill out a form to post jobs.  
   - **Success Page:** Shown after posting a job.  
   - **View All Jobs Page:** Displays all jobs.  

#### **Project Architecture:**  
- **Controller:** Handles HTTP requests (`GET`, `POST`).  
- **Service Layer:** Acts as a bridge between the Controller and Repository.  
- **Repository Layer:** Stores job posts (currently using a list, will be replaced with a database).  

#### **Future Improvements:**  
- **Database Integration:** Persist job posts beyond server restarts.  
- **Removing JSP:** Switching to a modern frontend (React). This will remove dependencies like **Jasper, Jakarta JSP, GlassFish JSP**.  

#### **Next Steps:**  
- Try implementing the project yourself to solidify understanding.  
- Stay tuned for database integration and frontend migration.