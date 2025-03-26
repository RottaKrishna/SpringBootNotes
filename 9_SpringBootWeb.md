A)

---

### **Spring for Web Applications**  
Spring is an umbrella framework containing multiple projects, including those for building web applications:  
- **Spring Web** (part of Spring Boot)  
- **Spring MVC** (for non-Spring Boot applications)  

#### **Why Build Web Applications?**  
- Web apps dominate the industry, and even mobile apps often rely on web-based backends.  
- The **backend server** is responsible for providing dynamic content (data) to the **client** (browser/mobile app).  
- Modern frontends (React, Angular, Vanilla JS) rely on backend APIs to fetch JSON data.  

#### **How Web Apps Work**  
1. A **client (browser or mobile app)** sends a request to the backend.  
2. The **server processes the request** and responds with data (often JSON).  
3. The **frontend displays** the received data dynamically.  

#### **Introduction to Servlets**  
- **Servlets** are Java components that handle HTTP requests and responses.  
- They process incoming client requests, interact with databases or other services, and return responses.  
- Servlets run inside a **Servlet Container (Web Container)** like **Tomcat**, which provides the environment for handling web requests.  

#### **Why Are We Learning Servlets?**  
- Even though Spring provides **Spring MVC** and **Spring Web**, they are built on top of servlets.  
- Understanding servlets helps in understanding the internal workings of Spring web applications.  
- We’ll start with basic servlets and later move to Spring MVC.  

---

This sets the foundation for understanding how Spring handles web applications. 

B)
### **Setting Up Servlets with Embedded Tomcat**  

#### **Running a Servlet Requires a Web Container**  
- To run servlets, we need a **container** like **Apache Tomcat**.  
- Traditionally, Java web applications are packaged as **.war (Web Archive)** files and deployed on a **Tomcat server**.  
- However, modern applications can also run as **.jar (Java Archive)** files with an **embedded Tomcat server** (which simplifies configuration).  

#### **Downloading and Setting Up Tomcat**  
1. Download **Apache Tomcat** (e.g., version 10.1.16) from the official site.  
2. Extract the files, and inside the **webapps** folder, you can deploy your projects.  
3. Start/Stop Tomcat using:
   - `startup.sh` (Linux/macOS) or `startup.bat` (Windows)
   - `shutdown.sh` (Linux/macOS) or `shutdown.bat` (Windows)  

#### **Using Embedded Tomcat (Preferred for Learning Spring)**  
Instead of manually configuring an external Tomcat server, we use an **embedded Tomcat** inside our project.  
- This allows us to **run the project directly** from the IDE without extra setup.  

---

### **Creating a Servlet Project**  
#### **Step 1: Create a Maven Project**  
1. Open your **IDE (e.g., IntelliJ, Eclipse, or VS Code).**  
2. Create a **New Project** → **Maven Project**  
3. Name it **ServletEx**  
4. Choose **Java 17**  
5. Set the package name to `com.telusko`  

---

#### **Step 2: Add Dependencies**  
Since **Servlet API** is not part of JDK, we need to add dependencies.  

1. **Jakarta Servlet API (Servlet 4.0.4)**  
   - Required to write servlet code.  

2. **Tomcat Embedded (Version 8.5.96)**  
   - Runs the servlet without requiring an external Tomcat installation.  

**Add these to `pom.xml`:**  

```xml
<dependencies>
    <!-- Jakarta Servlet API -->
    <dependency>
        <groupId>jakarta.servlet</groupId>
        <artifactId>jakarta.servlet-api</artifactId>
        <version>4.0.4</version>
        <scope>provided</scope>
    </dependency>

    <!-- Embedded Tomcat -->
    <dependency>
        <groupId>org.apache.tomcat.embed</groupId>
        <artifactId>tomcat-embed-core</artifactId>
        <version>8.5.96</version>
    </dependency>
</dependencies>
```

---

#### **Step 3: Reload Maven**  
- After adding dependencies, **reload** Maven to download and apply them.  
- Check **External Libraries** to confirm Jakarta Servlet API and Embedded Tomcat are included.  

---

### **Next Steps**  
Now that the setup is ready, we can start writing **our first servlet** in the next session.

C)
### **Creating and Running a Simple Servlet**  

#### **1. Creating the Servlet Class**  
A **servlet** is simply a **Java class** that processes requests and sends responses.  
To create a servlet:  
1. **Create a new Java class**: `HelloServlet.java`  
2. **Extend** `HttpServlet` to get request-response capabilities.  
3. **Override** the `service()` method to handle requests.  

