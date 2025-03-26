Here’s a structured summary of the transcript:

---

### **Introduction to Spring Framework (Without Spring Boot)**  
- So far, we’ve used **Spring Boot**, which simplifies the process with auto-configurations.  
- Now, we are shifting to **Spring Framework (without Spring Boot)** to understand **what happens behind the scenes**.  
- Instead of using annotations like `@Component` and `@Autowired` (which we used in Spring Boot), we will manually configure the beans.

---

### **Creating a New Spring Project (Without Spring Boot)**  
1. **Project Setup:**
   - Using **IntelliJ IDEA** (Eclipse can also be used).  
   - **Project Type:** **Maven** (not Spring Boot).  
   - **Project Name:** `Spring1`.  
   - **JDK Version:** At least **JDK 17+** (Spring 6 requires it).  
   - **Maven Archetype:** **Quick Start** (Basic structure with dependencies).  
   - **Group ID:** `com.telusko`  
   - **Artifact ID:** `Spring1`  
   - **Version:** `1.0`  

2. **Project Structure:**
   - **Generated Files:**  
     - `pom.xml` (Dependency management).  
     - `src/main/java/App.java` (Default main class, prints `"Hello World"`).  

---

### **Creating and Managing a Simple Object (`Alien` Class)**
1. **Modify `App.java` to create an Alien object manually:**  
   ```java
   Alien obj = new Alien();
   obj.code();
   ```
2. **Create `Alien` class in the same package:**
   ```java
   public class Alien {
       public void code() {
           System.out.println("Coding");
       }
   }
   ```
3. **Run the code and verify output:** `"Coding"`  

---

### **Introducing Spring Container (IoC Container)**
- **Spring’s job is to manage objects (beans) in a container (IoC Container).**  
- To do this, we need **ApplicationContext**.

#### **Types of Spring Containers:**
1. **BeanFactory** (Older, limited features).  
2. **ApplicationContext** (Recommended, extends `BeanFactory` with more features).  

---

### **Using `ApplicationContext` to Manage Objects**
1. **We need to add Spring dependencies (Spring Context) in `pom.xml`:**  
   - Go to **Maven Repository** and search for **Spring Context**.  
   - Add the latest stable **Spring Context dependency** in `pom.xml`:  
     ```xml
     <dependency>
         <groupId>org.springframework</groupId>
         <artifactId>spring-context</artifactId>
         <version>6.1.1</version>
     </dependency>
     ```
   - Save and refresh Maven dependencies.

2. **Use `ApplicationContext` instead of manually creating an object:**
   ```java
   ApplicationContext context = new ClassPathXmlApplicationContext("spring.xml");
   Alien obj = (Alien) context.getBean("alien");
   obj.code();
   ```
   - **`ApplicationContext`** creates a container.  
   - **`getBean("alien")`** fetches the object from the container.  

---

### **Error: `BeanFactory not initialized`**
- **Why does the error occur?**
  - `ClassPathXmlApplicationContext("spring.xml")` requires an XML configuration (`spring.xml`), which is missing.  
  - **Solution:** We need to **create and configure `spring.xml`**.  
  - This will be covered in the next lesson.

---

B)
### **Spring XML-Based Configuration for Bean Management**

In this video, we encountered an error:  
**"BeanFactory not initialized or already closed."**  
After analysis, we found that the issue was not with obtaining the Spring container but with retrieving the bean. The Spring container couldn't find the `Alien` bean.

#### **Solution: Configuring Beans in XML**
To resolve this, we need to inform Spring that it should manage the `Alien` object. We do this using an XML configuration file.

---

### **Steps to Configure Beans in XML**
#### **1. Create `spring.xml` in the `resources` folder**
- Since we are using `ClassPathXmlApplicationContext`, the XML file should be in the classpath.
- In a Maven/Gradle project, create a `resources` folder under `src/main`.

#### **2. Define the Bean in `spring.xml`**
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans 
                           http://www.springframework.org/schema/beans/spring-beans.xsd">
  
    <bean id="alien" class="com.telusko.Alien" />

