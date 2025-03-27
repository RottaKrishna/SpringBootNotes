A)
### **Building an E-Commerce Backend with Spring Boot**  

In this project, we will build the **backend** for a simple e-commerce application using **Spring Boot**. The frontend is developed using **React**, and our backend will provide **REST APIs** for various operations.  

---

## **1. Project Overview**
### **Features to Implement:**
✅ View all products  
✅ View a single product  
✅ Add a new product  
✅ Update product details  
✅ Delete a product  

### **Architecture Overview**
1. **Frontend:** React (for UI)  
2. **Backend:** Spring Boot (REST API)  
3. **Database:** PostgreSQL  

The backend will **expose REST endpoints** to communicate with the React frontend using **JSON data**.

---

## **2. Setting Up the Spring Boot Project**
- We will create a **Spring Boot application** with the required dependencies.

### **Dependencies (in `pom.xml`)**
```xml
<dependencies>
    <!-- Spring Boot Web for building REST APIs -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>

    <!-- Spring Boot JPA for database interaction -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-jpa</artifactId>
    </dependency>

    <!-- PostgreSQL Driver -->
    <dependency>
        <groupId>org.postgresql</groupId>
        <artifactId>postgresql</artifactId>
        <scope>runtime</scope>
    </dependency>

    <!-- Lombok (for reducing boilerplate code) -->
    <dependency>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
        <scope>provided</scope>
    </dependency>

    <!-- Spring Boot DevTools (for automatic restart) -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-devtools</artifactId>
        <scope>runtime</scope>
        <optional>true</optional>
    </dependency>
</dependencies>
```

---

## **3. Configuring the Database**
We will use **PostgreSQL** for storing product data.

### **`application.properties`**
```properties
# Database Configuration
spring.datasource.url=jdbc:postgresql://localhost:5432/ecommerce_db
spring.datasource.username=postgres
spring.datasource.password=yourpassword
spring.datasource.driver-class-name=org.postgresql.Driver

# JPA Configuration
spring.jpa.database-platform=org.hibernate.dialect.PostgreSQLDialect
spring.jpa.hibernate.ddl-auto=update
```

---

## **4. Creating the Product Model**
The **Product** entity represents a product in our system.

### **`Product.java`**
```java
import jakarta.persistence.*;
import lombok.*;

@Entity
@Getter
@Setter
@NoArgsConstructor
@AllArgsConstructor
@ToString
public class Product {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;
    private String brand;
    private String description;
    private double price;
    private String category;
    private int stock;
    private String releaseDate;
    private String imageUrl;
}
```

---

## **5. Creating the Repository**
### **`ProductRepository.java`**
```java
import org.springframework.data.jpa.repository.JpaRepository;

public interface ProductRepository extends JpaRepository<Product, Long> {
}
```
✔ Extends `JpaRepository` for basic CRUD operations.  

---

## **6. Creating the Service Layer**
### **`ProductService.java`**
```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import java.util.List;

@Service
public class ProductService {
    @Autowired
    private ProductRepository productRepository;

    public List<Product> getAllProducts() {
        return productRepository.findAll();
    }

    public Product getProductById(Long id) {
        return productRepository.findById(id).orElse(null);
    }

    public Product addProduct(Product product) {
        return productRepository.save(product);
    }

    public Product updateProduct(Long id, Product product) {
        if (productRepository.existsById(id)) {
            product.setId(id);
            return productRepository.save(product);
        }
        return null;
    }

    public void deleteProduct(Long id) {
        productRepository.deleteById(id);
    }
}
```
✔ Implements CRUD logic using `ProductRepository`.

---

## **7. Creating the Controller**
### **`ProductController.java`**
```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.*;

import java.util.List;

@RestController
@RequestMapping("/api/products")
@CrossOrigin(origins = "http://localhost:3000")
public class ProductController {
    @Autowired
    private ProductService productService;

    @GetMapping
    public List<Product> getAllProducts() {
        return productService.getAllProducts();
    }

    @GetMapping("/{id}")
    public Product getProductById(@PathVariable Long id) {
        return productService.getProductById(id);
    }

    @PostMapping
    public Product addProduct(@RequestBody Product product) {
        return productService.addProduct(product);
    }

    @PutMapping("/{id}")
    public Product updateProduct(@PathVariable Long id, @RequestBody Product product) {
        return productService.updateProduct(id, product);
    }

    @DeleteMapping("/{id}")
    public void deleteProduct(@PathVariable Long id) {
        productService.deleteProduct(id);
    }
}
```
✔ Provides endpoints for fetching, adding, updating, and deleting products.

