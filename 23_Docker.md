# 🐳 Docker Introduction – Video Summary

## 🎯 Overview

This video kicks off a new section focused on **Docker**, an essential tool in modern software development. The instructor introduces the goals and structure of the upcoming content.

## 📌 Key Topics Covered

1. **Why Docker?**
   - Understanding the *need* for Docker in software development.
   - Comparison with other existing solutions.

2. **Docker Setup**
   - How to install and configure Docker on your machine.

3. **Running Containers**
   - Basics of launching and managing Docker containers.

4. **Spring Boot with Docker**
   - Building and containerizing a Spring Boot application.

5. **Multi-Container Environments**
   - Introduction to using **Docker Compose** for orchestrating multiple containers.

## 🛠️ Learning Advice

- **Practice Alongside the Videos**: Hands-on experimentation is encouraged.
- Watching alone might not be effective — real learning happens by *doing*.

## ✅ Final Notes

This section promises a complete introduction to Docker fundamentals, practical applications with Spring Boot, and container orchestration using Docker Compose.

# 🧩 Why Do We Need Docker? – Video Summary

## 🎯 Problem Being Solved

Before diving into Docker, it's crucial to understand the **problem Docker aims to solve**: 
> "It works on my machine" syndrome.

## 💼 Typical Development Scenario

1. Developers build applications on **their local machines**.
2. These apps often:
   - Connect to databases (MySQL, PostgreSQL, MongoDB, etc.)
   - Rely on custom environment configurations.
3. The setup **works perfectly locally**.

## 🔄 Real-World Challenges

- When **sharing the project** with teammates, testers, or operations teams:
  - They must replicate the same environment setup.
  - Minor configuration mismatches lead to project failure.
  - OS or hardware differences can break the application.

### 🧪 Testing Phase
- QA teams need the exact same environment.
- Configuration errors = wasted time and confusion.

### 🚀 Production Deployment
- Ops teams use different OS and server configurations.
- Standalone code often fails to run reliably in these environments.

> 🧨 Developers get calls over the weekend to "fix" deployments that worked fine on their machine.

## 🧠 Insight

Simply sharing code isn’t enough — you need to **share the environment** too.

### Hypothetical Solution?
- "Why not just share your C: drive?"
  - 🛑 Not feasible: OS differences, hardware mismatches, security, etc.

## 💡 Historical Solution Before Docker

- **Virtualization**:
  - Using virtual machines (VMs) to simulate a complete OS environment.
  - Solves some problems, but is **resource-intensive** and **slow to manage**.

## ✅ Conclusion

We need a better, lighter way to package both:
- Application **code**
- Application **environment**

👉 Enter **Docker** (to be covered in the next part).


# 🧠 Understanding Virtualization – Video Summary

## 🚨 Problem Recap

- Applications that work on your machine may not work on someone else’s due to:
  - OS differences
  - Configuration mismatches
  - Missing dependencies
  - Time-consuming setups

## 🛠 Solution #1: Virtualization

Virtualization allows you to create **isolated environments** with their own OS, apps, and configurations.

---

## 🔍 What is Virtualization?

### 🧱 Basic System Layers:
- **Hardware** (CPU, RAM, etc.)
- **Host OS** (e.g., Windows, Linux)
- **Applications** (e.g., calculator)

### 👨‍💻 For Developers:
You need:
- Host OS (e.g., Windows)
- Hypervisor (e.g., VMware, VirtualBox)
- Guest OS (e.g., Ubuntu)
- Applications installed inside guest OS

> Virtual Machine (VM) = Software that pretends to be hardware so another OS can run inside it.

---

## 🖼 What Is an Image?

- A VM's **snapshot** (entire OS with configuration and running state)
- Can be saved as a file and **shared** with teams (QA, Ops)

> 🔁 Once someone has a hypervisor, they can run the same image and get the exact same environment.

---

## 💡 Historical Use Case (1960s–70s)