</beans>
```
- The **bean tag** is used to define a bean.
- `id="alien"` → This is the identifier used in `getBean("alien")`.
- `class="com.telusko.Alien"` → Fully qualified class name.

#### **3. Load the Spring Container in Java (`App.java`)**
```java
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class App {
    public static void main(String[] args) {
        ApplicationContext context = new ClassPathXmlApplicationContext("spring.xml");

        Alien obj = (Alien) context.getBean("alien");
        obj.code();
    }
}
```
- `ClassPathXmlApplicationContext("spring.xml")` → Loads the Spring container.
- `context.getBean("alien")` → Retrieves the managed bean.

---

### **Understanding the Fix**
Initially, we got an error because Spring didn't recognize the `Alien` bean. By defining it in `spring.xml` and including proper XML definitions, Spring successfully managed the object.

---

### **Adding More Beans**
If you need another class, e.g., `Laptop`, define it similarly:
```xml
<bean id="laptop" class="com.telusko.Laptop" />
```
Then, fetch it using:
```java
Laptop myLaptop = (Laptop) context.getBean("laptop");
```

---

### **Key Takeaways**
- Spring manages objects using a **container**.
- Objects managed by Spring are called **beans**.
- XML configuration must be placed in the `resources` folder.
- **Proper XML schema declaration** is necessary for Spring to recognize bean tags.
- Spring creates objects based on the XML configuration, which can then be retrieved via `getBean()`.

This marks the completion of our **first Spring project using XML-based configuration**.

C)
### **Understanding When Spring Creates an Object in the Container**

#### **Key Observation:**
Spring creates the object (bean) at the moment the container is initialized, not when `getBean()` is called.

#### **Experiment to Identify Object Creation Timing**

1. **Created a constructor in `Alien` class**
```java
public Alien() {
    System.out.println("Object Created");
}
```
2. **Commented out `getBean()` and method calls**
```java
ApplicationContext context = new ClassPathXmlApplicationContext("spring.xml");
// Alien obj = (Alien) context.getBean("alien");
// obj.code();
```
3. **Ran the code** → Output: `Object Created`

**Conclusion:** The object is created when `ApplicationContext` is initialized.

#### **How Many Objects Are Created?**
- If we define multiple beans in `spring.xml`, Spring will create objects for each.

##### **Example: One Class, One Bean**
**spring.xml:**
```xml
<bean id="alien" class="com.telusko.Alien" />
```
**Output:** `Object Created` (only once)

##### **Example: Two Different Classes**
**spring.xml:**
```xml
<bean id="alien" class="com.telusko.Alien" />
<bean id="laptop" class="com.telusko.Laptop" />
```
**Output:**
```
Object Created (Alien)
Object Created (Laptop)
```

##### **Example: Same Class, Multiple Beans**
**spring.xml:**
```xml
<bean id="alien1" class="com.telusko.Alien" />
<bean id="alien2" class="com.telusko.Alien" />
```
**Output:**
```
Object Created (Alien1)
Object Created (Alien2)
```

Spring creates multiple objects if multiple bean definitions exist for the same class.

#### **What If We Don’t Provide an `id`?**
Even without an `id`, Spring creates the object, but referencing it via `getBean()` becomes unclear.

#### **Does Calling `getBean()` Multiple Times Create New Objects?**
```java
Alien obj1 = (Alien) context.getBean("alien");
Alien obj2 = (Alien) context.getBean("alien");
```
- The answer will be explored in the next video.

#### **Summary**
1. Spring creates objects when `ApplicationContext` is initialized, not when `getBean()` is called.
2. Objects are created based on the number of `<bean>` entries in `spring.xml`.
3. If multiple `<bean>` definitions exist for the same class, multiple objects are created.
4. The impact of calling `getBean()` multiple times will be analyzed next.

D)
**Understanding Bean Scope in Spring Framework**

### **Recap from the Previous Discussion**
- We tested if calling `getBean` twice would create two different objects.
- The result showed that even though we called `getBean` twice, only **one object** was created.
- This proves that Spring maintains only **one instance** of the bean unless explicitly told otherwise.

### **Proving with Instance Variables**
- Created an instance variable `int age` inside `Alien`.
- Printed `age` for `obj1` and `obj2`, initially both displayed `0`.
- Set `obj1.age = 21`.
- Printed `age` again, and both `obj1` and `obj2` showed `21`.
- This confirms that both references point to the **same object**.

### **Why Does This Happen?**
- Spring beans follow different **scopes**.
- By default, Spring beans are **Singleton**, meaning:
  - Only **one instance** of the bean is created.
  - Every call to `getBean` returns the same instance.
  
### **Changing Bean Scope**
- The default scope is **Singleton**.
- To change this, we use the `scope` attribute inside the Spring XML configuration.
  
**Example:**
```xml
<bean id="alien1" class="com.example.Alien" scope="singleton" />
```
- This explicitly enforces the **default Singleton behavior**.

### **Prototype Scope**
- If you want a **new object** every time `getBean` is called, set the scope to **Prototype**.

**Example:**
```xml
<bean id="alien1" class="com.example.Alien" scope="prototype" />
```
- Now, each call to `getBean("alien1")` creates a **new** object.

**Proof:**
- When using **Singleton**, `Object Created` is printed **once**.
- When using **Prototype**, `Object Created` is printed **multiple times**, depending on the number of `getBean` calls.

### **Key Differences: Singleton vs. Prototype**
| Feature         | Singleton  | Prototype  |
|---------------|------------|-------------|
| **Instance Count** | One per container | New instance on every `getBean` call |
| **Object Creation** | Created at container initialization | Created when requested via `getBean` |
| **Memory Usage** | Less (single instance reused) | More (new object per request) |

### **Observation with Empty Code Execution**
- When `scope="singleton"`, the object is created **as soon as** the container loads.
- When `scope="prototype"`, the object is **not created** until `getBean` is explicitly called.

### **Conclusion**
- **Singleton scope** ensures a **single shared instance** across the application.
- **Prototype scope** ensures a **new object** is created **each time `getBean` is called**.
- Default behavior in Spring is **Singleton**.
- Other scopes like **Request, Session, and GlobalSession** are used in web applications (to be discussed later).

E)
**Setter Injection in Spring Framework**

### What is Injection?
Injection in Spring refers to the process of assigning values to object properties externally rather than hardcoding them inside the class. This follows the principle of Dependency Injection (DI), which helps in better manageability and loose coupling of objects.

### Implementing Setter Injection
We take an example of a class `Alien` that has a property `age`. Initially, the variable `age` is made private, and corresponding getter and setter methods are created.

#### Step 1: Define the Class
```java
public class Alien {
    private int age;
    
