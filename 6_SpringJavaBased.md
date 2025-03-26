A)
### **Java-Based Configuration in Spring**

#### **1️⃣ XML vs Java-Based Configuration**
- **XML Configuration:** Beans are defined in `spring.xml`.
- **Java-Based Configuration:** Beans are defined using Java classes and annotations.

---

#### **2️⃣ Setting Up Java-Based Configuration**
Instead of defining beans in XML, we use a Java class.

**📌 Steps to Convert to Java-Based Configuration:**
1. Create a configuration class (`AppConfig`).
2. Use `@Configuration` to mark it as a Spring configuration class.
3. Use `@Bean` to define beans.
4. Use `AnnotationConfigApplicationContext` instead of `ClassPathXmlApplicationContext`.

---

#### **3️⃣ Java-Based Configuration Example**
##### **Step 1: Create the `AppConfig` Class**
```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration  // Marks this class as a Spring configuration
public class AppConfig {

    @Bean  // Defines a bean similar to <bean id="desktop" class="Desktop"/>
    public Desktop desktop() {
        return new Desktop();
    }
}
```
---

##### **Step 2: Modify `App.java` to Use Java Config**
```java
import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;

public class App {
    public static void main(String[] args) {
        // Using Java-based configuration instead of XML
        ApplicationContext context = new AnnotationConfigApplicationContext(AppConfig.class);

        // Retrieving the Desktop bean
        Desktop dt = context.getBean(Desktop.class);
        dt.compile(); // Calling method on bean
    }
}
```
---

##### **Step 3: `Desktop` Class**
```java
public class Desktop {
    public void compile() {
        System.out.println("Compiling using Desktop.");
    }
}
```
---

#### **4️⃣ Key Differences: XML vs Java Config**
| Feature | XML-Based Config | Java-Based Config |
|---------|-----------------|------------------|
| Bean Definition | `<bean>` tag in `spring.xml` | `@Bean` method in a `@Configuration` class |
| Configuration File | `spring.xml` | Java class (`AppConfig.java`) |
| Container Type | `ClassPathXmlApplicationContext("spring.xml")` | `AnnotationConfigApplicationContext(AppConfig.class)` |
| Readability | Harder to read (XML syntax) | More readable (Java syntax) |

---

#### **5️⃣ Key Takeaways**
✅ **Java-based configuration** removes the need for XML.  
✅ `@Configuration` marks a class as a Spring configuration.  
✅ `@Bean` is used instead of `<bean>` tags.  
✅ `AnnotationConfigApplicationContext` is used instead of `ClassPathXmlApplicationContext`.  

---

#### **6️⃣ Next Step: Naming Beans**
By default, Spring generates bean names based on method names. In the next section, we'll see **how to set custom bean names**.

B)
### **Bean Names in Java-Based Spring Configuration**

---

### **1️⃣ What is a Bean Name?**
- By default, **Spring assigns the method name** as the bean name.
- If a method is named `desktop()`, then the bean name is **"desktop"**.

---

### **2️⃣ Default Bean Naming**
```java
@Configuration
public class AppConfig {

    @Bean
    public Desktop desktop() {  // Default bean name = "desktop"
        return new Desktop();
    }
}
```
💡 **Retrieving the Bean by Name:**
```java
Desktop dt = (Desktop) context.getBean("desktop");
```
---

### **3️⃣ Customizing the Bean Name**
- Use the `name` attribute in `@Bean` to **override** the default name.

```java
@Bean(name = "com2")
public Desktop desktop() {
    return new Desktop();
}
```
💡 **Retrieving the Bean by Custom Name:**
```java
Desktop dt = (Desktop) context.getBean("com2");
```
---

### **4️⃣ Defining Multiple Names for a Bean**
- You can assign multiple names to a bean.

```java
@Bean(name = {"com2", "desktop1", "Beast"})
public Desktop desktop() {
    return new Desktop();
}
```
💡 **Retrieving the Bean by Any Assigned Name:**
```java
Desktop dt1 = (Desktop) context.getBean("com2");
Desktop dt2 = (Desktop) context.getBean("desktop1");
Desktop dt3 = (Desktop) context.getBean("Beast");
```
---

### **5️⃣ Switching Back to Default Naming**
- If you **don't** specify a name, the method name is used.

```java
@Bean
public Desktop desktop() {  // Bean name = "desktop"
    return new Desktop();
}
```
---