### 🏢 Physical Servers:
- Big, powerful machines were underutilized by single apps.
- Needed **isolation** between multiple apps.

### ✅ Solution:
- Use VMs:
  - Each app runs inside its **own guest OS**
  - Each guest OS is isolated

---

## ⚠️ Limitations of Virtualization

1. 🐢 **Heavy Resource Usage**:
   - Every VM runs a full OS → High RAM & CPU usage
2. 💸 **Licensing Costs**:
   - Each OS (e.g., Windows) might need its own license
3. 🐌 **Performance Bottlenecks**:
   - Running multiple OSs slows down the system
4. 🧩 **Redundant Overhead**:
   - Running the whole OS just to run a small app

---

## ✅ Summary

- Virtualization solves the problem of **environment mismatch**.
- Great for **isolation** and **sharing OS-level environments**.
- But it has drawbacks: **resource-heavy**, **costly**, and **slow**.

---

## ⏭️ Coming Up Next

> **Solution to Virtualization’s Problems → Containerization (Docker!)**


# 📦 Introduction to Containerization – Video Summary

## 🚧 The Problem with Virtual Machines

- Traditional virtualization runs **multiple guest OSes** on a hypervisor.
- Each guest OS includes:
  - Web servers
  - Databases
  - Compilers
  - App code
- Problems:
  - **Heavy** (resource intensive)
  - **Slow** to boot
  - **Redundant OS overhead**
  - Difficult to **scale**

---

## 🚢 Real-World Analogy: Shipping Containers

- Just like how the shipping industry uses **standard containers** to move goods:
  - No need to unpack/repack
  - Same container moves via **truck → ship → port → truck**
- In software:
  - Think of your **app + dependencies** as a container
  - It can move across systems **without modification**

---

## 🛠 What Is a Container?

- A **lightweight**, standalone package that includes:
  - Your application
  - All dependencies (web server, compilers, DB, etc.)
- Runs **on top of the host OS**, without requiring a full guest OS
- Managed using tools like:
  - Docker 🐳 (most popular)
  - Podman
  - Others

---

## ⚙️ How It Works

### Without Virtualization:


# 🐳 What is Docker? – Video Summary

## 🧰 Docker: Definition

> **Docker is a platform and a set of tools** used to enable and manage **containerization** in software development.

---

## 🧩 Core Components of Docker

| Component       | Description |
|----------------|-------------|
| **Docker Engine** | Core service that runs and manages containers |
| **Containers**     | Lightweight, isolated units where apps run with all dependencies |
| **Images**         | Immutable blueprints for containers (used to create containers) |
| **Dockerfile**     | Script with instructions to build an image |
| **Docker Hub**     | Cloud repository of ready-made images (e.g., Tomcat, MongoDB) |
| **Networking**     | Built-in support for container networking, including port exposure |
| **Volumes**        | Persistent storage mechanism for container data |
| **Docker Compose** | Tool to define and run multi-container Docker applications |

---

## 📦 From Developer to Deployment

- You **configure** your app with a **Dockerfile**
- Build an **image** from it
- **Share** the image or push it to Docker Hub
- Anyone can **pull** the image and **run** it as a container
- Works the same across all environments (dev, test, prod)

---

## 💡 Why Use Docker?

| Benefit         | Description |
|----------------|-------------|
| 📤 Portability   | Same image works on any machine with Docker |
| 🔒 Security      | Containers are isolated from the host system |
| 📈 Scalability   | Run multiple instances of the same image easily |
| ⚙️ Reusability   | Use prebuilt images from Docker Hub |
| 💾 Persistence   | Volumes allow data to survive container restarts |

---

## 🔧 Next Steps

- ✅ Setup Docker on your machine
- ✅ Learn to run and manage containers
- ✅ Explore Docker Compose for multi-container apps

➡️ **Coming up next**: Docker installation and running your first container.