    public int getAge() {
        return age;
    }
    
    public void setAge(int age) {
        System.out.println("Setter called");
        this.age = age;
    }
}
```

#### Step 2: Configure Setter Injection in `spring.xml`
In `spring.xml`, we define the bean and set the property using the `<property>` tag.

```xml
<bean id="alien" class="com.example.Alien">
    <property name="age" value="21"/>
</bean>
```

- The `property` tag assigns a value to the `age` variable.
- The `name` attribute should match the variable name (`age` in this case).
- The `value` attribute specifies the value being assigned.

#### Step 3: Fetch Bean in Main Class
```java
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class Main {
    public static void main(String[] args) {
        ApplicationContext context = new ClassPathXmlApplicationContext("spring.xml");
        
        Alien obj = (Alien) context.getBean("alien");
        System.out.println("Age: " + obj.getAge());
    }
}
```

#### Step 4: Running the Code
- The output will be:
  ```
  Setter called
  Age: 21
  ```
- This confirms that the setter method is called while assigning the value.

### Key Points
1. **Spring creates the object first** and then calls the setter method.
2. **Values are injected externally** instead of setting them inside the code.
3. **Setter Injection is used for primitive data types** and can also be used for objects.
4. **Spring ensures loose coupling** by injecting dependencies via setter methods instead of hardcoding them inside the class.

This approach is useful when we need to **dynamically configure values** without modifying the Java code, making it more flexible and maintainable.

F)
Here are the clear and concise notes on **setter injection with reference attributes** in Spring:  

---

### **Setter Injection with Reference Attribute**  

**1. Introduction**  
- Previously, we injected **primitive values** using `<property name="age" value="21"/>`.  
- Now, we will see how to inject **objects (references)** instead of primitive values.  

**2. Problem with Direct Object Creation**  
- Consider a class `Laptop` with a `compile()` method:  
  ```java
  public class Laptop {
      public void compile() {
          System.out.println("Compiling");
      }
  }
  ```
- In the `Alien` class, we want to call `compile()` but need a `Laptop` object:  
  ```java
  public class Alien {
      private int age;
      private Laptop lap; // Reference variable

      public void code() {
          System.out.println("Coding");
          lap.compile();
      }

      // Getters and Setters
      public void setLap(Laptop lap) { this.lap = lap; }
  }
  ```
- If we **manually create** the object (`new Laptop()`), it **breaks dependency injection**.  
- Instead, we want **Spring to inject** the `Laptop` object.  

**3. Injecting a Reference in `spring.xml`**  
- Define the `Laptop` bean:  
  ```xml
  <bean id="lap1" class="com.example.Laptop"/>
  ```
- Inject `Laptop` into `Alien` using **ref**:  
  ```xml
  <bean id="alien" class="com.example.Alien">
      <property name="lap" ref="lap1"/>
  </bean>
  ```
  - **Note:**  
    - `<property name="lap" ref="lap1"/>` assigns the `Laptop` bean to `Alien`.  
    - The `ref` attribute links the `Laptop` object with `Alien`.  

**4. Multiple Object References**  
- If multiple `Laptop` beans exist, specify which one to use:  
  ```xml
  <bean id="lap2" class="com.example.Laptop"/>
  <bean id="alien2" class="com.example.Alien">
      <property name="lap" ref="lap2"/>
  </bean>
  ```
  - `alien` will use `lap1`, while `alien2` will use `lap2`.  

**5. Summary**  
| Injection Type | XML Property | Example |
|---------------|-------------|---------|
| **Primitive Value** | `value` | `<property name="age" value="21"/>` |
| **Reference (Object)** | `ref` | `<property name="lap" ref="lap1"/>` |

- **Setter Injection** uses `setLap(Laptop lap)` to assign the object.  
- `ref` ensures proper linking between beans.  
- Spring **automatically assigns the reference** during object creation.  

This completes setter injection with **reference attributes**. 

G)
### **Constructor Injection in Spring**

#### **Introduction**
Constructor Injection is a way to inject dependencies into a Spring Bean through its constructor. It is useful when dependencies are mandatory, as it ensures all required dependencies are available when the object is created.

---

### **Example: Constructor Injection**
#### **1. Defining the `Alien` Class**
```java
public class Alien {
    private int age;
    private Laptop laptop;