---

## **8. Running the Backend**
Start the Spring Boot application:
```bash
mvn spring-boot:run
```
✔ The backend is now running at `http://localhost:8080/api/products`.

---

## **9. Testing the APIs**
We can use **Postman** or **cURL** to test the endpoints.

### **Get All Products**
```bash
curl -X GET http://localhost:8080/api/products
```

### **Add a Product**
```bash
curl -X POST http://localhost:8080/api/products \
     -H "Content-Type: application/json" \
     -d '{
           "name": "HP Laptop",
           "brand": "HP",
           "description": "Gaming Laptop",
           "price": 899.99,
           "category": "Laptop",
           "stock": 10,
           "releaseDate": "2024-07-03",
           "imageUrl": "https://example.com/laptop.jpg"
         }'
```

---

## **10. Next Steps**
- **Connect React Frontend with the Backend.**
- **Implement User Authentication (if needed).**
- **Add Cart Management.**

B)
### **Summary of React Frontend Setup for the E-Commerce Project**

#### **Overview**
- The React project serves as the frontend for the e-commerce application.
- The goal is to develop a Spring Boot backend that interacts with this frontend.
- UI is already built with React; the focus is on backend development.

---

### **Steps to Set Up and Run the React Frontend**
1. **Download the Project**
   - Clone the repository using Git:
     ```sh
     git clone <repository_link>
     ```
   - OR download the project as a ZIP file and extract it.

2. **Ensure Node.js and npm are Installed**
   - Check Node.js version:
     ```sh
     node -v
     ```
   - Check npm version:
     ```sh
     npm -v
     ```
   - If not installed, download and install Node.js from [nodejs.org](https://nodejs.org).

3. **Install Dependencies**
   - Navigate to the project directory in the terminal.
   - Run:
     ```sh
     npm install
     ```
   - This installs all required dependencies (like React) inside the `node_modules` folder.

4. **Run the Project**
   - Start the development server:
     ```sh
     npm run dev
     ```
   - The frontend will be available at `http://localhost:5173` (or another port if configured).

---

### **Project Structure & Important Files**
- **`index.html`** → The root HTML file.
- **`main.jsx`** → The entry point where the app is rendered.
- **`App.js`** → Contains routes and components for navigation.
- **`components/`** → Contains individual React components.
  - **`Home.jsx`** → Fetches and displays all products.
  - **`ProdPage.jsx`** → Displays details of a single product.
  - **`AddProduct.jsx`** → Handles adding a new product.
  - **`Navbar.jsx`** → Contains the search bar.

---

### **Backend Dependency**
- The frontend fetches data from a backend running on `localhost:8080` using REST APIs.
- If the backend is not running, the UI will display "Disconnected" messages.

---

### **Alternative Testing Using Postman**
- If the backend is not connected, REST API calls can be tested using Postman.
- URLs like `http://localhost:8080/api/products` will return JSON responses.

---

### **Next Steps**
- Start developing the Spring Boot backend to serve data to this frontend.
- Implement REST APIs to handle product operations (Add, Update, Delete, Fetch).


C)
Here's a structured summary of the video along with clear notes for your reference:  

---

# **React Frontend Setup & Running the Project**
1. **Downloading and Setting Up**  
   - Download the frontend project from GitHub.  
   - Ensure **Node.js** and **npm** are installed:
     ```sh
     node -v
     npm -v
     ```
   - Install dependencies:
     ```sh
     npm install
     ```
   - Start the React frontend:
     ```sh
     npm run dev
     ```
   - The frontend runs, but shows "Something went wrong" since the backend is not yet running.

2. **Understanding the React Files**
   - The frontend fetches data from:  
     ```
     GET http://localhost:8080/api/product
     ```
   - If the backend is down, Postman will show a connection error.
   - **Key files:**
     - `index.html` → Main entry point.
     - `main.jsx` → Mounts the React app.
     - `App.js` → Defines routes.
     - `components/home.jsx` → Fetches product data from API.
     - `package.json` → Similar to `pom.xml`, manages dependencies and scripts.