### **6️⃣ Next Topic: Scope & Primary Beans**
- By default, all beans in Spring are **singleton**.
- If we want **different objects for different requests**, we use the **prototype** scope.
- How do we make a bean **primary** when multiple beans exist?  

➡ **Coming Up Next:** Prototype Scope & `@Primary` Annotation.

C)
### **Bean Scope in Java-Based Spring Configuration**

---

### **1️⃣ Default Scope: Singleton**
- By default, **Spring beans are singleton**.
- Only **one instance** is created and reused.

```java
@Configuration
public class AppConfig {

    @Bean
    public Desktop desktop() {  // Default scope: Singleton
        System.out.println("Desktop object created");
        return new Desktop();
    }
}
```
💡 **Retrieving the Bean Multiple Times:**
```java
Desktop dt1 = context.getBean(Desktop.class);
Desktop dt2 = context.getBean(Desktop.class);
```
- Output:
  ```
  Desktop object created
  Compiling using Desktop
  Compiling using Desktop
  ```
- Only **one object is created**, even though we requested it twice.

---

### **2️⃣ Changing Scope to Prototype**
- If we want **a new object every time**, we use the `@Scope` annotation.

```java
import org.springframework.context.annotation.Scope;
import org.springframework.beans.factory.config.ConfigurableBeanFactory;

@Bean
@Scope(ConfigurableBeanFactory.SCOPE_PROTOTYPE)  // Setting scope to prototype
public Desktop desktop() {
    System.out.println("Desktop object created");
    return new Desktop();
}
```
💡 **Retrieving the Bean Multiple Times:**
```java
Desktop dt1 = context.getBean(Desktop.class);
Desktop dt2 = context.getBean(Desktop.class);
```
- Output:
  ```
  Desktop object created
  Desktop object created
  Compiling using Desktop
  Compiling using Desktop
  ```
- **Two different objects** are created.

---

### **3️⃣ Summary: Singleton vs Prototype**
| Scope       | Behavior |
|------------|----------|
| **Singleton** | One instance per Spring container (default) |
| **Prototype** | A new instance every time `getBean()` is called |

---

### **4️⃣ What's Next?**
- We have seen **singleton** and **prototype**.
- What if **multiple beans of the same type exist**?  
- How do we **prioritize** one over another?

➡ **Next Topic:** `@Primary` Annotation & Qualifiers.

D)
### **Injecting Dependencies in Java-Based Spring Configuration**

---

### **1️⃣ Creating a Bean for Alien**
- Previously, we created a `Desktop` bean.
- Now, let's create an `Alien` bean.

```java
@Bean
public Alien alien() {
    Alien obj = new Alien();
    obj.setAge(25);  // Setting age manually
    obj.setCom(desktop());  // Injecting Desktop (Computer)
    return obj;
}
```
✅ **Now, `Alien` has an age and a `Computer` (Desktop) object.**

---

### **2️⃣ The Problem: Tightly Coupled Dependencies**
- The code **directly assigns** `Desktop` as the `Computer` implementation.
- This creates **tight coupling**.

💡 **Issue:**  
```java
obj.setCom(desktop());  // Hardcoding Desktop object
```
➡️ What if we want to use **Laptop** instead of **Desktop**?

---

### **3️⃣ The Solution: Constructor Injection**
- Instead of hardcoding `Desktop`, **pass** `Computer` as a method parameter.

```java
@Bean
public Alien alien(Computer com) {  // Injecting Computer dependency
    Alien obj = new Alien();
    obj.setAge(25);
    obj.setCom(com);  // Injecting Computer dynamically
    return obj;
}
```
✅ **Spring automatically injects an available `Computer` bean.**

---

### **4️⃣ How Does This Work?**
- Spring sees that `Alien` **needs a `Computer` object**.
- It **checks for beans implementing `Computer`**.
- If only one is found (**e.g., Desktop**), it **injects it automatically**.

---

### **5️⃣ What Happens If Multiple Beans Exist?**
- Suppose we have **both `Desktop` and `Laptop`**.

```java
@Bean
public Computer desktop() {
    return new Desktop();
}

@Bean
public Computer laptop() {
    return new Laptop();
}
```
➡ **Spring gets confused**:  
```
NoUniqueBeanDefinitionException: expected single matching bean but found 2
```
- **How to fix this?**
  - Use `@Primary` to give **one bean higher priority**.
  - Use `@Qualifier` to specify **which bean to inject**.

---

### **6️⃣ Next Topic: Handling Multiple Beans**
➡ **How does Spring decide which bean to use when multiple beans exist?**  
➡ **We will explore `@Primary` and `@Qualifier` next.**