    public Alien(int age, Laptop laptop) {
        this.age = age;
        this.laptop = laptop;
        System.out.println("Parameterized Constructor Called");
    }
}
```

#### **2. Defining the `Laptop` Class**
```java
public class Laptop {
    public Laptop() {
        System.out.println("Laptop Object Created");
    }
}
```

#### **3. Configuring Beans in `beans.xml`**
```xml
<bean id="lap1" class="com.telusko.Laptop" />

<bean id="alien" class="com.telusko.Alien">
    <constructor-arg index="0" value="21"/>
    <constructor-arg index="1" ref="lap1"/>
</bean>
```
- `constructor-arg`: Specifies arguments for the constructor.
- `index`: Defines the position of arguments.

---

### **Key Observations**
1. **Setter Injection vs. Constructor Injection**
   - **Setter Injection**: Used when values can be modified after object creation.
   - **Constructor Injection**: Used when values are mandatory at object creation.

2. **Ways to Resolve Parameter Ambiguity in Constructor Injection**
   - **Using Type**
     ```xml
     <constructor-arg type="int" value="21"/>
     <constructor-arg type="com.telusko.Laptop" ref="lap1"/>
     ```
   - **Using Index**
     ```xml
     <constructor-arg index="0" value="21"/>
     <constructor-arg index="1" ref="lap1"/>
     ```
   - **Using Name**
     ```xml
     <constructor-arg name="age" value="21"/>
     <constructor-arg name="laptop" ref="lap1"/>
     ```
   - **Using `@ConstructorProperties` Annotation**
     ```java
     import java.beans.ConstructorProperties;

     public class Alien {
         private int age;
         private Laptop laptop;

         @ConstructorProperties({"age", "laptop"})
         public Alien(int age, Laptop laptop) {
             this.age = age;
             this.laptop = laptop;
         }
     }
     ```

---

### **Best Practice**
- **Use Constructor Injection** when dependencies are mandatory.
- **Use Setter Injection** when dependencies are optional.
- **Index-based constructor injection** is generally preferred for its simplicity.

This covers **Constructor Injection in Spring** and when to prefer it over Setter Injection.

H)
### **Notes on Implementing Interfaces in Java (Alien, Computer, Laptop, Desktop)**

#### **Why Introduce an Interface?**
- Initially, the `Alien` class is dependent on a `Laptop` object.
- However, a `Laptop` is not the only device that can be used for coding; a `Desktop` can also be used.
- Instead of tightly coupling `Alien` with `Laptop`, we introduce a general `Computer` interface.
- This ensures flexibility—any future device (like smart glasses) can be used without modifying the `Alien` class.

#### **Steps to Implement the Interface**
1. **Create an Interface `Computer`**
   ```java
   public interface Computer {
       void compile();
   }
   ```
   
2. **Implement `Computer` Interface in `Laptop`**
   ```java
   public class Laptop implements Computer {
       @Override
       public void compile() {
           System.out.println("Compiling using Laptop");
       }
   }
   ```

3. **Create Another Class `Desktop` Implementing `Computer`**
   ```java
   public class Desktop implements Computer {
       @Override
       public void compile() {
           System.out.println("Compiling using Desktop");
       }
   }
   ```

4. **Modify the `Alien` Class to Use `Computer` Instead of `Laptop`**
   ```java
   public class Alien {
       private Computer com;

       public void setComputer(Computer com) {
           this.com = com;
       }

       public void code() {
           com.compile();
       }
   }
   ```

5. **Testing the Code**
   ```java
   public class Main {
       public static void main(String[] args) {
           Alien alien = new Alien();
           
           // Using a Laptop
           Computer laptop = new Laptop();
           alien.setComputer(laptop);
           alien.code();  // Output: Compiling using Laptop

           // Using a Desktop
           Computer desktop = new Desktop();
           alien.setComputer(desktop);
           alien.code();  // Output: Compiling using Desktop
       }
   }
   ```

#### **Key Takeaways**
- **Dependency Inversion Principle (DIP):** The `Alien` class depends on the abstract `Computer` interface instead of concrete classes (`Laptop`, `Desktop`).
- **Flexibility & Scalability:** Future implementations (e.g., a new `Tablet` class) can be added without modifying `Alien`.
- **Polymorphism:** The `code()` method in `Alien` can use different `Computer` objects (`Laptop`, `Desktop`).
- **Next Step:** In the next topic, we'll explore **autowiring** in Spring to inject dependencies automatically.

I)
### **Notes on Autowiring in Spring (byName & byType)**

#### **Problem Before Autowiring**
- The `Alien` class was dependent on `Laptop`, but we introduced the `Computer` interface.
- We created `Laptop` and `Desktop` classes implementing `Computer`.
- When we tried to run the code, we got an error: **Cannot resolve matching constructor**.
- The issue was that Spring couldn't automatically match the bean properties with the required dependencies.

---

### **Solution: Using Autowiring**

#### **1️⃣ Autowiring by Name (`@Autowired byName`)**
- If the variable name in the `Alien` class (`com`) matches a bean name (`com`), Spring can automatically wire it.

##### **Example Setup**
```xml
<bean id="com" class="Laptop"/>
```
```java
public class Alien {
    @Autowired
    @Qualifier("com")  // Matches with the bean id="com"
    private Computer com;