---

# **Backend Setup (Spring Boot)**
### **Step 1: Creating a New Spring Boot Project**
1. Go to **start.spring.io** and create a project:
   - **Group:** `com.telusko`
   - **Artifact:** `SpringEcom`
   - **Java Version:** `21`
   - **Dependencies:**
     - **Spring Web** (for REST API)
     - **Lombok** (reduces boilerplate code)
     - **PostgreSQL Driver** (for database)

2. Download and open the project in **IntelliJ** or any preferred IDE.

---

# **Step 2: Running a Basic API**
1. Check if the project runs successfully:
   - Run the project in IntelliJ.
   - Ensure it starts on **port 8080**.

2. **Creating a Simple Hello API**
   ```java
   @RestController
   public class HelloController {
       @GetMapping("/hello")
       public String greet() {
           return "Welcome to Telusko";
       }
   }
   ```
   - Test via Postman:  
     ```
     GET http://localhost:8080/hello
     ```
   - Should return: `"Welcome to Telusko"`

---

# **Step 3: Setting Up Product API**
1. **Create ProductController**
   ```java
   @RestController
   @RequestMapping("/api")
   public class ProductController {
       @GetMapping("/products")
       public String getProducts() {
           return "All Products";
       }
   }
   ```
   - Test in Postman:
     ```
     GET http://localhost:8080/api/products
     ```
   - Returns: `"All Products"`

2. **Why the Frontend Still Fails?**
   - The frontend expects **a list of product objects**, not just `"All Products"`.
   - Next step: Define a `Product` class and connect to a **database**.

---

# **Next Steps**
- Define the **Product entity** with fields like ID, name, brand, price, etc.
- Connect the project to **PostgreSQL**.
- Fetch products dynamically from the database.

D)
 

---

### **Creating the Product Model and Database Integration**
  
#### **1. Creating the Product Model Class**  
- A `Product` class is created inside the `model` package.  
- It is annotated with `@Entity` to map it to a database table.  
- **Dependencies Required:** Spring Data JPA and Lombok.  
- **Lombok Annotations Used:**  
  - `@Data` → Generates getters, setters, `toString`, etc.  
  - `@NoArgsConstructor` → Generates a no-arguments constructor.  
  - `@AllArgsConstructor` → Generates a constructor with all fields.  

#### **2. Fields in the Product Model**  
```java
@Entity
@Data
@NoArgsConstructor
@AllArgsConstructor
public class Product {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;    
    private String name;
    private String description;
    private String brand;
    private BigDecimal price;
    private String category;
    private Date releaseDate;
    private boolean productAvailable;
    private int stockQuantity;
}
```
- **`@Id`** → Defines the primary key.  
- **`@GeneratedValue(strategy = GenerationType.IDENTITY)`** → Auto-generates unique IDs.  
- Fields include `name`, `description`, `brand`, `price`, `category`, `releaseDate`, `productAvailable`, and `stockQuantity`.  

#### **3. Configuring Database Connection**  
- Add the PostgreSQL database configuration in `application.properties`:  
```properties
spring.datasource.url=jdbc:postgresql://localhost:5432/telusko
spring.datasource.username=postgres
spring.datasource.password=0000
spring.jpa.hibernate.ddl-auto=update
```
- `ddl-auto=update` ensures the table is created if it doesn't exist.  

#### **4. Verifying the Database Setup**  
- **Check in pgAdmin:** Ensure the database `telusko` exists.  
- **Restart the application:**  
  - The `product` table should be created automatically.  
  - Verify in pgAdmin by navigating to **Schemas → Tables → product**.  

#### **5. Next Steps**  
- The database is ready but contains no data.  
- Next, we need to insert dummy data and implement CRUD operations.  

---

This sets up the product model and ensures it’s correctly linked to the database. 


E)
---

## **Fetching All Products - Implementing Service and Repository Layers**

### **1. Objective**
- Retrieve all products from the database via a structured Spring Boot architecture using the Controller, Service, and Repository layers.