E)
### **Handling Multiple Beans in Spring Java Configuration**  

---

### **1️⃣ The Problem: Multiple Beans of the Same Type**  
- So far, we had only **one `Computer` bean** (`Desktop`).
- Now, we added a **second `Computer` bean** (`Laptop`).

```java
@Bean
public Laptop laptop() {
    return new Laptop();
}

@Bean
public Desktop desktop() {
    return new Desktop();
}
```
💡 **Problem:**  
- **Spring gets confused** when injecting a `Computer` bean into `Alien`.  
- It **doesn’t know whether to use `Desktop` or `Laptop`**.  

🛑 **Error Message:**  
```
NoUniqueBeanDefinitionException: expected single matching bean but found 2: desktop, laptop
```

---

### **2️⃣ Solution 1: Using `@Qualifier` (Manual Selection)**  
- We explicitly specify **which bean to inject** using `@Qualifier`.  

```java
@Bean
public Alien alien(@Qualifier("desktop") Computer com) {  // Explicitly choosing "desktop"
    Alien obj = new Alien();
    obj.setAge(25);
    obj.setCom(com);
    return obj;
}
```
✅ **Now, Spring always injects `Desktop`.**  
🚨 If the bean name is incorrect (`@Qualifier("desktop1")`), Spring throws an error.  

---

### **3️⃣ Solution 2: Using `@Primary` (Default Selection)**  
- Instead of specifying a `@Qualifier` every time,  
  we can mark **one bean as the default choice** using `@Primary`.  

```java
@Bean
@Primary  // Marks Laptop as the default Computer bean
public Laptop laptop() {
    return new Laptop();
}
```
✅ **Now, Spring automatically picks `Laptop`** whenever a `Computer` bean is needed.  

💡 **If `@Qualifier` is present, it takes priority over `@Primary`.**  

---

### **4️⃣ When to Use What?**
| Approach      | Use Case |
|--------------|---------|
| `@Qualifier` | When you want **explicit control** over which bean is injected. |
| `@Primary`   | When you want to set a **default bean**, but allow overrides with `@Qualifier`. |

---

### **5️⃣ Next Topic: `@Qualifier` in Spring Boot**
- In **Spring Boot**, we use `@Qualifier` differently in constructors.  
- We’ll explore **how to use it efficiently in Boot applications.**

F)
### **Simplifying Bean Management with `@Component` and `@ComponentScan`**  

---

### **1️⃣ Problem: Manual Configuration in XML/Java**  
- We used **XML configuration** and **Java-based configuration** with `@Bean` methods.
- But we still **had to manually define beans** in configuration files.  

💡 **We want Spring to manage beans automatically** instead of manually defining them.  

---

### **2️⃣ Solution: Using `@Component` (Stereotype Annotation)**  
- The `@Component` annotation tells Spring:  
  👉 “This class should be managed as a bean.”  
- Apply `@Component` on the classes where Spring should **automatically create objects**.  

```java
@Component  // Marks Alien as a Spring-managed bean
public class Alien {
    public Alien() {
        System.out.println("Alien object created");
    }
}
```

```java
@Component  // Marks Desktop as a Spring-managed bean
public class Desktop implements Computer {
    public Desktop() {
        System.out.println("Desktop object created");
    }
}
```

```java
@Component  // Marks Laptop as a Spring-managed bean
public class Laptop implements Computer {
    public Laptop() {
        System.out.println("Laptop object created");
    }
}
```

✅ **Now, Spring is responsible for creating objects!**  
❌ **But this alone is not enough.**  

---

### **3️⃣ The Problem: Spring Doesn’t Detect `@Component` Automatically**  
Even after adding `@Component`, Spring **still throws an error**:  
```
No qualifying bean of type Alien available
```
💡 **Why?**  
- Spring **doesn’t know where to search** for `@Component` classes.  

---

### **4️⃣ Solution: `@ComponentScan` (Tells Spring Where to Look)**  
- Add `@ComponentScan` in the configuration class to **scan specific packages** for `@Component` classes.  

```java
@Configuration
@ComponentScan(basePackages = "com.telusko")  // Scans for @Component classes
public class AppConfig {
}
```
✅ Now, Spring **automatically detects and manages** beans without needing `@Bean` methods.  

---