# 🐳 Docker Desktop Overview and Using Hello World Image

## 🔍 Docker Desktop UI Overview
- Docker Desktop has tabs for:
  - **Containers** – Running or stopped container instances.
  - **Images** – Pulled images available for creating containers.
  - **Volumes** – Data persisted by containers.

## 📦 Images vs Containers
- **Images**: Lightweight blueprints with instructions.
- **Containers**: Actual running environments; heavier than images.

## 📥 Downloading Images
- Images can be searched and pulled from Docker Hub via Docker Desktop.
- Prefer **official** and **verified** images for stability.
- Each image has:
  - Tags (versions like `latest`, `5.7`, etc.)
  - Download count and stars (popularity)

## 🐘 MySQL Example
- Pulling the MySQL image can take time due to size.
- Containers created from the image appear under the *Containers* tab.
- Images in use by containers cannot be deleted unless the container is first removed.

## 🧪 Running Hello World (Demo)
- Pull and run the lightweight `hello-world` image (official).
- If the image isn't found locally, it gets pulled from Docker Hub automatically.
- Running it:
  - Displays a confirmation message with output
  - Creates a short-lived container (exits after execution)

## 📊 Image & Container Info
- Docker assigns each container a unique ID and a random name (e.g., `nice_borg`).
- Images and containers can be inspected both in Docker Desktop and via command line.
- Naming containers explicitly is helpful when managing multiple containers.

## 🔁 Summary
- You can interact with Docker via GUI (Docker Desktop) or CLI.
- Images must be pulled before containers can be run.
- Hello World is a basic image to verify Docker is working.
- Containers that have exited still appear in the list and can be inspected or deleted later.


# 🐳 Docker CLI Essentials: Images, Containers, and Common Commands

## 🎯 What This Covers
- How to remove containers and images
- Using the Docker CLI help system
- Step-by-step breakdown of image and container management
- Common Docker commands and what they do

---

## 📘 Common Docker Commands (Conceptual Overview)

- `help`: Displays a list of Docker CLI commands and descriptions.
- `ps -a`: Shows all containers, including those that are stopped.
- `rm`: Removes a specified container.
- `rmi`: Removes a specified image. The image must not be used by any container.
- `run`: Pulls, creates, and starts a container in one go.
- `images`: Lists all locally available images.
- `start`: Starts a stopped container.
- `stop`: Stops a running container.
- `pause`: Pauses a running container.
- `search`: Searches Docker Hub for images.
- `pull`: Downloads an image from Docker Hub.
- `create`: Creates a container from an image without starting it.

---

## 🧼 Cleanup Process (No Commands Shown)

1. List all containers, identify the one you want to remove.
2. Remove the stopped container.
3. List all images to find the one that’s no longer needed.
4. Remove the image. This only works if no container is using it.

Note: If a container exists using an image, Docker won't allow you to delete the image without removing the container first.

---

## 🧪 Step-by-Step Workflow Summary

1. Search for the image on Docker Hub to verify availability.
2. Pull the image from Docker Hub to your local system.
3. Create a container using the image.
4. Start the container to run it.
5. Stop or pause the container if needed.
6. Remove the container once done.
7. Remove the image if it’s no longer needed.

---

## ✅ Final Notes

- The `run` command is a shortcut that performs search, pull, create, and start in one step.
- Managing containers and images separately gives you more control and clarity.
- Understanding container/image IDs or names helps when referencing them in commands.


# 🧱 Docker Architecture: A Concise Overview

## 👤 User Interaction
- As a **developer**, you interact with Docker using the **Docker Client**.
- The **Docker Client** is part of the Docker software and serves as your primary interface.

## 🔄 Docker Components

### 🧠 Docker Daemon (`dockerd`)
- A background process responsible for:
  - Pulling images from registries
  - Creating and managing containers
  - Configuring networks and volumes
- Listens to client commands via the **Docker API** and executes them.