### **2. Updating the Controller**
- Modify the controller to return a list of products instead of a string.
- Ensure necessary imports (`List` and `Product` model) are included.
- The controller should delegate the request to the service layer.

#### **Updated Controller Code:**
```java
@RestController
@RequestMapping("/products")
public class ProductController {
    
    @Autowired
    private ProductService productService;

    @GetMapping
    public List<Product> getAllProducts() {
        return productService.getAllProducts();
    }
}
```

### **3. Creating the Service Layer**
- The service layer handles business logic and communicates with the repository layer.
- Create a package named `service` and add a class `ProductService`.
- The service method fetches data from the repository.

#### **ProductService Implementation:**
```java
@Service
public class ProductService {
    
    @Autowired
    private ProductRepo productRepo;

    public List<Product> getAllProducts() {
        return productRepo.findAll();
    }
}
```

### **4. Creating the Repository Layer**
- Create a package `repo`.
- Implement a repository interface extending `JpaRepository`.
- This provides built-in database operations like `findAll()`.

#### **ProductRepo Implementation:**
```java
@Repository
public interface ProductRepo extends JpaRepository<Product, Integer> {
}
```

### **5. Verifying the Setup**
- Ensure `application.properties` contains PostgreSQL configurations:
```properties
spring.datasource.url=jdbc:postgresql://localhost:5432/telusko
spring.datasource.username=postgres
spring.datasource.password=0000
spring.jpa.hibernate.ddl-auto=update
```
- Restart the application and verify in Postman:
  - **GET Request:** `http://localhost:8080/products`
  - Response should be an empty list if no products exist.

### **6. Adding Sample Data**
- Instead of manually inserting data, use a provided SQL file with insert statements.
- Insert sample data into the database using pgAdmin.

### **7. Handling Cross-Origin Requests (CORS Issue)**
- If the frontend runs on a different port (e.g., `5173` for React/Vue), Spring Security blocks it by default.
- To allow cross-origin requests, add `@CrossOrigin` on the controller:

```java
@CrossOrigin(origins = "*")
@RestController
@RequestMapping("/products")
public class ProductController {
    // Code remains the same
}
```

### **8. Verifying UI Integration**
- Refresh the frontend to check if data appears.
- If CORS issues persist, verify port configurations.

### **9. Next Steps**
- Implement fetching a single product by ID.
- Display product details on a separate UI page.

---

This concludes fetching all products via the controller, service, and repository layers. The next step is implementing individual product retrieval.

F)
clear and simple note summarizing how to return an appropriate HTTP status code along with a response in Spring Boot:

---

### **Returning HTTP Status Codes in Spring Boot**
When returning data from an API, it's essential to include an appropriate **HTTP status code** to indicate the request's outcome.

#### **HTTP Status Codes Overview:**
1. **200 Series (Success)**
   - `200 OK` → Default for successful requests.
   - `201 Created` → Used when a resource is successfully created.
   - `202 Accepted` → Indicates that the request has been accepted for processing.

2. **400 Series (Client Errors)**
   - `400 Bad Request` → When the client sends an invalid request.
   - `401 Unauthorized` → When authentication is required but not provided.
   - `404 Not Found` → When the requested resource is missing.

3. **500 Series (Server Errors)**
   - `500 Internal Server Error` → When an unexpected server-side error occurs.

---

### **How to Return a Custom HTTP Status Code in Spring Boot**
Instead of returning a raw list of products, wrap it in a `ResponseEntity<T>` object, which allows us to send both **data** and **status code**.

#### **Example Implementation**
Modify the controller method to return a `ResponseEntity`:

```java
@GetMapping("/products")
public ResponseEntity<List<Product>> getAllProducts() {
    List<Product> products = productService.getAllProducts();
    
    // Return products with 200 OK status
    return new ResponseEntity<>(products, HttpStatus.OK);
}
```

#### **How It Works**
- `ResponseEntity<List<Product>>` → Wraps the response.
- `HttpStatus.OK` → Sends **200 OK** status along with the data.

---

### **Testing the Response in Postman**
1. **Before adding `ResponseEntity`**: Only the list of products was returned, with a default status of `200 OK`.
2. **After using `ResponseEntity`**: You can now explicitly define the status code.
   - Example: If we return `HttpStatus.ACCEPTED (202)`, the response in Postman shows:
     ```json
     Status: 202 Accepted
     ```

