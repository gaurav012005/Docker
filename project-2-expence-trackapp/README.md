# 🚀 Expense Tracker App - Dockerized 🐳

Welcome to the **Expense Tracker App**! This project is a full-stack web application built with **Spring Boot**, **Thymeleaf**, and **MySQL**, containerized using **Docker** for seamless deployment. 📦

---

## 📌 Tech Stack
- **Apache Maven** 🪶 - Build and dependency management
- **Spring Boot** 🌱 - Backend framework
- **Thymeleaf** 🖥️ - Templating engine for UI
- **MySQL** 🛢️ - Database for storing expenses
- **Docker & Docker Compose** 🐳 - Containerization and orchestration

---

## 🛠️ Prerequisites
Before running this project, ensure you have:
- **Docker** installed 🐳
- **Docker Compose** installed 📦

---

## 🚀 Getting Started
### 1️⃣ Clone the Repository
```sh
git clone https://github.com/gaurav012005/docker.git
cd docker/project-2-expence-trackapp
```

### 2️⃣ Build and Run with Docker
#### **Using Docker Compose (Recommended)**
```sh
docker-compose up -d --build
```
This command will:
✅ Pull the necessary images
✅ Build the application
✅ Start MySQL and Spring Boot app in separate containers

#### **Manual Build and Run**
If you prefer to run manually:
```sh
# Build the image
docker build -t expensetracker .

# Run MySQL container
docker run -d --name mysql-container -e MYSQL_ROOT_PASSWORD=Test@123 -e MYSQL_DATABASE=expenses_tracker -p 3306:3306 mysql:latest

# Run Expense Tracker app
docker run -d --name expensetracker --link mysql-container:mysql -p 8080:8080 expensetracker
```

---

## 📂 Project Structure
```
📁 project-2-expence-trackapp/
 ┣ 📜 Dockerfile
 ┣ 📜 docker-compose.yml
 ┣ 📂 src/
 ┃ ┣ 📂 main/java/com/expenseapp/
 ┃ ┃ ┗ 📜 Application.java
 ┃ ┣ 📂 main/resources/
 ┃ ┃ ┣ 📜 application.properties
 ┃ ┃ ┗ 📜 templates/
 ┣ 📂 sql/
 ┃ ┗ 📜 schema.sql
 ┣ 📜 README.md
```

---

## 📜 Dockerfile Explanation
```dockerfile
# Stage 1 - Build JAR
FROM maven:3.8.3-openjdk-17 AS builder
WORKDIR /app
COPY . .
RUN mvn clean package -DskipTests=true

# Stage 2 - Run Application
FROM openjdk:17-alpine
WORKDIR /app
COPY --from=builder /app/target/*.jar /app/expenseapp.jar
EXPOSE 8080
ENTRYPOINT ["java", "-jar", "/app/expenseapp.jar"]
```
- **Multi-stage build** reduces image size
- **JAR built using Maven** and copied to a lightweight Java image
- **Exposes port 8080** to serve the application

---

## 📜 Docker Compose Explanation
```yaml
version: "3.8"
services:
  mysql:
    image: mysql:latest
    container_name: mysql
    environment:
      MYSQL_ROOT_PASSWORD: Test@123
      MYSQL_DATABASE: expenses_tracker
    volumes:
      - mysql-data:/var/lib/mysql
    networks:
      - appbridge
    healthcheck:
      test: ["CMD", "mysqladmin", "ping", "-h", "localhost", "-uroot", "-pTest@123"]
      interval: 10s
      timeout: 5s
      retries: 5
      start_period: 30s
    restart: always

  mainapp:
    image: expensetracker:v1
    container_name: expensetracker
    environment:
      SPRING_DATASOURCE_USERNAME: root
      SPRING_DATASOURCE_URL: "jdbc:mysql://mysql:3306/expenses_tracker?allowPublicKeyRetrieval=true&useSSL=false"
      SPRING_DATASOURCE_PASSWORD: Test@123
    ports:
      - "8080:8080"
    networks:
      - appbridge
    depends_on:
      mysql:
        condition: service_healthy
    restart: always

networks:
  appbridge:

volumes:
  mysql-data:
```
- **Two services**: MySQL and Spring Boot App
- **MySQL container** with persistent storage
- **Depends on health check** to ensure DB is ready before app starts

---

## 🎯 Usage
- **Access the app** at: [`http://localhost:8080`](http://localhost:8080) 🌐
- **Stop the containers:** `docker-compose down` 🛑
- **View logs:** `docker logs -f expensetracker` 📜

---

## 🤝 Contributing
Want to improve the project? Feel free to fork and contribute! 🏆

---

## 📬 Contact
For queries, reach out to [Gaurav](mailto:your-email@example.com) 📧

---

### 🎉 Happy Coding! 🚀🐳