#### **2. Writing the `HelloServlet` Code**
```java
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;
import java.io.IOException;

public class HelloServlet extends HttpServlet {
    public void service(HttpServletRequest request, HttpServletResponse response) throws IOException {
        System.out.println("In Service");
    }
}
```
- **`service()` method** gets executed when a request is made.  
- **`HttpServletRequest`** stores client request data.  
- **`HttpServletResponse`** sends responses to the client.  
- For now, we just **print "In Service"** to check if the method is called.  

---

#### **3. Running the Servlet**  
Simply **running the main method** does not trigger the servlet.  
Since servlets are part of **web applications**, we need to:  
1. **Run an Embedded Tomcat Server.**  
2. **Send a request via a web browser (localhost:8080).**  

---

#### **4. Starting Embedded Tomcat**
Modify `App.java` to start Tomcat:  

```java
import org.apache.catalina.startup.Tomcat;

public class App {
    public static void main(String[] args) throws Exception {
        System.out.println("Hello World");

        // Step 1: Create a Tomcat instance
        Tomcat tomcat = new Tomcat();

        // Step 2: Start Tomcat
        tomcat.start();

        // Step 3: Keep the server running
        tomcat.getServer().await();
    }
}
```
- `Tomcat tomcat = new Tomcat();` → Creates a Tomcat server instance.  
- `tomcat.start();` → Starts the server.  
- `tomcat.getServer().await();` → Keeps the server running.  

---

#### **5. Testing the Server**
1. **Run `App.java`** → This starts Tomcat.  
2. **Open a browser** and visit `http://localhost:8080`  
3. **Error:** "Cannot reach" or **404 Not Found**  
   - **Why?** The servlet is not yet mapped to a URL.  
   - **Solution:** We need to tell Tomcat how to handle `localhost:8080/hello`.  

---

### **Next Step: Mapping a URL to Our Servlet**
In the next section, we will **map `/hello` to `HelloServlet`** so that Tomcat knows when to execute it.

D)
  

---

### **Mapping a Servlet in Embedded Tomcat**  

#### **1. Configuring Servlet Mapping**  
- **Old Way (XML):** Used `web.xml` to map URLs to servlets in external Tomcat.  
- **Modern Way (Annotations):** Uses `@WebServlet("/hello")` on the servlet class (works with external Tomcat).  
- **For Embedded Tomcat:** Configuration must be done manually in Java code.  

#### **2. Setting Up Servlet Mapping in Embedded Tomcat**  
1. **Get the Context Object:**  
   ```java
   Context context = tomcat.addContext("", null);
   ```
   - First parameter: Application name (`""` for default).  
   - Second parameter: Directory structure (`null` to keep it default).  

2. **Add the Servlet:**  
   ```java
   Tomcat.addServlet(context, "HelloServlet", new HelloServlet());
   ```
   - First: `context` object.  
   - Second: Name of the servlet (`HelloServlet`).  
   - Third: Object of the servlet class (`new HelloServlet()`).  

3. **Map the Servlet to a URL:**  
   ```java
   context.addServletMappingDecoded("/hello", "HelloServlet");
   ```
   - First: URL path (`"/hello"`).  
   - Second: Servlet name (must match the one used in `addServlet`).  

#### **3. Running the Tomcat Server**  
- **Start the server after mapping:**  
   ```java
   tomcat.start();
   tomcat.getServer().await();  // Keeps Tomcat running  
   ```
- **Default Port:** `8080`  
- **Change Port (Optional):**  
   ```java
   tomcat.setPort(8081);
   ```

#### **4. Testing**  
- **Start Tomcat, then visit:**  
   ```
   http://localhost:8080/hello
   ```
- **Console Output:** Should print `In Service`, confirming the servlet is working.  
- **Next Step:** Send a response to display content in the browser.  

---

Next, we'll learn how to return text (e.g., `"Hello, World!"`) to the client.

E)
### **Sending a Response from a Servlet**  

#### **1. Writing a Response Using `getWriter()`**
- The response object is empty by default; we need to write content to it.
- We obtain a `PrintWriter` to write the response:  

  ```java
  response.getWriter().print("Hello World");
  ```
- This method may throw an `IOException`, so we handle it properly.

#### **2. Using `PrintWriter` for Better Readability**  
Instead of calling `getWriter()` directly every time, store it in a variable:  

```java
PrintWriter out = response.getWriter();
out.println("Hello World");
```
- This makes it look similar to `System.out.println()`, but instead of printing to the console, it writes to the response object.

#### **3. Adding HTML Formatting**  
- We can include HTML tags inside the response:
  
  ```java
  out.println("<h2><b>Hello World</b></h2>");
  ```
- However, by default, the response treats this as plain text, displaying the tags instead of rendering them.

#### **4. Setting the Content Type**  
To ensure the browser interprets the response as HTML:  

```java
response.setContentType("text/html");
```
- This makes the browser render the HTML properly.