---

This method provides **better control** over the responses and allows API clients to handle different scenarios effectively.

G)
### **Fetching a Product by ID in Spring Boot**
When a user clicks on a product in the frontend, we need to **fetch the product details** from the backend. The request will be in the format:

```
GET /product/{id}
```

### **1. Implementing the Controller Method**
We define an API endpoint to fetch a product by its ID.

#### **Code:**
```java
@GetMapping("/product/{id}")
public ResponseEntity<Product> getProductById(@PathVariable int id) {
    Product product = productService.getProductById(id);

    if (product != null) {
        return new ResponseEntity<>(product, HttpStatus.OK);
    } else {
        return new ResponseEntity<>(HttpStatus.NOT_FOUND);
    }
}
```

#### **Explanation:**
- `@GetMapping("/product/{id}")` → Maps the request to `/product/{id}`.
- `@PathVariable int id` → Captures the ID from the URL.
- Calls `productService.getProductById(id)`.
- If the product **exists**, return it with `200 OK`.
- If the product **does not exist**, return `404 Not Found`.

---

### **2. Implementing the Service Layer**
We fetch the product from the database using Spring Data JPA.

#### **Code:**
```java
public Product getProductById(int id) {
    return productRepository.findById(id).orElse(null);
}
```

#### **Explanation:**
- `findById(id)` returns an **Optional<Product>**.
- `orElse(null)` returns **null** if the product is not found.

---

### **3. Handling Not Found Case**
Instead of returning `null`, we can:
1. Return an **empty product** with `id = -1` (to indicate missing data).
2. Throw a **custom exception** (better practice).

Example of returning an empty product:
```java
public Product getProductById(int id) {
    return productRepository.findById(id).orElse(new Product(-1, "N/A", "N/A", "N/A", 0));
}
```

---

### **4. Testing with Postman**
- **Valid Request:**  
  `GET /product/8`  
  Response:
  ```json
  {
    "id": 8,
    "name": "Galaxy S22",
    "category": "Mobile",
    "brand": "Samsung",
    "price": 1200,
    "stock": 5
  }
  ```
- **Invalid Request:**  
  `GET /product/100`  
  Response:
  ```
  Status: 404 Not Found
  ```

---

### **5. Formatting Date in JSON Response**
By default, dates may not be in a user-friendly format. We can customize it using `@JsonFormat`.

#### **Modify Product Entity**
```java
@JsonFormat(shape = JsonFormat.Shape.STRING, pattern = "dd-MM-yyyy")
private Date createdDate;
```

#### **Explanation:**
- Converts the date format to **"dd-MM-yyyy"** (e.g., `25-03-2025`).
- Uses **Jackson Library** (`@JsonFormat`) to format the response.

---

### **Conclusion**
- Implemented `GET /product/{id}` API.
- Handled missing products with `404 Not Found`.
- Improved JSON response format.
- Ensured frontend compatibility.

This is the standard way to fetch product details in a Spring Boot application. 

H)
### **Notes: Handling Image Upload in Spring Boot**

#### **1. Understanding the Requirement**
- We want to allow the admin to add a new product with an image.
- The product information (JSON) and the image file will be sent separately from the client to the server.
- The image will be stored in the database in binary format.

---

### **2. Updating the Product Entity**
To store image details, we add three fields:
1. `imageName`: Stores the image filename.
2. `imageType`: Stores the image MIME type (e.g., `image/jpeg`).
3. `imageData`: Stores the actual image in byte format.

```java
@Entity
public class Product {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    private String name;
    private String category;
    private double price;
    private int quantity;
    private LocalDate releaseDate;

    private String imageName;
    private String imageType;

    @Lob // Large Object - used for binary data
    private byte[] imageData;
    
    // Getters and Setters
}
```

---

### **3. Implementing Image Upload in Controller**
- We use `@RequestPart` to handle both JSON and file separately.
- `MultipartFile` is used to receive the image.