    public void code() {
        com.compile();
    }
}
```
- **If a bean with the exact name is found, it gets wired automatically.**
- **If names don’t match, Spring will fail to autowire and throw an error.**

---

#### **2️⃣ Autowiring by Type (`@Autowired byType`)**
- Instead of searching for a bean by name, Spring looks for a bean with a matching **type**.

##### **Example Setup**
```java
public class Alien {
    @Autowired  // No need for Qualifier
    private Computer com;

    public void code() {
        com.compile();
    }
}
```
- Spring searches for a **bean of type `Computer`**, not by name.
- **If only one bean of `Computer` type exists, it gets wired.**
- **If multiple beans exist (both `Laptop` and `Desktop`), Spring gets confused and throws an error.**

---

### **Handling Multiple Implementations (Laptop & Desktop Conflict)**
- When both `Laptop` and `Desktop` exist, Spring doesn’t know which one to choose.
- **Solution: Use `@Qualifier` to specify the exact bean.**

```java
public class Alien {
    @Autowired
    @Qualifier("laptopBean")  // Specify exact bean name
    private Computer com;

    public void code() {
        com.compile();
    }
}
```
- Now Spring will inject only the `Laptop` bean.

---

### **Key Takeaways**
| Approach | How It Works | Pros | Cons |
|----------|-------------|------|------|
| `@Autowired byName` | Matches bean name with variable name | Works well when names are consistent | Breaks if names don’t match |
| `@Autowired byType` | Matches bean type, ignoring names | Cleaner and more flexible | Fails if multiple beans of the same type exist |
| `@Qualifier("beanName")` | Specifies exactly which bean to use | Resolves conflicts when multiple beans exist | Needs manual configuration |

- **Best Practice:** Use `@Autowired` with `@Qualifier` to avoid confusion when multiple implementations exist.
- **Next Step:** We will explore another way to resolve conflicts in the next video.

I)
### **Solving the Multiple Bean Conflict Using `@Primary` in Spring**

#### **Problem Recap**
- When using `@Autowired` with `byType`, Spring gets confused if multiple beans exist for the same type.
- Example: We have both `Laptop` and `Desktop` beans implementing `Computer`, so Spring doesn't know which one to pick.

#### **Solution: Using `@Primary`**
- We can **mark one bean as the default** by using `@Primary`.
- Spring will choose the **primary bean** if there is ambiguity.

---

### **1️⃣ Using `@Primary` to Resolve Conflict**
```java
@Component
@Primary  // Marks this as the default bean for Computer
public class Laptop implements Computer {
    public void compile() {
        System.out.println("Compiling using Laptop");
    }
}