#### **5. Introduction to HTTP Methods**  
- **GET Request** (default in browser) → Retrieves data.  
- **POST Request** → Sends data to the server.  
- **PUT Request** → Updates existing data.  
- **DELETE Request** → Removes data.  

#### **6. Handling Specific Request Types in Servlets**  
Instead of a generic `service()` method, use:  
- `doGet(HttpServletRequest req, HttpServletResponse res)` → Handles `GET` requests.  
- `doPost(HttpServletRequest req, HttpServletResponse res)` → Handles `POST` requests.  

Example:  
```java
@Override
protected void doGet(HttpServletRequest req, HttpServletResponse res) throws IOException {
    res.setContentType("text/html");
    PrintWriter out = res.getWriter();
    out.println("<h2>Hello, World!</h2>");
}
```

#### **7. Why Use MVC Instead?**  
- Mixing HTML and Java in servlets makes the code messy.  
- In **MVC (Model-View-Controller)**:
  - **Servlet (Controller)** handles logic and request processing.  
  - **JSP (View)** handles the UI.  
  - **Java Classes (Model)** handle business logic and data.  
- Using MVC makes code **cleaner, modular, and easier to maintain**.  

---

### **Next Steps**
- We’ve successfully sent HTML responses from a servlet.
- Next, we’ll explore MVC and how Spring Boot simplifies this process.

F)
  

---

## **Building a Web Application with Java**  

1. **Servlets as Web Applications:**  
   - A web application consists of a **backend (server-side logic)** and a **frontend (UI built with HTML, CSS, JS, or frameworks like React)**.  
   - Servlets handle client requests, process them, and send responses.  
   - Data can be fetched from a database and returned to the client.  

2. **Issues with Servlets for HTML Content:**  
   - Writing HTML inside Java (Servlets) makes code bulky and hard to manage.  
   - Solution: **JSP (Java Server Pages)** allows writing HTML with embedded Java code.  

3. **Understanding JSP:**  
   - JSP is a **view technology** that allows writing Java inside an HTML page.  
   - This keeps presentation and business logic separate.  

4. **MVC Pattern in Java Web Applications:**  
   - **Model:** Represents data (Java class/POJO).  
   - **View:** JSP or other view technologies for UI.  
   - **Controller:** Handles requests (Servlets in Java EE or Spring controllers).  
   - Example: A client requests student details → Controller (Servlet) processes it → Retrieves model data → Sends it to the view (JSP).  

5. **How JSP Works in Tomcat:**  
   - Tomcat is both a **web server** and a **Servlet container**.  
   - JSP pages are internally converted into Servlets before execution.  

6. **Moving to Spring Boot:**  
   - Instead of managing Servlets and JSP manually, Spring Boot provides a cleaner way to build web applications using the **MVC pattern**.  
   - The next step is learning **Spring Boot for web development**.  

G)
### **Summary: Creating a Web Application with Spring Boot**  

1. **Two Approaches for Spring MVC**  
   - **Spring Framework** (requires manual configuration).  
   - **Spring Boot** (automates configuration, making development easier).  
   - The tutorial focuses on **Spring Boot first**, then compares it with traditional Spring MVC.  

2. **Setting Up a Spring Boot Project**  
   - Use **Spring Initializr** ([start.spring.io](https://start.spring.io)).  
   - Choose:
     - **Maven** as the build tool.  
     - **Java 21** as the language version.  
     - **Spring Boot 3.2** as the framework version.  
     - **Packaging:** `JAR` (since Spring Boot includes an embedded Tomcat server).  
     - **Dependency:** `Spring Web` (for building web applications).  

3. **Embedded Tomcat in Spring Boot**  
   - Unlike traditional deployment (WAR files on an external Tomcat), Spring Boot uses an **embedded Tomcat**, allowing applications to run as standalone JARs.  
   - The server **automatically runs on port 8080**.  

4. **Running the Spring Boot Application**  
   - Unzip and open the project in **IntelliJ IDEA Community Edition**.  
   - Run the **main application file**.  
   - The embedded Tomcat starts on **port 8080**.  
   - Visiting `http://localhost:8080` gives a **404 error** (no homepage defined yet).  

5. **Next Step: Creating a Controller**  
   - Instead of manually creating **Servlets** (like in traditional Java EE), Spring Boot uses **controllers** for handling requests.  
   - The next video will introduce **Spring Boot controllers**.  

H)
### **Summary: Creating a Homepage in Spring Boot with JSP**  

#### **1. Problem: 404 Error on Homepage**  
- Accessing `http://localhost:8080` returns **404 Not Found**.  
- This happens because there’s no homepage (view) defined yet.  

#### **2. Creating a Homepage with JSP**  
- JSP (Java Server Pages) is used as the **view technology** to render HTML.  
- Steps to create a homepage:
  1. **Create a new folder** inside `src/main/` called **webapp** (`src/main/webapp`).
  2. **Inside `webapp`**, create a file named **index.jsp**.  
  3. Write basic HTML code:
     ```jsp
     <%@ page language="java" %>
     <html>
       <body>
         <h2>Hello World</h2>
       </body>
     </html>
     ```
  - The `<%@ page language="java" %>` directive tells the server that JSP may contain Java code.  

#### **3. Why Doesn’t index.jsp Work Directly?**  
- In Spring Boot, JSP pages are **not called directly**.  
- Requests go through a **controller**, which returns the appropriate view.  
- Currently, **there is no controller** in the project, so the request isn’t reaching `index.jsp`.  

#### **4. Next Step: Creating a Controller**  
- The controller will handle requests and return views.  
- Instead of manually extending **Servlet**, Spring Boot provides an easy way to define controllers.  
- The next lesson will cover how to create a **Spring Boot controller** to serve the homepage.  

I)
### **Summary: Creating a Controller in Spring Boot**  