```java
@RestController
@RequestMapping("/api/products")
public class ProductController {
    
    @Autowired
    private ProductService productService;

    @PostMapping
    public ResponseEntity<?> addProduct(
        @RequestPart("product") Product product,
        @RequestPart("imageFile") MultipartFile imageFile) {
        
        try {
            Product savedProduct = productService.addProduct(product, imageFile);
            return new ResponseEntity<>(savedProduct, HttpStatus.CREATED);
        } catch (Exception e) {
            return new ResponseEntity<>(e.getMessage(), HttpStatus.INTERNAL_SERVER_ERROR);
        }
    }
}
```

---

### **4. Implementing Service Layer**
- Extracts image details and saves them into the database.

```java
@Service
public class ProductService {

    @Autowired
    private ProductRepository productRepository;

    public Product addProduct(Product product, MultipartFile imageFile) throws IOException {
        product.setImageName(imageFile.getOriginalFilename());
        product.setImageType(imageFile.getContentType());
        product.setImageData(imageFile.getBytes());
        
        return productRepository.save(product);
    }
}
```

---

### **5. Testing via Postman**
- **Use `multipart/form-data` in the `Body` section**:
  - **Key**: `product` → **Value**: JSON with product details.
  - **Key**: `imageFile` → **Value**: Select an image file.

---

### **6. Next Steps**
- Currently, images are stored, but not retrieved in UI.
- In the next step, we will **implement image retrieval** so that images appear in the frontend.

---

This implementation ensures a structured way to handle product uploads with images while keeping the backend logic clean and efficient.


I)
### **Spring Boot: Fetching Product Images via API**

#### **Problem: Image Not Displaying in UI**
- The UI requests the product details and the product image separately.
- The image request URL is incorrect or not mapped properly in the backend.

#### **Solution: Create a New API Endpoint to Fetch Images**
1. **Define a New Mapping for Image Retrieval**
   ```java
   @GetMapping("/product/image/{productId}")
   public ResponseEntity<byte[]> getImageByProductId(@PathVariable Long productId) {
       Product product = productService.getProductById(productId);
       
       if (product == null || product.getImageData() == null) {
           return ResponseEntity.notFound().build();
       }
       
       return ResponseEntity.ok()
               .contentType(MediaType.IMAGE_JPEG) // Set appropriate image type
               .body(product.getImageData());
   }
   ```
   - This method:
     - Accepts a product ID from the UI.
     - Fetches the product details.
     - Retrieves and returns only the image data.
     - Returns a 404 response if the product or image is missing.

2. **Restart the Spring Boot Application**
   - Ensure the server restarts correctly and check for errors.

3. **Test the API in Postman**
   - Send a GET request to:
     ```
     http://localhost:8080/product/image/13
     ```
   - Verify if the image data is returned correctly.

4. **Verify UI Updates**
   - Refresh the UI to check if images are now displayed properly.

#### **Next Steps**
- Implement **Update** and **Delete** functionality for product images.

J)
### **Spring Boot: Implementing Update and Delete Operations**

#### **Switching to Frontend Version 4**
- The UI code used earlier was from frontend version 3.
- To enable update and delete functionality, switched to frontend version 4.
- Ran the project using:
  ```sh
  npm run dev
  ```

### **Implementing Product Update Functionality**
#### **Issue Encountered**
- When updating a product, the UI displayed an "Invalid date" error.
- This was due to a change in the date format in the UI.
- Issue was resolved by handling it on the frontend.

#### **Backend Changes for Update**
- Created a new update handler in the controller:
  ```java
  @PutMapping("/product/update/{id}")
  public ResponseEntity<String> updateProduct(@PathVariable Long id,
                                              @RequestParam("product") String productJson,
                                              @RequestParam(value = "image", required = false) MultipartFile imageFile) {
      try {
          Product updatedProduct = productService.updateProduct(id, productJson, imageFile);
          return ResponseEntity.ok("Updated");
      } catch (IOException e) {
          return ResponseEntity.status(HttpStatus.BAD_REQUEST).body(e.getMessage());
      }
  }
  ```

#### **Updating the Service Layer**
- Since Spring Data JPA does not have an `update` method, `save` is used for both insert and update operations.
  ```java
  public Product updateProduct(Long id, String productJson, MultipartFile imageFile) throws IOException {
      Product product = objectMapper.readValue(productJson, Product.class);
      product.setId(id);
      if (imageFile != null) {
          product.setImageData(imageFile.getBytes());
      }
      return productRepository.save(product);
  }
  ```