### 📦 Docker Internals
- **Images**: Blueprints for containers.
- **Containers**: Running instances of images.
- **Networks**: Virtual networking layer for container communication.
- **Volumes**: Persistent data storage shared between containers.

### 🌐 Docker Registry
- External repository that stores Docker images.
- Can be **public** (like Docker Hub) or **private** (used by organizations).

## 🧭 Communication Flow
1. **User** issues a command via the Docker Client.
2. The **Docker Client** sends the request to the **Docker API**.
3. The **Docker Daemon** receives the request and:
   - Pulls images from the **Registry**
   - Creates and starts containers
   - Manages networking and volumes

---

## ✅ Summary
- **Docker Client** is your interface to Docker.
- **Docker Daemon (`dockerd`)** runs in the background and executes your commands.
- **Docker Registry** is where images are stored.
- Core Docker components include **images, containers, networks, and volumes**.
- The entire process is coordinated via the **Docker API**.


# ☕ Running JDK Inside Docker

## 🧠 Overview
This video demonstrates how to use **JDK within a Docker container**, specifically focusing on running **JShell** in an isolated environment.

---

## 📌 Why Run JDK in Docker?

- Running Java apps requires **JDK** (not just JVM) for compilation, debugging, profiling, etc.
- Instead of sharing your app with the JDK setup separately, Docker lets you package everything together.
- Ideal for sharing across teams without environment setup concerns.

---

## 🔍 What Was Demonstrated?

### ✅ JShell on Host Machine
- Ran basic Java commands using JShell on the local system to show typical usage.

### ✅ Pulling a JDK Docker Image
- Used Docker CLI to **search and pull** an appropriate OpenJDK image (e.g., `openjdk:22-jdk`) from Docker Hub.
- Chose an image compatible with the system architecture (e.g., ARM64 for Apple Silicon).

### ✅ Running JShell Inside Docker
- Started a container interactively using:
  - `docker run -it openjdk:22-jdk`
- Verified JShell worked inside the container:
  - Declared a variable
  - Printed "Hello world" using `System.out.println()`

### ⚠️ Detached vs Interactive Mode
- Emphasized using **interactive mode (`-it`)** over detached mode (`-d`) for direct input/output access.

---

## 🧪 Outcome
- Successfully launched a **JShell session inside a Docker container** running OpenJDK.
- Proved that Java apps (and tools like JShell) can be executed in a containerized environment.

---

## 🔜 Coming Up
- The next video will explore how to **run full Java applications**, including complex ones like **Spring Boot**, inside Docker containers.


# 🧪 Creating and Running a Spring Boot App in Docker (Part 1)

## Overview  
This video demonstrates how to build a simple Spring Boot "Hello World" web application, package it as a JAR, and prepare it for Docker deployment.

## Steps Covered