#### **1. Why Do We Need a Controller?**  
- A Controller **handles requests** and **returns views** in a Spring Boot application.  
- JSP pages **cannot be accessed directly**; they must go through a Controller.  

#### **2. Creating a Controller**  
- Inside the main package, create a **simple Java class** named `HomeController`.  
- Annotate it with `@Controller` to tell Spring Boot that it should handle web requests.  
- Example:  
  ```java
  import org.springframework.stereotype.Controller;

  @Controller
  public class HomeController {
  }
  ```
- `@Controller` is a **stereotype annotation** that makes this class a Spring-managed component.  

#### **3. Adding a Method to Handle Requests**  
- The Controller needs a **method** to return the `index.jsp` page.  
- Example:  
  ```java
  @Controller
  public class HomeController {
      public String home() {
          return "index.jsp";
      }
  }
  ```
- This method **returns the name** of the JSP page (`index.jsp`).  

#### **4. Why Doesn’t This Work?**  
- Running the application still gives **404 Not Found**.  
- **Possible Issues:**
  - The **home() method is not being called**.
  - There is **no URL mapping** telling Spring Boot which request should call `home()`.  

#### **5. Solution: Add URL Mapping**  
- Each Controller method must be mapped to a **specific URL** (like `/home`, `/students`, etc.).
- In the next step, we will use **`@RequestMapping` or `@GetMapping`** to link the homepage request to the `home()` method.

J)
### **Summary: Mapping Requests in Spring Boot**  

#### **1. Why Do We Need Request Mapping?**  
- The browser sends requests, but **Spring Boot doesn’t know which method to call**.  
- We must **map URLs to controller methods**.  

#### **2. Using `@RequestMapping`**  
- `@RequestMapping("path")` tells Spring Boot which URL triggers which method.  
- Example:  
  ```java
  import org.springframework.stereotype.Controller;
  import org.springframework.web.bind.annotation.RequestMapping;

  @Controller
  public class HomeController {
      @RequestMapping("/homepage")
      public String home() {
          return "index.jsp";
      }
  }
  ```
- When the user visits **`localhost:8080/homepage`**, this method will execute.  

#### **3. Issue: JSP Page Downloads Instead of Displaying**  
- Instead of rendering `index.jsp`, the browser **downloads it as a file**.  
- This happens because **Spring Boot does not support JSP by default**.  

#### **4. Solution: Add Tomcat Jasper Dependency**  
- We need to add the **Tomcat Jasper** dependency in `pom.xml`.  
- Go to **Maven Repository** and find the version matching your **Tomcat version** (e.g., `10.1.16`).  
- Add this dependency in `pom.xml`:  
  ```xml
  <dependency>
      <groupId>org.apache.tomcat.embed</groupId>
      <artifactId>tomcat-embed-jasper</artifactId>
      <scope>provided</scope>
  </dependency>
  ```
- Reload Maven (`mvn clean install`).  

#### **5. Verifying the Fix**  
- Restart the application and open **`localhost:8080/homepage`**.  
- Now, **`index.jsp` renders correctly** instead of downloading.  

#### **6. Alternative to `@RequestMapping`**  
- `@RequestMapping` works for all request types (`GET`, `POST`, etc.).  
- We also have:  
  - `@GetMapping` → For `GET` requests  
  - `@PostMapping` → For `POST` requests  
- We’ll explore these in later steps.  

K)
### **Summary: Sending Data to the Server in Spring Boot**  

#### **1. Understanding Client-Server Communication**  
- The client **sends data** to the backend.  
- The backend **processes the request** and returns a response.  
- Example: Adding two numbers and returning the result.  

#### **2. Creating a Simple HTML Form**  
- A form is created with:  
  - **Two input fields** (for numbers)  
  - **A submit button** (to send data to the server)  