- Restarted the application and verified that the update functionality worked correctly in the UI.

#### **Optimization: Combining Add and Update Methods**
- Since both add and update use `save()`, they were merged into a single method `addOrUpdateProduct`.
  ```java
  public Product addOrUpdateProduct(Product product, MultipartFile imageFile) throws IOException {
      if (imageFile != null) {
          product.setImageData(imageFile.getBytes());
      }
      return productRepository.save(product);
  }
  ```

### **Implementing Product Delete Functionality**
#### **Backend Changes for Delete**
- Created a delete mapping in the controller:
  ```java
  @DeleteMapping("/product/{id}")
  public ResponseEntity<String> deleteProduct(@PathVariable Long id) {
      Product product = productService.getProductById(id);
      if (product != null) {
          productService.deleteProduct(id);
          return ResponseEntity.ok("Deleted");
      }
      return ResponseEntity.status(HttpStatus.NOT_FOUND).body("Product not found");
  }
  ```

- Implemented delete logic in the service layer:
  ```java
  public void deleteProduct(Long id) {
      productRepository.deleteById(id);
  }
  ```

#### **Verification**
- Restarted the server and tested the delete functionality in the UI.
- Products were successfully deleted from both the frontend and database.

### **Summary**
- **Update:** Implemented update functionality using `save()` method.
- **Optimization:** Merged add and update operations into a single method.
- **Delete:** Implemented delete functionality using `deleteById()`.
- Successfully verified update and delete operations via UI.

This concludes the implementation of update and delete operations in Spring Boot.

K)


---

### **Implementing Search Feature in Spring Boot**
#### **1. Understanding the Search Feature**
- The search feature will allow users to search for products using a keyword.
- The search request follows the format:  
  ```
  GET /product/search?keyword=someValue
  ```
- The frontend sends a query parameter (`keyword`), which will be handled in the backend.

#### **2. Implementing Search in the Controller**
- Create a method in the controller to handle the search request.
- Use `@GetMapping("/product/search")` to define the endpoint.
- Accept `keyword` as a query parameter using `@RequestParam`.

**Code:**
```java
@GetMapping("/product/search")
public ResponseEntity<List<Product>> searchProducts(@RequestParam String keyword) {
    System.out.println("Searching with: " + keyword);  // Debugging log
    List<Product> products = productService.searchProducts(keyword);
    return new ResponseEntity<>(products, HttpStatus.OK);
}
```

#### **3. Implementing Search in the Service Layer**
- The service layer will call the repository to fetch matching products.

**Code:**
```java
public List<Product> searchProducts(String keyword) {
    return productRepository.searchProducts(keyword);
}
```

#### **4. Implementing Search in the Repository (Using JPQL)**
- Define a custom query using `@Query` annotation in `ProductRepository`.
- Use JPQL to perform case-insensitive search in multiple fields (`name`, `description`, `brand`, and `category`).
- Use `LIKE` with wildcard `%` to allow partial matching.

**Code:**
```java
@Repository
public interface ProductRepository extends JpaRepository<Product, Long> {
    @Query("SELECT p FROM Product p WHERE " +
           "LOWER(p.name) LIKE LOWER(CONCAT('%', :keyword, '%')) OR " +
           "LOWER(p.description) LIKE LOWER(CONCAT('%', :keyword, '%')) OR " +
           "LOWER(p.brand) LIKE LOWER(CONCAT('%', :keyword, '%')) OR " +
           "LOWER(p.category) LIKE LOWER(CONCAT('%', :keyword, '%'))")
    List<Product> searchProducts(@Param("keyword") String keyword);
}
```

#### **5. Handling Large Object (LOB) Issue in PostgreSQL**
- PostgreSQL may throw an error due to handling large objects (e.g., images stored in `BYTEA` format).
- Fix: Set **auto-commit to false** in `application.properties`:

```
spring.datasource.hikari.auto-commit=false
```

#### **6. Optimizing Search Requests**
- Currently, search runs on every keystroke, increasing the load on the server.
- Possible optimizations:
  - Trigger search after **3-5 characters**.
  - Add a **"Search" button** instead of instant search.
  - Implement **debouncing** in the frontend.

---

This completes the search feature implementation!