### 🚀 Spring Boot App Creation  
The app is generated using [start.spring.io](https://start.spring.io) with the following settings:
- **Project Type**: Maven  
- **Language**: Java  
- **Spring Boot Version**: 3.1.5  
- **Group ID**: `com.telusko`  
- **Artifact**: `RestDemo`  
- **Packaging**: Jar  
- **Dependency**: Spring Web

### 🧑‍💻 REST Controller Setup  
A simple `HelloController` class is added, which maps the root URL (`/`) to return `Hello World`.

### ✅ Running the App Locally  
- Changed the port to `8081` in `application.properties` due to a port conflict.  
- Ran the app using IntelliJ IDEA and verified the response in the browser (`Hello World` shown on port 8081).

### 📦 Creating the JAR File  
- Built the project using Maven (`mvn package`).  
- The output JAR file `RestDemo.jar` is created in the `target/` directory.

### 🔄 Running the JAR Locally  
- The app is executed using `java -jar RestDemo.jar`.  
- Verified that the application runs successfully on port `8080`.

## 📌 Next Step  
In the upcoming video, the JAR file will be moved into a Docker container and executed inside it.

# 📦 Running Spring Boot JAR Inside Docker (Part 2)

## 🎯 Objective
Deploy a Spring Boot `JAR` file inside a Docker container, create a custom Docker image, and expose it to run on the host machine.

## 🧩 Key Steps

### 1. Copying JAR into Running Container
- Stopped the running JAR process.
- Used `docker cp` to move the `rest-demo.jar` into the `/tmp` directory of the container.

### 2. Verifying Copy
- Used `docker exec <container> ls /tmp` to confirm the JAR file exists in the container.

### 3. Creating Docker Image
- Created an image from the running container using `docker commit`.
- The first image (`v1`) ran the default `jshell` (inherited from the base JDK image).

### 4. Setting Default Run Command
- Used `docker commit` with a `--change` flag to specify a custom `CMD`:
  - `["java", "-jar", "/tmp/rest-demo.jar"]`
- Created a new version of the image (`v2`) with the correct entry point.

### 5. Running the Image with Port Binding
- Specified port mapping using `-p 8081:8081` to expose Docker container’s internal port to the host.
- Successfully accessed the app via browser (`http://localhost:8081`).

## ✅ Outcome
A Docker image that runs a Spring Boot app and is ready to be shared or pushed to Docker Hub. The app runs reliably across environments thanks to containerization.

## 🔁 Next Steps
Push the image to a registry or use `Dockerfile` for repeatable builds.

# 🐳 Automating Docker Image Creation Using Dockerfile

## 🎯 Objective
Eliminate repetitive manual steps for building Docker images by using a `Dockerfile`.

## 🧩 Problem with Manual Steps
- Previously, the image was created manually using:
  - Pulling a base JDK container.
  - Copying the JAR file into it.
  - Running `docker commit` with custom entrypoints.
- ❌ Manual steps become inefficient for complex projects with multiple services or commands.

## ✅ Solution: Dockerfile
Use a `Dockerfile` to define all build steps once, allowing automated image creation.

### 🔧 Key Instructions in Dockerfile
1. **Base Image**  
   `FROM openjdk:22-jdk` – Uses OpenJDK as the base.

2. **Copy Application**  
   `ADD target/RestDemo.jar RestDemo.jar` – Adds the JAR to the image.

3. **Set Entrypoint**  
   `ENTRYPOINT ["java", "-jar", "RestDemo.jar"]` – Defines the run command.

### 🏗️ Build the Image
- Run `docker build -t telusko-rest-demo:v3 .` to build the image from the Dockerfile.
- Docker uses cache to optimize repeat builds.

### 🚀 Run the Container
- Run `docker run -p 8081:8081 telusko-rest-demo:v3` to start the app.
- Access it on `http://localhost:8081`.

## 📌 Outcome
- Created a reusable Docker image using a `Dockerfile`.
- Simplified build process into a single command.
- Ensures consistency and automation for future changes.

## 🔄 Future Use
The Dockerfile will be reused for continuous updates and deployments.


# 📦 Dockerfile & Image Automation – Summary

## 🎯 Objective
The video explains how to automate Docker image creation for a Java project using a `Dockerfile`, instead of manually performing multiple Docker steps.

## 🧠 Problem with Manual Steps
Previously, each time a change was made to the project:
- The base image (like JDK) had to be selected
- The JAR file needed to be manually copied into a container
- The container had to be committed to form a new image

This process was repetitive and inefficient, especially for larger or more complex projects.

## ✅ Solution: Dockerfile
A `Dockerfile` automates this by allowing you to:
- Define the base image (e.g., JDK)
- Specify which files to copy (like the built JAR)
- Set the startup command to run the application

This setup only needs to be written once. After that, the image can be rebuilt with a single command whenever changes are made.

## 🔁 Workflow Using Dockerfile
1. Create a `Dockerfile` in your project directory
2. Define all steps (base image, file additions, entry point)
3. Build the image using a simple command
4. Run the container with the new image

## 🧪 Result
Once the image is built from the `Dockerfile`, it runs just like the manually created one, successfully serving the Java application on the defined port.

## 📌 Key Takeaway
Using a `Dockerfile`:
- Saves time and reduces manual errors
- Scales well for larger applications
- Is essential for consistent, repeatable Docker image builds


# 📦 Dockerizing a Spring Boot App with Dockerfile

## 🎯 Objective

The goal is to simplify and automate the containerization of a Spring Boot application using a Dockerfile instead of manually performing multiple Docker steps.

---

## 🔄 Current Manual Process (Before Dockerfile)

Previously, each time the application changed, the following manual steps were required:
- Selecting a base container with JDK installed
- Copying the JAR file into that container
- Committing the container to create an image
- Running the image to test it

While this worked, repeating these steps for every small change was inefficient and error-prone.

---

## 🛠 Solution: Use a Dockerfile

To automate image creation, a Dockerfile is introduced:
- It defines all the required steps in a single configuration file
- Helps rebuild the image consistently and easily

Key elements included in the Dockerfile:
- The base image (e.g., OpenJDK)
- Instructions to copy the JAR file into the container
- An entry point command to run the JAR

Optional metadata like maintainer name or exposed ports can also be added.

---

## ✅ Benefits of Dockerfile

- Simplifies image creation to a single command
- Reduces human error by avoiding manual repetition
- Makes containerization scalable for more complex apps
- Provides consistency across builds

---

## 🧪 Building and Running the Image

Once the Dockerfile is written:
- An image is created using a single command
- This image can then be run to start the application

---

## 🧠 Additional Notes

- Docker uses caching to speed up image creation for unchanged layers
- A `.dockerignore` file can be used to exclude unnecessary files from being copied into the container
- When the image is run, the application should behave as expected, serving responses on the defined port

---

## ✅ Summary

Using a Dockerfile is a clean and efficient way to containerize a Spring Boot app. It eliminates the need for repetitive manual Docker steps and enables quick rebuilding of containers, especially useful when the application grows in complexity.

Next steps involve refining and expanding the use of Dockerfiles in multi-container setups using Docker Compose.


# 🚀 Dockerizing a Spring Boot + PostgreSQL App

## ✅ What’s Set Up So Far
- A Spring Boot web application is ready.
- PostgreSQL is used as the database.
- Initial data is loaded via `data.sql`.
- Application runs and fetches data from the DB correctly.

## 📦 Dockerizing the App
- A `Dockerfile` is added to containerize the Spring Boot application.
- The JAR file is built using `mvn clean package` (with a custom `finalName` to simplify the JAR name).
- This allows us to create a Docker image for the app.

## 🐳 Introducing Docker Compose
- Docker Compose is used to orchestrate **multiple containers**: one for the app and one for PostgreSQL.
- A `docker-compose.yml` file is created:
  - Defines services for `app` and `postgres`.
  - App service uses the `Dockerfile` to build the image and exposes ports (e.g., `8090:8080`).
  - Postgres service uses the official image, sets environment variables (`POSTGRES_USER`, `POSTGRES_PASSWORD`, `POSTGRES_DB`), and maps ports (e.g., `5433:5432`).

## ⚠️ Issue Encountered
- Docker containers are isolated, so they can’t connect to each other by default.
- When the app tries to connect to the database, it fails with a "connection refused" error due to network misconfiguration.

## 🔧 What’s Next
- The solution involves setting up a shared network and using service names (not `localhost`) for DB access.
- This configuration step will be covered in the next part.

## 📝 Takeaway
- Docker Compose simplifies multi-container setups.
- Configuration can be tricky, but it’s done once per project.
- Proper networking is crucial for container-to-container communication.


# 🐳 Docker Compose: Connecting Spring Boot with PostgreSQL

## 🎯 Goal
Set up a Spring Boot app and PostgreSQL database to run as **separate Docker containers** that can communicate with each other using **Docker Compose**.

---

## 🔧 Key Fixes & Concepts

### 1. 🛠 Update Database Host
- **Problem**: Spring Boot app couldn't connect to PostgreSQL.
- **Cause**: `localhost` was used as the DB host in `application.properties`.
- **Fix**: Replace `localhost` with `postgres` (the **service name** in `docker-compose.yml`) so that Docker's internal DNS can resolve it.

### 2. 🔐 Correct DB Credentials
- **Ensure** the Spring Boot app uses the correct:
  - **Username**: `Navin`
  - **Password**: `1234`
  - **Database name**: `code`

These values must match what's configured in the PostgreSQL service in Docker Compose.

---

## 🔁 Build and Run Flow

1. Skip tests during build to avoid DB-related test failures.
2. Rebuild the JAR using Maven with tests disabled.
3. Restart containers using Docker Compose to apply changes.

---

## 🔌 Networking: Making Containers Talk

### Problem:
Containers are running but not communicating.

### Solution:
Use a **custom Docker network**:
- Define a network (`s-network`) in the `docker-compose.yml`.
- Attach both the Spring Boot app and PostgreSQL services to the same network using the `networks` property.

This ensures both containers are on the same virtual network and can reference each other by service name.

---

## 🌐 Port Issue Resolved

- Originally, the app was running on a port different from what was expected in the browser.
- Fixed by ensuring the app runs on the correct mapped port (e.g., `8090`).
- Once corrected, API endpoints (like `/students`) returned expected results.

---

## ✅ Final Outcome

- Two Docker containers (Spring Boot app & PostgreSQL) were successfully run via Docker Compose.
- A custom Docker network ensured reliable inter-container communication.
- The app correctly fetched data from the PostgreSQL container.

---

## 📦 What’s Next?

### Volume Persistence
Currently, container data is lost on restart. The next step is to:
- Introduce **Docker Volumes** to persist PostgreSQL data across container restarts.
- This allows the database to retain new records even after containers are rebuilt or stopped.

Stay tuned for the next video where we cover Docker volumes!



# 📦 Docker Volumes: Persisting PostgreSQL Data in Containers

## 🎯 Goal
Ensure **PostgreSQL data persists** even after Docker containers are stopped, removed, or rebuilt, using **Docker volumes**.

---

## 🧪 Testing Data Persistence

- A new REST endpoint (`/addStudent`) was added to insert dummy student data into the database.
- After hitting this endpoint multiple times, new entries were visible in the database.
- However, restarting the containers using `docker-compose down` followed by `up` caused the newly added entries to disappear.

---

## ❌ Problem

- **Data loss** occurs when containers are removed because containers store data **ephemerally**.
- Without volume mapping, all data stored in the PostgreSQL container is lost on removal.

---

## ✅ Solution: Docker Volumes

### What Are Volumes?
- Volumes are **persistent storage locations** on your host machine.
- They ensure database data is stored **outside the container lifecycle**, so it survives container restarts or deletions.

### How It Was Fixed:
1. A volume named `postgres-s-data` was defined in the `docker-compose.yml`.
2. This volume was mapped to the **PostgreSQL data directory** inside the container.
3. After rebuilding and restarting containers, previously inserted data was still present.

---

## 🔁 Verifying Persistence

- After volume setup:
  - Two new students were added via `/addStudent`.
  - Containers were restarted.
  - Upon refresh, the data remained — **proving persistence**.

---

## 🏁 Final Outcome

- Application data is now **retained across container restarts** using Docker volumes.
- This simulates a production-ready setup where database state is preserved.

---

## 🚀 What's Next?

- This marks the end of the basic Docker series.
- Next steps may include:
  - Deploying Dockerized apps to the **cloud**
  - Managing containers using orchestration tools like **Kubernetes**

Thanks for following along!