- Example HTML form:  
  ```html
  <form action="/add" method="GET">
      <label>Enter First Number:</label>
      <input type="number" id="num1" name="num1"><br>
      <label>Enter Second Number:</label>
      <input type="number" id="num2" name="num2"><br>
      <button type="submit">Submit</button>
  </form>
  ```
- When submitted, this sends a **GET request** to `/add` with `num1` and `num2` as parameters.  

#### **3. Adding Basic Styling with CSS**  
- A `style.css` file is added to improve the UI.  
- CSS is linked in the HTML:  
  ```html
  <link rel="stylesheet" type="text/css" href="style.css">
  ```

#### **4. Handling the Server Request**  
- The form submits data to **`/add`**, but currently, there is **no controller** to handle it.  
- The browser shows **Error 404** because the server **doesn't recognize the `/add` URL**.  

#### **5. What’s Next?**  
- We need to create a **Spring Boot Controller** to handle the `/add` request.  
- The controller will:  
  - Read `num1` and `num2` from the URL.  
  - Add them.  
  - Return the result to the client.  

L)
### **Summary: Handling Requests in Spring Boot Controller**  

#### **1. Creating a Controller to Handle Requests**  
- A Spring Boot **Controller** processes user requests.  
- Multiple requests can be handled within the same controller.  
- Example:  
  - **User-related operations** → `UserController`  
  - **Product-related operations** → `ProductController`  
  - **Order-related operations** → `OrderController`  
- Since we only have a simple addition operation, we’ll handle it in `HomeController`.  

#### **2. Defining a Method to Handle `/add` Request**  
- The method should:  
  - Accept user inputs.  
  - Process the request.  
  - Return a **new page (`result.jsp`)** displaying the result.  

##### **HomeController.java** (Initial Setup)
```java
@Controller
public class HomeController {

    @RequestMapping("/add")
    public String add() {
        return "result.jsp"; // Returning the result page
    }
}
```
- The `@RequestMapping("/add")` annotation maps the `/add` request to the `add()` method.  
- `result.jsp` must exist, otherwise, we get an error.  

#### **3. Creating `result.jsp` Page**  
- This page will **display the result** of the addition.  
- For now, we just add a placeholder:  
  ```jsp
  <html>
  <body>
      <h2>Result is:</h2>
  </body>
  </html>
  ```
- Submitting the form will now redirect us to `result.jsp`.  

#### **4. Understanding DispatcherServlet**  
- `DispatcherServlet` is responsible for **mapping** URLs to the correct methods.  
- It checks:  
  - Which method should handle `/add`  
  - Which view (JSP page) should be returned  

#### **5. Accepting Values from the Client (Servlet Approach)**  
- The form sends two numbers (`num1`, `num2`) via **GET request**.  
- We extract values using `HttpServletRequest`.  
- Example:  

##### **Updated HomeController.java**
```java
import javax.servlet.http.HttpServletRequest;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller
public class HomeController {

    @RequestMapping("/add")
    public String add(HttpServletRequest req) {
        int num1 = Integer.parseInt(req.getParameter("num1"));
        int num2 = Integer.parseInt(req.getParameter("num2"));

        int result = num1 + num2;
        System.out.println("Result: " + result); // Debugging in console

        return "result.jsp";
    }
}
```
- `req.getParameter("num1")` gets the **first number** from the form.  
- `Integer.parseInt(...)` converts it from **String to int**.  
- The result is printed in the **console** for now.  

#### **6. Testing the Code**  
- Run the Spring Boot app.  
- Enter two numbers in the form and click **Submit**.  
- The result is printed in the console.  

#### **7. Next Steps**  
- Currently, the result is **only visible in the console**.  
- We need to display it **on the webpage (result.jsp)**.  
- This will be covered in the next step.  

M)
### **Summary of the Transcript**

#### **Goal: Display the Computation Result on the Result Page Instead of the Console**
1. **Current Issue:** The addition result is printed on the console, but we want to display it on `result.jsp`.

#### **Passing Data from Controller to JSP**
- We need to pass the computed result from the controller to the JSP page.
- There are multiple ways to send this data:
  - **Passing via URL parameters:** Appending `?result=95` to the URL.
  - **Using Session:** Storing the result in a session object.

#### **Using Session to Pass Data**
1. **Create an HttpSession Object in the Controller:**
   ```java
   @RequestMapping("/add")
   public String add(HttpServletRequest req, HttpSession session) {
       int num1 = Integer.parseInt(req.getParameter("num1"));
       int num2 = Integer.parseInt(req.getParameter("num2"));
       int result = num1 + num2;

       session.setAttribute("result", result);
       return "result.jsp";
   }
   ```
   - **Spring injects** `HttpSession`, so we don’t need to manually instantiate it.
   - The result is stored using `session.setAttribute("result", result);`.