@Component
public class Desktop implements Computer {
    public void compile() {
        System.out.println("Compiling using Desktop");
    }
}
```
```java
@Component
public class Alien {
    @Autowired
    private Computer com;

    public void code() {
        com.compile();
    }
}
```
✅ **When we run this, Spring automatically picks `Laptop` because of `@Primary`.**  
✅ **If no `@Primary` is set, Spring throws an error due to ambiguity.**

---

### **2️⃣ Overriding `@Primary` with Explicit Bean Selection (`@Qualifier`)**
Even though `Laptop` is marked as `@Primary`, we can still override it using `@Qualifier`:

```java
@Component
public class Alien {
    @Autowired
    @Qualifier("desktop")  // Explicitly choosing Desktop
    private Computer com;

    public void code() {
        com.compile();
    }
}
```
✅ Now, even though `Laptop` is `@Primary`, `Desktop` will be used because `@Qualifier("desktop")` is specified.

---

### **Key Takeaways**
| Approach | Behavior | When to Use |
|----------|----------|--------------|
| `@Primary` | Sets a default bean if multiple exist | When one bean should be preferred in case of conflict |
| `@Qualifier("beanName")` | Overrides `@Primary` and selects a specific bean | When you need explicit selection |

- **`@Primary` is used for default selection** when multiple beans exist.
- **`@Qualifier` takes priority over `@Primary`** when explicitly used.
- **If neither is used, Spring throws an error** when multiple beans exist for the same type.

---

### **Final Code for Reference**
```java
@Component
@Primary
public class Laptop implements Computer {
    public void compile() {
        System.out.println("Compiling using Laptop");
    }
}

@Component
public class Desktop implements Computer {
    public void compile() {
        System.out.println("Compiling using Desktop");
    }
}

@Component
public class Alien {
    @Autowired
    @Qualifier("desktop")  // Overrides @Primary selection
    private Computer com;

    public void code() {
        com.compile();
    }
}
```
✅ **With `@Primary`, Spring chooses `Laptop` by default.**  
✅ **With `@Qualifier`, we can explicitly override it.**

J)
### **Lazy Initialization of Beans in Spring**

#### **1️⃣ What is Lazy Initialization?**
By default, Spring **creates all singleton beans** when the container starts (eager initialization).  
However, with **lazy initialization**, a bean is **not created until it is first requested**.

---

#### **2️⃣ Problem with Eager Initialization**
- By default, Spring loads **all singleton beans at startup**, even if they are not needed immediately.
- This can **slow down application startup**, especially when there are **many beans**.
- Example:
  ```java
  @Component
  public class Desktop {
      public Desktop() {
          System.out.println("Desktop Object Created");
      }
  }
  ```
  - Even if we **don’t use** `Desktop`, its constructor runs at startup.

---

#### **3️⃣ Solution: `@Lazy` Annotation**
To make a bean lazy, we add `@Lazy`:
```java
@Component
@Lazy  // This bean will be created only when first used
public class Desktop {
    public Desktop() {
        System.out.println("Desktop Object Created");
    }
}
```
✅ Now, `Desktop` **is not created at startup**.  
✅ It is created **only when first accessed**.

---

#### **4️⃣ Example Code with Lazy and Eager Beans**
```java
@Component
public class Laptop {
    public Laptop() {
        System.out.println("Laptop Object Created");
    }
}