### **5️⃣ Verifying the Setup**  
**Run the application:**  
```java
public class App {
    public static void main(String[] args) {
        AnnotationConfigApplicationContext context = 
            new AnnotationConfigApplicationContext(AppConfig.class);

        Alien obj = context.getBean(Alien.class);
        System.out.println(obj);

        context.close();
    }
}
```
💡 **Expected Output:**  
```
Alien object created
Desktop object created
Laptop object created
```
🎉 **Spring is now handling bean creation automatically!**  

---

### **6️⃣ The Next Challenge: Connecting Beans (`@Autowired` Needed!)**  
- Our objects are created, but **they are not connected**.
- Example: `Alien` needs a `Computer`, but **it doesn’t know which one to use**.
- **Solution:** Use `@Autowired` to wire dependencies automatically.  

👉 **We'll cover `@Autowired` in the next lesson!**

G)
### **🔗 Connecting Beans Using `@Autowired` in Spring**  

---

### **1️⃣ Problem: Default Values & Null References**  
- Spring **creates objects**, but properties like `age` and `comp` are **not initialized**.  
- `age` is `0`, and `comp` is `null`.  
- We need to tell Spring to **automatically assign dependencies** instead of manually setting them.  

---

### **2️⃣ Solution: `@Autowired` for Dependency Injection**  
The `@Autowired` annotation tells Spring:  
👉 “Find a matching bean and inject it here.”  

#### ✅ **Field Injection (Directly Injecting into Fields)**
```java
@Component
public class Alien {
    private int age = 20;

    @Autowired  // Spring will inject an available Computer bean
    private Computer comp;

    public void code() {
        comp.compile();
    }
}
```
**Problem Solved:**  
- Spring **automatically finds a `Computer` bean** and assigns it.  

---

### **3️⃣ Handling Multiple Implementations (`@Qualifier`)**  
🚨 **Issue:** What if we have **two beans** (`Laptop` and `Desktop`) implementing `Computer`?  
```
No qualifying bean of type 'Computer' available: expected single matching bean but found 2
```
👉 **Solution:** Use `@Qualifier` to specify **which bean** to inject.  

```java
@Component
public class Alien {
    @Autowired
    @Qualifier("laptop")  // Specify the bean name (class name with the first letter lowercase)
    private Computer comp;
}
```
### **How to Name Beans in `@Component`?**
- By **default**, Spring assigns the **class name in lowercase**:
  ```java
  @Component  // Bean name will be "laptop"
  public class Laptop implements Computer { }
  
  @Component  // Bean name will be "desktop"
  public class Desktop implements Computer { }
  ```
- ✅ Now, `@Qualifier("laptop")` ensures that **Laptop is injected** instead of Desktop.  

---

### **4️⃣ Custom Bean Names in `@Component`**
If you want a **custom name**, specify it inside `@Component`:  
```java
@Component("com2")  // Now the bean name is "com2"
public class Laptop implements Computer { }
```
Then refer to it in `@Qualifier`:  
```java
@Autowired
@Qualifier("com2")  // Inject "com2" bean
private Computer comp;
```

---

### **5️⃣ Alternative: `@Primary` (Preferred Bean Selection)**
Instead of `@Qualifier`, you can **mark one bean as the default choice** using `@Primary`:  
```java
@Component
@Primary  // This bean is selected when multiple beans exist
public class Laptop implements Computer { }
```
Now Spring **automatically picks** `Laptop` without needing `@Qualifier`.  

---

### **6️⃣ Three Ways to Inject Dependencies**
1️⃣ **Field Injection (Not Recommended)**  
```java
@Autowired
private Computer comp;
```
2️⃣ **Constructor Injection (Best Practice)**
```java
@Autowired
public Alien(Computer comp) {
    this.comp = comp;
}
```
3️⃣ **Setter Injection (Alternative)**
```java
@Autowired
public void setComp(Computer comp) {
    this.comp = comp;
}
```
💡 **Constructor injection is preferred** because:  
✔ Ensures object dependencies are set when the object is created.  
✔ Avoids issues of uninitialized fields.  

---

### **🎯 Summary**
| Issue | Solution |
|------|---------|
| `@Autowired` can’t find `Computer` | Add `@Component` to the class |
| Multiple beans of the same type | Use `@Qualifier` or `@Primary` |
| Want a custom bean name | Use `@Component("customName")` |
| Best way to inject dependencies | Use Constructor Injection |

📌 **Next Lesson: `@Scope` for Controlling Bean Lifecycles**

H)
### **🔄 Handling Multiple Beans: `@Primary` vs `@Qualifier`**  

---