2. **Retrieving Session Data in JSP:**
   - **Using JSP scriptlet:**
     ```jsp
     <%= session.getAttribute("result") %>
     ```
   - **Using JSTL Expression Language (Preferred Way):**
     ```jsp
     ${result}
     ```
   - **Why JSTL?**
     - Cleaner and more readable.
     - Avoids writing Java code in JSP.

#### **Fixing Errors**
- If `<%= session.getAttribute("result") %>` throws an error, ensure you add an `=` sign because it's a direct expression.
- Using JSTL (`${result}`) automatically looks into session/request attributes and retrieves the value.

#### **Key Takeaways**
- **Session allows data persistence across pages.**
- **Spring simplifies object injection.**
- **JSTL expressions are cleaner than scriptlets.**
- **Spring MVC provides better alternatives than using raw JSP.**

In the next step, we will see a cleaner Spring MVC approach instead of relying on JSP with scriptlets.

N)


#### **Simplifying Request Handling in Spring MVC**
Previously, we used `HttpServletRequest` to retrieve query parameters, but Spring provides a simpler way using method parameters.

---

### **1. Directly Using Method Parameters**
Instead of:
```java
public String add(HttpServletRequest req, HttpSession session) {
    int num1 = Integer.parseInt(req.getParameter("num1"));
    int num2 = Integer.parseInt(req.getParameter("num2"));
}
```
We can directly use method parameters:
```java
public String add(int num1, int num2) {
    int result = num1 + num2;
    return "result.jsp";
}
```
- Spring automatically maps query parameters (`?num1=5&num2=3`) to method arguments.

---

### **2. Handling Different Parameter Names with `@RequestParam`**
If the URL parameter names differ from method parameter names, use `@RequestParam`:
```java
public String add(@RequestParam("num1") int numA, @RequestParam("num2") int numB) {
    int result = numA + numB;
    return "result.jsp";
}
```
- The annotation ensures `num1` and `num2` from the URL map to `numA` and `numB` in the method.

---

### **3. Making `@RequestParam` Optional**
You can provide default values or make them optional:
```java
public String add(@RequestParam(value = "num1", required = false, defaultValue = "0") int numA,
                  @RequestParam(value = "num2", required = false, defaultValue = "0") int numB) {
    int result = numA + numB;
    return "result.jsp";
}
```
- If `num1` or `num2` is missing, it defaults to `0` instead of throwing an error.

---

### **Key Takeaways**
- **Direct parameter binding** avoids `HttpServletRequest`.
- **`@RequestParam`** allows flexibility when query parameter names differ.
- **Optional parameters** prevent errors when parameters are missing.

Next, we’ll remove the session object for an even cleaner approach.

o)


#### **Replacing `HttpSession` with `Model` in Spring MVC**
Previously, we used `HttpSession` to transfer data between the controller and view. Now, we will replace it with `Model`.

---

### **1. What is `Model` in Spring MVC?**
- `Model` is an interface introduced in Spring 2.5.
- It is used to pass data from the controller to the view.
- Unlike `HttpSession`, it is specific to a request and does not persist across multiple requests.

---

### **2. Replacing `HttpSession` with `Model`**
Before:
```java
public String add(@RequestParam("num1") int num1, 
                  @RequestParam("num2") int num2, 
                  HttpSession session) {
    int result = num1 + num2;
    session.setAttribute("result", result);
    return "result.jsp";
}
```
After:
```java
public String add(@RequestParam("num1") int num1, 
                  @RequestParam("num2") int num2, 
                  Model model) {
    int result = num1 + num2;
    model.addAttribute("result", result);
    return "result";
}
```
- `model.addAttribute("result", result);` replaces `session.setAttribute("result", result);`
- The view (JSP) will still access `${result}` using JSTL.

---

### **3. Removing `.jsp` Extension from View Name**
Hardcoded `.jsp` extensions make switching views (e.g., Thymeleaf, FreeMarker) difficult. Instead, configure Spring to resolve views dynamically.

#### **Spring Configuration**
Modify `application.properties` (for Spring Boot):
```
spring.mvc.view.prefix=/WEB-INF/views/
spring.mvc.view.suffix=.jsp
```
- This tells Spring MVC that views are stored in `/WEB-INF/views/` and have a `.jsp` suffix.

Now, update the controller:
```java
return "result";  // No .jsp extension
```
Spring will resolve it as:
```
/WEB-INF/views/result.jsp
```

---

### **4. Changing View Folder Location**
Instead of keeping JSP files in `webapp/WEB-INF/views`, move them to a custom `views` folder.

- Move JSP files to `src/main/resources/templates/views`
- Configure `InternalResourceViewResolver` in `SpringConfig`:
```java
@Bean
public InternalResourceViewResolver viewResolver() {
    InternalResourceViewResolver resolver = new InternalResourceViewResolver();
    resolver.setPrefix("/WEB-INF/views/");
    resolver.setSuffix(".jsp");
    return resolver;
}
```
Now, the application will find views automatically without specifying `.jsp`.