@Component
@Lazy
public class Desktop {
    public Desktop() {
        System.out.println("Desktop Object Created");
    }
}

@Component
public class Alien {
    @Autowired
    private Laptop laptop; // Eager bean (default)

    @Autowired
    private Desktop desktop; // Lazy bean

    public void code() {
        System.out.println("Alien is coding...");
        desktop.toString(); // Accessing the lazy bean
    }
}
```
#### **5️⃣ Expected Output**
If we **don’t call `desktop`**, the output is:
```
Laptop Object Created
```
✅ `Desktop` is **not created** because it's lazy.

If we **call `desktop`**, the output is:
```
Laptop Object Created
Desktop Object Created
Alien is coding...
```
✅ `Desktop` is created **only when accessed**.

---

#### **6️⃣ Lazy Beans in `spring.xml` Configuration**
In XML-based configuration, we use:
```xml
<bean id="desktop" class="com.example.Desktop" lazy-init="true"/>
```
✅ This achieves the same behavior as `@Lazy`.

---

#### **7️⃣ Important Notes**
| Feature | Lazy Initialization (`@Lazy`) | Eager Initialization (Default) |
|---------|-------------------|-------------------|
| Bean Creation | Delayed until first use | Created at startup |
| Performance | Faster startup | Slower startup |
| When to Use | When a bean **isn’t always needed** | When a bean **is always needed** |

- **Lazy beans remain singleton** (unless `@Scope("prototype")` is used).
- If a **non-lazy bean depends on a lazy bean**, the lazy bean is **still created at startup**.

---

### **8️⃣ When to Use Lazy Initialization?**
✅ Use `@Lazy` when:
- The bean is **not always required**.
- It **consumes resources** (e.g., database connections, heavy computations).
- You **want a faster application startup**.

✅ Keep default (eager) when:
- The bean is **always used immediately**.
- It **initializes critical components**.

---

### **9️⃣ Final Thoughts**
Lazy initialization **reduces startup time** and **optimizes memory usage** by **only creating beans when needed**.  
However, it **should be used wisely**, as delaying object creation can sometimes cause unexpected delays later.

K)
### **Inner Beans in Spring**

#### **1️⃣ What is an Inner Bean?**
- A **bean declared inside another bean** is called an **inner bean**.
- It is **only accessible by the outer bean**.
- It **cannot be used anywhere else** in the application.

---

#### **2️⃣ Problem with Normal Beans**
By default, when we define a bean in `spring.xml`, it is **available for the entire application**.  

Example:
```xml
<bean id="laptop" class="com.example.Laptop"/>
<bean id="alien" class="com.example.Alien">
    <property name="com" ref="laptop"/>
</bean>
```
✅ `Alien` gets `Laptop` via `ref="laptop"`, but  
❌ `Laptop` is accessible globally, which **may not be needed**.

---

#### **3️⃣ Solution: Using Inner Beans**
If we want `Laptop` to be **used only by Alien**, we define it as an **inner bean**:
```xml
<bean id="alien" class="com.example.Alien">
    <property name="com">
        <bean class="com.example.Laptop"/>
    </property>
</bean>
```
✅ `Laptop` is now **embedded** inside `Alien`.  
✅ `Laptop` **cannot be used** by any other bean.

---

#### **4️⃣ Java Code for Inner Beans**
**Alien.java**
```java
public class Alien {
    private Computer com;

    public void setCom(Computer com) {
        this.com = com;
    }

    public void code() {
        System.out.println("Coding...");
        com.compile();
    }
}
```
**Laptop.java**
```java
public class Laptop implements Computer {
    public void compile() {
        System.out.println("Code compiled in Laptop.");
    }
}
```
---

#### **5️⃣ Key Points about Inner Beans**
| Feature | Normal Bean (`ref="id"`) | Inner Bean |
|---------|--------------------|------------|
| Scope | Global (usable by all beans) | Local (used only by the outer bean) |
| Reusability | Can be reused by multiple beans | Cannot be reused |
| Definition | Defined separately | Defined inside another bean |

---

#### **6️⃣ When to Use Inner Beans?**
✅ When a bean **is only needed by one other bean**.  
✅ When you **don’t want other beans to access it**.  
❌ **Don’t use it** if the bean should be **shared across multiple components**.