### **1️⃣ Problem: Multiple Beans of the Same Type**
When multiple beans implement the same interface, Spring **doesn’t know which one to inject**, leading to this error:  
```
No qualifying bean of type 'Computer' available: expected single matching bean but found 2
```
We previously solved this using `@Qualifier`, but **another approach** is `@Primary`.

---

### **2️⃣ Solution: Using `@Primary`**
To set a **default bean**, annotate one of the beans with `@Primary`:
```java
@Component
@Primary  // Marks this as the default bean
public class Desktop implements Computer {
    public void compile() {
        System.out.println("Compiling on Desktop...");
    }
}
```
**Now, whenever Spring is confused, it will pick `Desktop` by default.**  
No need for `@Qualifier` in this case.

---

### **3️⃣ `@Primary` vs. `@Qualifier` - Which One Wins?**
If we **explicitly specify a bean** using `@Qualifier`, it **overrides** `@Primary`.

```java
@Component
public class Laptop implements Computer {
    public void compile() {
        System.out.println("Compiling on Laptop...");
    }
}
```
```java
@Autowired
@Qualifier("laptop")  // Even though Desktop is @Primary, this takes priority
private Computer comp;
```
📌 **Priority Order:**  
✔ **If `@Qualifier` is used → Spring picks the specified bean.**  
✔ **If `@Qualifier` is NOT used → Spring picks the `@Primary` bean.**  

---

### **4️⃣ Example: Both `@Primary` and `@Qualifier`**
#### **Desktop as `@Primary`**
```java
@Component
@Primary  // Default bean
public class Desktop implements Computer {
    public void compile() {
        System.out.println("Compiling on Desktop...");
    }
}
```
#### **Laptop without `@Primary`**
```java
@Component
public class Laptop implements Computer {
    public void compile() {
        System.out.println("Compiling on Laptop...");
    }
}
```
#### **Autowired with `@Qualifier`**
```java
@Autowired
@Qualifier("laptop")  // Overrides @Primary and picks Laptop
private Computer comp;
```
🔹 **Spring picks** `Laptop` because `@Qualifier` has higher priority than `@Primary`.  

---

### **🎯 Summary**
| Scenario | Bean Selected |
|-----------|--------------|
| Only `@Primary` used | That bean is chosen |
| `@Qualifier` used | Qualifier bean is chosen (overrides `@Primary`) |
| No `@Primary` or `@Qualifier` | Error (if multiple beans exist) |

📌 **Use `@Primary` when you want a default, but use `@Qualifier` for precise control.**

I)
### **🔄 Understanding `@Scope` and `@Value` Annotations**  

---

## **1️⃣ `@Scope` – Controlling Bean Scope**
In Spring, **bean scope** defines how many instances of a bean will be created.

📌 **Common Scopes:**
- **Singleton (default):** One instance per Spring container.
- **Prototype:** New instance every time a bean is requested.

### **🛠 How to Define Scope?**
Use the `@Scope` annotation on a class:
```java
@Component
@Scope("prototype")  // Creates a new instance every time
public class Alien {
    public Alien() {
        System.out.println("Alien object created!");
    }
}
```
Now, each time `Alien` is requested, a **new** instance is created.

✔ **By default, beans are singleton, so no need for `@Scope("singleton")`.**

---

## **2️⃣ `@Value` – Injecting Values into Fields**
Sometimes, you want to assign values **from external sources** (like `application.properties`), instead of hardcoding them.

### **🔹 Hardcoding a Value (Not Recommended)**
```java
private int age = 21;  // Direct assignment
```
This makes it **difficult to change later**.

### **🔹 Using `@Value` (Recommended)**
```java
@Value("21")
private int age;
```
This allows **injecting values dynamically**.

---

## **3️⃣ Injecting Values from a Property File**
Instead of hardcoding, we can **store values in `application.properties`**.

#### **📝 `application.properties`**
```properties
alien.age=25
```
#### **🛠 Injecting the Property Value**
```java
@Value("${alien.age}")  // Fetching value from properties file
private int age;
```
Now, the value comes from `application.properties` and can be changed **without modifying the code**.

---

## **🎯 Summary**
| Annotation | Purpose |
|------------|---------|
| `@Scope("prototype")` | Creates a new bean instance every time |
| `@Scope("singleton")` | Uses a single shared instance (default) |
| `@Value("21")` | Assigns a hardcoded value |
| `@Value("${alien.age}")` | Injects value from `application.properties` |

📌 **Use `@Scope` to manage bean lifecycle and `@Value` for flexible value injection.**