---

### **Key Takeaways**
- **Use `Model` instead of `HttpSession`** for passing request-scoped data.
- **Remove hardcoded `.jsp` extensions** for better flexibility.
- **Change the view folder location** by configuring the `ViewResolver`.

Next, we’ll fix issues related to finding the new view folder.

p)
### **Summary: Configuring View Resolver in Spring MVC**

Previously, we moved JSP files to a new `views` folder and removed the `.jsp` extension from controller return values. This caused issues because Spring couldn’t locate the views. The solution is to configure a **View Resolver**.

---

### **1. What is a View Resolver?**
- A **View Resolver** helps map logical view names (e.g., `"result"`) to actual JSP files.
- Spring MVC needs to know **where** the JSP files are located and **what extension** they use.

---

### **2. Configuring View Resolver using `application.properties`**
Spring Boot provides a properties file to configure such settings. The key properties for view resolution are:

```properties
spring.mvc.view.prefix=/views/
spring.mvc.view.suffix=.jsp
```

- **`spring.mvc.view.prefix`** → Specifies the folder where views (JSPs) are stored.
- **`spring.mvc.view.suffix`** → Specifies the file extension (`.jsp` in this case).

With this setup, when the controller returns `"result"`, Spring will automatically resolve it to:
```
/views/result.jsp
```
without needing the `.jsp` extension in the code.

---

### **3. Restarting and Testing**
- After adding the properties, restart the application.
- Access `http://localhost:8080`, submit values, and verify that the result is displayed.

---

### **4. CSS Issue**
- The CSS is not loading properly. This is likely because static files (CSS, JS, images) should be placed under:
  ```
  src/main/resources/static/
  ```
- We will fix the CSS issue separately.

---

### **Key Takeaways**
✅ **View Resolver** allows mapping logical names to actual JSP files.  
✅ **`application.properties`** helps configure prefixes (`/views/`) and suffixes (`.jsp`).  
✅ Restarting the application is required for changes to take effect.  
✅ **Next**, we need to fix the CSS issue.

Q)
### **Summary: Understanding ModelAndView in Spring MVC**

Previously, we used **Model** to pass data from the controller to the view. Now, we introduce **ModelAndView**, which allows us to pass both **data** and **view information** in a single object.

---

### **1. Fixing CSS Issue**
- **Static files** (e.g., `.css`) should be placed inside:
  - `src/main/webapp/` (for traditional setups)
  - **OR** `src/main/resources/static/` (recommended in Spring Boot)
- Moving `.css` to `static/` ensures it loads properly.

---

### **2. What is ModelAndView?**
- **Model**: Used to pass data (`addAttribute`).
- **ModelAndView**: Combines **Model (data)** and **View (JSP page)** into one object.

---

### **3. Replacing Model with ModelAndView**
#### **Old Approach (Using Model)**
```java
@RequestMapping("/add")
public String add(@RequestParam int num1, @RequestParam int num2, Model model) {
    int result = num1 + num2;
    model.addAttribute("result", result);
    return "result";  // Returning the view name
}
```
- **Problem**: We return two separate things: **Model (data)** and **View name**.

#### **New Approach (Using ModelAndView)**
```java
@RequestMapping("/add")
public ModelAndView add(@RequestParam int num1, @RequestParam int num2) {
    int result = num1 + num2;
    
    ModelAndView mv = new ModelAndView();
    mv.addObject("result", result);  // Adding data
    mv.setViewName("result");  // Setting view name
    
    return mv;  // Returning ModelAndView object
}
```
✅ **Advantage**: Combines **data** and **view** into a **single object**, making the controller cleaner.

---

### **4. Fixing the 404 Error**
- Initially, we forgot to **set the view name** (`setViewName("result")`), which caused a **404 error**.
- After adding `mv.setViewName("result")`, the page loaded correctly.

---

### **5. When to Use Model vs. ModelAndView?**
| Approach         | Use Case |
|-----------------|--------------------------------------|
| **Model**       | When you only need to pass **data** |
| **ModelAndView** | When you need to pass **data + view name** |

---

### **Final Thoughts**
✅ **Use `ModelAndView` when returning both data & view.**  
✅ **Use `Model` when only passing data.**  
✅ **Ensure CSS and other static files are placed in `static/` or `webapp/`.**  
✅ **Restart the application after making changes.**

R)
### **Summary: Understanding the Need for @ModelAttribute in Spring MVC**

So far, we have learned about **Model** and **ModelAndView** to pass data between a controller and a view. Now, we introduce **@ModelAttribute**, which allows us to **directly bind form data to an object**.

