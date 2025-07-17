# Maven - Introduction & Importance

## What is Maven?

Maven is a project management tool used in Java development. It helps in compiling, running, testing, packaging, and deploying a project.

## Why do we need Maven?

A Java project often requires external libraries (JAR files). For example, to connect Java with a database, we need a database connector JAR (MySQL Connector, Postgres Connector, etc.). If using frameworks like Hibernate or Spring, multiple JAR files are required, including transitive dependencies (dependencies of dependencies).

## Problems without Maven

- Manually searching and downloading JAR files is time-consuming
- Keeping version compatibility between dependencies (e.g., Hibernate & Spring) is challenging
- Sharing projects with teammates requires them to download the same dependencies manually

## How Maven Helps

- Manages dependencies automatically
- Ensures version compatibility between different libraries
- Provides plugins for compiling, testing, running, and deploying

## Other Tools

**Alternatives:** Gradle, Ivy, etc.

Maven is beginner-friendly and widely used in Java development.

## How Maven Works

Developers specify dependencies in Maven. For example: "Maven, I need Hibernate dependency (specific version)." Maven fetches and manages dependencies automatically.

## Focus of This Section

Understanding Maven for dependency management (not plugins). Maven simplifies working with frameworks like Spring and Hibernate.

## **B) Setting Up Maven**

### **Ways to Use Maven:**

- **Install Locally**: Download from Maven's official website, set up the path, and use it via the command line.
- **Use with IDE (Preferred)**: Most IDEs like IntelliJ IDEA and Eclipse support Maven by default.

### **Creating a Maven Project in IntelliJ IDEA:**

- Open IntelliJ IDEA → Click on New Project.
- Choose Java as the language.
- Select Maven as the build system to maintain a consistent project structure across different IDEs.
- Click Create, and IntelliJ will set up the Maven project automatically.

---

## **Maven Lifecycle (Automation of Project Stages):**

Found under the Maven tool window in IDEs.

### **Key lifecycle phases:**

- **Compile**: Compiles all source files.
- **Test**: Runs test cases.
- **Package**: Creates a JAR file.
- **Install**: Installs the project to the local repository.
- **Deploy**: Deploys the project (requires configuration).

---

## **🔌 Full Maven Build Lifecycle**

Maven supports 3 core lifecycles:

- **Clean** – Cleans old builds (`mvn clean`)
- **Default** – Full build process (compile → test → package → install → deploy)
- **Site** – Generates documentation (`mvn site`)

### **Default Lifecycle Phases (with full flow):**

1. validate
2. compile
3. test
4. package
5. verify
6. install
7. deploy

Use this flow:

```bash
mvn clean install
Give this also in text format like the text you have given for remaining
```

Plugins automate tasks like compiling, packaging, testing, and deployment.

Accessible through IDEs or command-line execution.

**Next Topic: Maven Archetypes (Project Templates).**

---

## C) Maven Basics and Archetypes

### Maven Archetypes (Project Templates)

Maven provides a feature called Archetypes, which act as project templates.

Instead of manually setting up configurations, developers can use pre-built templates to streamline project creation.

**Why Use Archetypes?**

- Automates project setup.
- Reduces manual configuration effort.
- Ensures consistency in multiple projects within an organization.
- Allows the creation of custom archetypes for reusable templates.

---

## Maven Dependency Management

### Understanding Dependencies in Maven

A dependency refers to an external library (JAR file) required for a project.

Maven simplifies dependency management by automatically fetching required JAR files.

### Fetching Dependencies Manually (Traditional Way):

1. Search for a library (e.g., MySQL Connector) on Google.
2. Download the JAR file from the official site.
3. Manually add it to the project.
4. Manage versions and updates manually.

### Fetching Dependencies Using Maven:

1. Go to [https://mvnrepository.com](https://mvnrepository.com/)
2. Search for the required library (e.g., MySQL Connector).
3. Copy the Maven dependency XML snippet.
4. Paste it into the `pom.xml` file inside the `<dependencies>` tag.
5. Reload Maven (via Maven Tool Window in IntelliJ IDEA).

Maven downloads the required JAR files automatically.

---

### Structure of a Maven Dependency (GAV)

Each dependency follows a GAV structure:

- **Group ID**: Identifies the organization or project (e.g., `com.mysql`)
- **Artifact ID**: Name of the specific library (e.g., `mysql-connector-j`)
- **Version**: Defines the version of the library (e.g., `8.0.26`)

```xml
<dependency>
  <groupId>com.mysql</groupId>
  <artifactId>mysql-connector-j</artifactId>
  <version>8.0.26</version>
</dependency>

```

---

### Maven Transitive Dependencies

Some dependencies require other libraries to function.

Maven automatically downloads and manages these transitive dependencies.

Example: Adding Hibernate as a dependency will also download its required libraries (e.g., GlassFish, JDBC drivers, etc.).

**Adding Hibernate Dependency**

1. Search for `hibernate-core` in mvnrepository.com
2. Copy the dependency XML
3. Paste it inside `<dependencies>` in `pom.xml`
4. Reload Maven to download all required files

```xml
<dependency>
  <groupId>org.hibernate</groupId>
  <artifactId>hibernate-core</artifactId>
  <version>5.4.32.Final</version>
</dependency>

```

---

### Maven Project Object Model (POM.XML)

The `pom.xml` file is the heart of a Maven project.

Defines:

- Project metadata (name, version, etc.)
- Dependencies
- Plugins
- Build configurations

### Key Elements in `pom.xml`:

```xml
<project>
  <modelVersion>4.0.0</modelVersion>
  <groupId>com.example</groupId>
  <artifactId>my-maven-project</artifactId>
  <version>1.0-SNAPSHOT</version>

  <dependencies>
    <!-- Place all dependencies here -->
  </dependencies>
</project>

```

---

### Sharing Maven Projects

No need to share JAR files.

Simply share the `pom.xml` file.

The receiver can reload Maven and automatically fetch all dependencies.

---

### Next Steps

- Understanding how Maven resolves dependencies behind the scenes.
- Exploring Maven repositories and local caching mechanisms.

---

## D) Maven Dependency Management

### Maven Archetypes

- A templating tool for creating predefined project structures.
- Useful for setting up multiple projects with the same configuration.

### Dependency Management

- Dependencies (JAR files) are managed via `pom.xml`
- Uses GAV (Group ID, Artifact ID, Version) for unique identification
- Can have transitive dependencies

### POM (Project Object Model) File

- `pom.xml` defines project metadata, dependencies, and configurations
- Can include plugins for additional functionality

### Effective POM (Super POM)

- The actual POM used by Maven, including default settings
- Can be viewed in IntelliJ via Right Click → Maven → Show Effective POM

### Maven Plugins

- Automate compile, clean, package, and test phases
- Many default plugins are included in the Effective POM

### Project Sharing in Maven

- Instead of sharing JARs, share the `pom.xml` file

---

## E) Maven Archetypes

### What Are Archetypes?

- Templates used to generate project structures
- Predefined configurations reduce manual setup effort

### Using Maven Archetypes in IntelliJ

- When creating a new project, select "Maven Archetypes"
- Choose from available archetypes like: `quickstart`, `webapp`, `j2ee-simple`

### Fetching Archetypes from Maven Central

- External archetypes can be loaded
- More options are available online, including Spring Boot templates

### Spring Boot Project Using Archetypes

- Downloads required dependencies: `spring-boot-starter`, `webmvc`, `jdbc`, `H2`
- Generates configuration files like `application.properties`

### Archetypes in Eclipse

- Eclipse also supports archetypes
- Offers selection during project creation

---

## F) Maven in ecclipse

### Creating a Maven Project in Eclipse

- Open Eclipse IDE → File > New > Maven Project
- Uncheck "Create a simple project" to allow archetype selection

### Selecting Archetypes in Eclipse

- Choose an archetype (e.g., `mvc-archetype`)
- Define Group ID and Artifact ID
- Click Finish to generate the structure

### Viewing the Effective POM

- Eclipse allows viewing the effective POM

### Key Takeaway

- Maven functionality is consistent across IDEs

---

## G)  How Maven Works Behind the Scenes

### Dependency Resolution Process

1. Maven checks your local `.m2` repository
2. If not found, fetches from Maven Central
3. Downloads and stores for future use

### Local Repository (.m2 Folder)

- Stores previously downloaded dependencies

### Remote Repository

- Maven Central is the default
- Additional repositories can be configured

### Security & Vulnerabilities

- IDEs show warnings for outdated or insecure dependencies

### Company-Wide Repository

- Organizations may use Nexus or Artifactory
- Developers may need permission to fetch external libraries

### Fixing Dependency Issues

- Delete corrupted `.m2` folders
- Update versions

---

## H)Dependency Scopes in Maven

---

Dependency scopes in Maven define **when and where** a dependency is available during different phases of your project's build lifecycle. Think of scopes as rules that tell Maven: "Use this dependency only during specific phases like compilation, testing, or runtime."

### Why Are Dependency Scopes Important?

1. **Optimize build performance** - Only include necessary dependencies at each phase
2. **Avoid classpath conflicts** - Prevent unnecessary JARs from being included in final deployment
3. **Manage container-provided libraries** - Handle dependencies provided by application servers
4. **Separate test dependencies** - Keep test libraries separate from production code

### The Six Dependency Scopes

| Scope | Compile Time | Runtime | Test Time | Included in Package | Use Case |
| --- | --- | --- | --- | --- | --- |
| **compile** | ✅ | ✅ | ✅ | ✅ | Default scope for most dependencies |
| **provided** | ✅ | ❌ | ✅ | ❌ | Container-provided libraries (servlets, JPA) |
| **runtime** | ❌ | ✅ | ✅ | ✅ | Database drivers, logging implementations |
| **test** | ❌ | ❌ | ✅ | ❌ | Testing frameworks (JUnit, Mockito) |
| **system** | ✅ | ✅ | ✅ | ❌ | Local system JARs (deprecated) |
| **import** | N/A | N/A | N/A | N/A | For importing BOMs only |

### Detailed Explanation of Each Scope

### 1. **compile** (Default Scope)

```xml
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-core</artifactId>
    <version>5.3.21</version>
    <!-- scope is compile by default -->
</dependency>

```

- Available during compilation, testing, and runtime
- Included in the final JAR/WAR file
- Most common scope for application dependencies

### 2. **provided**

```xml
<dependency>
    <groupId>javax.servlet</groupId>
    <artifactId>javax.servlet-api</artifactId>
    <version>4.0.1</version>
    <scope>provided</scope>
</dependency>

```

- Available during compilation and testing
- **NOT included** in final package
- Expected to be provided by the runtime environment (Tomcat, JBoss, etc.)
- Common for: Servlet API, JPA API, EJB API

### 3. **runtime**

```xml
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>8.0.33</version>
    <scope>runtime</scope>
</dependency>

```

- Not needed during compilation
- Available during testing and runtime
- Included in final package
- Common for: Database drivers, logging implementations (Logback, Log4j)

### 4. **test**

```xml
<dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <version>4.13.2</version>
    <scope>test</scope>
</dependency>

```

- Only available during test compilation and execution
- Never included in final package
- Common for: JUnit, Mockito, TestNG, Spring Test

### 5. **system** (Deprecated)

```xml
<dependency>
    <groupId>com.example</groupId>
    <artifactId>custom-lib</artifactId>
    <version>1.0</version>
    <scope>system</scope>
    <systemPath>${project.basedir}/lib/custom-lib.jar</systemPath>
</dependency>

```

- Similar to `provided` but requires explicit path
- **Avoid using** - use local repository installation instead

### 6. **import**

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-dependencies</artifactId>
    <version>2.7.0</version>
    <type>pom</type>
    <scope>import</scope>
</dependency>

```

- Only used in `<dependencyManagement>` section
- Imports dependency versions from a BOM (Bill of Materials)

### Practical Examples

**Web Application Example:**

```xml
<dependencies>
    <!-- Business logic - compile scope -->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-context</artifactId>
        <version>5.3.21</version>
    </dependency>

    <!-- Servlet API - provided by Tomcat -->
    <dependency>
        <groupId>javax.servlet</groupId>
        <artifactId>javax.servlet-api</artifactId>
        <version>4.0.1</version>
        <scope>provided</scope>
    </dependency>

    <!-- Database driver - runtime only -->
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>8.0.33</version>
        <scope>runtime</scope>
    </dependency>

    <!-- Testing framework - test only -->
    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>4.13.2</version>
        <scope>test</scope>
    </dependency>
</dependencies>

```

### Best Practices

1. **Use `provided` for container APIs** - servlet-api, jpa-api
2. **Use `runtime` for implementation JARs** - database drivers, logging
3. **Use `test` for testing dependencies** - JUnit, Mockito
4. **Default to `compile`** for most application dependencies
5. **Avoid `system` scope** - use local repository instead

### Impact on Transitive Dependencies

Dependency scopes also affect transitive dependencies:

- `compile` dependencies bring their transitive dependencies
- `provided` and `test` dependencies don't pass their transitive dependencies to your project
- `runtime` dependencies include their transitive dependencies at runtime

Understanding dependency scopes helps you build cleaner, more efficient Maven projects with proper separation of concerns between compilation, testing, and runtime phases.

## 🧩 Dependency Scopes-Summary

| Scope | Description |
| --- | --- |
| `compile` | Default. Used everywhere. |
| `provided` | Required at compile, provided by container |
| `runtime` | Not needed during compile, only at runtime |
| `test` | Available only during test phase |
| `system` | Like `provided` but with local path (deprecated) |
| `import` | For importing BOMs |

---

## 📋 BOM - Bill Of Materials

---

**BOM (Bill of Materials)** in Maven is a special type of POM file that centralizes and manages dependency versions across multiple projects or modules. Think of it as a "version catalog" that ensures all related dependencies use compatible versions.

### What Problem Does BOM Solve?

Without BOM, you might face:

```xml
<!-- Different projects using different versions -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-core</artifactId>
    <version>5.3.21</version>
</dependency>
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-web</artifactId>
    <version>5.2.15</version> <!-- Version mismatch! -->
</dependency>

```

This can lead to:

- **Version conflicts** between related libraries
- **Runtime errors** due to incompatible versions
- **Maintenance nightmare** when updating versions across multiple projects

### How BOM Works

BOM acts as a **parent catalog** that defines compatible versions for a family of related dependencies:

### 1. **BOM Definition** (spring-boot-dependencies.pom)

```xml
<project>
    <modelVersion>4.0.0</modelVersion>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-dependencies</artifactId>
    <version>2.7.0</version>
    <packaging>pom</packaging>

    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>org.springframework</groupId>
                <artifactId>spring-core</artifactId>
                <version>5.3.21</version>
            </dependency>
            <dependency>
                <groupId>org.springframework</groupId>
                <artifactId>spring-web</artifactId>
                <version>5.3.21</version>
            </dependency>
            <dependency>
                <groupId>org.hibernate</groupId>
                <artifactId>hibernate-core</artifactId>
                <version>5.6.9.Final</version>
            </dependency>
        </dependencies>
    </dependencyManagement>
</project>

```

### 2. **Using BOM in Your Project**

```xml
<project>
    <dependencyManagement>
        <dependencies>
            <!-- Import the BOM -->
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-dependencies</artifactId>
                <version>2.7.0</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
        </dependencies>
    </dependencyManagement>

    <dependencies>
        <!-- No version needed - comes from BOM -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-core</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.hibernate</groupId>
            <artifactId>hibernate-core</artifactId>
        </dependency>
    </dependencies>
</project>

```

### Key Components of BOM Usage

### 1. **`<dependencyManagement>` Section**

- Defines version constraints without actually adding dependencies
- Acts as a "template" for child projects
- Only declares what versions should be used

### 2. **`<scope>import</scope>`**

- Special scope only used in `<dependencyManagement>`
- Imports version definitions from another POM
- Merges the imported BOM's dependency management into your project

### 3. **`<type>pom</type>`**

- Indicates you're importing a POM file, not a JAR
- Required when using `scope=import`

### Real-World Example: Spring Cloud BOM

```xml
<dependencyManagement>
    <dependencies>
        <!-- Spring Cloud BOM -->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-dependencies</artifactId>
            <version>2023.0.1</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
    </dependencies>
</dependencyManagement>

<dependencies>
    <!-- All these get compatible versions from BOM -->
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-starter-config</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-starter-gateway</artifactId>
    </dependency>
</dependencies>

```

### Benefits of Using BOM

1. **Version Consistency**: All related dependencies use tested, compatible versions
2. **Simplified Maintenance**: Update one BOM version instead of multiple individual versions
3. **Reduced Conflicts**: Eliminates version mismatches between related libraries
4. **Enterprise Standards**: Teams can create company-wide BOMs for standardization
5. **Easier Upgrades**: Upgrading framework versions becomes a single-line change

### BOM vs Regular Dependencies

| Aspect | Regular Dependencies | BOM |
| --- | --- | --- |
| **Purpose** | Add actual JARs to classpath | Define version constraints |
| **Packaging** | `jar`, `war`, etc. | `pom` |
| **Scope** | `compile`, `test`, etc. | `import` (only in dependencyManagement) |
| **Effect** | Adds to classpath | Sets version rules |
| **Location** | `<dependencies>` | `<dependencyManagement>` |

### Creating Your Own BOM

```xml
<!-- company-bom.pom -->
<project>
    <groupId>com.mycompany</groupId>
    <artifactId>company-bom</artifactId>
    <version>1.0.0</version>
    <packaging>pom</packaging>

    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-web</artifactId>
                <version>2.7.0</version>
            </dependency>
            <dependency>
                <groupId>com.mycompany</groupId>
                <artifactId>common-utils</artifactId>
                <version>2.1.0</version>
            </dependency>
        </dependencies>
    </dependencyManagement>
</project>

```

### Common BOM Examples

- **Spring Boot**: `spring-boot-dependencies`
- **Spring Cloud**: `spring-cloud-dependencies`
- **Jackson**: `jackson-bom`
- **AWS SDK**: `aws-java-sdk-bom`
- **Testcontainers**: `testcontainers-bom`

### Best Practices

1. **Always use BOMs** for framework families (Spring, AWS SDK, etc.)
2. **Import BOMs in `dependencyManagement`** never in `dependencies`
3. **Override versions carefully** - only when necessary
4. **Create company BOMs** for internal library standardization
5. **Keep BOM versions updated** regularly

BOM is essentially Maven's way of saying: "Here's a curated list of dependency versions that work well together - use this instead of guessing!"

---

## 🧱Multi-Module Maven Project

Used in enterprise projects with microservices or separate layers.

```xml
<modules>
  <module>auth-service</module>
  <module>payment-service</module>
</modules>

```

Parent `pom.xml` manages common plugins/dependencies.

---

## 🌍  Maven Profiles

Use `profiles` for environment-specific builds.

```xml
<profiles>
  <profile>
    <id>dev</id>
    <properties>
      <env>dev</env>
    </properties>
  </profile>
</profiles>

```

Run using:

```bash
mvn clean install -Pdev

```

---

## 🔐  settings.xml Configuration

Located in:

- Windows: `C:\\Users\\you\\.m2\\settings.xml`
- Mac/Linux: `~/.m2/settings.xml`

### Use Cases:

- Configure proxy
- Define private repos (Nexus, Artifactory)
- Store credentials securely

---