---

### **1. Why Do We Need @ModelAttribute?**
Currently, we are handling form data manually:
1. **Accepting values one by one** using `@RequestParam`.
2. **Creating an object** and setting values using setters.
3. **Sending the object** to the view.

This works but **becomes inefficient** when the class has many attributes (e.g., 10+ fields). Instead of manually mapping values, we can **automate this using @ModelAttribute**.

---

### **2. The Traditional Approach (Before @ModelAttribute)**

#### **Step 1: Create an Entity (Alien.java)**
```java
public class Alien {
    private int aid;
    private String aname;

    // Getters and Setters
    public int getAid() { return aid; }
    public void setAid(int aid) { this.aid = aid; }

    public String getAname() { return aname; }
    public void setAname(String aname) { this.aname = aname; }

    // toString Method
    @Override
    public String toString() {
        return "Alien [aid=" + aid + ", aname=" + aname + "]";
    }
}
```

---

#### **Step 2: Handling Form Submission in Controller**
```java
@Controller
public class AlienController {

    @RequestMapping("/addAlien")
    public ModelAndView addAlien(@RequestParam int aid, @RequestParam String aname) {
        Alien alien = new Alien();
        alien.setAid(aid);
        alien.setAname(aname);

        ModelAndView mv = new ModelAndView();
        mv.addObject("alien", alien);
        mv.setViewName("result");
        return mv;
    }
}
```
✅ **This works**, but:
- If we had 10+ attributes, we would need **10 `@RequestParam` parameters**.
- Every field must be manually mapped to the object.
- **Not scalable** for larger forms.

---

### **3. The Better Approach: Using @ModelAttribute**
Instead of manually mapping values, **Spring can automatically bind form data to an object**.

#### **Updated Controller Using @ModelAttribute**
```java
@RequestMapping("/addAlien")
public ModelAndView addAlien(@ModelAttribute Alien alien) {
    ModelAndView mv = new ModelAndView();
    mv.addObject("alien", alien);
    mv.setViewName("result");
    return mv;
}
```
✅ **Advantages:**
- **No need for @RequestParam**.
- **Spring automatically binds form data** to the `Alien` object.
- **Cleaner and scalable code**.

---

### **4. How Does @ModelAttribute Work?**
- Spring looks at the form field names in `index.jsp`.
- If they match the **variable names** in `Alien.java`, Spring automatically assigns values to the object.
- No need to manually set `alien.setAid(aid)` and `alien.setAname(aname)`.

---

### **5. What's Next?**
✅ We learned why `@ModelAttribute` is useful.  
➡️ Next, we will see how to **implement** it properly in forms and controllers.

S)
### **Notes on @ModelAttribute in Spring MVC**

#### **1. Basic Need for @ModelAttribute**
- When handling form submissions, instead of manually extracting parameters (`@RequestParam`), we can bind form data directly to an object.
- This avoids manually setting values to an object.
- Example: Instead of:
  ```java
  public String addAlien(@RequestParam int aid, @RequestParam String aname, Model model) {
      Alien alien = new Alien();
      alien.setAid(aid);
      alien.setAname(aname);
      model.addAttribute("alien", alien);
      return "result";
  }
  ```
  We can use:
  ```java
  public String addAlien(@ModelAttribute Alien alien) {
      return "result";
  }
  ```

#### **2. How @ModelAttribute Works**
- Spring automatically creates an instance of the `Alien` class.
- It binds request parameters (`aid`, `aname`) to matching fields in the object.
- No need for explicit setters in the controller.

#### **3. Customizing Model Attribute Name**
- By default, the model attribute name is the class name in lowercase (`alien`).
- You can customize it using:
  ```java
  public String addAlien(@ModelAttribute("alien1") Alien alien) {
      return "result";
  }
  ```
  - Now, in JSP, you must refer to `${alien1}` instead of `${alien}`.

#### **4. @ModelAttribute on Method Level**
- Instead of using `model.addAttribute()`, we can use `@ModelAttribute` at the method level to provide common data.
- Example:
  ```java
  @ModelAttribute("course")
  public String getCourseName() {
      return "Java";
  }
  ```
  - This value (`Java`) is added to the model automatically for every request.

#### **5. When @ModelAttribute is Optional**
- If the parameter name matches the model attribute name, the annotation can be omitted.
- Example:
  ```java
  public String addAlien(Alien alien) {  // No need for @ModelAttribute
      return "result";
  }
  ```

#### **6. When to Use @ModelAttribute**
- When binding form data to objects.
- When passing common data across multiple handlers.
- When simplifying model attribute creation.

#### **7. When NOT to Use @ModelAttribute**
- If the object is manually constructed inside the method.
- If using `@RequestParam` is sufficient (for simple cases).

This makes form handling cleaner and avoids repetitive code.